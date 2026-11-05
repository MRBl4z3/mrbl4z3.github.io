---
title: "Accessibility statements now have a home on GitHub"
description: "GitHub now highlights ACCESSIBILITY.md files and helps maintainers document accessibility goals, known barriers, supported environments, contributor expectations, and reporting processes. The post Acc..."
pubDate: "2026-11-05T16:00:00.000Z"
authors: ["e-learning"]
organisations: ["github"]
category: "web-development"
tags: ["github", "web-development"]
thumbnail: "https://images.unsplash.com/photo-1573804633927-bfcbcd909acd?auto=format&fit=crop&w=800&h=450&q=80"
heroImage: "https://images.unsplash.com/photo-1573804633927-bfcbcd909acd?auto=format&fit=crop&w=800&h=450&q=80"
keywords: ["github", "tech news", "developer"]
canonicalUrl: "https://github.blog/open-source/maintainers/accessibility-statements-now-have-a-home-on-github/"
---

<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN" "http://www.w3.org/TR/REC-html40/loose.dtd">
<html><body><p class="wp-block-paragraph">Accessibility work often lives in issue trackers, contribution guidelines, audit reports, team knowledge, and conversations with users. When that information has no predictable home, people who encounter a barrier must search for it, and maintainers lose context as contributors and project ownership change.</p>



<p class="wp-block-paragraph">An <code>ACCESSIBILITY.md</code> file, now visible on repository overview pages, gives a project one durable place to explain its accessibility goals, contributor expectations, supported environments, known limitations, and process for reporting barriers.</p>



<h2 id="h-github-makes-accessibility-statements-easier-to-find" class="wp-block-heading">GitHub makes accessibility statements easier to find</h2>



<figure class="wp-block-image size-large"><img data-recalc-dims="1" fetchpriority="high" decoding="async" height="680" width="1024" src="https://github.blog/wp-content/uploads/2026/11/Screenshot-2026-11-02-at-3.20.14-PM.png?resize=1024%2C680" alt="GitHub&rsquo;s Community Standards page under Insights, showing an Accessibility card under &ldquo;Additional community file&rdquo; with an Add button for creating a statement." class="wp-image-99951" srcset="https://github.blog/wp-content/uploads/2026/11/Screenshot-2026-11-02-at-3.20.14-PM.png?w=1264 1264w, https://github.blog/wp-content/uploads/2026/11/Screenshot-2026-11-02-at-3.20.14-PM.png?w=300 300w, https://github.blog/wp-content/uploads/2026/11/Screenshot-2026-11-02-at-3.20.14-PM.png?w=768 768w, https://github.blog/wp-content/uploads/2026/11/Screenshot-2026-11-02-at-3.20.14-PM.png?w=1024 1024w" sizes="(max-width: 1000px) 100vw, 1000px" /></figure>



<p class="wp-block-paragraph">GitHub now recognizes the <code>ACCESSIBILITY.md</code> file in the repository&rsquo;s root, <code>.github/</code> directory, or <code>docs/</code> directory. A repository can also inherit a statement from its organization&rsquo;s <code>.github/</code> repository. For public repositories, maintainers can add or propose a statement from <strong>Insights</strong> &gt; <strong>Community Standards</strong> using a concise starter document.</p>



<p class="wp-block-paragraph">GitHub&rsquo;s community health files help people understand how a project works:</p>



<ul class="wp-block-list">
<li>Contribution guidelines explain how to propose changes</li>



<li>Codes of conduct establish expectations for participation</li>



<li>Security policies explain how to report vulnerabilities</li>
</ul>



<p class="wp-block-paragraph">Recognizing <code>ACCESSIBILITY.md</code> gives accessibility information a dedicated place alongside that guidance.</p>



<p class="wp-block-paragraph">New repositories also include an <code>accessibility</code> issue label by default. The statement explains how to report a barrier and what to expect next, while the label helps maintainers organize and track the resulting work.</p>



<h2 class="wp-block-heading" id="accessibility-statements-make-project-practices-visible">Accessibility statements make project practices visible</h2>



<p class="wp-block-paragraph"><a href="https://github.com/cli/cli/blob/trunk/.github/ACCESSIBILITY.md">GitHub CLI&rsquo;s accessibility statement</a>, for example, documents settings, known barriers, and workarounds. It explains why table output can be difficult to follow with a screen reader and provides a command that prints pull request numbers and titles one per line instead. It also asks contributors changing prompts or output to show the results with relevant accessibility settings enabled. Developers using the CLI get a specific workaround; developers contributing to it get clear expectations for their changes.</p>



<p class="wp-block-paragraph">The GitHub Annotation Toolkit&rsquo;s <a href="https://github.com/github/annotation-toolkit/blob/main/ACCESSIBILITY.md">accessibility statement</a> documents a limitation of its Figma library: annotations use canvas position, connecting lines, and matching numbers to visually convey relationships. Access to individual canvas objects does not necessarily communicate what those relationships are.</p>



<p class="wp-block-paragraph">The statement gives collaborators a concrete alternative: provide a structured text description that identifies each annotated element, its intended semantics and behavior, and any relevant ordering. For developers preparing or receiving a design handoff, this explains what information needs to accompany the visual annotations.</p>



<p class="wp-block-paragraph">The file also explains how to report accessibility barriers and explicitly states that the library has not undergone keyboard-only or screen-reader testing. Readers get both practical guidance and clear limits on what the project has verified. Showing this information in the repository overview makes it easier to find alongside the project&rsquo;s other documentation.</p>



<figure class="wp-block-image size-large"><img data-recalc-dims="1" decoding="async" height="538" width="1024" src="https://github.blog/wp-content/uploads/2026/11/Screenshot-2026-11-02-at-3.20.25-PM.png?resize=1024%2C538" alt="GitHub&rsquo;s file editor displaying the ACCESSIBILITY.md starter template, with instructions for documenting accessibility priorities, contributor expectations, and how to report barriers. " class="wp-image-99952" srcset="https://github.blog/wp-content/uploads/2026/11/Screenshot-2026-11-02-at-3.20.25-PM.png?w=1244 1244w, https://github.blog/wp-content/uploads/2026/11/Screenshot-2026-11-02-at-3.20.25-PM.png?w=300 300w, https://github.blog/wp-content/uploads/2026/11/Screenshot-2026-11-02-at-3.20.25-PM.png?w=768 768w, https://github.blog/wp-content/uploads/2026/11/Screenshot-2026-11-02-at-3.20.25-PM.png?w=1024 1024w" sizes="(max-width: 1000px) 100vw, 1000px" /></figure>



<h2 class="wp-block-heading" id="document-what-the-project-can-maintain">Document what the project can maintain</h2>



<p class="wp-block-paragraph">There is no one fixed format for an <code>ACCESSIBILITY.md</code> file. A small library, command-line tool, documentation site, and large web application have different users, risks, and maintenance capacity. Start with accurate information that the project can keep current.</p>



<ul class="wp-block-list">
<li><strong>Priorities and goals:</strong> Explain why accessibility matters to the project and which user experiences receive attention.</li>



<li><strong>Contributor expectations:</strong> Describe the accessibility checks, evidence, or review practices expected for changes, including instructions for human contributors and coding agents.</li>



<li><strong>Reporting accessibility barriers:</strong> Provide a direct, accessible reporting path. Do not require disability disclosure, screenshots, or recordings.</li>



<li><strong>Response process:</strong> Explain how maintainers acknowledge, triage, track, and communicate about accessibility issues. If the project uses severity levels, let maintainers assign or confirm them during triage.</li>



<li><strong>Supported environments:</strong> List the platforms, browsers, input methods, and assistive technologies the project supports or has tested.</li>



<li><strong>Known limitations:</strong> Describe barriers in terms of the affected experience, include available workarounds, and link to tracked remediation work.</li>



<li><strong>Ownership and maintenance:</strong> Name the team, role, or maintainer responsible for keeping the statement and related practices current.</li>



<li><strong>Feedback and improvements:</strong> Explain how users and contributors can suggest changes to the statement or the project&rsquo;s accessibility practices.</li>
</ul>



<p class="wp-block-paragraph">Keep in mind that a project can name a Web Content Accessibility Guidelines version and level as a target. A target does not establish verified conformance. A conformance claim requires an evaluation and should identify the evaluated scope, date, method, and evaluator.</p>



<h2 class="wp-block-heading" id="what-the-most-useful-accessibility-md-files-contain">What the most useful ACCESSIBILITY.md files contain</h2>



<p class="wp-block-paragraph">The most useful accessibility statements serve people who arrive after something has already gone wrong. Plain language, descriptive links, and a clear reporting route matter more than legal or standards-heavy language.</p>



<p class="wp-block-paragraph">Useful reporting guidance invites relevant context, such as the affected task, page, observed behavior, browser, operating system, and assistive technology. It makes screenshots and recordings optional and avoids asking someone to prove a disability.</p>



<p class="wp-block-paragraph">The statement should also set expectations that maintainers can consistently meet. That can include acknowledgement, status updates, available workarounds, expected timelines, and an opportunity for the reporter to verify a fix.</p>



<h2 class="wp-block-heading" id="start-with-an-example-and-adapt-it">Start with an example and adapt it</h2>



<p class="wp-block-paragraph">The <a href="https://github.com/open-source-accessibility/accessibility-toolkit/blob/main/ACCESSIBILITY.md">Open Source Accessibility Toolkit</a> provides a detailed public example.</p>



<p class="wp-block-paragraph">GitHub&rsquo;s Community Standards workflow provides a shorter starter with prompts that projects can replace with their own information. The goal is not to produce a perfect policy before publishing the file. Start with accurate information, name the gaps, and update the statement as the project&rsquo;s accessibility practices mature.</p>



<p class="wp-block-paragraph">Adding an accessibility statement makes accessibility work more discoverable to users, contributors, and maintainers. Normalizing that practice across open source projects helps make accessibility part of how communities plan, build, report, and improve their work.</p>



<div class="wp-block-group post-content-cta has-global-padding is-layout-constrained wp-block-group-is-layout-constrained">
<p class="wp-block-paragraph"><a href="https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/adding-an-accessibility-page-to-your-repository">Add an accessibility statement to your repository &gt;</a></p>
</div>
</body></html>
<p>The post <a href="https://github.blog/open-source/maintainers/accessibility-statements-now-have-a-home-on-github/">Accessibility statements now have a home on GitHub</a> appeared first on <a href="https://github.blog">The GitHub Blog</a>.</p>

---
*Artikel ini otomatis diambil dari [GitHub Blog](https://github.blog/open-source/maintainers/accessibility-statements-now-have-a-home-on-github/). Baca artikel selengkapnya di sumber asli.*
