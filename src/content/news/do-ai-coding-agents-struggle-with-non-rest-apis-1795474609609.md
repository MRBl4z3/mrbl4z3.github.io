---
title: "Do AI coding agents struggle with non-REST APIs?"
description: "Should your next API be REST, or something else? And which one will AI coding agents handle best? We tested REST, POST-only and RPC-style APIs with six models across 1,356 runs, and what decided wheth..."
pubDate: "2026-11-23T10:25:28.000Z"
authors: ["e-learning"]
organisations: ["microsoft"]
category: "web-development"
tags: ["microsoft", "web-development"]
thumbnail: "https://images.unsplash.com/photo-1573804633927-bfcbcd909acd?auto=format&fit=crop&w=800&h=450&q=80"
heroImage: "https://images.unsplash.com/photo-1573804633927-bfcbcd909acd?auto=format&fit=crop&w=800&h=450&q=80"
keywords: ["microsoft", "tech news", "developer"]
canonicalUrl: "https://devblogs.microsoft.com/blog/do-ai-coding-agents-struggle-with-non-rest-apis/"
---

<p>Plenty of APIs skip REST conventions. Some send every write over POST, and some name every endpoint after an action, like <code>/updateBin</code> or <code>/deleteBin</code>. On paper, agents should have a harder time with those. REST maps the intent onto the HTTP method, so changing a record is a <code>PATCH</code> and removing it is a <code>DELETE</code>. In comparison, an RPC-style API makes the agent work out what each endpoint is called and what it does. We expected that to cost agents something, either in failed tasks or in extra calls and tokens.</p>
<p>It didn&#8217;t, at least when the agent had the docs. Across 1,356 runs with six models from two vendors, agents succeeded just as often with REST, POST-only and RPC-style APIs, at the same cost in failed calls and tokens. Without docs, REST won by a wide margin, because agents can guess REST URLs from convention and have no way to guess what a non-REST API calls its endpoints.</p>
<h2>Where non-REST APIs could trip up an agent</h2>
<p>&#8220;Non-REST&#8221; usually changes two things at once. It limits the API to GET and POST, and it replaces resource paths with endpoints named after verbs, and either change could cost an agent. A model that has seen a lot of REST might reach for <code>DELETE</code> out of habit and get a 404. A model faced with a dozen verb-named endpoints might pick the wrong one, for example a full replace where a partial update was needed, and wipe data it was supposed to keep. If that happened, agents would complete fewer tasks or need more failed calls and tokens to get there, so we measured both.</p>
<h2>One API, three styles</h2>
<p>We built a fictional inventory API called Binstock, with bins and the items stored in them, so no model could have memorized it. We served it in three styles with identical data and behavior. Here&#8217;s the same partial update in each:</p>
<pre><code class="http"># rest
PATCH /bins/B-103

# postonly
POST /bins/B-103/update

# rpc
POST /updateBin?code=B-103
</code></pre>
<p>The middle style is there to separate the two changes. POST-only keeps the resource paths from REST but drops <code>PUT</code>, <code>PATCH</code> and <code>DELETE</code>, so comparing it with REST isolates the HTTP methods. Comparing it with RPC isolates the naming.</p>
<table>
<thead>
<tr>
<th>Operation</th>
<th>REST</th>
<th>POST-only</th>
<th>RPC</th>
</tr>
</thead>
<tbody>
<tr>
<td>Read a bin</td>
<td><code>GET /bins/{code}</code></td>
<td><code>GET /bins/{code}</code></td>
<td><code>GET /getBin?code=…</code></td>
</tr>
<tr>
<td>Create a bin</td>
<td><code>POST /bins</code></td>
<td><code>POST /bins</code></td>
<td><code>POST /createBin</code></td>
</tr>
<tr>
<td>Replace a bin</td>
<td><code>PUT /bins/{code}</code></td>
<td><code>POST /bins/{code}/replace</code></td>
<td><code>POST /replaceBin</code></td>
</tr>
<tr>
<td>Update a bin</td>
<td><code>PATCH /bins/{code}</code></td>
<td><code>POST /bins/{code}/update</code></td>
<td><code>POST /updateBin</code></td>
</tr>
<tr>
<td>Delete a bin</td>
<td><code>DELETE /bins/{code}</code></td>
<td><code>POST /bins/{code}/delete</code></td>
<td><code>POST /deleteBin</code></td>
</tr>
</tbody>
</table>
<p>We mocked each style with <a href="https://aka.ms/devproxy">Dev Proxy</a> and its <a href="https://learn.microsoft.com/en-us/microsoft-cloud/dev/dev-proxy/technical-reference/crudapiplugin"><code>CrudApiPlugin</code></a>, with in-memory data that reset for every run. Any path or method a style didn&#8217;t define returned a 404, so a <code>DELETE</code> against the RPC API failed just like it would against a real one, and every wrong guess showed up in the proxy log. We&#8217;ve <a href="/blog/how-to-test-agent-skills-without-hitting-real-apis">written before</a> about why mocks like this make evals repeatable. Here, they also let us serve one API in three shapes without building three servers.</p>
<p>We wrote 15 tasks, 5 reads and 10 writes, covering creates, partial updates, full replaces, deletes, multi-step changes and bulk changes. The prompts described the outcome the user wanted. A task said &#8220;Bin B-103 has been taken out of service&#8221;, because a prompt that names the operation, like &#8220;update&#8221; or &#8220;delete&#8221;, hands the agent the name of the matching RPC endpoint. One task was a deliberate trap that asked to change a single field on a bin, where a full replace would have wiped the others.</p>
<p>After each run, the grader read the full state of the API and checked that it equaled the seed data plus the intended change and nothing else, so any valid route to the right result passed. We ran everything in <a href="https://aka.ms/vally">Vally</a> with the GitHub Copilot SDK as the agent, and we set the decision rules before the first run. We&#8217;d call two API styles equivalent if the 95% confidence interval on their difference in success rate stayed within 10 points, and a general claim needed at least three models agreeing with no model pointing the other way.</p>
<h2>With the docs, every style scored the same</h2>
<p>In the main run, the prompt gave the agent the base URL and an OpenAPI description of the API. The description listed every operation but didn&#8217;t explain how replace and update differ, so the agent still had to work that out. Claude Opus 5.5, Claude Sonnet 5.5, GPT-6.1 Sol and GPT-6.1 Terra each ran every task five times in every style, 900 runs in total.</p>
<table>
<thead>
<tr>
<th>Style</th>
<th align="right">Success</th>
<th align="right">Runs without a failed call</th>
<th align="right">Tokens per run</th>
</tr>
</thead>
<tbody>
<tr>
<td>REST</td>
<td align="right">100%</td>
<td align="right">82%</td>
<td align="right">≈70k</td>
</tr>
<tr>
<td>POST-only</td>
<td align="right">100%</td>
<td align="right">82%</td>
<td align="right">≈70k</td>
</tr>
<tr>
<td>RPC</td>
<td align="right">99.7%</td>
<td align="right">77%</td>
<td align="right">≈71k</td>
</tr>
</tbody>
</table>
<p>The single RPC failure was a 10-minute timeout. RPC had about 5 points fewer runs without a failed call, which hints at a little more stumbling, but the difference wasn&#8217;t significant. REST used about 700 fewer tokens per run than RPC, roughly 1%. Each model came out equivalent in every comparison, including the ones that isolate the HTTP methods and the naming.</p>
<p>Still, the styles might have tied only because these models are too strong for the tasks, so we ran the same setup on two smaller models, Claude Haiku 5.5 and MAI Code 2 Flash, twice per task. Both completed every task in every style, with token use within about 3% across styles.</p>
<h2>Without the docs, agents had to guess</h2>
<p>Most agents can reach some documentation, which is why the docs run was the primary test. To see what happens when they can&#8217;t, we gave Claude Sonnet 5.5 and GPT-6.1 Terra only the base URL and ran every task twice per style.</p>
<table>
<thead>
<tr>
<th>Model</th>
<th align="right">REST</th>
<th align="right">POST-only</th>
<th align="right">RPC</th>
</tr>
</thead>
<tbody>
<tr>
<td>Claude Sonnet 5.5</td>
<td align="right">93%</td>
<td align="right">40%</td>
<td align="right">0%</td>
</tr>
<tr>
<td>GPT-6.1 Terra</td>
<td align="right">97%</td>
<td align="right">60%</td>
<td align="right">0%</td>
</tr>
</tbody>
</table>
<p>No model ever guessed an endpoint like <code>/updateBin</code>. On the RPC API, agents averaged about 28 failed calls and then gave up. POST-only landed in the middle because its reads use the same paths as REST, so agents found them, while writes that needed <code>/update</code> or <code>/delete</code> endpoints failed.</p>
<p>REST runs mostly succeeded, but agents still averaged about 10 failed calls per task before they found the right URLs. Convention gets an agent close to a REST endpoint, and the 404s steer it the rest of the way.</p>
<p>The token numbers look strange until you read them per correct result. POST-only used 48% to 56% more tokens than REST, because partial success kept agents probing, and its failed runs cost 160k to 170k tokens each. RPC looked cheap only because agents gave up early. Per correct result, REST cost about 80k tokens, POST-only about 230k, and RPC never got there.</p>
<h2>Inside a bigger task, the styles still tied</h2>
<p>All of this used short tasks, about four API calls each with the docs, with an OpenAPI description handed to the agent in the prompt. In practice, an API call is usually one step in a larger piece of work, and the docs are a reference page the agent has to find. So we built a small Node.js project with a <code>handover.md</code> file and gave the agent one instruction:</p>
<pre><code class="text">Please take care of today's shift handover in handover.md.
</code></pre>
<p>The handover asked the agent to fix a failing unit test, caused by an off-by-one <code>getMonth()</code> bug, and to apply two to four floor events to Binstock before writing a daily report, sometimes with a question about the inventory on top. The project&#8217;s README linked to the API and to an HTML reference page served by the mock. We modeled the page on the <a href="https://learn.microsoft.com/en-us/microsoft-cloud/dev/dev-proxy/technical-reference/proxy-api">Dev Proxy API reference</a>, with a section for every operation describing its request and response, so it held the same information as the OpenAPI description, in prose.</p>
<p>Claude Sonnet 5.5 and GPT-6.1 Terra ran 8 handovers twice in each style, 96 runs in total. Every run succeeded, and every run found the reference page through the README link and used it correctly. Token use was within 3% across styles, because the coding and the report dominated the cost. POST-only had slightly fewer runs without a failed call, 72% against 84% for REST and 81% for RPC, but neither that nor the difference in failed calls was significant.</p>
<h2>What we&#8217;d test next</h2>
<p>We kept Binstock deliberately small and simple, and here are a few ideas for follow-up research.</p>
<p>One idea would be to test the middle ground between handing the agent the docs and giving it nothing. Serve an RPC API that lists its operations at the root URL or in the body of every 404, and run the no-docs tasks again. If agents then complete anywhere near the 95% of tasks they managed on REST, an RPC API can make itself usable to agents that never see its docs.</p>
<p>Another would scale up the API. An RPC API with a hundred endpoints, some with near-identical names like <code>updateBin</code> and <code>updateBinItems</code>, would show whether agents start picking the wrong one once the list gets long. Adding auth and pagination, which most real APIs need, would show whether the extra steps hit one style harder than the others. Our longest tasks took about nine API calls, so a workflow with dozens of dependent calls would show whether RPC&#8217;s slightly lower share of runs without a failed call adds up over a longer session.</p>
<p>The handover experiment changed the task size and the docs format at once, so if one style had done worse, we couldn&#8217;t have said which change caused it. Running the short tasks with the prose reference, and the handover with the OpenAPI description, would tell the two apart. And since every run used the GitHub Copilot SDK in Vally, repeating the tasks in other harnesses would show whether the results hold beyond this one.</p>
<h2>What to do with your API</h2>
<p>If you maintain a non-REST API, you can leave its style as it is. With docs in reach, the agents we tested used POST-only and RPC APIs as well as REST, at the same cost. Put the effort into getting agents to those docs, because without them an agent has no way to arrive at <code>/updateBin</code>. Link an OpenAPI description from your docs and your README. Consider making the API describe itself, for example by listing the available operations at the root URL or in the body of a 404. We haven&#8217;t tested that yet, but it gives an agent that arrives without docs a way to find the right endpoint.</p>
<p>REST APIs have room to improve too. Agents without docs made about 10 failed calls per task even on REST, and the same <a href="/blog/your-best-ax-fixes-are-hiding-in-agent-trajectories">trajectory analysis</a> that surfaces a guessed URL in your docs will surface a guessed endpoint in your API.</p>
<p>If you evaluate agents against an API, decide whether you&#8217;re testing how well they use it or how well they find their way around it. Handing the agent the docs tests usage, and in our runs every style passed. Leaving the docs out tests discovery, which favors REST by design because its URLs follow a convention agents can guess. Both are useful questions, but <a href="/blog/the-hidden-variables-in-your-agent-eval">a hidden choice in your setup</a> can decide which one you&#8217;re answering.</p>
<h2>So, do AI coding agents struggle with non-REST APIs?</h2>
<p>None of the agents we tested struggled with non-REST APIs, as long as they could read the docs. From Claude Opus 5.5 down to MAI Code 2 Flash, every model used POST-only and RPC APIs as reliably and as cheaply as REST. Like with <a href="/blog/dont-rewrite-your-cli-for-agents">CLI args and JSON</a>, the plausible story didn&#8217;t survive measurement.</p>
<p>What agents struggle with is guessing, and REST is easier to guess. Claude Sonnet 5.5 and GPT-6.1 Terra completed none of the RPC tasks without the docs, and nearly all of them with the docs.</p>
<p>The post <a href="https://devblogs.microsoft.com/blog/do-ai-coding-agents-struggle-with-non-rest-apis/">Do AI coding agents struggle with non-REST APIs?</a> appeared first on <a href="https://devblogs.microsoft.com">Microsoft for Developers</a>.</p>

---
*Artikel ini otomatis diambil dari [Microsoft Developer](https://devblogs.microsoft.com/blog/do-ai-coding-agents-struggle-with-non-rest-apis/). Baca artikel selengkapnya di sumber asli.*
