---
title: "Your best AX fixes are hiding in agent trajectories"
description: "All five agent runs produced a working script, and nothing in the eval score looked wrong. The trajectories told a different story. Every run read a DLL as raw bytes to find plugin names, and three sc..."
pubDate: "2026-11-05T15:40:49.000Z"
authors: ["e-learning"]
organisations: ["microsoft"]
category: "web-development"
tags: ["microsoft", "web-development"]
thumbnail: "https://images.unsplash.com/photo-1573804633927-bfcbcd909acd?auto=format&fit=crop&w=800&h=450&q=80"
heroImage: "https://images.unsplash.com/photo-1573804633927-bfcbcd909acd?auto=format&fit=crop&w=800&h=450&q=80"
keywords: ["microsoft", "tech news", "developer"]
canonicalUrl: "https://devblogs.microsoft.com/blog/your-best-ax-fixes-are-hiding-in-agent-trajectories/"
---

<p>An eval score tells you whether the agent finished the task. To improve how agents work with your product, you need to see where they struggled, and that&#8217;s in the trajectory, the record of every step the agent took and why. Agents are persistent. They guess another URL after a 404 and try values until an error goes away, so the struggle rarely shows up in the score.</p>
<p>We saw this when we evaluated <a href="https://aka.ms/devproxy">Dev Proxy</a>, a local proxy that intercepts your app&#8217;s HTTP requests. We asked GitHub Copilot Chat with Claude Sonnet 5.5 and no extensions, five times, to write a CI script that uses the Dev Proxy API to record which GitHub APIs a Node.js tool calls. All five scripts worked. Yet the trajectories showed 15 problems, and each run took 29 to 39 requests and up to 1.2 million tokens.</p>
<h2>What to look for in a trajectory</h2>
<p>Agents can trip over your technology in many ways. We picked the patterns we think you&#8217;re most likely to find in your own trajectories, from the obvious to the surprising, and for each one show what it looked like with Dev Proxy and which fix it pointed to.</p>
<h3>Agents guess your URLs before they search</h3>
<p>Agents often skip the search. They fetch the URL where they expect your page to be, based on what they remember about your docs and how docs sites are usually organized. Every wrong guess costs a step and tokens, and after a miss or two, some agents give up on your docs entirely. From then on, they work from memory and trial and error, which is where outdated and invented answers come from.</p>
<p>Before reading any docs, four of the five Dev Proxy runs tried to fetch <code>https://learn.microsoft.com/en-us/microsoft-cloud/dev/dev-proxy/technical-reference/api</code>, which returned a 404. The model remembered where Dev Proxy keeps its reference and got only the page name wrong, since the real page is <code>technical-reference/proxy-api</code>. Two runs spent five to seven extra steps finding it, and the other two gave up on the docs altogether.</p>
<p>The guessing continued after the agents read the docs. Once one run found the real API page, it fetched <code>technical-reference/plugins/executionsummaryplugin</code>, likely because the docs&#8217; navigation groups plugins in their own section. That page lives at <code>technical-reference/executionsummaryplugin</code>. In total, eight fetches across four runs returned a 404.</p>
<p><strong>Check your product:</strong> A URL structure that feels logical to your team can still send agents to the wrong place, because agents predict paths from patterns, like the structure of your navigation. Look through your 404 logs for docs paths that almost exist. The logs might not tell you an agent sent them, but each one is a redirect worth adding. Print your docs URL in your CLI output too, so agents don&#8217;t have to guess.</p>
<h3>Agents probe when your interface stops one step short</h3>
<p>Teams often write help output and error messages for a person who can open the docs when something&#8217;s unclear. An agent, however, treats them as the spec. When they say what&#8217;s wrong without saying what&#8217;s right, the agent guesses a value, reads the next error and guesses again. Each round costs a request, and if no guess works, the agent either stops or builds around the gap.</p>
<p>Dev Proxy lists its API endpoints like this:</p>
<pre><code class="text">POST   /proxy                         Update proxy status (e.g. start/stop recording)
GET    /proxy/rootCertificate         Get the root certificate
</code></pre>
<p>It doesn&#8217;t show request bodies or required parameters. So when one run called <code>/proxy/rootCertificate</code> exactly as listed, it got a 400:</p>
<pre><code class="json">{ "errors": { "format": ["The format field is required."] } }
</code></pre>
<p>The error doesn&#8217;t list valid values, so the agent tried <code>pem</code>, <code>crt</code>, <code>cer</code> and <code>der</code>. Only <code>crt</code> worked.</p>
<p><strong>Check your product:</strong> Run your CLI help and trigger your most common errors. Make sure each one tells the agent what a valid input looks like, with an example or the list of allowed values.</p>
<h3>Every run reinvents the same workaround</h3>
<p>When your product lacks something the task needs, agents build it themselves. In a single run, that&#8217;s noise. When every run writes the same workaround, your product is missing a feature, and every developer whose agent hits that gap pays for the workaround in tokens and ends up with code that&#8217;s more fragile than the feature would have been.</p>
<p>Dev Proxy writes its reports shortly after recording stops, with no signal when they&#8217;re done. Four runs polled the output folder for up to 30 seconds. The fifth just waited two seconds:</p>
<pre><code class="javascript">await api('POST', '/proxy', { recording: false });
await sleep(2000); // let the reporters flush their files
</code></pre>
<p>On a busy CI runner, that will eventually fail. Polling is safer, but every run that polled had to guess which file to wait for.</p>
<p>The strangest workaround though, was about plugins. Dev Proxy has no command that lists them, so all five runs read the plugin DLL as raw bytes and searched it for class names:</p>
<pre><code class="powershell">$b = [IO.File]::ReadAllBytes("$d\plugins\DevProxy.Plugins.dll")
$s = [Text.Encoding]::Unicode.GetString($b)
[regex]::Matches($s, '\b[A-Z]\w*(Plugin|Reporter)\b') | % { $_.Value } | sort -u
</code></pre>
<p>It worked, so no score would ever show it, but it&#8217;s far from robust not to mention efficient. A trajectory does show it, because the same odd step shows up in every run.</p>
<p><strong>Check your product:</strong> Every operation that finishes after the call returns needs a way to tell the agent it&#8217;s done. Everything an agent has to choose from, like plugins or templates, needs a command that lists it.</p>
<h3>Agents fill gaps from memory</h3>
<p>Developers use your tool together with other runtimes and frameworks, and your docs often stop at your tool&#8217;s edge. When the docs don&#8217;t explain how the pieces fit together, agents fill the gap with what they learned in training. If the model happens to know, the run succeeds and you never hear about it. If it doesn&#8217;t, or it learned an older version, the agent writes code that looks right and quietly does nothing.</p>
<p>For example, to route a Node.js app through Dev Proxy, the app&#8217;s <code>fetch</code> needs an opt-in setting that only recent Node versions support. Every run knew this from memory:</p>
<blockquote>
<p>since undici doesn&#8217;t respect HTTPS_PROXY by default, I need NODE_USE_ENV_PROXY enabled, which requires Node 22.21.0+ or 24.0+.</p>
</blockquote>
<p>This time, all five got it right, but none found it in the Dev Proxy docs. A model that knew less would have recorded nothing, without an error, and <a href="/blog/your-models-knowledge-cutoff-isnt-its-capability-cutoff">you can&#8217;t predict what a model knows about your product</a>. In a trajectory, these gaps show up as reasoning that states a fact without citing a source.</p>
<p><strong>Check your product:</strong> List the runtimes and frameworks your developers pair your tool with, and make sure your docs show the setup for each. For the ones they skip, run a task and see what the agent fills in.</p>
<h3>Agents reject defaults built for a first run</h3>
<p>Default configs and starter templates usually serve a first run. They let developers check that the setup works and see what the product can do within minutes. For that job, they&#8217;re the right default, but for a specific task, they often get in the way. When an agent rejects your default, it builds its own setup from scratch, and each run builds a different one, so developers get inconsistent results for the same task.</p>
<p>Dev Proxy&#8217;s default config simulates random errors on a sample API. It shows right away that requests are flowing through the proxy and what Dev Proxy can do with them, but for recording real traffic, it&#8217;s in the way. The agents avoided it immediately:</p>
<blockquote>
<p>I should avoid the default config since it&#8217;ll inject random errors, so I need to set up my own configuration instead.</p>
</blockquote>
<p>When they created a new config, it came out empty. So each run built its own, and they picked three different plugins for the same goal.</p>
<p><strong>Check your product:</strong> Keep the default that makes a first run work, and next to it, offer a preset for each common task developers give agents, so every run starts from the same, correct place. List those presets in your CLI help, which agents read early.</p>
<h3>Agents trust whatever search shows them</h3>
<p>Agents often read your guidance through a search engine, and search indexes serve snapshots. A page you updated last month can still show up in search with its old content, and the same goes for your translations and the skills you ship for agents. Agents can&#8217;t tell an old snapshot from the current page, so after a breaking change, an outdated copy that ranks well keeps producing broken code long after you&#8217;ve fixed the source.</p>
<p>Since v3.3.1, the Dev Proxy API requires a token. One run&#8217;s search returned two results indexed before that change. One was the Ukrainian translation of the API reference, crawled in June. The other was a file from Dev Proxy&#8217;s own agent skill, which had been updated with auth six weeks before the run. Both showed calls without a token, which now return a 401:</p>
<pre><code class="bash">curl -X POST http://localhost:8897/proxy \
  -H "Content-Type: application/json" \
  -d '{"recording": true}'
</code></pre>
<p>The run got auth right only because it had already read the CLI help. CLI help ships with the installed version, so it can&#8217;t go stale.</p>
<p><strong>Check your product:</strong> Take a few tasks developers give agents and run the web searches an agent would run for them, like &#8220;<your product> API authentication&#8221;. Check whether the results, including translations and your own skills, describe your current version. You can&#8217;t refresh someone else&#8217;s index, but you can state version requirements at the top of each page, where they show up in search snippets and tell the agent which version a snapshot describes.</your></p>
<h3>Agents assume the defaults your help text doesn&#8217;t state</h3>
<p>Agents learn your CLI from its help text, and they take it literally. When an option reads like a switch you turn on, agents assume it&#8217;s off unless they pass it. If it&#8217;s actually on by default, they never pass the value that turns it off, because nothing told them to. For most options, the cost is a run that behaves differently than the agent expected. For a default that changes something outside your tool, the agent ships a script with a side effect it doesn&#8217;t know about, and because the task still succeeds, your eval won&#8217;t catch it either.</p>
<p>Dev Proxy sets itself as the system proxy by default, which routes all of the machine&#8217;s traffic through it. On a developer&#8217;s machine, that&#8217;s convenient, because every app goes through the proxy without extra setup. But the help text reads like an opt-in, and it doesn&#8217;t mention that turning the option off takes an explicit <code>--as-system-proxy false</code>:</p>
<pre><code class="text">--as-system-proxy                      Set Dev Proxy as the system proxy
</code></pre>
<p>One run found out by accident, when a status command showed <code>System proxy: Yes</code>, and turned it off. Another turned it off in its config, and the other three never noticed. On a Windows CI runner, their scripts would reroute all the runner&#8217;s traffic, and their kill fallback would skip the cleanup that undoes it.</p>
<p>Still, all three passed, because the system proxy wasn&#8217;t part of this scenario&#8217;s evaluation criteria. Meanwhile, the judge wrongly failed the run that did turn it off. The score was wrong in both directions, and only the trajectories showed it.</p>
<p><strong>Check your product:</strong> Read your help text the way an agent would. Every option should state its default, and every option that&#8217;s on by default should show how to turn it off. Pay extra attention to defaults that change anything outside your tool, like system settings or installed certificates, and consider turning those off when your tool runs unattended.</p>
<h2>Fix your product and docs before you reach for a skill</h2>
<p>Some problems need a change to your product, like a command that lists plugins or help text that states its defaults. Others only need better docs, like a redirect for a guessed URL or a setup guide for the runtime your developers use. The pattern you found in the trajectory usually tells you which one, and both are <a href="/blog/the-ax-stack-whats-fixed-where-you-can-win">parts of the AX stack you control</a>.</p>
<p>Shipping a skill is tempting, because it feels like less work than changing your product. But a skill moves the problem to your developers. Agents now get it right only for developers who find the skill and install it. Those developers also have to keep it current, because an installed skill is one more copy of your docs that falls behind your product. In comparison, a fix in your product or docs reaches every agent that uses them, with nothing for developers to install.</p>
<p>With SPFx, a trajectory showed the agent reading the release notes while it worked. So instead of shipping a skill, we <a href="https://devblogs.microsoft.com/microsoft365dev/behind-spfx-dev-skills-testing-what-agents-know-and-fixing-what-they-miss/">fixed the release notes</a>, and every run used the right tool without anything extra installed. Start with your product and docs, and reach for a skill when they can&#8217;t carry what the agent needs.</p>
<h2>How to start reading trajectories</h2>
<p>Read your trajectories yourself, including the ones from passing runs. You know your product inside out, so you&#8217;ll spot workarounds that an agent reviewer would take for normal steps. <a href="https://marketplace.visualstudio.com/items?itemName=waldek.atif-visualizer">ATIF Preview</a> turns a trajectory file into a step-by-step timeline in VS Code, and an agent can help you compare runs, as long as it cites the step behind each finding.</p>
<p>Even then, each finding is a hypothesis until you run the same tasks with and without the fix and compare quality and cost, as described in <a href="/blog/is-your-agent-extension-actually-working">how to measure AI agent extension effectiveness</a>. Every change you make should leave agents better off with your product than before, and the measurement is how you prove it did.</p>
<p>The post <a href="https://devblogs.microsoft.com/blog/your-best-ax-fixes-are-hiding-in-agent-trajectories/">Your best AX fixes are hiding in agent trajectories</a> appeared first on <a href="https://devblogs.microsoft.com">Microsoft for Developers</a>.</p>

---
*Artikel ini otomatis diambil dari [Microsoft Developer](https://devblogs.microsoft.com/blog/your-best-ax-fixes-are-hiding-in-agent-trajectories/). Baca artikel selengkapnya di sumber asli.*
