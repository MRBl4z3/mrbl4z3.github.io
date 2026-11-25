---
title: "How to choose the next safe step in application modernization"
description: "Learn how to assess a legacy application, choose a bounded modernization step, and reuse the method with a GitHub Copilot agent skill. The post How to choose the next safe step in application moderniz..."
pubDate: "2026-11-25T15:00:00.000Z"
authors: ["e-learning"]
organisations: ["github"]
category: "web-development"
tags: ["github", "web-development"]
thumbnail: "https://images.unsplash.com/photo-1573804633927-bfcbcd909acd?auto=format&fit=crop&w=800&h=450&q=80"
heroImage: "https://images.unsplash.com/photo-1573804633927-bfcbcd909acd?auto=format&fit=crop&w=800&h=450&q=80"
keywords: ["github", "tech news", "developer"]
canonicalUrl: "https://github.blog/ai-and-ml/github-copilot/how-to-choose-the-next-safe-step-in-application-modernization/"
---

<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN" "http://www.w3.org/TR/REC-html40/loose.dtd">
<html><body><p class="wp-block-paragraph">A request to modernize an application often starts with a technology question: Which framework should replace the old one? Before you choose, find out what still works. Then collect evidence that can act as a regression net for that behavior and show where modernization is necessary.</p>



<p class="wp-block-paragraph">I used an evidence-first method on four public applications, from a small Node.js game to a large Java enterprise resource planning (ERP) application. Each needed a different treatment, but the way I collected evidence, chose the next experiment, and decided on possible proof points was repeatable.</p>



<p class="wp-block-paragraph">This post walks through that process so you can apply it to one application of your own. It also shows how to encode the procedure in a <a href="https://docs.github.com/en/enterprise-cloud@latest/copilot/concepts/agents/about-agent-skills" target="_blank" rel="noopener">GitHub Copilot agent skill</a> while keeping architectural judgment with the people responsible for the system.</p>



<h2 id="h-build-evidence-before-choosing-a-target" class="wp-block-heading">Build evidence before choosing a target</h2>



<p class="wp-block-paragraph">The label on an application rarely tells you enough. &ldquo;Java microservices&rdquo; doesn&rsquo;t tell you which Java Development Kit (JDK) runs in production, where that version is pinned, whether the tests execute, or which interfaces other systems depend on. &ldquo;Legacy monolith&rdquo; doesn&rsquo;t tell you whether the business core is broken or whether one retired platform dependency prevents the application from starting.</p>



<p class="wp-block-paragraph">Before you recommend a change, build a small evidence inventory:</p>



<ul class="wp-block-list">
<li>Manifests, lockfiles, entry points, and deployable components</li>



<li>Verified commands for installing, building, testing, packaging, and running</li>



<li>Runtime pins in containers, CI workflows, deployment files, and developer tooling</li>



<li>External contracts such as APIs, database behavior, file formats, and user workflows</li>



<li>End-of-life frameworks and dependencies, with evidence that the application still uses them</li>
</ul>



<p class="wp-block-paragraph">Record uncertainty while you work. I use three simple labels: documented, inferred, and unverified. If two files disagree about the Java version, resolve the contradiction. If you can&rsquo;t determine whether a CI check blocks merges, say so. Record the unknown rather than treating a guess as a finding.</p>



<p class="wp-block-paragraph">That inventory gives you something concrete to review with the people who own the application before you propose changes.</p>



<h2 id="h-choose-the-least-disruptive-treatment-that-works" class="wp-block-heading">Choose the least disruptive treatment that works</h2>



<p class="wp-block-paragraph">Once you understand the application, make the decision per component. A single system can contain code that should stay, dependencies that should be swapped, and one subsystem that genuinely needs a rewrite.</p>



<p class="wp-block-paragraph">First, check whether a capability is still needed. Remove it only after verifying that it is unused or superseded and confirming that decision with its owner. Removal is a separate decision, and it&rsquo;s not necessarily the most disruptive one.</p>



<p class="wp-block-paragraph">For a capability you still need, start with the option that changes the least:</p>



<ol class="wp-block-list">
<li><strong>Keep it.</strong> Supported code that still fits the job doesn&rsquo;t need a modernization story.</li>



<li><strong>Upgrade it.</strong> Move to a supported major version and fix the breaking changes.</li>



<li><strong>Swap it.</strong> Replace an abandoned dependency while preserving the existing design.</li>



<li><strong>Wrap it.</strong> Stabilize a useful interface and replace the internals in increments.</li>



<li><strong>Rewrite it.</strong> Use this when no responsible upgrade path exists or the architecture cannot meet the requirement.</li>
</ol>



<p class="wp-block-paragraph">This is a decision order, not a universal ranking of effort or risk. Ask what evidence justifies choosing a more disruptive option. &ldquo;The code is old&rdquo; is not enough. A known vulnerability with no patched release for the pinned version, a dependency that fails on the required runtime, or a measured reliability target the current design cannot meet can justify escalation.</p>



<h2 id="h-decide-what-proof-is-possible" class="wp-block-heading">Decide what proof is possible</h2>



<p class="wp-block-paragraph">The next question is whether the component can run well enough to produce evidence, even if the legacy CI pipeline is not green.</p>



<p class="wp-block-paragraph">Run a time-boxed feasibility check. Try to install dependencies, build on an available toolchain, start the component, and execute at least one meaningful test. Stop when the time box expires. The result sends you down one of two paths.</p>



<p class="wp-block-paragraph"><strong>Capture behavior, then upgrade.</strong> Use this path when the old application can still run at an acceptable cost. Record its externally visible behavior, fix the test harness where practical, and use those checks to verify the upgrade.</p>



<p class="wp-block-paragraph"><strong>Build a small modern slice first.</strong> Use this path when the old component cannot run until you have already changed it. Move the thinnest useful workflow to supported technology, prove it, and expand from there. Requiring a complete legacy test suite in this situation is circular. Restoring the test suite is part of the modernization.</p>



<p class="wp-block-paragraph">I use the following safety levels, adapted from the <a href="https://github.com/github/awesome-copilot/blob/1899b18da3fa5183652f86165917d553cba1850a/skills/doc-and-modernize/SKILL.md#L350-L373" target="_blank" rel="noopener"><code>doc-and-modernize</code> skill</a>, to describe the strongest regression protection available:</p>



<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Level</th><th>Strongest honest protection</th></tr></thead><tbody><tr><td>L0</td><td>No usable regression net; treat the old code and available documentation as the specification.</td></tr><tr><td>L1</td><td>Small reversible changes, old and new paths in parallel, smoke checks, and domain review.</td></tr><tr><td>L2</td><td>Captured inputs and outputs, snapshots, or other behavioral comparisons.</td></tr><tr><td>L3</td><td>Automated checks cover part of the system, with the remaining gaps named.</td></tr><tr><td>L4</td><td>An automated CI gate combines lint, unit, behavioral, and end-to-end checks for the agreed scope.</td></tr></tbody></table></figure>



<p class="wp-block-paragraph">State what you can prove and what risk remains. Even an L4 gate cannot establish that every possible behavior is correct. APIs, database effects, files, rendered output, and business workflows often make durable comparison points because those behaviors can survive a framework or language change.</p>



<h2 id="h-piggymetrics-capture-behavior-then-upgrade" class="wp-block-heading">PiggyMetrics: capture behavior, then upgrade</h2>



<p class="wp-block-paragraph"><a href="https://github.com/samqbush/piggymetricsv2/tree/d1b37dbfff5e973bd09d08753f0b41585b7225d6" target="_blank" rel="noopener">PiggyMetrics</a> was the clearest example of an application that looked worse than it was. The project used Java 8, Spring Boot 2.0.3, Spring Cloud Finchley, Zuul, Hystrix, and the old <code>spring-security-oauth2</code> library.</p>



<p class="wp-block-paragraph">It would have been easy to call the application unworkable and start drawing a new architecture. The feasibility check told a different story.</p>



<p class="wp-block-paragraph">The <a href="https://github.com/samqbush/piggymetricsv2/blob/d1b37dbfff5e973bd09d08753f0b41585b7225d6/ARCHITECTURE.md#L97-L129" target="_blank" rel="noopener">recorded feasibility results</a> show these two probes against the legacy checkout with JDK 8:</p>



<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Command</th><th>Recorded result</th><th>What it established</th></tr></thead><tbody><tr><td><code>mvn -pl config -am compile</code></td><td>Exit code 0</td><td>The selected config module and required reactor dependencies compiled.</td></tr><tr><td><code>mvn -pl account-service -am test</code></td><td>14 tests ran: five passed and nine errored</td><td>Some service logic passed; embedded MongoDB initialization blocked the other tests.</td></tr></tbody></table></figure>



<p class="wp-block-paragraph">These are historical results, not commands expected to reproduce those failures on the modernized checkout. The nine errors traced through <code>Failed to load ApplicationContext</code> to an embedded MongoDB download returning HTTP 403. The pinned binary also targeted i386, which would not run on the Apple Silicon host.</p>



<p class="wp-block-paragraph">The probes did <strong>not</strong> establish that the whole application ran or that its integration behavior was correct. They did give me a narrower next step: replace the test dependency and rerun the affected tests before changing the production stack.</p>



<p class="wp-block-paragraph">That result saved me from making a very tidy, very expensive mistake. I had already started thinking about which parts of the architecture I might replace. One broken test dependency did not justify redesigning the application, and five passing tests established behavior I could check for unintended changes during modernization.</p>



<p class="wp-block-paragraph">The <a href="https://github.com/samqbush/piggymetricsv2/blob/d1b37dbfff5e973bd09d08753f0b41585b7225d6/MODERNIZATION_PLAN.md#L29-L65" target="_blank" rel="noopener">modernization plan</a> turned that evidence into a capture-then-upgrade sequence:</p>



<ol class="wp-block-list">
<li>Replace the embedded database dependency with Testcontainers and establish a green baseline on the existing stack.</li>



<li>Put that baseline in GitHub Actions before changing the production runtime.</li>



<li>Move from Java 8 to Java 21 and Spring Boot 3.3.</li>



<li>Replace obsolete components behind their existing interfaces: Zuul with Spring Cloud Gateway, Hystrix with Resilience4j, and the retired OAuth stack with Spring Authorization Server and JSON Web Token (JWT) resource servers.</li>



<li>Carry the baseline forward and add contract checks around the parts whose behavior changed.</li>
</ol>



<p class="wp-block-paragraph">The first acceptance gate was a passing baseline on the old stack, not merely the successful installation of a replacement dependency. Running checks in CI and enforcing them as merge requirements were also separate steps in the plan.</p>



<p class="wp-block-paragraph">The <a href="https://github.com/samqbush/piggymetricsv2/blob/d1b37dbfff5e973bd09d08753f0b41585b7225d6/README.md#modernization-at-a-glance" target="_blank" rel="noopener">result summary</a> records the Java and Spring upgrades while retaining the microservice decomposition that the application exists to demonstrate. Eureka and Cloud Config stayed; the two monitoring modules used by the retired Hystrix and Turbine stack were removed. The in-scope build went from nine modules to seven.</p>



<p class="wp-block-paragraph">Those versions describe the experiment, not a recommendation to select the same versions today. Choose targets against your current support requirements. The useful lesson is how the repository evidence narrowed the change.</p>



<h2 id="h-the-method-changes-with-the-system" class="wp-block-heading">The method changes with the system</h2>



<p class="wp-block-paragraph">The same process produced different answers on two smaller applications. Their pinned reports describe these choices:</p>



<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Application</th><th>What stayed</th><th>What changed</th><th>Strategy</th></tr></thead><tbody><tr><td><a href="https://github.com/samqbush/browserquest-v2/blob/e535c16838f12f2d7d641f94f8cf4cd45b63dbd0/BEFORE_AFTER.md" target="_blank" rel="noopener">BrowserQuest</a></td><td>Canvas renderer, pathfinding, world model, protocol, and game-loop model</td><td>Node 0.4.7 to Node 22, two obsolete WebSocket stacks to <code>ws</code>, plus tests and GitHub Actions</td><td>Upgrade and swap</td></tr><tr><td><a href="https://github.com/samqbush/doomv2/blob/523f0446768136f1be0af81f73264093997c8170/README.TXT" target="_blank" rel="noopener">DOOM</a></td><td>Renderer, 35 Hz simulation model, and WAD game-data format</td><td>X11 video and OSS audio replaced with SDL2; platform code adapted for modern 64-bit systems</td><td>Preserve the core and replace the platform</td></tr></tbody></table></figure>



<p class="wp-block-paragraph">The comparison needs one important qualification: <a href="https://github.com/samqbush/doomv2/blob/523f0446768136f1be0af81f73264093997c8170/README.TXT" target="_blank" rel="noopener">DOOM&rsquo;s checks</a> froze outputs from the modernized engine itself. They protected against later regressions; they did not independently prove parity with a period-correct legacy binary. Naming that limit is part of choosing an honest safety level.</p>



<h3 id="h-scaling-the-method-without-pretending-the-product-is-finished" class="wp-block-heading">Scaling the method without pretending the product is finished</h3>



<p class="wp-block-paragraph">I then applied the method to <a href="https://github.com/samqbush/adempiere2/blob/f6813cc5488725ccff865deda16d840e468cb975/docs/modernization/modernization-effort-report.md" target="_blank" rel="noopener">ADempiere</a>, a large Java suite covering ERP, customer relationship management, manufacturing, supply chain, accounting, and point of sale.</p>



<p class="wp-block-paragraph">The scale changed the controls, but not the method. The work established a reproducible build, moved the runtime to JDK 21, preserved SOAP web-service contracts while replacing XFire with CXF and Jakarta, captured legacy web behavior, and introduced a modern web path for selected users. The first modern business partner write was compared with a result frozen from the legacy application.</p>



<p class="wp-block-paragraph">The <a href="https://github.com/samqbush/adempiere2/blob/f6813cc5488725ccff865deda16d840e468cb975/docs/modernization/modernization-effort-report.md#executive-summary" target="_blank" rel="noopener">fixed measurement report</a> covers 30 merged modernization pull requests and 102,587.72 reconstructed Copilot credits through the first accepted portable demo. The credit total is not a dollar cost. More important, the report says exactly what the result was not: it was not a claim that every ADempiere screen or production deployment had been modernized.</p>



<p class="wp-block-paragraph">I stopped because the proof-of-concept question had been answered. The result demonstrated a build on the selected runtime, old and new paths running together, and a real business write checked against the legacy result. Completing the rest would have required a product funding decision, not another technical proof. Stopping at that boundary was part of the method.</p>



<h2 id="h-turn-the-method-into-an-agent-skill" class="wp-block-heading">Turn the method into an agent skill</h2>



<p class="wp-block-paragraph">After repeating this work, I didn&rsquo;t want the evidence checklist and decision rules to live only in my notes. I encoded them in the public <a href="https://github.com/github/awesome-copilot/tree/main/skills/doc-and-modernize" target="_blank" rel="noopener"><code>doc-and-modernize</code> agent skill</a>.</p>



<p class="wp-block-paragraph"><a href="https://docs.github.com/en/enterprise-cloud@latest/copilot/concepts/agents/about-agent-skills" target="_blank" rel="noopener">Agent skills</a> are folders of instructions, scripts, and resources that Copilot can load when a task is relevant. A project skill can live with one repository:</p>


<div class="wp-block-code-wrapper">
<pre class="wp-block-code language-plaintext"><code>.github/skills/doc-and-modernize/
&#9500;&#9472;&#9472; SKILL.md
&#9500;&#9472;&#9472; references/
&#9492;&#9472;&#9472; scripts/
</code></pre>
<clipboard-copy aria-label="Copy" class="code-copy-btn" data-copy-feedback="Copied!" value=".github/skills/doc-and-modernize/
&#9500;&#9472;&#9472; SKILL.md
&#9500;&#9472;&#9472; references/
&#9492;&#9472;&#9472; scripts/" tabindex="0" role="button"><svg aria-hidden="true" height="16" viewbox="0 0 16 16" version="1.1" width="16" class="octicon octicon-copy js-clipboard-copy-icon"><path d="M0 6.75C0 5.784.784 5 1.75 5h1.5a.75.75 0 0 1 0 1.5h-1.5a.25.25 0 0 0-.25.25v7.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-1.5a.75.75 0 0 1 1.5 0v1.5A1.75 1.75 0 0 1 9.25 16h-7.5A1.75 1.75 0 0 1 0 14.25Z"></path><path d="M5 1.75C5 .784 5.784 0 6.75 0h7.5C15.216 0 16 .784 16 1.75v7.5A1.75 1.75 0 0 1 14.25 11h-7.5A1.75 1.75 0 0 1 5 9.25Zm1.75-.25a.25.25 0 0 0-.25.25v7.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-7.5a.25.25 0 0 0-.25-.25Z"></path></svg><svg aria-hidden="true" height="16" viewbox="0 0 16 16" version="1.1" width="16" class="octicon octicon-check js-clipboard-check-icon"><path d="M13.78 4.22a.75.75 0 0 1 0 1.06l-7.25 7.25a.75.75 0 0 1-1.06 0L2.22 9.28a.751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018L6 10.94l6.72-6.72a.75.75 0 0 1 1.06 0Z"></path></svg></clipboard-copy></div>


<p class="wp-block-paragraph">Only <code>SKILL.md</code> is required; the resource folders shown here are optional. The file needs YAML frontmatter with a <code>name</code> and a <code>description</code> explaining what the skill does and when to use it, followed by the instructions. The <a href="https://docs.github.com/en/enterprise-cloud@latest/copilot/how-tos/copilot-cli/customize-copilot/add-skills#creating-and-adding-a-skill" target="_blank" rel="noopener">creation guide</a> includes the full format.</p>



<p class="wp-block-paragraph">Paraphrased from the <a href="https://github.com/github/awesome-copilot/blob/1899b18da3fa5183652f86165917d553cba1850a/skills/doc-and-modernize/SKILL.md#L330-L349" target="_blank" rel="noopener">commit-pinned skill source</a>, its decision fork is:</p>



<ul class="wp-block-list">
<li>If the old component can run at an acceptable cost, capture its behavior before upgrading.</li>



<li>If it cannot run without changes, build and verify a small modern slice first, then expand it.</li>
</ul>



<p class="wp-block-paragraph">The full skill adds the feasibility check, testability milestone, safety levels, phase gates, and evidence requirements.</p>



<p class="wp-block-paragraph">The useful distinction is procedure versus policy. A shared skill can require repository evidence, bounded experiments, explicit safety levels, and runnable exit criteria. Your organization still needs to supply the parts only it knows:</p>



<ul class="wp-block-list">
<li>Approved runtimes, frameworks, and hosting services</li>



<li>Application criticality and acceptable residual risk</li>



<li>Security, compliance, and change-control requirements</li>



<li>Which legacy or production results may be used as the expected answer</li>



<li>Who can approve the architecture and declare a production change complete</li>
</ul>



<p class="wp-block-paragraph">The skill should make those decisions visible. It shouldn&rsquo;t make them for you.</p>



<h2 id="h-start-with-one-bounded-application" class="wp-block-heading">Start with one bounded application</h2>



<p class="wp-block-paragraph">Choose an application with an identified owner, known business value, and one workflow you can observe. Run the documentation workflow first. Review the architecture, commands, runtime surfaces, and unknowns with the people who own the system.</p>



<p class="wp-block-paragraph">Then approve one experiment, not a portfolio rewrite.</p>



<p class="wp-block-paragraph">Inspect the skill and its supporting files before adding it to a project. GitHub CLI&rsquo;s <code>gh skill</code> commands are <a href="https://docs.github.com/en/enterprise-cloud@latest/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/add-skills#managing-skills-with-github-cli" target="_blank" rel="noopener">in public preview</a>; check the current installation guidance when you&rsquo;re ready to use them.</p>



<div class="wp-block-group post-content-cta has-global-padding is-layout-constrained wp-block-group-is-layout-constrained">
<p class="wp-block-paragraph"><a href="https://github.com/github/awesome-copilot/tree/main/skills/doc-and-modernize">Explore the doc-and-modernize agent skill &gt;</a></p>
</div>
</body></html>
<p>The post <a href="https://github.blog/ai-and-ml/github-copilot/how-to-choose-the-next-safe-step-in-application-modernization/">How to choose the next safe step in application modernization</a> appeared first on <a href="https://github.blog">The GitHub Blog</a>.</p>

---
*Artikel ini otomatis diambil dari [GitHub Blog](https://github.blog/ai-and-ml/github-copilot/how-to-choose-the-next-safe-step-in-application-modernization/). Baca artikel selengkapnya di sumber asli.*
