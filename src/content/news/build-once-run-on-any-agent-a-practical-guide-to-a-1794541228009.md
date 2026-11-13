---
title: "Build once, run on any agent: A practical guide to agent skills"
description: "Agent skills capture repeatable engineering work in files teams can review and reuse, while tools, project context, and permissions remain specific to the host that runs them. The post Build once, run..."
pubDate: "2026-11-12T22:54:52.000Z"
authors: ["e-learning"]
organisations: ["github"]
category: "web-development"
tags: ["github", "web-development"]
thumbnail: "https://images.unsplash.com/photo-1573804633927-bfcbcd909acd?auto=format&fit=crop&w=800&h=450&q=80"
heroImage: "https://images.unsplash.com/photo-1573804633927-bfcbcd909acd?auto=format&fit=crop&w=800&h=450&q=80"
keywords: ["github", "tech news", "developer"]
canonicalUrl: "https://github.blog/ai-and-ml/github-copilot/build-once-run-on-any-agent-a-practical-guide-to-agent-skills/"
---

<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN" "http://www.w3.org/TR/REC-html40/loose.dtd">
<html><body><p class="wp-block-paragraph">How many times this week did you re-explain the same task to an agent? How many times did you ask your colleague with deep institutional knowledge how to solve a specific problem? That context shouldn&rsquo;t be lost in a chat or locked in one person&rsquo;s head. Agent skills let you capture repeatable instructions so you and your agents can reuse them. The skill you write for yourself tonight can later become part of a team&rsquo;s pipeline.</p>



<p class="wp-block-paragraph">An agent skill is a versioned directory centered on a <code>SKILL.md</code> file that describes when and how to do a repeatable task. You can share those instructions across agents, but the environment still needs to provide the tools, credentials, and permissions to carry them out. A skill can describe how to query a database, for example, but it doesn&rsquo;t grant access to that database.</p>



<p class="wp-block-paragraph"><em>I covered this topic live with Shishir Tewari, Senior Manager, Data Engineering at Procore, during GitHub Universe 2026. Our session was titled <a href="https://reg.githubuniverse.com/flow/github/universe26/attendee-portal/page/sessioncatalog?search=BLD1787B">&ldquo;Build once, run on any agent: a practical guide to agent skills&rdquo;</a>.</em></p>



<h2 id="h-define-one-job-and-when-it-applies" class="wp-block-heading">Define one job and when it applies</h2>



<p class="wp-block-paragraph">A skill works best when it has a clear job, input, and output. For example, let&rsquo;s say you&rsquo;re building an agent that checks AI-generated changes against the original request. If the same agent assesses its own work, its explanation might influence the review. With <a href="https://github.com/githubevents/build-once-run-on-any-agent/tree/main/demos/proof-agent">Proof Agent</a>, we use a separate verifier to examine the changes and provide evidence for its findings. The skill&rsquo;s frontmatter names the task and describes its purpose:</p>


<div class="wp-block-code-wrapper">
<pre class="wp-block-code language-plaintext"><code>---
name: proof-agent
description: Adversarial verification of AI-generated work. Spawns an independent verifier to check for false claims, broken code, and security issues.
---</code></pre>
<clipboard-copy aria-label="Copy" class="code-copy-btn" data-copy-feedback="Copied!" value="---
name: proof-agent
description: Adversarial verification of AI-generated work. Spawns an independent verifier to check for false claims, broken code, and security issues.
---" tabindex="0" role="button"><svg aria-hidden="true" height="16" viewbox="0 0 16 16" version="1.1" width="16" class="octicon octicon-copy js-clipboard-copy-icon"><path d="M0 6.75C0 5.784.784 5 1.75 5h1.5a.75.75 0 0 1 0 1.5h-1.5a.25.25 0 0 0-.25.25v7.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-1.5a.75.75 0 0 1 1.5 0v1.5A1.75 1.75 0 0 1 9.25 16h-7.5A1.75 1.75 0 0 1 0 14.25Z"></path><path d="M5 1.75C5 .784 5.784 0 6.75 0h7.5C15.216 0 16 .784 16 1.75v7.5A1.75 1.75 0 0 1 14.25 11h-7.5A1.75 1.75 0 0 1 5 9.25Zm1.75-.25a.25.25 0 0 0-.25.25v7.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-7.5a.25.25 0 0 0-.25-.25Z"></path></svg><svg aria-hidden="true" height="16" viewbox="0 0 16 16" version="1.1" width="16" class="octicon octicon-check js-clipboard-check-icon"><path d="M13.78 4.22a.75.75 0 0 1 0 1.06l-7.25 7.25a.75.75 0 0 1-1.06 0L2.22 9.28a.751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018L6 10.94l6.72-6.72a.75.75 0 0 1 1.06 0Z"></path></svg></clipboard-copy></div>


<p class="wp-block-paragraph">At the center is a simple rule: self-verification is not verification.</p>



<p class="wp-block-paragraph">The <code>When to Verify</code> section in Proof Agent&rsquo;s <a href="https://github.com/githubevents/build-once-run-on-any-agent/blob/main/demos/proof-agent/SKILL.md"><code>SKILL.md</code></a> says when to invoke the skill. The remaining instructions explain what the verifier reviews and what evidence to include in its findings.</p>



<h2 id="h-coordinate-multiple-skills-for-a-larger-workflow" class="wp-block-heading">Coordinate multiple skills for a larger workflow</h2>



<p class="wp-block-paragraph">When the judgment that makes a data pipeline correct lives in four different people&rsquo;s heads, those four people are the roadmap. Each change to the pipeline depends on their availability and knowledge of the data. <a href="https://github.com/githubevents/build-once-run-on-any-agent/tree/main/demos/dataforge">Data Forge</a> is an example of how to capture parts of that judgment in skills that work together.</p>



<p class="wp-block-paragraph">In the <a href="https://github.com/githubevents/build-once-run-on-any-agent/blob/main/demos/dataforge/examples/support-desk/DEMO.md">support-desk pipeline example</a>, we start with a request for metrics that count incidents created and resolved. Before building the pipeline, the <code>forge-init</code> skill discovers details such as source tables and naming conventions, then writes them to a resolved manifest. The other skills read that manifest for the project context they need.</p>



<p class="wp-block-paragraph">The <code>forge-builder</code> skill then coordinates the work from the issue through to a pull request. It uses <code>forge-dbx</code> to query the data platform and <code>forge-lint</code> to apply the team&rsquo;s standards as it designs and builds the pipeline. A person reviews the proposed decisions along the way, and the resulting pull request contains the pipeline code, schema changes, and tests.</p>



<p class="wp-block-paragraph">Each skill has a defined responsibility, and they share project context instead of rediscovering it independently. When a step changes, you can change one skill and leave the rest alone.</p>



<p class="wp-block-paragraph">Skills fit bounded, repeatable work that benefits from reusable judgment, such as a checklist, release, migration, or metric pipeline. If a workflow needs a stable interface, shared state, an audit trail, long-running control, or permissions enforced outside the model, an application is a better fit.</p>



<h2 id="h-keep-project-configuration-out-of-the-skill" class="wp-block-heading">Keep project configuration out of the skill</h2>



<p class="wp-block-paragraph">We don&rsquo;t want to hardcode every project&rsquo;s names, database, or conventions into the skills. In the Data Forge support-desk example, a <a href="https://github.com/githubevents/build-once-run-on-any-agent/blob/main/demos/dataforge/examples/support-desk/dataforge.manifest.yaml">manifest</a> describes the project, metric store, and platform so the skills can use the right project details without baking them into their instructions:</p>


<div class="wp-block-code-wrapper">
<pre class="wp-block-code"><code>project:
  name: "support-desk"
  store_name: "support_metric_store"

platform:
  engine: "duckdb"
  pipeline_framework: "spark_python"
  duckdb_dir: "warehouse"</code></pre>
<clipboard-copy aria-label="Copy" class="code-copy-btn" data-copy-feedback="Copied!" value='project:
  name: "support-desk"
  store_name: "support_metric_store"

platform:
  engine: "duckdb"
  pipeline_framework: "spark_python"
  duckdb_dir: "warehouse"' tabindex="0" role="button"><svg aria-hidden="true" height="16" viewbox="0 0 16 16" version="1.1" width="16" class="octicon octicon-copy js-clipboard-copy-icon"><path d="M0 6.75C0 5.784.784 5 1.75 5h1.5a.75.75 0 0 1 0 1.5h-1.5a.25.25 0 0 0-.25.25v7.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-1.5a.75.75 0 0 1 1.5 0v1.5A1.75 1.75 0 0 1 9.25 16h-7.5A1.75 1.75 0 0 1 0 14.25Z"></path><path d="M5 1.75C5 .784 5.784 0 6.75 0h7.5C15.216 0 16 .784 16 1.75v7.5A1.75 1.75 0 0 1 14.25 11h-7.5A1.75 1.75 0 0 1 5 9.25Zm1.75-.25a.25.25 0 0 0-.25.25v7.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-7.5a.25.25 0 0 0-.25-.25Z"></path></svg><svg aria-hidden="true" height="16" viewbox="0 0 16 16" version="1.1" width="16" class="octicon octicon-check js-clipboard-check-icon"><path d="M13.78 4.22a.75.75 0 0 1 0 1.06l-7.25 7.25a.75.75 0 0 1-1.06 0L2.22 9.28a.751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018L6 10.94l6.72-6.72a.75.75 0 0 1 1.06 0Z"></path></svg></clipboard-copy></div>


<p class="wp-block-paragraph">With the discovered details in the resolved manifest, the other skills don&rsquo;t have to make their own assumptions about the project.</p>



<p class="wp-block-paragraph">Keep discovered project details separate from team standards. The <code>forge-init</code> initialization skill can inspect existing schemas and code patterns; the <code>forge-lint</code> skill supplies the rules to follow before code is generated. A pattern found in a repository isn&rsquo;t automatically a rule to preserve, so make discovered details reviewable and let people confirm or change them where business judgment is needed. To check portability, ask: could another team use these skills with its own manifest without rewriting them?</p>



<h2 id="h-share-execution-paths-between-skills" class="wp-block-heading">Share execution paths between skills</h2>



<p class="wp-block-paragraph">The <code>forge-dbx</code> skill gives the other Data Forge skills one way to access the data platform. It reads the manifest and selects a Databricks or DuckDB adapter, so each skill doesn&rsquo;t need its own connection and query logic.</p>



<p class="wp-block-paragraph">The manifest can choose DuckDB for the local support-desk demo, but it can&rsquo;t install the DuckDB CLI or grant database access. Those remain setup responsibilities for the environment and its user. The manifest picks the engine; the host provides the access.</p>



<h2 id="h-put-checks-and-approval-points-in-the-workflow" class="wp-block-heading">Put checks and approval points in the workflow</h2>



<p class="wp-block-paragraph">The Data Forge support-desk workflow has seven phases, with a human checkpoint at each one. The <a href="https://github.com/githubevents/build-once-run-on-any-agent/blob/main/demos/dataforge/README.md#the-skills">workflow overview</a> shows how the skills fit together. Before a phase proceeds, the workflow checks that the previous phase is complete; before schema changes run, it asks for approval.</p>



<p class="wp-block-paragraph">The Proof Agent example applies the same principle to code review. A GitHub Actions workflow gives the verifier the original request and code changes, but not the coding agent&rsquo;s own assessment. The verifier returns <code>PASS</code>, <code>FAIL</code>, or <code>PARTIAL</code> and must cite evidence in the changed files. The GitHub Action does a static review of the diff; it doesn&rsquo;t execute code or run tests. In the demo workflow, we set <code>block-on-fail: false</code> so findings were advisory. By default, a <code>FAIL</code> fails the check, which blocks the merge if the check is required. The aim is to keep test results and merge requirements in your repository&rsquo;s CI checks and rules.</p>



<h2 id="h-package-for-each-agent-client-then-verify-its-setup" class="wp-block-heading">Package for each agent client, then verify its setup</h2>



<p class="wp-block-paragraph">Supporting multiple clients can mean keeping several copies of the same instructions in sync. In the Data Forge example, we keep <code>skills/*/SKILL.md</code> as the source and use a packaging script to generate client-specific files: <code>.claude-plugin/plugin.json</code> for Claude Code, <code>.cursor-plugin/plugin.json</code> for Cursor, and <code>.github/agents/*.agent.md</code> for GitHub Copilot. The script also includes a check mode for CI:</p>


<div class="wp-block-code-wrapper">
<pre class="wp-block-code"><code>node scripts/package.mjs --check</code></pre>
<clipboard-copy aria-label="Copy" class="code-copy-btn" data-copy-feedback="Copied!" value="node scripts/package.mjs --check" tabindex="0" role="button"><svg aria-hidden="true" height="16" viewbox="0 0 16 16" version="1.1" width="16" class="octicon octicon-copy js-clipboard-copy-icon"><path d="M0 6.75C0 5.784.784 5 1.75 5h1.5a.75.75 0 0 1 0 1.5h-1.5a.25.25 0 0 0-.25.25v7.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-1.5a.75.75 0 0 1 1.5 0v1.5A1.75 1.75 0 0 1 9.25 16h-7.5A1.75 1.75 0 0 1 0 14.25Z"></path><path d="M5 1.75C5 .784 5.784 0 6.75 0h7.5C15.216 0 16 .784 16 1.75v7.5A1.75 1.75 0 0 1 14.25 11h-7.5A1.75 1.75 0 0 1 5 9.25Zm1.75-.25a.25.25 0 0 0-.25.25v7.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-7.5a.25.25 0 0 0-.25-.25Z"></path></svg><svg aria-hidden="true" height="16" viewbox="0 0 16 16" version="1.1" width="16" class="octicon octicon-check js-clipboard-check-icon"><path d="M13.78 4.22a.75.75 0 0 1 0 1.06l-7.25 7.25a.75.75 0 0 1-1.06 0L2.22 9.28a.751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018L6 10.94l6.72-6.72a.75.75 0 0 1 1.06 0Z"></path></svg></clipboard-copy></div>


<p class="wp-block-paragraph">Clients can differ in their available models, tools, credentials, and permissions. After installing a skill, check that the client recognizes it for the intended task, loads its references, can use the required tools, and returns the evidence the skill calls for.</p>



<h2 id="h-three-practices-for-building-agent-skills" class="wp-block-heading">Three practices for building agent skills</h2>



<ol class="wp-block-list">
<li><strong>Put business context in one file.</strong> Keep project-specific names, schemas, and conventions in a manifest, and have each skill read it instead of duplicating those facts. This gives the team one visible place to update configuration.</li>



<li><strong>Give related skills one consistent way to use the platform.</strong> Have related skills use the same path to run queries, jobs, and schema changes instead of each defining its own commands and connection logic. This makes the workflow&rsquo;s access easier to understand and its behavior easier to check in each client.</li>



<li><strong>Use instructions to guide the work, and checks to verify it.</strong> A skill can tell an agent which standards to follow and when to ask for approval. Add human review at decisions that need context or judgment, and use tests and CI checks to catch issues before a change moves forward. If a check must block a merge or deployment, configure that requirement in your repository&rsquo;s rules.</li>
</ol>



<p class="wp-block-paragraph">Start with the task you&rsquo;re tired of explaining. Write down what it needs, where it stops, and what a reviewer sees. Then try it with another agent and improve it as you learn what works.</p>



<div class="wp-block-group post-content-cta has-global-padding is-layout-constrained wp-block-group-is-layout-constrained">
<p class="wp-block-paragraph"><a href="https://docs.github.com/en/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/add-skills">Add an agent skill to GitHub Copilot &gt;</a></p>
</div>
</body></html>
<p>The post <a href="https://github.blog/ai-and-ml/github-copilot/build-once-run-on-any-agent-a-practical-guide-to-agent-skills/">Build once, run on any agent: A practical guide to agent skills</a> appeared first on <a href="https://github.blog">The GitHub Blog</a>.</p>

---
*Artikel ini otomatis diambil dari [GitHub Blog](https://github.blog/ai-and-ml/github-copilot/build-once-run-on-any-agent-a-practical-guide-to-agent-skills/). Baca artikel selengkapnya di sumber asli.*
