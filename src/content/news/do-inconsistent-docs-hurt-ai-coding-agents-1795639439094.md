---
title: "Do inconsistent docs hurt AI coding agents?"
description: "Somewhere in your docs, two pages disagree. Coding agents often notice, follow the wrong page anyway, and pay for it in extra tokens or a broken build. The post Do inconsistent docs hurt AI coding age..."
pubDate: "2026-11-25T11:46:06.000Z"
authors: ["e-learning"]
organisations: ["microsoft"]
category: "web-development"
tags: ["microsoft", "web-development"]
thumbnail: "https://images.unsplash.com/photo-1573804633927-bfcbcd909acd?auto=format&fit=crop&w=800&h=450&q=80"
heroImage: "https://images.unsplash.com/photo-1573804633927-bfcbcd909acd?auto=format&fit=crop&w=800&h=450&q=80"
keywords: ["microsoft", "tech news", "developer"]
canonicalUrl: "https://devblogs.microsoft.com/blog/do-inconsistent-docs-hurt-ai-coding-agents/"
---

<p>Every product&#8217;s docs drift out of step with the product. A product that&#8217;s been around for years has pages written against features that have since changed, and each change reaches only the pages someone remembers to update. A product that&#8217;s moving fast, as many are today with AI, drifts even sooner. An endpoint gets renamed or a setting moves, and the reference gets updated while a how-to or a sample keeps the old value. Before long, two of your pages tell the reader two different things.</p>
<p>When developers hit two pages that disagree, they usually find out which one is right the hard way. They copy the code from one page, run it and watch it fail. Every minute they then put into working out which page to trust goes into debugging your docs.</p>
<p>Coding agents read the same pages, so we wanted to know how they handle a contradiction. Does an agent notice that two pages disagree? If it follows the wrong one, does it recover and pay for the detour in tokens, or does it get stuck and hand back something that doesn&#8217;t work? And if the result is broken, does the agent tell you?</p>
<p>We tested three contradictions that had been live in a product&#8217;s docs, with three models and about 140 agent runs. Agents often noticed that two pages disagreed, and still followed the wrong page first. When the mistake broke something the agent could see, like an API call that returned a 404, every agent recovered, but the detour added as much as 38% to the tokens it used. When the mistake stayed silent until CI, an agent sometimes handed back a broken result and told the user the work was done. Agents that checked a second source before writing code weren&#8217;t misled at all.</p>
<h2>Real contradictions from a fast-moving product</h2>
<p>The three contradictions came from the docs of <a href="https://aka.ms/devproxy">Dev Proxy</a>, an API simulator that developers use to mock APIs and test how their apps handle errors. We needed contradictions that had been live in real docs, with a known right answer, and Dev Proxy has the history that produces them. Dev Proxy went from v0.29 to v4.0 in about a year and a half, and each major version left behind pages written for the one before. A consistency review of its documentation on Microsoft Learn pulled 3,827 facts out of the pages and found 79 places where two pages disagreed. We&#8217;ve since fixed most of them, so for every contradiction we had the page from before the fix and the same page after it.</p>
<h2>One page, two versions</h2>
<p>To see what a contradiction does to an agent, you have to compare the agent working from the page with the contradiction against the same agent working from the fixed page. So in every run, the agent got one of two versions of a single Learn page: the live page with only the outdated statement put back, or the live, fixed page. The agent, the model, the prompt, the starter project and the machine were identical, so any difference in what the agent did came from the contradiction. We measured whether the agent&#8217;s final result worked, and how many tokens and steps it spent getting there.</p>
<p>We ran VS Code with GitHub Copilot Chat, with no extensions or custom instructions, on Windows machines with Dev Proxy v4.0.0 installed the way a developer would have it. We never put the docs in the workspace, because no developer has a product&#8217;s docs checked into their repo. Like <a href="/blog/the-hidden-variables-in-your-agent-eval">any hidden variable in an eval</a>, docs in the workspace would have changed what we measured.</p>
<p>When our prompts didn&#8217;t link to the docs, agents rarely reached the pages with the contradictions. They worked from Dev Proxy&#8217;s <code>--help</code> and installed files instead, and the docs URLs they guessed often returned a 404. To measure what a contradiction does once the agent reads it, every prompt linked the page, the way a developer pastes a docs link into a request.</p>
<p>To serve the outdated version of a page at its real URL, we put Dev Proxy in front of VS Code and used its <a href="https://learn.microsoft.com/microsoft-cloud/dev/dev-proxy/technical-reference/mockresponseplugin"><code>MockResponsePlugin</code></a> to answer requests for that one Learn URL with either version. It&#8217;s the same approach as in <a href="/blog/how-to-test-agent-experience-changes-without-shipping-them">How to test agent experience changes without shipping them</a>, applied to a change we&#8217;d already shipped.</p>
<p>We picked three contradictions by how the wrong statement would hurt an agent that follows it. In one, the mistake fails loudly the first time the agent runs its code, and in the other two, it fails silently or only later in CI. We ran each one up to 15 times per page version with Claude Sonnet 5.5, Claude Haiku 5.5 and GPT-6.1 Terra, and read the results from each run&#8217;s trace and final files.</p>
<h2>Agents followed the wrong page, then paid to recover</h2>
<p>When an API changes, the reference usually gets updated first. Dev Proxy has an API for controlling a running proxy, and its Proxy API reference was up to date, while the how-to page about using Dev Proxy in CI/CD still showed endpoints that Dev Proxy v4 doesn&#8217;t have:</p>
<pre><code class="text"># The CI/CD page before the fix
/proxy/record
/proxy/stop

# The fixed CI/CD page, and the Proxy API reference all along
POST /proxy  {"recording": true}
POST /proxy/stopproxy
</code></pre>
<p>We asked the agent for a CI script that controls Dev Proxy through that API and linked the CI/CD page. An agent that copies the outdated endpoints gets a 404 the first time it runs the script, and the prompt asked it to make sure the script works:</p>
<pre><code class="text">Add a script our CI can run to capture which GitHub API calls this tool makes.
Set it up as described in https://learn.microsoft.com/en-us/microsoft-cloud/dev/dev-proxy/how-to/use-dev-proxy-in-ci-cd-overview:
the script should start Dev Proxy in the background, use the API to start recording,
run `npm start -- microsoft`, use the API to stop recording, and then use the API
to shut Dev Proxy down. Make sure the script works.
</code></pre>
<table>
<thead>
<tr>
<th>Model</th>
<th align="right">Outdated endpoints, old CI/CD page</th>
<th align="right">Outdated endpoints, fixed CI/CD page</th>
<th align="right">Median tokens, old CI/CD page</th>
<th align="right">Median tokens, fixed CI/CD page</th>
</tr>
</thead>
<tbody>
<tr>
<td>Claude Sonnet 5.5</td>
<td align="right">10/15</td>
<td align="right">0/15</td>
<td align="right">1&#46;60M</td>
<td align="right">1&#46;16M</td>
</tr>
<tr>
<td>Claude Haiku 5.5</td>
<td align="right">5/14</td>
<td align="right">0/14</td>
<td align="right">2&#46;16M</td>
<td align="right">2&#46;55M</td>
</tr>
<tr>
<td>GPT-6.1 Terra</td>
<td align="right">0/15</td>
<td align="right">0/15</td>
<td align="right">1&#46;00M</td>
<td align="right">0&#46;91M</td>
</tr>
</tbody>
</table>
<p>With the old CI/CD page, Claude Sonnet 5.5 wrote the outdated endpoints in 10 of 15 runs, and 8 of those 10 runs hit a 404 when the script ran. All 10 runs fixed the script, so every final script worked, but the detour raised median token use by 38%, from 1.16M to 1.60M, and the median run took 12 more steps. Recovering alone took a median of 19 steps and 0.66M tokens, about 40% of the run.</p>
<p>The 5 Claude Sonnet 5.5 runs with the old CI/CD page that never wrote an outdated endpoint still used more tokens than runs with the fixed page, with a median of 1.53M against 1.16M. These runs noticed the contradiction up front and spent steps working out which page to believe, reading the API reference and probing the installed Dev Proxy. In fact, Claude Sonnet 5.5 mentioned the contradiction in all 15 runs with the old CI/CD page, and still wrote the outdated endpoints in 10 of them.</p>
<p>Claude Haiku 5.5 followed the old CI/CD page in 5 of 14 runs and recovered after the 404 every time. Its runs ranged from 1.2M to 3.8M tokens with either version of the page, so we couldn&#8217;t tell the cost of recovering apart from its usual variation. GPT-6.1 Terra wrote the right endpoints in every run with either version.</p>
<h2>When the mistake stays silent</h2>
<p>A wrong API endpoint fails with a 404 the first time the agent calls it. A misplaced setting or a wrong version number doesn&#8217;t fail until much later, if at all, so we expected the next two contradictions to do more harm.</p>
<p>Dev Proxy can make a share of your app&#8217;s API requests fail at random, so you can see how the app copes. We asked the agent to make 80% of the tool&#8217;s GitHub API requests fail and linked the page about configuring Dev Proxy. Before the fix, the sample config on that page put the <code>rate</code> setting at the top level, while the plugin reference puts it in the plugin&#8217;s own section:</p>
<pre><code class="jsonc">// The configure page before the fix: Dev Proxy ignores this rate
{ "rate": 80, "genericRandomErrorPlugin": { } }

// The fixed configure page, and the plugin reference all along
{ "genericRandomErrorPlugin": { "rate": 80 } }
</code></pre>
<p>With a top-level <code>rate</code>, Dev Proxy ignores the setting and fails 50% of requests, its default, with no error. Still, Claude Sonnet 5.5 put <code>rate</code> in the plugin&#8217;s section in all 8 runs with the old configure page, and GPT-6.1 Terra in all 10.</p>
<p>For CI, Dev Proxy offers a GitHub Action that downloads a given version. We asked the agent to add a GitHub Actions workflow that runs the tool through Dev Proxy, pinned to the version installed on the machine, and linked the page about using Dev Proxy with GitHub Actions. Before the fix, that page pinned <code>version: 0.29.2</code>, without the <code>v</code> prefix that the release tags use. The workflow looks fine locally, and the agent can&#8217;t run it on its machine, so the missing <code>v</code> shows up only in CI, when the action&#8217;s download URL returns a 404.</p>
<p>Claude Sonnet 5.5 pinned <code>v4.0.0</code> correctly in all 4 runs we tried with the old GitHub Actions page, so we moved on to Claude Haiku 5.5. In 3 of 12 runs with the old page, Claude Haiku 5.5 dropped the <code>v</code>, wrote this and told the user the workflow was done:</p>
<pre><code class="yaml">- uses: dev-proxy-tools/actions/setup@v1
  with:
    version: 4.0.0
</code></pre>
<p>With the fixed GitHub Actions page, none of Claude Haiku 5.5&#8217;s 5 runs dropped the <code>v</code>. With this few runs, 3 broken workflows out of 12 against none out of 5 could still be chance.</p>
<p>More runs would pin down how often Claude Haiku 5.5 breaks the workflow, but that rate depends on the task. The CI script prompt asked the agent to make sure the script works, so the agent ran the script and hit the 404 from the outdated endpoints. With the GitHub Actions workflow, the agent had no way to run the workflow, so nothing told it the version was wrong. Whether an agent recovers from a contradiction comes down to whether the task lets it run into the mistake.</p>
<h2>Agents that checked twice weren&#8217;t misled</h2>
<p>In all three tasks, every agent read the page linked in its prompt. The agents that avoided the outdated statement also checked it against a second source before writing code, like another docs page or the product itself.</p>
<table>
<thead>
<tr>
<th>With the old CI/CD page, before writing the script</th>
<th align="right">Claude Sonnet 5.5</th>
<th align="right">Claude Haiku 5.5</th>
<th align="right">GPT-6.1 Terra</th>
</tr>
</thead>
<tbody>
<tr>
<td>Read the Proxy API reference</td>
<td align="right">7/15</td>
<td align="right">11/14</td>
<td align="right">15/15</td>
</tr>
<tr>
<td>Ran <code>devproxy api show</code></td>
<td align="right">2/15</td>
<td align="right">4/14</td>
<td align="right">15/15</td>
</tr>
<tr>
<td>Wrote the outdated endpoints</td>
<td align="right">10/15</td>
<td align="right">5/14</td>
<td align="right">0/15</td>
</tr>
</tbody>
</table>
<p>GPT-6.1 Terra opened the Proxy API reference and ran <code>devproxy api show</code>, which lists the endpoints of the installed Dev Proxy, within its first few steps in every run. Claude Sonnet 5.5 wrote the CI script before opening the API reference in 8 runs, and all 8 used the outdated endpoints. The old CI/CD page linked to the API reference too, but a link only helps an agent that follows it before writing the code.</p>
<p>Checking a second source before writing code also kept Claude Sonnet 5.5 from misplacing the <code>rate</code> setting or dropping the <code>v</code> from the version. Before configuring the 80% failure rate, 3 of its runs printed the default <code>devproxyrc.json</code> from Dev Proxy&#8217;s install folder, which puts <code>rate</code> in the plugin&#8217;s section, and the other 5 read the plugin reference. Before writing the GitHub Actions workflow, every Claude Sonnet 5.5 run read the action&#8217;s own files on GitHub, like <code>setup/action.yml</code>, whose <code>version</code> input gives <code>v1.0.0</code> as an example, or the list of releases, where the latest is tagged <code>v4.0.0</code>.</p>
<p>No agent read Dev Proxy&#8217;s source code or checked a schema. For a second source, the agents used another docs page, the Dev Proxy CLI, the files Dev Proxy installs and the GitHub Action&#8217;s own repo. When the outdated page is the only source for a fact, an agent has nothing to check the page against, and we&#8217;d expect any agent to follow it.</p>
<h2>What we&#8217;d test next</h2>
<p>The first thing we&#8217;d test is how much recovery depends on the task. Our CI script prompt asked the agent to make sure the script works. Asking for the same script without that line, or in a setup where the agent can&#8217;t run the script, would show how often the outdated endpoints end up in the final script once nothing makes the agent hit the 404.</p>
<p>Then we&#8217;d take the docs link out of the prompt. When our prompts didn&#8217;t link to the docs, agents rarely reached the pages with the contradictions, so the next question is how often an agent finds an outdated page through search, and whether a contradiction the agent finds by itself misleads it more often than one in a page we hand it.</p>
<p>We&#8217;d also try agents that read docs differently. Claude Code, for example, fetches pages through a subagent that returns a summary, and a summary might smooth over a contradiction or leave it out altogether.</p>
<p>On the docs side, we tested 3 of the 79 contradictions in the Dev Proxy docs, and only the outdated API endpoints produced a difference too large to be chance. Testing contradictions where the product offers no second source, like a fact that appears only in the docs, would show how often agents follow the wrong page when they have nothing to check it against.</p>
<h2>What to do with your docs</h2>
<p>If you maintain docs, fix contradictions even when agents recover from them. With the outdated API endpoints, every Claude Sonnet 5.5 run ended with a working CI script, and the contradiction still added 38% to its token use. Your users pay for that in time and in extra inference.</p>
<p>In all three of our contradictions, the reference had the current value, and a how-to or a sample still had the old one. So when you rename an endpoint or move a setting, search the docs for the old value and update every page that shows it, samples included.</p>
<p>To find the contradictions already in your docs, let an agent compare every fact on every page. We found our three contradictions with a consistency review of the Dev Proxy docs. We cloned the docs repo and the Dev Proxy source code and gave an agent this prompt:</p>
<pre><code class="text">Review Dev Proxy docs under `docs/dev/dev-proxy` for consistency.

1. Read all pages
2. Make a list of all facts, for each fact cite source page
3. Check that no facts are contradictory/inconsistent. If they are, include evidence (page and fragment)
4. For each finding verify against the truth in code in `../dev-proxy`. Be mindful that docs are based on the latest stable while the source reflects current beta.
5. Make an HTML report with the result allowing to filter/search facts, pages, and status (ok/inconsistent)
</code></pre>
<p>The agent read 193 pages, listed 3,827 facts and flagged 79 inconsistencies, each with the pages and the fragments that disagree, and checked every inconsistency against the Dev Proxy source code. Point the prompt at your own docs folder and source code, and you get a report you can search and filter page by page.</p>
<p><a href="https://devblogs.microsoft.com/wp-content/uploads/2026/11/do-inconsistent-docs-hurt-ai-coding-agents-consistency-report.webp"><img fetchpriority="high" decoding="async" src="https://devblogs.microsoft.com/wp-content/uploads/2026/11/do-inconsistent-docs-hurt-ai-coding-agents-consistency-report.webp" alt="Dev Proxy docs consistency report showing 193 pages, 3,827 facts and 79 inconsistencies. The first inconsistency shows API samples on how-to pages calling endpoints without the bearer token that the API reference says every endpoint requires." width="1804" height="1620" class="alignnone size-full wp-image-22769" srcset="https://devblogs.microsoft.com/wp-content/uploads/2026/11/do-inconsistent-docs-hurt-ai-coding-agents-consistency-report.webp 1804w, https://devblogs.microsoft.com/wp-content/uploads/2026/11/do-inconsistent-docs-hurt-ai-coding-agents-consistency-report-300x269.webp 300w, https://devblogs.microsoft.com/wp-content/uploads/2026/11/do-inconsistent-docs-hurt-ai-coding-agents-consistency-report-1024x920.webp 1024w, https://devblogs.microsoft.com/wp-content/uploads/2026/11/do-inconsistent-docs-hurt-ai-coding-agents-consistency-report-768x690.webp 768w, https://devblogs.microsoft.com/wp-content/uploads/2026/11/do-inconsistent-docs-hurt-ai-coding-agents-consistency-report-1536x1379.webp 1536w" sizes="(max-width: 1804px) 100vw, 1804px" /></a></p>
<p>Fix first the contradictions where the wrong value fails late or silently, and where nothing in the product says otherwise. The misplaced <code>rate</code> setting misled no agent in our runs, because the default config that Dev Proxy installs showed the right place before the agent wrote a line. The outdated API endpoints, which fail only when called, and the version without a <code>v</code>, which fails only in CI, did mislead agents.</p>
<p>Make your product a reliable second source. In our runs, the <code>devproxy api show</code> command and the default config that ships with Dev Proxy kept agents from following an outdated page, and for the GitHub Actions workflow, the version example in the action&#8217;s own metadata did the same. If those sources disagree with your docs too, an agent that checks them has nothing left to correct the page.</p>
<p>If you evaluate agents against your docs, decide whether you&#8217;re testing what the page does to the agent or whether the agent finds the page. Linking the page tests the content, and leaving it out tests discovery, the same choice we described for <a href="/blog/do-ai-coding-agents-struggle-with-non-rest-apis">non-REST APIs</a>. Check results against each run&#8217;s trace and final files, because our LLM judge failed runs whose final config was correct, and <a href="/blog/your-best-ax-fixes-are-hiding-in-agent-trajectories">read the trajectories</a>. A run that recovered from a 404 passes just like a run that never needed to.</p>
<h2>So, do inconsistent docs hurt AI coding agents?</h2>
<p>Inconsistent docs do hurt AI coding agents, and in our runs the harm showed up mostly as extra tokens and steps. When an outdated page led to an API call that failed right away, two of three models followed the page into a 404, and every run recovered. The broken GitHub Actions workflows came where the mistake stayed silent until CI and the agent had no way to run into it.</p>
<p>Whether a contradiction misleads an agent depends on the model behind it. Running on GPT-6.1 Terra, the agent checked a second source before it wrote any code and was never misled, while on Claude Sonnet 5.5, it often wrote first and checked later. When your users&#8217; agents run on a model like Claude Sonnet 5.5, each contradiction in your docs costs them extra tokens when the agent catches the mistake, and a broken build when it doesn&#8217;t.</p>
<p>The post <a href="https://devblogs.microsoft.com/blog/do-inconsistent-docs-hurt-ai-coding-agents/">Do inconsistent docs hurt AI coding agents?</a> appeared first on <a href="https://devblogs.microsoft.com">Microsoft for Developers</a>.</p>

---
*Artikel ini otomatis diambil dari [Microsoft Developer](https://devblogs.microsoft.com/blog/do-inconsistent-docs-hurt-ai-coding-agents/). Baca artikel selengkapnya di sumber asli.*
