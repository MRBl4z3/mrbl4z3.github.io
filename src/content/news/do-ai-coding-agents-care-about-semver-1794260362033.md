---
title: "Do AI coding agents care about SemVer?"
description: "We gave AI coding agents the same breaking upgrade twice. Labeled 2.0.0, Claude Sonnet 5.5 checked it in all 10 runs. Labeled 1.4.3, it checked in two and often left the app broken. Nearly 1,000 runs ..."
pubDate: "2026-11-09T16:01:03.000Z"
authors: ["e-learning"]
organisations: ["microsoft"]
category: "web-development"
tags: ["microsoft", "web-development"]
thumbnail: "https://images.unsplash.com/photo-1573804633927-bfcbcd909acd?auto=format&fit=crop&w=800&h=450&q=80"
heroImage: "https://images.unsplash.com/photo-1573804633927-bfcbcd909acd?auto=format&fit=crop&w=800&h=450&q=80"
keywords: ["microsoft", "tech news", "developer"]
canonicalUrl: "https://devblogs.microsoft.com/blog/do-ai-coding-agents-care-about-semver/"
---

<p>Semantic versioning tells developers how risky an upgrade is. A patch fixes bugs, a minor release adds features without breaking anything and a major release breaks things, so you know how closely to look before you upgrade. On paper, that signal is redundant when an AI coding agent does the upgrade. The agent has no muscle memory tied to the old API. It can read the new code and run the tests, so why would it care what the version number says? We suspected it wouldn&#8217;t, and that SemVer had become something only people need.</p>
<p>Two models proved us wrong. Across two experiments and almost 1,000 runs, the version number alone changed what they did with byte-identical code. Given an upgrade that broke the app, Claude Sonnet 5.5 ran the tests or read the new code in 2 of 10 runs when the upgrade was labeled a patch, and in all 10 when it was labeled a major release. Other models checked every upgrade, whatever the label said. Some agents still read SemVer, and for them, a patch label is a reason to check less.</p>
<h2>Why an agent might still read the version number</h2>
<p>SemVer describes what the maintainer intended a release to do. Whether it works with your app is something an agent can check directly, by reading the code and running the tests.</p>
<p>Still, models learn from repositories and docs where SemVer is everywhere. Somewhere in that data, they may have picked up the same habits developers have: a major release deserves a careful look, and a patch is safe to take. If they did, the label could decide how thoroughly an agent checks an upgrade, even when the code behind it is the same.</p>
<p>We wanted to know whether a patch, minor or major label changes how agents behave, and if it does, whether that changes the outcome. We ran both experiments in <a href="https://aka.ms/vally">Vally</a>, Microsoft&#8217;s evaluation tool for AI agents, with GitHub Copilot CLI as the agent.</p>
<h2>Same code, different version number</h2>
<p>We built a small Node.js app that depends on <code>fluxkit</code>, an npm library we made up. A fictional library meant no model could rely on what it remembered about the API or its release history. The new version of <code>fluxkit</code> renamed a function the app calls, so upgrading without updating the app leaves it broken.</p>
<p>We gave the agents a deliberately minimal instruction, in one of two versions:</p>
<pre><code class="text"># patch
Bump fluxkit to 1.4.3.

# major
Bump fluxkit to 2.0.0.
</code></pre>
<p>The library code behind both versions was byte-identical, breaking change included, so only the version number differed. If agents ignored the label, they&#8217;d behave the same in both conditions. If they treated a major release as riskier, they&#8217;d check the upgrade more often. We counted a run as checked when the agent ran the tests or the app, or read the updated library code.</p>
<table>
<thead>
<tr>
<th>Model</th>
<th align="right">Checked: patch</th>
<th align="right">Checked: major</th>
<th>Verdict</th>
</tr>
</thead>
<tbody>
<tr>
<td>Claude Sonnet 5.5</td>
<td align="right">2/10</td>
<td align="right">10/10</td>
<td>Effect</td>
</tr>
<tr>
<td>Claude Opus 5.5</td>
<td align="right">70/70</td>
<td align="right">70/70</td>
<td>No meaningful effect</td>
</tr>
<tr>
<td>GPT-6.1 Sol</td>
<td align="right">70/70</td>
<td align="right">70/70</td>
<td>No meaningful effect</td>
</tr>
<tr>
<td>GPT-6.1 Terra</td>
<td align="right">70/70</td>
<td align="right">70/70</td>
<td>No meaningful effect</td>
</tr>
</tbody>
</table>
<p>Claude Sonnet 5.5 reacted to the label more than we expected any model to. Asked for a patch, it often changed only the dependency version and stopped there, leaving the app broken. Asked for a major release, it checked every time. The other three models checked every upgrade under both labels. In a five-run pilot, GPT-6 Sol did the same, before we replaced it with GPT-6.1 Terra for the full run.</p>
<h2>Reviewing a teammate&#8217;s update</h2>
<p>&#8220;Bump this dependency&#8221; is the simplest upgrade there is. More often, a teammate or a dependency bot prepares a branch, and someone reviews it before it&#8217;s merged. When that someone is an agent, it has to decide on its own whether the update is safe to merge, and we wanted to see whether the label still changes how carefully it checks.</p>
<p>We prepared update branches and asked the agents to review them:</p>
<pre><code class="text">Review the update-&lt;lib&gt;-&lt;version&gt; branch and merge it into main if it's good.
</code></pre>
<p>The version number appeared only in the branch name and the commit message, with no release notes or pull request description to lean on. We built six scenarios, three with a breaking change and three without, and gave each one a patch, a minor and a major label, with the same code under all three. Six models each got 30 runs per label, 540 runs in total, and we measured how often the agent merged an update without checking the new version first.</p>
<p>Before running anything, we decided what would count as a real difference. We set the line at 25 percentage points and classified each result as an effect, no meaningful effect or inconclusive, based on its confidence interval. Not every observed difference is reliable, and deciding upfront kept us from reading too much into the small ones.</p>
<table>
<thead>
<tr>
<th>Model</th>
<th align="right">Unchecked merges: patch</th>
<th align="right">Unchecked merges: major</th>
<th>Verdict</th>
</tr>
</thead>
<tbody>
<tr>
<td>Claude Haiku 5.5</td>
<td align="right">11/30 (37%)</td>
<td align="right">2/30 (7%)</td>
<td>Effect</td>
</tr>
<tr>
<td>Claude Sonnet 5.5</td>
<td align="right">4/30 (13%)</td>
<td align="right">0/30</td>
<td>Inconclusive, leaning toward an effect</td>
</tr>
<tr>
<td>GPT-6 Luna</td>
<td align="right">9/30 (30%)</td>
<td align="right">5/30 (17%)</td>
<td>Inconclusive</td>
</tr>
<tr>
<td>Claude Opus 5.5</td>
<td align="right">0/30</td>
<td align="right">0/30</td>
<td>No meaningful effect</td>
</tr>
<tr>
<td>GPT-6.1 Sol</td>
<td align="right">0/30</td>
<td align="right">0/30</td>
<td>No meaningful effect</td>
</tr>
<tr>
<td>GPT-6.1 Terra</td>
<td align="right">0/30</td>
<td align="right">0/30</td>
<td>No meaningful effect</td>
</tr>
</tbody>
</table>
<p>The pattern from the first experiment held. Claude Haiku 5.5 merged a patch-labeled update without checking it in 37% of runs, and a major-labeled one in 7%. Claude Sonnet 5.5 and GPT-6 Luna leaned the same way, but neither difference cleared the bar we&#8217;d set. Claude Opus 5.5, GPT-6.1 Sol and GPT-6.1 Terra checked every update, whatever the label.</p>
<p>Across all six models, unchecked merges dropped as the label suggested a bigger change:</p>
<table>
<thead>
<tr>
<th>Version label</th>
<th align="right">Unchecked merges (180 runs per label)</th>
</tr>
</thead>
<tbody>
<tr>
<td>Patch</td>
<td align="right">13%</td>
</tr>
<tr>
<td>Minor</td>
<td align="right">9%</td>
</tr>
<tr>
<td>Major</td>
<td align="right">4%</td>
</tr>
</tbody>
</table>
<p>Within Claude 5.5, the smaller the model, the more the label swayed it. Claude Haiku 5.5 showed a clear effect, Claude Sonnet 5.5 showed one in the first experiment and leaned that way in the second, and Claude Opus 5.5 ignored the label in both. We can&#8217;t make the same comparison for GPT, because no single generation covers Luna, Sol and Terra.</p>
<h2>Did skipping the check break anything?</h2>
<p>An agent that skips the check gets away with it as long as the update works. Mostly, the agents judged the code itself. They merged the non-breaking updates in 268 of 270 runs and the breaking ones in 13 of 270. Four of those 13 merges left <code>main</code> broken, two by Claude Haiku 5.5 and two by GPT-6 Luna. That&#8217;s rare, but all four came with a patch or a minor label, the ones that tell a developer there&#8217;s nothing to worry about.</p>
<p>Every model, including the three that checked every update regardless, talked about compatibility more often when reviewing a major release. So the label reached even the models whose behavior it left unchanged.</p>
<h2>What this doesn&#8217;t show</h2>
<p>We broke SemVer on purpose, shipping a breaking change under a patch label to see whether agents trust the label over the code. Most real patch releases are what they claim to be, so our results describe how agents handle the exceptions. Our apps were also small, and our updates came without release notes, which real upgrades usually include and an agent might read.</p>
<p>The 13-point differences for Claude Sonnet 5.5 and GPT-6 Luna may well be real, but with 30 runs per label, they&#8217;re too uncertain to call either way. Three models ignoring the label doesn&#8217;t mean models are outgrowing SemVer. Those three did, and the next one you switch to might not. We also didn&#8217;t test whether another versioning scheme would work better for agents.</p>
<h2>What to do with your version numbers</h2>
<p>If you maintain a library, keep using SemVer, and keep it honest. Agents recognize it, and some of them act on it. A breaking change shipped as a patch is exactly the case where those agents skip the check, and every broken <code>main</code> in our runs came with a patch or a minor label. Your version numbers are part of the experience agents have with your product, the same way your <a href="/blog/your-best-ax-fixes-are-hiding-in-agent-trajectories">docs and CLI help</a> are.</p>
<p>If agents upgrade your dependencies, don&#8217;t let the label decide how much checking happens. Some models check every update on their own, and some check only when the version number looks risky. Build verification into the workflow, for example with CI that must pass on every dependency branch, so a patch gets the same checks as a major release.</p>
<p>If you evaluate agents, add the version number to your list of <a href="/blog/the-hidden-variables-in-your-agent-eval">hidden variables</a>. With Claude Haiku 5.5 or Sonnet 5.5, a scenario that bumps a patch version measures a different agent than one that bumps a major version. Test with <a href="/blog/what-ai-benchmarks-are-not-telling-you">the models your developers actually use</a>, because testing only Claude Opus 5.5 would have told you agents ignore the label, and testing only Claude Haiku 5.5 would have told you the opposite. Read the trajectories too, because a run where the agent bumped a patch and got lucky passes just like a run where it checked.</p>
<h2>So, do AI coding agents care about SemVer?</h2>
<p>Some do. We expected agents to read the code and ignore the label, and Claude Opus 5.5, GPT-6.1 Sol and GPT-6.1 Terra did exactly that. Others used the label to decide how hard to look, the same way a developer skimming a Dependabot PR would.</p>
<p>A version number can&#8217;t prove that an upgrade is safe, for agents or for people. <strong>The version number tells an agent how worried to be. Only the tests tell it whether it should be.</strong></p>
<p>The post <a href="https://devblogs.microsoft.com/blog/do-ai-coding-agents-care-about-semver/">Do AI coding agents care about SemVer?</a> appeared first on <a href="https://devblogs.microsoft.com">Microsoft for Developers</a>.</p>

---
*Artikel ini otomatis diambil dari [Microsoft Developer](https://devblogs.microsoft.com/blog/do-ai-coding-agents-care-about-semver/). Baca artikel selengkapnya di sumber asli.*
