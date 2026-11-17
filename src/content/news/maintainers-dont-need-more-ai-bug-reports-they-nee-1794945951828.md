---
title: "Maintainers don’t need more AI bug reports. They need fixes. "
description: "In the Codename MDASH pilot, GitHub and Microsoft turned thousands of raw AI findings into 99 checked, private reports with suggested fixes for six widely used open source projects. Maintainers have a..."
pubDate: "2026-11-17T18:29:29.000Z"
authors: ["e-learning"]
organisations: ["github"]
category: "web-development"
tags: ["github", "web-development"]
thumbnail: "https://images.unsplash.com/photo-1573804633927-bfcbcd909acd?auto=format&fit=crop&w=800&h=450&q=80"
heroImage: "https://images.unsplash.com/photo-1573804633927-bfcbcd909acd?auto=format&fit=crop&w=800&h=450&q=80"
keywords: ["github", "tech news", "developer"]
canonicalUrl: "https://github.blog/open-source/maintainers/maintainers-dont-need-more-ai-bug-reports-they-need-fixes/"
---

<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN" "http://www.w3.org/TR/REC-html40/loose.dtd">
<html><body><p class="wp-block-paragraph">AI has made finding potential vulnerabilities fast and cheap. Fixing them still takes human judgment, and in open source, that work falls to maintainers. Many are already flooded with AI-generated security findings, including duplicates and reports that aren&rsquo;t vulnerabilities at all. Another raw scan doesn&rsquo;t help them. Anyone pointing a new model at a project should ask its maintainers first and come with fixes, not just findings.</p>



<p class="wp-block-paragraph">Maintainers are under intense pressure from this flood of reports. As Christian Grobmeier, a maintainer of Log4j, has put it, the <a href="https://youtu.be/t74ClffSUW0?si=WYa9E6mKPSVhOW1O&amp;t=1815">AI defending open source has to be better than the AI attacking it</a>.</p>



<p class="wp-block-paragraph">AI shouldn&rsquo;t add to maintainers&rsquo; workload. So, with maintainers, GitHub and Microsoft ran a pilot together: could AI help maintainers ship fixes instead of handing them more findings?</p>



<p class="wp-block-paragraph">GitHub and Microsoft are using <a href="https://learn.microsoft.com/en-us/security-exposure-management/ai-code-security-overview" target="_blank" rel="noopener">Codename MDASH</a>, Microsoft&rsquo;s multi-model agentic scanning harness, to find and fix security vulnerabilities in widely used open source software, strengthening the foundation that much of the world&rsquo;s code is built on.</p>



<h2 id="h-the-process" class="wp-block-heading">The process</h2>



<p class="wp-block-paragraph">Our pilot had two goals: send only findings that matter, and never send a maintainer an unverified finding. Here&rsquo;s how it worked:</p>



<ol class="wp-block-list">
<li><strong>Find.</strong> <a href="https://www.microsoft.com/en-us/security/blog/2026/05/12/defense-at-ai-speed-microsofts-new-multi-model-agentic-security-system-tops-leading-industry-benchmark/">Codename MDASH</a>, an AI system from Microsoft Security, scanned each project using Microsoft&rsquo;s <a href="https://microsoft.ai/models/mai-cyber-1-1-flash/">MAI Cyber model</a> and suggested fixes for what it found.</li>



<li><strong>Filter. </strong>An <a href="https://github.blog/security/ai-supported-vulnerability-triage-with-the-github-security-lab-taskflow-agent/">AI triage agent from the GitHub Security Lab</a>, built on tailored Copilot skills, removed duplicates, checked validity, and focused on high-impact bugs, so maintainers never had to review raw scan results.</li>



<li><strong>Verify.</strong> <a href="https://forge-blog.github.io/">Researchers from Microsoft FORGE Lab</a> checked every remaining finding by hand.</li>



<li><strong>Report privately. </strong>Reports reached maintainers through GitHub private vulnerability reporting, with a proof of concept and a suggested fix.</li>



<li><strong>Fix. </strong>Maintainers decided what to fix, how to fix it, and when.</li>
</ol>



<p class="wp-block-paragraph">Reports and fixes stayed on GitHub, where maintainers already work.</p>



<p class="wp-block-paragraph">The result: Codename MDASH, using Microsoft&rsquo;s MAI Cyber model, flagged thousands of possible issues across the six projects. After AI triage and hand review by seven Microsoft FORGE Lab researchers, maintainers received 99 private reports. As of November 15, they had fixed 55, and 14 of those fixes were already in a release.</p>



<figure class="wp-block-image size-large"><img data-recalc-dims="1" fetchpriority="high" decoding="async" height="576" width="1024" src="https://github.blog/wp-content/uploads/2026/11/01-participating-projects.png?resize=1024%2C576" alt="Logos for AutoGPT, Caddy, Express, FastMCP, ImageMagick, and Netty." class="wp-image-100254" srcset="https://github.blog/wp-content/uploads/2026/11/01-participating-projects.png?w=3200 3200w, https://github.blog/wp-content/uploads/2026/11/01-participating-projects.png?w=300 300w, https://github.blog/wp-content/uploads/2026/11/01-participating-projects.png?w=768 768w, https://github.blog/wp-content/uploads/2026/11/01-participating-projects.png?w=1024 1024w, https://github.blog/wp-content/uploads/2026/11/01-participating-projects.png?w=1536 1536w, https://github.blog/wp-content/uploads/2026/11/01-participating-projects.png?w=2048 2048w, https://github.blog/wp-content/uploads/2026/11/01-participating-projects.png?w=3000 3000w" sizes="(max-width: 1000px) 100vw, 1000px" /></figure>



<aside data-color-mode="light" data-dark-theme="dark" data-light-theme="light_dimmed" class="wp-block-group post-aside--large p-4 p-md-6 is-style-light-dimmed has-global-padding is-layout-constrained wp-block-group-is-layout-constrained is-style-light-dimmed--1" style="border-top-width:4px">
<h2 id="h-participating-projects" class="wp-block-heading h5-mktg gh-aside-title is-typography-preset-h5" style="margin-top:0">Participating projects</h2>



<p class="wp-block-paragraph">Six projects took part, all alumni of the <a href="https://github.com/open-source/github-secure-open-source-fund">GitHub Secure Open Source Fund</a>. Their maintainers agreed to the scan, received every report privately, decided what to fix, and shared what worked:</p>



<ul class="wp-block-list">
<li><a href="https://github.com/Significant-Gravitas/AutoGPT">AutoGPT</a>, an AI agent platform</li>



<li><a href="https://github.com/caddyserver/caddy">Caddy</a>, a web server</li>



<li><a href="https://github.com/expressjs/express">Express</a>, a web framework for <a href="https://github.com/nodejs/node">Node.js</a></li>



<li><a href="https://github.com/prefecthq/fastmcp">FastMCP</a>, a framework for building MCP servers</li>



<li><a href="https://github.com/ImageMagick/ImageMagick">ImageMagick</a>, image processing software</li>



<li><a href="https://github.com/netty/netty">Netty</a>, a networking framework for Java</li>
</ul>
</aside>



<figure class="wp-block-image size-large"><img data-recalc-dims="1" decoding="async" height="571" width="1024" src="https://github.blog/wp-content/uploads/2026/11/mdash-pilot-result-highres.png?resize=1024%2C571" alt="Infographic titled &lsquo;Codename MDASH Pilot: The Result.&rsquo; It shows 9,791 possible issues flagged across six open-source projects, leading to 99 private reports sent to maintainers after AI triage and human review&mdash;about one report for every 100 raw findings." class="wp-image-100265" srcset="https://github.blog/wp-content/uploads/2026/11/mdash-pilot-result-highres.png?w=4616 4616w, https://github.blog/wp-content/uploads/2026/11/mdash-pilot-result-highres.png?w=300 300w, https://github.blog/wp-content/uploads/2026/11/mdash-pilot-result-highres.png?w=768 768w, https://github.blog/wp-content/uploads/2026/11/mdash-pilot-result-highres.png?w=1024 1024w, https://github.blog/wp-content/uploads/2026/11/mdash-pilot-result-highres.png?w=1536 1536w, https://github.blog/wp-content/uploads/2026/11/mdash-pilot-result-highres.png?w=2048 2048w, https://github.blog/wp-content/uploads/2026/11/mdash-pilot-result-highres.png?w=3000 3000w" sizes="(max-width: 1000px) 100vw, 1000px" /></figure>



<h2 id="h-what-we-learned" class="wp-block-heading">What we learned</h2>



<p class="wp-block-paragraph">These lessons come from what the six projects fixed and what their maintainers chose to share. Six projects is a small sample, so read them as early lessons.</p>



<h3 id="h-1-fixes-matter-more-than-findings" class="wp-block-heading">1. Fixes matter more than findings</h3>



<p class="wp-block-paragraph">Fixing is where maintainers spend their time. So every report arrived already checked, with a suggested fix to review, adapt, and merge. Maintainers started from a well-scoped patch, not a raw finding.</p>



<p class="wp-block-paragraph">The first fixes shipped about a week after the initial scan. Caddy&rsquo;s maintainers fixed all 22 of the reports they received in a week or less. Maintainers used the suggested fixes, some as written and some with changes.</p>



<p class="wp-block-paragraph">AutoGPT&rsquo;s maintainers had recently shipped a new check, and it was broken. The report landed three days later and pointed at the right line. They took the fix further than the suggested patch, wrote it within a day, and had it in production less than a week later.</p>



<blockquote class="wp-block-quote is-layout-flow wp-block-quote-is-layout-flow">
<p class="wp-block-paragraph">Finding vulnerabilities is no longer our bottleneck. Fixing them is the hard part, and that depends on maintainers.</p>
<cite>Netty maintainer&nbsp;</cite></blockquote>



<h3 id="h-2-codename-mdash-with-researcher-review-gave-maintainers-high-quality-findings-with-fixes" class="wp-block-heading">2. Codename MDASH, with researcher review, gave maintainers high-quality findings with fixes</h3>



<p class="wp-block-paragraph">Maintainers got a short list, not a flood. Express maintainers said the reports cut through the noise. Caddy maintainers said they surfaced obscure bugs and filled gaps that earlier reports had missed. For FastMCP, the scan went beyond what AI scanners had given them before. ImageMagick, maintained by a two-person team, received 22 reports.</p>



<p class="wp-block-paragraph">AI did the finding, researchers did the checking, and maintainers stayed in charge.</p>



<p class="wp-block-paragraph">One ImageMagick report looked like a kind of issue the maintainers had seen before. Its summary showed it wasn&rsquo;t and pointed to the exact lines of code. They were able to push a fix in 15 minutes.</p>



<blockquote class="wp-block-quote is-layout-flow wp-block-quote-is-layout-flow">
<p class="wp-block-paragraph">Normally, a security advisory would take us around 30 minutes to 1 hour on average to read, understand, and make a fix. It is a huge difference.</p>
<cite>ImageMagick maintainer</cite></blockquote>



<blockquote class="wp-block-quote is-layout-flow wp-block-quote-is-layout-flow">
<p class="wp-block-paragraph">Codename MDASH was able to find unknown, obscure security bugs in our project with succinct reports, and it filled in gaps from some prior reports that hadn&rsquo;t been noticed.</p>
<cite>Caddy maintainer</cite></blockquote>



<h3 id="h-3-open-source-still-has-gaps-to-close" class="wp-block-heading">3. Open source still has gaps to close</h3>



<p class="wp-block-paragraph">The pilot worked because people did careful work at every step, and that work doesn&rsquo;t scale yet. Seven researchers spent about two days per project checking findings by hand. Meanwhile, low-quality reports still reach maintainers every day.</p>



<p class="wp-block-paragraph">For open source, fixes need to be delivered via private vulnerability reporting (PVR). Maintainers need to run CI/CD tests on private forks to verify those fixes. And to close the gap between defenders and attackers, maintainers need access to cyber-capable models to verify a proof of concept, before and after the fix.</p>



<blockquote class="wp-block-quote is-layout-flow wp-block-quote-is-layout-flow">
<p class="wp-block-paragraph">The Codename MDASH scan provided unique insights into the security of our project above and beyond what AI scanners have provided us in the past</p>
<cite>FastMCP maintainer</cite></blockquote>



<h2 id="h-why-it-matters" class="wp-block-heading">Why it matters</h2>



<p class="wp-block-paragraph">If you&rsquo;re a maintainer, this approach changes what a security report costs you. A checked, private report that comes with a suggested fix takes less of your time. You shouldn&rsquo;t have to triage a scanner&rsquo;s output. That&rsquo;s the sender&rsquo;s job.</p>



<p class="wp-block-paragraph">If you use AI to find vulnerabilities in open source, maintainers&rsquo; feedback points to this standard:</p>



<ul class="wp-block-list">
<li>Check every finding before you send it, against what the project already knows and against its threat model and security policy.</li>



<li>Report it privately.</li>



<li>Include a suggested fix. Let maintainers set the pace.</li>
</ul>



<blockquote class="wp-block-quote is-layout-flow wp-block-quote-is-layout-flow">
<p class="wp-block-paragraph">Features are easier and easier to ship, but in the future maintaining software will probably come down to who cares enough to manage the security.</p>
<cite>AutoGPT maintainer</cite></blockquote>



<blockquote class="wp-block-quote is-layout-flow wp-block-quote-is-layout-flow">
<p class="wp-block-paragraph">Codename MDASH is very useful for cutting through the noise of vulnerability reports.</p>
<cite>Express maintainer</cite></blockquote>



<h2 id="h-what-you-can-do-today" class="wp-block-heading">What you can do today</h2>



<ul class="wp-block-list">
<li>If you maintain any open source project, turn on <a href="https://docs.github.com/en/code-security/how-tos/report-and-fix-vulnerabilities/configure-vulnerability-reporting/configure-for-a-repository">private vulnerability reporting</a> so researchers can report security issues to you without making them public. If your project is on GitHub, turn on core security features in about two minutes with <br><a href="https://gh.io/gh-secure"><code>gh-secure</code></a>.</li>



<li>If you build on open source, turn on <a href="https://docs.github.com/code-security/concepts/supply-chain-security/dependabot-alerts">Dependabot alerts</a> so you know when a dependency you use needs a security update.</li>



<li>If you send AI-generated vulnerability reports to open source projects, hold them to the standard above before you send them.</li>



<li>Repositories using GitHub code security can use <a href="https://docs.github.com/en/enterprise-cloud@latest/code-security/concepts/code-scanning/autofix-for-code-scanning#using-agentic-autofix">agentic autofix</a> to help automatically remediate their alerts with Copilot</li>
</ul>



<h2 id="h-what-happens-next" class="wp-block-heading">What happens next</h2>



<ol class="wp-block-list">
<li><strong>Fixes, where maintainers already work.</strong> GitHub is bringing these lessons into private vulnerability reporting and security advisories, where GitHub Security Lab is already investing to reduce the burden of low-quality reports on maintainers.</li>



<li><strong>MAI Cyber 2.</strong> Today Microsoft announced <a href="https://aka.ms/introducing-mai-cyber-2-flash">MAI Cyber 2</a>. The Codename MDASH pilot ran on the earlier MAI Cyber model. GitHub and Microsoft will keep working together to help secure open source.</li>



<li><strong>Closing the gaps, starting with the <a href="https://github.com/open-source/github-secure-open-source-fund">GitHub Secure Open Source Fund</a>. </strong>We&rsquo;re working with Microsoft to scale researcher review without cutting corners. GitHub will offer every alumni project AI security scans, with findings checked before they&rsquo;re reported privately. Alumni also get <a href="https://github.com/features/copilot/plans">GitHub Copilot Max</a>. We&rsquo;ll bring this support to future sessions of the Fund.</li>
</ol>



<p class="wp-block-paragraph">Everyone downstream benefits from this system. When a maintainer publishes a security advisory on GitHub and it&rsquo;s reviewed for the GitHub Advisory Database, Dependabot can alert the repositories that depend on that package. That&rsquo;s the idea behind the GitHub Secure Open Source Fund: 216 projects so far, with more than 300 maintainers fixing more than 4,000 vulnerabilities in software downloaded more than 20 billion times a month.</p>



<h2 id="h-thank-you" class="wp-block-heading">Thank you</h2>



<p class="wp-block-paragraph">To the maintainers of AutoGPT, Caddy, Express, FastMCP, ImageMagick, and Netty: thank you for your time, your trust, and your feedback. Thank you also to the Codename MDASH team at Microsoft Security, the MAI Cyber team, the researchers of Microsoft FORGE Lab, and GitHub Security Lab.</p>



<p class="wp-block-paragraph">Maintainers do the fixing. Our job is to make sure they never do it alone.</p>



<p class="wp-block-paragraph"><em>Hear from maintainers about their experience with Codename MDASH. &#128071;</em></p>



<figure class="wp-block-video"><video height="1080" style="aspect-ratio: 1920 / 1080;" width="1920" controls poster="https://github.blog/wp-content/uploads/2026/11/Screenshot-2026-11-16-at-8.27.46-PM.png" src="https://github.blog/wp-content/uploads/2026/11/MDASH-Updated-Final-Video-1-1.mp4"></video></figure>



<h2 id="h-about-the-github-secure-open-source-fund" class="wp-block-heading">About the GitHub Secure Open Source Fund</h2>



<p class="wp-block-paragraph">All six projects in this pilot are alumni of the <a href="https://github.com/open-source/github-secure-open-source-fund">GitHub Secure Open Source Fund</a>, which gives each project $10,000 through GitHub Sponsors, plus security training and a supportive alumni community.</p>



<p class="wp-block-paragraph">The GitHub Secure Open Source Fund, powered by <a href="https://github.com/open-source/sponsors">GitHub Sponsors</a>, funds open source projects that underpin the digital supply chain, catalyze innovation, and are critical to the AI stack. Want your company to help secure the open source it depends on? Become a funding partner through the Fund page. <a href="https://github.com/open-source/github-secure-open-source-fund">Learn more about the Fund or apply</a>.</p>



<figure class="wp-block-image size-large"><img data-recalc-dims="1" decoding="async" height="576" width="1024" src="https://github.blog/wp-content/uploads/2026/11/TEST-MDASH-Slide-1-4K-1.png?resize=1024%2C576" alt="GitHub Secure Open Source Fund slide: &lsquo;Fund one. Protect millions of users.&rsquo; It reports 216 projects improved their security, with 300+ maintainers, 20+ billion monthly downloads, 4,000+ vulnerabilities fixed, 850+ secret leaks prevented or resolved, and 1,500+ vulnerable dependencies updated. Figures are from a February 17, 2026 program report and current program data." class="wp-image-100274" srcset="https://github.blog/wp-content/uploads/2026/11/TEST-MDASH-Slide-1-4K-1.png?w=3840 3840w, https://github.blog/wp-content/uploads/2026/11/TEST-MDASH-Slide-1-4K-1.png?w=300 300w, https://github.blog/wp-content/uploads/2026/11/TEST-MDASH-Slide-1-4K-1.png?w=768 768w, https://github.blog/wp-content/uploads/2026/11/TEST-MDASH-Slide-1-4K-1.png?w=1024 1024w, https://github.blog/wp-content/uploads/2026/11/TEST-MDASH-Slide-1-4K-1.png?w=1536 1536w, https://github.blog/wp-content/uploads/2026/11/TEST-MDASH-Slide-1-4K-1.png?w=2048 2048w, https://github.blog/wp-content/uploads/2026/11/TEST-MDASH-Slide-1-4K-1.png?w=3000 3000w" sizes="(max-width: 1000px) 100vw, 1000px" /></figure>



<p class="wp-block-paragraph">Through the Fund, maintainers receive security training and <a href="https://azure.microsoft.com/en-us">Azure</a> credits from <a href="https://www.microsoft.com/en-us/startups">Microsoft for Startups.</a> That support sits alongside <a href="https://blogs.microsoft.com/on-the-issues/2025/06/04/microsoft-launches-new-european-security-program/">Microsoft&rsquo;s European Security Program</a> and its <a href="https://www.microsoft.com/en-us/trust-center/security/secure-future-initiative">Secure Future Initiative</a>, whose principle is security above all else. Most organizations are downstream in the same way. If your software depends on open source, do the same: back the maintainers you rely on.</p>



<h3 id="h-all-alumni-of-the-github-secure-open-source-fund" class="wp-block-heading">All alumni of the GitHub Secure Open Source Fund</h3>



<p class="wp-block-paragraph"><strong>AI, machine learning, and intelligent systems</strong></p>



<p class="wp-block-paragraph"><a href="https://github.com/aipotheosis-labs/aci">ACI.dev</a> &bull; <a href="https://github.com/albumentations-team/AlbumentationsX">AlbumentationsX</a> &bull; <a href="https://github.com/arviz-devs/arviz">ArviZ</a> &bull; <a href="https://github.com/Significant-Gravitas/gravitasml">AutoGPT/GravitasML</a> &bull; <a href="https://github.com/camel-ai/camel">CAMEL-AI</a> &bull; <a href="https://github.com/Garudex-Labs/Caracal">Caracal</a> &bull; <a href="https://github.com/cocoindex-io/cocoindex">CocoIndex</a> &bull; <a href="https://github.com/vitali87/code-graph-rag">Code-Graph-RAG</a> &bull; <a href="https://github.com/mlco2/codecarbon">CodeCarbon</a> &bull; <a href="https://github.com/topoteretes/cognee">Cognee</a> &bull; <a href="https://github.com/langchain-ai/deepagents">Deep Agents</a> &bull; <a href="https://github.com/dipy/dipy">DIPY</a> &bull; <a href="https://github.com/arc53/DocsGPT">DocsGPT</a> &bull; <a href="https://github.com/PrefectHQ/fastmcp">FastMCP</a> &bull; <a href="https://github.com/jupyter">Jupyter</a> &bull; <a href="https://github.com/LadybugDB/ladybug">LadybugDB</a> &bull; <a href="https://github.com/langchain-ai/langchain">LangChain</a> &bull; <a href="https://github.com/matplotlib/matplotlib">Matplotlib</a> &bull; <a href="https://github.com/czlonkowski/n8n-mcp">n8n-MCP</a> &bull; <a href="https://github.com/Nasiko-Labs/nasiko">Nasiko</a> &bull; <a href="https://github.com/numpy/numpy">NumPy</a> &bull; <a href="https://github.com/ollama/ollama">Ollama</a> &bull; <a href="https://github.com/onnx/onnx">ONNX</a> &bull; <a href="https://github.com/OpenBB-finance/OpenBB">OpenBB Platform</a> &bull; <a href="https://github.com/openclaw/openclaw">OpenClaw</a> &bull; <a href="https://github.com/opencv/opencv">OpenCV</a> &bull; <a href="https://github.com/open-metadata/OpenMetadata">OpenMetadata</a> &bull; <a href="https://github.com/opensearch-project/OpenSearch">OpenSearch</a> &bull; <a href="https://github.com/VectifyAI/PageIndex">PageIndex</a> &bull; <a href="https://github.com/pandas-dev/pandas">pandas</a> &bull; <a href="https://github.com/pydantic/pydantic">Pydantic</a> &bull; <a href="https://github.com/pymc-devs/pymc">PyMC</a> &bull; <a href="https://github.com/alexrudall/ruby-openai">Ruby-OpenAI</a> &bull; <a href="https://github.com/BerkeleyLearnVerify/Scenic">Scenic</a> &bull; <a href="https://github.com/scikit-learn/scikit-learn">scikit-learn</a> &bull; <a href="https://github.com/scipy/scipy">SciPy</a> &bull; <a href="https://github.com/oraios/serena">Serena</a> &bull; <a href="https://github.com/theagentplane/tokenops">TokenOps</a> &bull; <a href="https://github.com/traceroot-ai/traceroot">TraceRoot</a> &bull; <a href="https://github.com/ml-energy/zeus">Zeus</a></p>



<p class="wp-block-paragraph"><strong>Build systems, supply chain, and release tooling</strong></p>



<p class="wp-block-paragraph"><a href="https://github.com/apache/airflow">Apache Airflow</a> &bull; <a href="https://github.com/babel/babel">Babel</a> &bull; <a href="https://github.com/ublue-os/bluefin">Bluefin</a> &bull; <a href="https://github.com/bootc-dev/bootc">bootc</a> &bull; <a href="https://github.com/browserslist/browserslist">Browserslist</a> &bull; <a href="https://github.com/changesets/changesets">Changesets</a> &bull; <a href="https://github.com/abiosoft/colima">Colima</a> &bull; <a href="https://github.com/concourse/concourse">Concourse</a> &bull; <a href="https://github.com/cucumber">Cucumber</a> &bull; <a href="https://github.com/CycloneDX/cyclonedx-python-lib">CycloneDX Python Library</a> &bull; <a href="https://github.com/fluxcd/flux2">Flux</a> &bull; <a href="https://github.com/foundry-rs/foundry">Foundry</a> &bull; <a href="https://github.com/GitoxideLabs/gitoxide">Gitoxide</a> &bull; <a href="https://github.com/golangci/golangci-lint">golangci-lint</a> &bull; <a href="https://github.com/goreleaser/goreleaser">GoReleaser</a> &bull; <a href="https://github.com/wighawag/hardhat-deploy">hardhat-deploy</a> &bull; <a href="https://github.com/jenkinsci/jenkins">Jenkins</a> &bull; <a href="https://github.com/jreleaser/jreleaser">JReleaser</a> &bull; <a href="https://github.com/jupyter/docker-stacks">Jupyter Docker Stacks</a> &bull; <a href="https://github.com/NixOS/nixpkgs">NixOS/Nixpkgs</a> &bull; <a href="https://github.com/isaacs/node-lru-cache">node-lru-cache</a> &bull; <a href="https://github.com/oapi-codegen/oapi-codegen">oapi-codegen</a> &bull; <a href="https://github.com/postcss/postcss">PostCSS</a> &bull; <a href="https://github.com/pypi/warehouse">PyPI/Warehouse</a> &bull; <a href="https://github.com/isaacs/rimraf">rimraf</a> &bull; <a href="https://github.com/go-task/task">Task</a> &bull; <a href="https://github.com/termux">Termux</a> &bull; <a href="https://github.com/terrapkg/packages">Terra</a> &bull; <a href="https://github.com/vercel/turborepo">Turborepo</a> &bull; <a href="https://github.com/warp-tech/warpgate">Warpgate</a> &bull; <a href="https://github.com/webpack/webpack">webpack</a> &bull; <a href="https://github.com/zizmorcore/zizmor">zizmor</a></p>



<p class="wp-block-paragraph"><strong>Core programming languages, runtimes, and foundational libraries</strong></p>



<p class="wp-block-paragraph"><a href="https://github.com/apache/commons-parent">Apache Commons</a> &bull; <a href="https://github.com/apache/groovy">Apache Groovy</a> &bull; <a href="https://github.com/raphw/byte-buddy">Byte Buddy</a> &bull; <a href="https://github.com/zloirock/core-js">core-js</a> &bull; <a href="https://github.com/python/cpython">CPython</a> &bull; <a href="https://github.com/typelevel/fs2">FS2</a> &bull; <a href="https://github.com/gleam-lang/gleam">Gleam</a> &bull; <a href="https://github.com/himmelblau-idm/himmelblau">Himmelblau</a> &bull; <a href="https://github.com/bigskysoftware/htmx">htmx</a> &bull; <a href="https://github.com/llvm/llvm-project">LLVM</a> &bull; <a href="https://github.com/modernc-org/sqlite">modernc.org/sqlite</a> &bull; <a href="https://github.com/napi-rs/napi-rs">NAPI-RS</a> &bull; <a href="https://github.com/nodejs/node">Node.js</a> &bull; <a href="https://github.com/apple/pkl">Pkl</a> &bull; <a href="https://github.com/pyodide/pyodide">Pyodide</a> &bull; <a href="https://github.com/rustls/rustls">Rustls</a> &bull; <a href="https://github.com/termcolor/termcolor">termcolor</a> &bull; <a href="https://github.com/time-rs/time">time-rs</a></p>



<p class="wp-block-paragraph"><strong>Developer tools and productivity platforms</strong></p>



<p class="wp-block-paragraph"><a href="https://github.com/airqo-platform">AirQo</a> &bull; <a href="https://github.com/foss42/apidash">API Dash</a> &bull; <a href="https://github.com/ArduPilot/ardupilot">ArduPilot</a> &bull; <a href="https://github.com/assertj">AssertJ</a> &bull; <a href="https://github.com/asyncapi">AsyncAPI Initiative</a> &bull; <a href="https://github.com/bevyengine/bevy">Bevy</a> &bull; <a href="https://github.com/twbs/bootstrap">Bootstrap</a> &bull; <a href="https://github.com/kovidgoyal/calibre">calibre</a> &bull; <a href="https://github.com/jawah/charset_normalizer">Charset Normalizer</a> &bull; <a href="https://github.com/cheeriojs/cheerio">Cheerio</a> &bull; <a href="https://github.com/bee-san/ciphey">Ciphey</a> &bull; <a href="https://github.com/spf13/cobra">Cobra</a> &bull; <a href="https://github.com/instavm/coderunner">CodeRunner</a> &bull; <a href="https://github.com/Siilwyn/css-declaration-sorter">CSS Declaration Sorter</a> &bull; <a href="https://github.com/sayantikabanik/DataJourney">DataJourney</a> &bull; <a href="https://github.com/diesel-rs/diesel">Diesel</a> &bull; <a href="https://github.com/egovernments/DIGIT-Core">DIGIT</a> &bull; <a href="https://github.com/electron/electron">Electron</a> &bull; <a href="https://github.com/frappe/erpnext">ERPNext</a> &bull; <a href="https://github.com/fabricjs/fabric.js">fabric.js</a> &bull; <a href="https://github.com/gitpython-developers/GitPython">GitPython</a> &bull; <a href="https://github.com/hoppscotch/hoppscotch">Hoppscotch</a> &bull; <a href="https://github.com/ImageMagick/ImageMagick">ImageMagick</a> &bull; <a href="https://github.com/jquery/jquery">jQuery</a> &bull; <a href="https://github.com/nodeca/js-yaml">js-yaml</a> &bull; <a href="https://github.com/jhy/jsoup">jsoup</a> &bull; <a href="https://github.com/junit-team">JUnit</a> &bull; <a href="https://github.com/yt-project/libyt">libyt</a> &bull; <a href="https://github.com/aces/Loris">LORIS</a> &bull; <a href="https://github.com/mapstruct/mapstruct">MapStruct</a> &bull; <a href="https://github.com/mastodon/mastodon">Mastodon</a> &bull; <a href="https://github.com/mathesar-foundation/mathesar">Mathesar</a> &bull; <a href="https://github.com/mautic/mautic">Mautic</a> &bull; <a href="https://github.com/mermaid-js/mermaid">Mermaid</a> &bull; <a href="https://github.com/messageformat">MessageFormat</a> &bull; <a href="https://github.com/mockoon/mockoon">Mockoon</a> &bull; <a href="https://github.com/nativescript">NativeScript</a> &bull; <a href="https://github.com/vercel/next.js">Next.js</a> &bull; <a href="https://github.com/nuxt">Nuxt</a> &bull; <a href="https://github.com/nvm-sh/nvm">nvm</a> &bull; <a href="https://github.com/ohmyzsh/ohmyzsh">Oh My Zsh</a> &bull; <a href="https://github.com/processing/p5.js">p5.js</a> &bull; <a href="https://github.com/pillarjs/path-to-regexp">Path-to-RegExp</a> &bull; <a href="https://github.com/pelias/pelias">Pelias Geocoder</a> &bull; <a href="https://github.com/pestphp/pest">Pest</a> &bull; <a href="https://github.com/python-pillow/Pillow">Pillow</a> &bull; <a href="https://github.com/App-vNext/Polly">Polly</a> &bull; <a href="https://github.com/ProyectoRespira">Proyecto Respira</a> &bull; <a href="https://github.com/pypeit/PypeIt/">PypeIt</a> &bull; <a href="https://github.com/fabiocaccamo/python-benedict">python-benedict</a> &bull; <a href="https://github.com/ljharb/qs">qs</a> &bull; <a href="https://github.com/kriasoft/react-starter-kit">React Starter Kit</a> &bull; <a href="https://github.com/readest/readest">Readest</a> &bull; <a href="https://github.com/browserify/resolve">Resolve</a> &bull; <a href="https://github.com/RocketChat/Rocket.Chat">Rocket.Chat</a> &bull; <a href="https://github.com/SeleniumHQ/selenium">Selenium</a> &bull; <a href="https://github.com/shadcn-ui/ui">shadcn/ui</a> &bull; <a href="https://github.com/sphinx-doc/sphinx">Sphinx</a> &bull; <a href="https://github.com/spyder-ide/spyder">Spyder</a> &bull; <a href="https://github.com/kevinburke/ssh_config">ssh_config</a> &bull; <a href="https://github.com/stackql/stackql">StackQL</a> &bull; <a href="https://github.com/Stirling-Tools/Stirling-PDF">Stirling PDF</a> &bull; <a href="https://github.com/sveltejs/svelte">Svelte</a> &bull; <a href="https://github.com/thunderbird/thunderbird-android">Thunderbird for Android</a> &bull; <a href="https://github.com/ToolJet/ToolJet">ToolJet</a> &bull; <a href="https://github.com/jonobr1/two.js">Two.js</a> &bull; <a href="https://github.com/spf13/viper">Viper</a> &bull; <a href="https://github.com/pypa/virtualenv">virtualenv</a> &bull; <a href="https://github.com/vuetifyjs/vuetify">Vuetify</a> &bull; <a href="https://github.com/wevm/wagmi">Wagmi</a> &bull; <a href="https://github.com/webdriverio/webdriverio">WebdriverIO</a> &bull; <a href="https://github.com/xyflow/xyflow">xyflow</a> &bull; <a href="https://github.com/eemeli/yaml">YAML</a> &bull; <a href="https://github.com/yiisoft">Yii Framework</a> &bull; <a href="https://github.com/yjs/yjs">Yjs</a></p>



<p class="wp-block-paragraph"><strong>Web, networking, APIs, and infrastructure services</strong></p>



<p class="wp-block-paragraph"><a href="https://github.com/actix/actix-web">actix-web</a> &bull; <a href="https://github.com/aio-libs/aiohttp">aiohttp</a> &bull; <a href="https://github.com/apache/apisix">Apache APISIX</a> &bull; <a href="https://github.com/apache/shiro">Apache Shiro</a> &bull; <a href="https://github.com/apache/solr">Apache Solr</a> &bull; <a href="https://github.com/apache/zookeeper">Apache ZooKeeper</a> &bull; <a href="https://github.com/arkime/arkime">Arkime</a> &bull; <a href="https://github.com/caddyserver/caddy">Caddy</a> &bull; <a href="https://github.com/soheilhy/cmux">cmux</a> &bull; <a href="https://github.com/cryptomator/cryptomator">Cryptomator</a> &bull; <a href="https://github.com/curl">curl</a> &bull; <a href="https://github.com/CycloneDX/cyclonedx-dotnet">CycloneDX .NET Library</a> &bull; <a href="https://github.com/CycloneDX/cdxgen">CycloneDX cdxgen</a> &bull; <a href="https://github.com/ente-io/ente">Ente</a> &bull; <a href="https://github.com/etcd-io/etcd">etcd</a> &bull; <a href="https://github.com/evcc-io/evcc">evcc</a> &bull; <a href="https://github.com/expressjs/express">Express</a> &bull; <a href="https://github.com/external-secrets/external-secrets">external-secrets</a> &bull; <a href="https://github.com/fastapi/fastapi">FastAPI</a> &bull; <a href="https://github.com/fastify/fastify">Fastify</a> &bull; <a href="https://github.com/haraka/Haraka">Haraka</a> &bull; <a href="https://github.com/helmetjs/helmet">Helmet.js</a> &bull; <a href="https://github.com/home-assistant/core">Home Assistant</a> &bull; <a href="https://github.com/hummingbird-project/hummingbird">Hummingbird</a> &bull; <a href="https://github.com/panva/jose">jose</a> &bull; <a href="https://github.com/keycloak/keycloak">Keycloak</a> &bull; <a href="https://github.com/keyshade-xyz/keyshade">Keyshade</a> &bull; <a href="https://github.com/kgateway-dev/kgateway">kgateway</a> &bull; <a href="https://github.com/apache/logging-log4j2">Log4j</a> &bull; <a href="https://github.com/gabriel-vasile/mimetype">mimetype</a> &bull; <a href="https://github.com/netbirdio/netbird">NetBird</a> &bull; <a href="https://github.com/netty/netty">Netty</a> &bull; <a href="https://github.com/ruby-oauth/oauth2">Oauth2 (Ruby)</a> &bull; <a href="https://github.com/oauthlib/oauthlib">OAuthlib</a> &bull; <a href="https://github.com/openapistack">openapi-stack</a> &bull; <a href="https://github.com/openwisp/openwisp-controller">OpenWISP</a> &bull; <a href="https://github.com/pgpainless/pgpainless">PGPainless</a> &bull; <a href="https://github.com/quic-go/quic-go">quic-go</a> &bull; <a href="https://github.com/scanapi/scanapi">ScanAPI</a> &bull; <a href="https://github.com/aboutcode-org/scancode-toolkit">ScanCode</a> &bull; <a href="https://github.com/GyulyVGC/sniffnet">Sniffnet</a> &bull; <a href="https://github.com/python-social-auth/social-app-django">Social Auth App Django</a> &bull; <a href="https://github.com/stalwartlabs/stalwart">Stalwart</a> &bull; <a href="https://github.com/Kludex/starlette">Starlette</a> &bull; <a href="https://github.com/faisalman/ua-parser-js">UAParser.js</a> &bull; <a href="https://github.com/urllib3/urllib3">urllib3</a> &bull; <a href="https://github.com/vapor/vapor">Vapor</a> &bull; <a href="https://github.com/dmno-dev/varlock">varlock</a> &bull; <a href="https://github.com/decentralized-identity/veramo">Veramo</a> &bull; <a href="https://github.com/go-webauthn/webauthn">WebAuthn (Go)</a> &bull; <a href="https://github.com/zitadel/zitadel">ZITADEL</a></p>



<p class="wp-block-paragraph"></p>
</body></html>
<p>The post <a href="https://github.blog/open-source/maintainers/maintainers-dont-need-more-ai-bug-reports-they-need-fixes/">Maintainers don&#8217;t need more AI bug reports. They need fixes. </a> appeared first on <a href="https://github.blog">The GitHub Blog</a>.</p>

---
*Artikel ini otomatis diambil dari [GitHub Blog](https://github.blog/open-source/maintainers/maintainers-dont-need-more-ai-bug-reports-they-need-fixes/). Baca artikel selengkapnya di sumber asli.*
