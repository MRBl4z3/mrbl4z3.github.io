---
title: "Is “sort by impact” just vibes?"
description: "You ask an agent for your top five, sorted by impact, and it hands you a confident list. But you never said what impact means, so what did the agent actually sort by? We ran four models through the sa..."
pubDate: "2026-11-24T15:23:13.000Z"
authors: ["e-learning"]
organisations: ["microsoft"]
category: "web-development"
tags: ["microsoft", "web-development"]
thumbnail: "https://images.unsplash.com/photo-1573804633927-bfcbcd909acd?auto=format&fit=crop&w=800&h=450&q=80"
heroImage: "https://images.unsplash.com/photo-1573804633927-bfcbcd909acd?auto=format&fit=crop&w=800&h=450&q=80"
keywords: ["microsoft", "tech news", "developer"]
canonicalUrl: "https://devblogs.microsoft.com/blog/is-sort-by-impact-just-vibes/"
---

<p>&#8220;Give me the top five, sorted by impact.&#8221; People ask AI agents for this all the time, whether it&#8217;s a backlog or a pile of growth ideas. Yet &#8220;impact&#8221; is one of the vaguest words in the language. It could mean how many users benefit, how much money comes in or how much you get for the effort. Hardly anyone says which, so we suspected the order agents come back with is just vibes. It looks plausible, but there&#8217;s nothing behind it.</p>
<p>We were wrong, though not in a reassuring way. Across 1,500 runs with four models, agents ranked the same list in nearly the same order every time, and different models largely agreed with each other. Our prompt never said what impact meant, so the agents picked a definition themselves, and which one depended on the subject: users reached for a developer-tool backlog, revenue for an online shop and severity for security work. When every item came with a dollar figure, &#8220;impact&#8221; meant the biggest dollar figure, every time.</p>
<h2>Three kinds of vibes</h2>
<p>When you ask an agent to sort a list by impact, the order you get back can be vibes in three ways, and each one needs to be fixed in a different way.</p>
<table>
<thead>
<tr>
<th>Kind of vibes</th>
<th>What you&#8217;d see</th>
<th>What fixes it</th>
</tr>
</thead>
<tbody>
<tr>
<td>Noise</td>
<td>Ask again and you get a different order</td>
<td>Ask several times, or don&#8217;t trust the ranking</td>
</tr>
<tr>
<td>Artifact</td>
<td>The order copies something irrelevant, like the order of your list</td>
<td>Shuffle the list before you ask</td>
</tr>
<tr>
<td>Hidden default</td>
<td>The same order every time, based on a definition of impact the agent picked</td>
<td>Tell the agent what impact means</td>
</tr>
</tbody>
</table>
<h2>Eight items, ranked over and over</h2>
<p>If we let the agent come up with its own ideas, we&#8217;d be testing two things at once: which ideas it picks and how it ranks them. So we gave it a fixed list of eight items to rank. The first list was a backlog for <code>devkit</code>, a made-up open-source CLI with a paid Team tier:</p>
<pre><code class="text">I maintain devkit, an open-source CLI for scaffolding and deploying serverless apps.
The core is free; a paid Team tier adds collaboration features.

Here's our backlog:

- win-paths: Fix the crash on Windows when the project path contains spaces
- sso: Add SSO/SAML login to the Team tier
- quickstart: Rewrite the getting-started docs and add a 5-minute quickstart
- deploy-speed: Cut `devkit deploy` cold start from 40s to 10s
- ...

Sort these by impact, most impactful first.
</code></pre>
<p>Every reply had to end with a <code>RANKING:</code> line listing all eight items, so we could compare orders automatically. Each list came in two versions. One had titles only, the way people usually paste a list into a chat. The other added data to each item, like &#8220;hits ~15% of users; est. 1 day&#8221;. Then we asked for the ranking in several ways:</p>
<table>
<thead>
<tr>
<th>What we asked</th>
<th>What it tells us</th>
</tr>
</thead>
<tbody>
<tr>
<td>&#8220;Sort these by impact&#8221;, 15 times</td>
<td>Noise: do repeated runs agree?</td>
</tr>
<tr>
<td>The same, with the list reversed</td>
<td>Artifact: does an item&#8217;s position in the list affect its rank?</td>
</tr>
<tr>
<td>Come up with 5 ideas, then sort them by impact</td>
<td>Artifact: is the ranking just the order the ideas came out in?</td>
</tr>
<tr>
<td>&#8220;Sort by impact, where impact means&#8230;&#8221; users reached, revenue or value per day of effort</td>
<td>Hidden default: which definition does plain &#8220;impact&#8221; match?</td>
</tr>
<tr>
<td>&#8220;Define impact first, then sort&#8221;</td>
<td>Does it make the ranking more consistent?</td>
</tr>
</tbody>
</table>
<p>Claude Sonnet 5.5, Claude Haiku 5.5, GPT-6.1 Sol and GPT-6 Luna ranked the backlog 15 times per prompt. We ran everything in <a href="https://aka.ms/vally">Vally</a>, Microsoft&#8217;s evaluation tool for AI agents, with the GitHub Copilot SDK as the agent.</p>
<h2>The first round said &#8220;impact means reach&#8221;</h2>
<p>On the backlog, the &#8220;sort by impact&#8221; ranking came closest to sorting by users reached. When the agents explained their ranking, most said they&#8217;d ranked by reach or by a mix that included it. Impact meant reach, apparently.</p>
<p>One domain is one data point, so we added two more, and this time we wrote down beforehand what would count as a result. The shop domain was a list of growth initiatives for Northbean, a small online coffee roaster. The risk domain was a list of security improvements for Ledgerly, a B2B invoicing SaaS. In the version with data, every item now had three numbers: how many people it reaches, how much money it&#8217;s worth and how many days it takes.</p>
<pre><code class="text">- cart-email: Send an abandoned-cart reminder email
  (reaches ~8,000 people/month; est. +$40k/yr revenue; est. 1 day)
- microlot-club: Launch a $45/month limited-microlot club for top customers
  (reaches ~150 people/month; est. +$70k/yr revenue; est. 2 days)
</code></pre>
<p>We picked the numbers so that sorting by reach, by money and by effort gave three completely unrelated orders. That way, we could tell which number the agents followed. We also had Claude Haiku 5.5 read every answer and note what the agent said it had ranked by. Claude Sonnet 5.5 and GPT-6.1 Sol ran both new domains, 15 times per prompt.</p>
<h2>Ask again and you get the same order</h2>
<p>Pick any two items from a list, and two runs of the same prompt put them in the same order 86% to 100% of the time, depending on the model and the list. That&#8217;s about as consistent as when we spelled out what impact meant. Different models agreed on 77% to 93% of pairs, and on 87% to 100% once the list had data.</p>
<p>An item&#8217;s position in the input had almost nothing to do with its rank, and reversing the list changed the full ranking only slightly. With titles only, 64% to 100% of runs picked the same #1 item. The backlog was the only list where reversing it changed the top pick. Claude Sonnet 5.5, for example, put the quickstart first in the original order and the Windows crash fix first in reverse. When agents came up with five ideas and then sorted them, the sorted order had little to do with the order they&#8217;d thought of them in, and no run handed the list back unchanged.</p>
<h2>The hidden default depends on the domain</h2>
<p>So what were the agents ranking by? With titles only, it depended on what the list was about. Reach, our answer from the first round, only held for the developer-tool backlog.</p>
<table>
<thead>
<tr>
<th>Domain</th>
<th>Ranking closest to</th>
<th>What the agents said they ranked by</th>
</tr>
</thead>
<tbody>
<tr>
<td>Developer-tool backlog</td>
<td>Users reached</td>
<td>Reach, or a mix that included it</td>
</tr>
<tr>
<td>Online shop</td>
<td>Revenue</td>
<td>Money (GPT-6.1 Sol), or a mix (Claude Sonnet 5.5)</td>
</tr>
<tr>
<td>Security</td>
<td>Losses avoided</td>
<td>Severity</td>
</tr>
</tbody>
</table>
<p>On the security list, the agents ranked by how bad each risk was, and enforcing hardware-key MFA for all employees came first in every run for both models. Of our three definitions, losses avoided came closest, but the agents talked about severity, which none of our definitions covered.</p>
<h2>With dollar figures, impact meant the biggest dollar figure</h2>
<p>When every item stated reach, money and effort, Claude Sonnet 5.5 and GPT-6.1 Sol sorted by the money column on both the shop and the security list. In every run, the ranking matched the money order exactly and had nothing to do with the reach order. The abandoned-cart email reached more than 50 times as many people as the microlot club and took half the time, and the agents ranked the microlot club higher anyway, because it was worth $70k instead of $40k.</p>
<h2>Asking the agent to define impact didn&#8217;t help</h2>
<p>On paper, the obvious fix is to make the agent say what it means by impact before it ranks anything. In practice, it didn&#8217;t make the rankings any more consistent, and for three of the four models, it made them less so.</p>
<h2>What we&#8217;d test next</h2>
<p>The first thing we&#8217;d test is whether people do any better. Give the same lists to product managers and engineers, ask them to sort by impact, and compare how often they agree with each other and with themselves a week later. If people pick different definitions too, the problem is the request itself, and you&#8217;d need to define impact no matter who does the ranking.</p>
<p>We&#8217;d also widen the lists. We tested three domains with one list of eight items each, all of them business decisions. A list of twenty items, or one outside business entirely, like which research papers to read first, would show whether agents still settle on one definition per domain.</p>
<h2>What to do when you ask for a ranking</h2>
<p>If you ask an agent to sort anything by impact, or by a word that&#8217;s just as vague, like priority or importance, say what you mean by it, such as expected revenue or value per day of effort. If you don&#8217;t, the agent picks one for you, and it might not be the one you had in mind. Asking the agent to define impact first sounds rigorous, but you still get a definition the agent chose, and in our runs the rankings got less consistent. If you&#8217;re not sure which definition fits, ask for several rankings, one per definition, and compare them. Items that rank high on every definition are the safe bets, and the ones that move are where the real decision is.</p>
<p>Watch what data you put in the list, because the numbers you add define &#8220;impact&#8221; as much as the words do. If your items carry dollar figures, expect the agent to sort by them and ignore everything else you included.</p>
<p>If you build skills, prompts or agents that rank things for people, like triaging incoming issues or summarizing review findings, put the definition in the instructions. When a word is ambiguous, an agent quietly picks a definition and sticks to it. Like <a href="/blog/models-dont-have-preferences-they-have-context">the preferences people attribute to models</a>, that definition comes from the context, such as the subject of the list or the numbers in it.</p>
<p>If you evaluate agents that rank, add the word itself to your list of <a href="/blog/the-hidden-variables-in-your-agent-eval">hidden variables</a>. The same prompt tests one definition on a shop&#8217;s growth ideas and a different one on a security backlog.</p>
<h2>So, is &#8220;sort by impact&#8221; just vibes?</h2>
<p>&#8220;Sort by impact&#8221; turned out far more consistent than we expected. Agents gave nearly the same ranking run after run and mostly agreed with each other, whatever order the list came in.</p>
<p>It&#8217;s vibes in a subtler way. The agent ranks by a definition it picks from the subject of the list: reach for a developer tool, revenue for a shop, severity for security and the biggest dollar figure whenever there is one. Because the ranking comes back the same every time and sounds well-reasoned, it&#8217;s easy to mistake for a judgment you agreed with.</p>
<p>The post <a href="https://devblogs.microsoft.com/blog/is-sort-by-impact-just-vibes/">Is &#8220;sort by impact&#8221; just vibes?</a> appeared first on <a href="https://devblogs.microsoft.com">Microsoft for Developers</a>.</p>

---
*Artikel ini otomatis diambil dari [Microsoft Developer](https://devblogs.microsoft.com/blog/is-sort-by-impact-just-vibes/). Baca artikel selengkapnya di sumber asli.*
