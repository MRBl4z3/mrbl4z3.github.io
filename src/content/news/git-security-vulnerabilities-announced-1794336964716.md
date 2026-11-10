---
title: "Git security vulnerabilities announced"
description: "Git is releasing updates addressing six security vulnerabilities affecting recursive clones, reference handling, merges, binary patches, and imports. Here is what users need to know. The post Git secu..."
pubDate: "2026-11-10T18:00:00.000Z"
authors: ["e-learning"]
organisations: ["github"]
category: "web-development"
tags: ["github", "web-development"]
thumbnail: "https://images.unsplash.com/photo-1573804633927-bfcbcd909acd?auto=format&fit=crop&w=800&h=450&q=80"
heroImage: "https://images.unsplash.com/photo-1573804633927-bfcbcd909acd?auto=format&fit=crop&w=800&h=450&q=80"
keywords: ["github", "tech news", "developer"]
canonicalUrl: "https://github.blog/open-source/git/git-security-vulnerabilities-announced-7/"
---

<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN" "http://www.w3.org/TR/REC-html40/loose.dtd">
<html><body><p class="wp-block-paragraph">Today, the Git project is releasing new versions to address six security vulnerabilities. These include arbitrary code execution when recursively cloning a malicious repository, writes to unintended files, and memory corruption when processing merges or binary patches.</p>



<p class="wp-block-paragraph">The fixes are included in <a href="https://github.com/git/git/blob/v2.56.1/Documentation/RelNotes/2.56.1.adoc">Git 2.56.1</a>, with backports to Git 2.55.1, 2.54.1, 2.53.1, 2.52.1, 2.51.3, and 2.50.2. We recommend updating to Git 2.56.1 or the patched release for your maintenance series, and updating any applications that bundle Git.</p>



<h2 id="h-vulnerabilities-in-git" class="wp-block-heading">Vulnerabilities in Git</h2>



<h3 id="h-cve-2026-111760-code-execution-during-recursive-clones" class="wp-block-heading">CVE-2026-111760: Code execution during recursive clones</h3>



<p class="wp-block-paragraph">Git checks paths before writing files into a working tree, including rejecting paths that would overwrite its own repository metadata. However, an integer truncation in Git&rsquo;s index handling can cause an extremely long path to pass validation under one name and then be written using a shorter name.Collapse comment</p>



<p class="wp-block-paragraph">An attacker can use this discrepancy to replace a submodule&rsquo;s <code>.git</code> file with a symbolic link pointing to a directory containing malicious configuration or <a href="https://git-scm.com/docs/githooks">hooks</a>. When Git subsequently uses that directory, it can execute attacker-controlled code during a recursive clone.</p>



<p class="wp-block-paragraph">The fix rejects extremely long index entry paths before their lengths can be truncated.</p>



<p class="wp-block-paragraph">[<a href="https://github.com/git/git/security/advisories/GHSA-vp2w-7xxj-jvxr">Advisory</a>]</p>



<h3 class="wp-block-heading" id="cve-2026-113408-path-traversal-through-a-servers-advertised-head">CVE-2026-113408: Path traversal through a server&rsquo;s advertised HEAD</h3>



<p class="wp-block-paragraph">When cloning an empty repository using Git protocol v2, the server can advertise a <code>HEAD</code> symref pointing to an unborn branch (one that has no commits yet). Git did not validate the symref&rsquo;s target before storing it locally.</p>



<p class="wp-block-paragraph">A malicious server can supply a name containing path-traversal components. With the files reference backend, a later operation that updates the branch can write outside the intended reference directory.</p>



<p class="wp-block-paragraph">Git now validates the advertised target before accepting it. The release also validates other reference names and symbolic-reference targets received through protocol v2.</p>



<p class="wp-block-paragraph">[<a href="https://gitlab.com/gitlab-org/git/-/work_items/809">Advisory</a>]</p>



<h3 class="wp-block-heading" id="cve-2026-114467-stack-corruption-when-removing-signatures">CVE-2026-114467: Stack corruption when removing signatures</h3>



<p class="wp-block-paragraph">When stripping GPG signatures from a message, Git assumed there would be at most two signatures. A specially crafted message containing more than two signatures can cause Git to write past the end of a fixed-size stack array.</p>



<p class="wp-block-paragraph">This can be triggered by merging a tag containing more than two signatures, causing memory corruption and a potential denial of service. Hosting services that perform these merges on behalf of users can also be affected.</p>



<p class="wp-block-paragraph">The fix removes the fixed-size array and handles messages with any number of signatures.</p>



<p class="wp-block-paragraph">[<a href="https://gitlab.com/gitlab-org/git/-/work_items/810">Advisory</a>]</p>



<h3 class="wp-block-heading" id="cve-2026-114469-out-of-bounds-reads-when-applying-binary-patches">CVE-2026-114469: Out-of-bounds reads when applying binary patches</h3>



<p class="wp-block-paragraph">Git rejects patches larger than approximately 1 GiB because its patch application code cannot safely handle larger inputs. However, this check only considered the size of the patch as supplied.</p>



<p class="wp-block-paragraph">Binary patches are compressed and encoded, so a relatively small patch can expand beyond that limit. A crafted patch can bypass the input-size check and cause an out-of-bounds read, crashing Git.</p>



<p class="wp-block-paragraph">Git now also checks the declared uncompressed size before processing a binary patch.</p>



<p class="wp-block-paragraph">[<a href="https://gitlab.com/gitlab-org/git/-/work_items/811">Advisory</a>]</p>



<h3 class="wp-block-heading" id="cve-2026-105437-heap-corruption-during-large-merges">CVE-2026-105437: Heap corruption during large merges</h3>



<p class="wp-block-paragraph">Git&rsquo;s merge machinery calculates how much memory it needs for a merge result before writing the output. That calculation can overflow when the result is very large, including when many conflicts are combined with a large <code>conflict-marker-size</code> attribute.</p>



<p class="wp-block-paragraph">The overflow can cause Git to allocate a buffer that is too small, then write outside it, corrupting heap memory. This affects applications and hosting services that use Git to merge untrusted content, as well as local merges.</p>



<p class="wp-block-paragraph">The fix uses size-appropriate types and checked arithmetic, rejecting merges whose output size cannot be represented safely.</p>



<p class="wp-block-paragraph">[<a href="https://gitlab.com/gitlab-org/git/-/work_items/814">Advisory</a>]</p>



<h3 class="wp-block-heading" id="cve-2026-114466-filesystem-access-through-fast-import-streams">CVE-2026-114466: Filesystem access through fast-import streams</h3>



<p class="wp-block-paragraph"><a href="https://git-scm.com/docs/git-fast-import"><code>git fast-import</code></a> reads a stream of commands to build a repository. Some commands can access files outside the repository, so Git already restricts them when they appear in an untrusted input stream.</p>



<p class="wp-block-paragraph">Three additional features lacked that protection. The <code>rewrite-submodules-from</code> and <code>rewrite-submodules-to</code> features can read arbitrary files, potentially exposing their contents through error messages. The <code>export-pack-edges</code> option can create files or append data to existing files.</p>



<p class="wp-block-paragraph">These features are now rejected when supplied through the input stream unless the caller explicitly enables <code>--allow-unsafe-features</code>. Only enable that option when you trust the program generating the stream.</p>



<p class="wp-block-paragraph">[<a href="https://gitlab.com/gitlab-org/git/-/work_items/812">Advisory</a>]</p>



<h2 class="wp-block-heading" id="additional-hardening">Additional hardening</h2>



<p class="wp-block-paragraph">Alongside these fixes, the release adds overflow checks to related index entry allocations during checkout and tree processing. These checks complement the path-length limits above, preventing allocation sizes from being truncated or overflowing before Git copies a path into memory.</p>



<h2 class="wp-block-heading" id="protecting-github-users">Protecting GitHub users</h2>



<ul class="wp-block-list">
<li>We have deployed fixes for these vulnerabilities across github.com, GitHub Enterprise Cloud, and GitHub Enterprise Cloud with data residency.</li>



<li>Updates for GitHub Enterprise Server are available in versions 3.19.15, 3.20.11, 3.21.9, and 3.22.4. We recommend that administrators upgrade as soon as possible.</li>



<li>We are coordinating releases of GitHub Desktop and the GitHub Copilot app that include patched versions of Git.</li>



<li>GitHub Codespaces and GitHub Actions will also receive patched versions of Git.</li>
</ul>



<h2 class="wp-block-heading" id="update-git">Update Git</h2>



<p class="wp-block-paragraph">Updating is the most effective way to protect against these vulnerabilities. Install a patched version from your operating system or Git distributor, and update applications that include their own copy of Git. Administrators should also update Git installations used by automation and services that process repositories on behalf of users.</p>



<p class="wp-block-paragraph">If you cannot update immediately, clone only from trusted servers and avoid recursive clones of untrusted repositories. Avoid merging untrusted content or tags, or processing binary patches or fast-import streams from untrusted sources. These precautions do not replace installing the fixes.</p>



<div class="wp-block-group post-content-cta has-global-padding is-layout-constrained wp-block-group-is-layout-constrained">
<p class="wp-block-paragraph"><a href="https://git-scm.com/downloads">Get an updated version of Git &gt;</a></p>
</div>
</body></html>
<p>The post <a href="https://github.blog/open-source/git/git-security-vulnerabilities-announced-7/">Git security vulnerabilities announced</a> appeared first on <a href="https://github.blog">The GitHub Blog</a>.</p>

---
*Artikel ini otomatis diambil dari [GitHub Blog](https://github.blog/open-source/git/git-security-vulnerabilities-announced-7/). Baca artikel selengkapnya di sumber asli.*
