---
title: "Publish Visual Studio Marketplace extensions securely with Trusted Publishing"
description: "Publish Visual Studio Code and Visual Studio extensions from GitHub Actions or Azure Pipelines without storing a long-lived personal access token. The post Publish Visual Studio Marketplace extensions..."
pubDate: "2026-10-28T16:59:00.000Z"
authors: ["e-learning"]
organisations: ["microsoft"]
category: "web-development"
tags: ["microsoft", "web-development"]
thumbnail: "https://images.unsplash.com/photo-1573804633927-bfcbcd909acd?auto=format&fit=crop&w=800&h=450&q=80"
heroImage: "https://images.unsplash.com/photo-1573804633927-bfcbcd909acd?auto=format&fit=crop&w=800&h=450&q=80"
keywords: ["microsoft", "tech news", "developer"]
canonicalUrl: "https://devblogs.microsoft.com/blog/trusted-publishing-visual-studio-marketplace/"
---

<p>We are excited to announce <strong>Trusted Publishing for Visual Studio Marketplace</strong>
—a simpler and safer way to publish Visual Studio Code and
Visual Studio extensions from GitHub Actions or Azure Pipelines without storing
a long-lived Azure DevOps personal access token (PAT).</p>
<p>Trusted Publishing uses federated identity to verify your release workflow and
grant access for the publish—no publishing secret to store or rotate.</p>
<h2>Why Trusted Publishing?</h2>
<p>Automated Marketplace publishing has traditionally required storing a PAT as a
CI/CD secret. Trusted Publishing offers a safer alternative:</p>
<ul>
<li><strong>No long-lived secrets</strong>—there is no publishing credential to store or rotate
in your CI/CD system.</li>
<li><strong>Short-lived access</strong>—for GitHub Actions, Marketplace issues a temporary
credential that expires after 15 minutes.</li>
<li><strong>Federated identity</strong>—Marketplace trusts a configured GitHub workflow or
Azure Pipelines workload identity.</li>
</ul>
<p>Existing PAT-based publishing continues to work, so you can adopt Trusted
Publishing when you are ready.</p>
<h2>How it works</h2>
<ol>
<li>The CI/CD system presents its federated identity to Visual Studio Marketplace.</li>
<li>Marketplace verifies the GitHub Trusted Publishing policy or the Azure
Pipelines workload identity.</li>
<li>The publishing tool uses the verified identity to publish the extension
without a PAT.</li>
</ol>
<p>The process is automatic when you publish with OIDC—there is no PAT to retrieve
or manage.</p>
<h2>Supported CI/CD providers</h2>
<table>
<thead>
<tr>
<th>Provider</th>
<th>Status</th>
<th>Authentication and setup</th>
</tr>
</thead>
<tbody>
<tr>
<td><strong>GitHub Actions</strong></td>
<td>Supported</td>
<td>Create a policy on the publisher&#8217;s <strong>Trusted Publishing</strong> page and publish with GitHub OIDC.</td>
</tr>
<tr>
<td><strong>Azure Pipelines</strong></td>
<td>Supported</td>
<td>Configure a Microsoft Entra-backed WIF service connection and add its identity as a publisher contributor. No Marketplace policy is required.</td>
</tr>
</tbody>
</table>
<p>For Azure Pipelines, see
<a href="https://learn.microsoft.com/azure/devops/pipelines/library/add-devops-entra-service-connection?view=azure-devops#step-2-create-the-service-connection">configure a Microsoft Entra-backed WIF service connection</a>
and
<a href="https://learn.microsoft.com/visualstudio/extensibility/walkthrough-publishing-a-visual-studio-extension?view=visualstudio#add-additional-users-to-manage-your-publisher-account">add its identity as a publisher contributor</a>.</p>
<h2>Get started with GitHub Actions</h2>
<p>Create a Marketplace policy that identifies the GitHub repository and workflow
allowed to publish.</p>
<p><img decoding="async" src="https://devblogs.microsoft.com/wp-content/uploads/2026/10/trusted_publish_managepage-1.webp" alt="The Trusted Publishing tab for configuring GitHub Actions policies in Visual Studio Marketplace" /></p>
<h3>1. Create a Trusted Publishing policy</h3>
<ol>
<li>Sign in to <a href="https://marketplace.visualstudio.com/manage">Visual Studio Marketplace</a>.</li>
<li>Open the publisher that owns your extension.</li>
<li>Select <strong>Trusted Publishing</strong>.</li>
<li>Expand <strong>Create</strong>, then fill in the fields described below.
<table>
<thead>
<tr>
<th>Field</th>
<th>Value</th>
</tr>
</thead>
<tbody>
<tr>
<td><strong>Policy name</strong></td>
<td>A descriptive name such as <code>Release workflow</code>.</td>
</tr>
<tr>
<td><strong>Repository owner</strong></td>
<td>The exact GitHub organization or user login that owns the repository, such as <code>contoso</code>.</td>
</tr>
<tr>
<td><strong>Repository</strong></td>
<td>The repository name without the owner, such as <code>sample-extension</code>.</td>
</tr>
<tr>
<td><strong>Workflow</strong></td>
<td>The workflow filename under <code>.github/workflows</code>, such as <code>publish.yml</code>.</td>
</tr>
<tr>
<td><strong>Environment (optional)</strong></td>
<td>If the workflow uses a GitHub environment, enter its exact name, such as <code>production</code>.</td>
</tr>
</tbody>
</table>
</li>
<li>Select <strong>Create</strong>.<img decoding="async" src="https://devblogs.microsoft.com/wp-content/uploads/2026/10/trusted_publishing_create_policy.webp" alt="Creating a Trusted Publishing policy with fictional test account data" /></li>
</ol>
<p>For a private repository, run the configured workflow within 48 hours of creating
the policy. The first successful token exchange permanently activates the policy.</p>
<h3>2. Publish a Visual Studio Code extension</h3>
<p>To configure your GitHub Actions workflow and publish without a PAT, follow the
<a href="https://code.visualstudio.com/api/working-with-extensions/publishing-extension?from=20423#secure-automated-publishing-to-visual-studio-marketplace">Secure automated publishing to Visual Studio Marketplace</a>
guide in the Visual Studio Code documentation.</p>
<h3>3. Publish a Visual Studio extension</h3>
<p>To publish your Visual Studio extension from the command line, follow the
<a href="https://learn.microsoft.com/visualstudio/extensibility/walkthrough-publishing-a-visual-studio-extension-via-command-line?view=visualstudio">Publish a Visual Studio extension via command line</a>
guide in the Visual Studio documentation.</p>
<h2>Publishing from Azure Pipelines</h2>
<p>Azure Pipelines uses an Azure DevOps service connection with Workload Identity
Federation (WIF) instead of a GitHub Trusted Publishing policy.</p>
<blockquote><p>Do not create a policy on the Marketplace
Trusted Publishing page for Azure Pipelines.</p></blockquote>
<p>Before publishing:</p>
<ol>
<li><a href="https://learn.microsoft.com/azure/devops/pipelines/library/add-devops-entra-service-connection?view=azure-devops#step-2-create-the-service-connection">Configure an Azure Pipelines service connection</a>
that uses <strong>Workload Identity Federation</strong>. Back the connection with either a
Microsoft Entra app registration and service principal or a managed identity.</li>
<li><a href="https://learn.microsoft.com/visualstudio/extensibility/walkthrough-publishing-a-visual-studio-extension?view=visualstudio#add-additional-users-to-manage-your-publisher-account">Add that identity to your Visual Studio Marketplace publisher</a>
and grant it the <strong>Contributor</strong> role.</li>
<li>Configure the publishing step to use the WIF service connection.</li>
</ol>
<p>The pipeline can then publish Visual Studio Code or Visual Studio extensions
without storing a Marketplace PAT.</p>
<h2>GitHub policy lifecycle</h2>
<p>GitHub policy activation differs based on repository visibility. For a private
repository, a new policy is temporarily active for 48 hours. The first successful
token exchange makes the policy permanently active. If the policy is not used
during that period, it expires and can be reopened for another 48 hours.</p>
<p>You can view the status and last use of each policy from the
<strong>Trusted Publishing</strong> page.</p>
<h2>Migrating from a PAT</h2>
<p>If your GitHub Actions workflow already publishes with <code>vsce</code>, migration is
straightforward:</p>
<ol>
<li>Create a Trusted Publishing policy for the repository and workflow.</li>
<li>Add <code>id-token: write</code> to the publishing job.</li>
<li>Update the publishing command to use OIDC.</li>
<li>Run the workflow and verify that the OIDC publish succeeds.</li>
<li>After the OIDC publish succeeds, remove the Marketplace PAT from the workflow
and repository secrets.</li>
</ol>
<p>We recommend keeping the PAT until you have verified the first Trusted Publishing
run. After the migration succeeds, remove the unused secret so it cannot be used
accidentally by another workflow step.</p>
<p>For Azure Pipelines,
<a href="https://learn.microsoft.com/azure/devops/pipelines/library/add-devops-entra-service-connection?view=azure-devops#step-2-create-the-service-connection">configure the WIF service connection</a>
and
<a href="https://learn.microsoft.com/visualstudio/extensibility/walkthrough-publishing-a-visual-studio-extension?view=visualstudio#add-additional-users-to-manage-your-publisher-account">publisher identity</a>
first, update the publishing step to use that connection, and remove the PAT
only after the federated publish succeeds. No Marketplace Trusted Publishing
policy is needed.</p>
<h2>Try Trusted Publishing Today</h2>
<p>Trusted Publishing makes automated extension releases easier to maintain and
safer by default—without requiring a long-lived publishing secret.</p>
<p>To get started:</p>
<ol>
<li>Open your publisher in
<a href="https://marketplace.visualstudio.com/manage">Visual Studio Marketplace publisher management</a>.</li>
<li>Create a policy under <strong>Trusted Publishing</strong>.</li>
<li>Update your GitHub Actions workflow to use OIDC.</li>
<li>Publish your extension without a PAT.</li>
</ol>
<p>Using Azure Pipelines instead? Follow the Azure Pipelines setup guide to
publish with Workload Identity Federation (WIF), without a PAT.</p>
<p>For complete setup instructions and troubleshooting guidance, visit
<a href="https://aka.ms/vsm/trustedpublishing">the Trusted Publishing documentation</a>.</p>
<h2>Thank you</h2>
<p>Thank you to our extension publishers for investing in more secure releases.
As you adopt Trusted Publishing, we would love to hear what works well and
where we can improve—from configuring your first policy to publishing without
a PAT.</p>
<p><a href="https://github.com/microsoft/vsmarketplace/discussions/categories/feedback">Share your feedback</a>
in the Visual Studio Marketplace GitHub discussions.</p>
<p>The post <a href="https://devblogs.microsoft.com/blog/trusted-publishing-visual-studio-marketplace/">Publish Visual Studio Marketplace extensions securely with Trusted Publishing</a> appeared first on <a href="https://devblogs.microsoft.com">Microsoft for Developers</a>.</p>

---
*Artikel ini otomatis diambil dari [Microsoft Developer](https://devblogs.microsoft.com/blog/trusted-publishing-visual-studio-marketplace/). Baca artikel selengkapnya di sumber asli.*
