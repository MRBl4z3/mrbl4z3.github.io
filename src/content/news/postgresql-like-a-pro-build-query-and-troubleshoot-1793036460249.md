---
title: "PostgreSQL Like A Pro: Build, query, and troubleshoot without leaving VS Code"
description: "Build, query, and troubleshoot without leaving VS Code. The post PostgreSQL Like A Pro: Build, query, and troubleshoot without leaving VS Code appeared first on Microsoft for Developers...."
pubDate: "2026-10-26T15:05:12.000Z"
authors: ["e-learning"]
organisations: ["microsoft"]
category: "web-development"
tags: ["microsoft", "web-development"]
thumbnail: "https://images.unsplash.com/photo-1573804633927-bfcbcd909acd?auto=format&fit=crop&w=800&h=450&q=80"
heroImage: "https://images.unsplash.com/photo-1573804633927-bfcbcd909acd?auto=format&fit=crop&w=800&h=450&q=80"
keywords: ["microsoft", "tech news", "developer"]
canonicalUrl: "https://devblogs.microsoft.com/blog/postgresql-like-a-pro-vscode/"
---

<p><a id="post-22566-OLE_LINK2"></a> You&#8217;re building an app in VS Code. Then you need to check a table, understand how two tables connect, or figure out why a query is slow.</p>
<p>That often means opening another tool and finding your way back to the code.</p>
<p>The <strong>PostgreSQL extension for VS Code</strong> brings common database tasks into your editor. You can explore database objects, write and run SQL, and use database context with GitHub Copilot as you work.</p>
<p>Here are four ways to make that part of your development workflow, from finding your first table to reviewing a performance recommendation. Each section includes a video from <a href="https://aka.ms/postgresql-like-a-pro-vscode"><strong>PostgreSQL Like a Pro: VS Code Edition</strong></a> if you&#8217;d like to follow along.</p>
<h2>1. Get connected and find your way around <img src="https://s.w.org/images/core/emoji/17.0.2/72x72/1f50c.png" alt="🔌" class="wp-smiley" style="height: 1em; max-height: 1em;" /></h2>
<p>Before writing a query, you need to know what&#8217;s in the database.</p>
<p>Install Microsoft&#8217;s PostgreSQL extension, then select the PostgreSQL icon in the Activity Bar to add a connection.</p>
<p>You can connect to on-premises or cloud databases and save connection profiles to reuse later. For a database on Azure, the Azure connection path lets you sign in with Microsoft Entra ID and select your subscription, server, and database.</p>
<p>Once connected, expand the server, database, and schemas to browse tables, views, functions, sequences, and extensions. Start with one table your application uses. You don&#8217;t need to understand the entire database to take the next step.</p>
<blockquote><p><img src="https://s.w.org/images/core/emoji/17.0.2/72x72/1f4a1.png" alt="💡" class="wp-smiley" style="height: 1em; max-height: 1em;" /> <strong>Try this:</strong> Save a connection profile, then find one table your application uses and explore its structure.</p></blockquote>
<p><iframe src="//www.youtube.com/embed/BnuMxMZUKLg" width="280" height="157" allowfullscreen="allowfullscreen"></iframe></p>
<h2>2. Write SQL you can explain <img src="https://s.w.org/images/core/emoji/17.0.2/72x72/270d.png" alt="✍" class="wp-smiley" style="height: 1em; max-height: 1em;" /></h2>
<p>You know what the application needs. Now you need a query that returns the right data.</p>
<p>Suppose you need to find <strong>which customers generated the most revenue in a quarter</strong>. That means joining customer and payment data, adding a date filter, and grouping and sorting the results.</p>
<p>The query editor&#8217;s IntelliSense helps with SQL syntax, table and column names, and join expressions. Once you have a starting query, you can ask Copilot to help refine a filter or analyze its performance using query plans and execution metrics.</p>
<p>For a query like this, aggregating payments before joining is one possible rewrite to investigate, not a rule to apply everywhere.</p>
<p><strong>Check the results as well as the execution plan.</strong> A faster query still needs to answer the right question. Review generated SQL before running it, and compare the original and revised results.</p>
<blockquote><p><img src="https://s.w.org/images/core/emoji/17.0.2/72x72/1f4a1.png" alt="💡" class="wp-smiley" style="height: 1em; max-height: 1em;" /> <strong>Try asking:</strong> &#8220;Analyze this query&#8217;s performance. What does the execution plan tell us, and what should I check before changing it?&#8221;</p></blockquote>
<p><iframe title="YouTube video player" src="https://www.youtube.com/embed/lLp58sDAixQ" width="800" height="450" frameborder="0" allowfullscreen="allowfullscreen"></iframe></p>
<h2>3. Understand a database you didn&#8217;t build <img src="https://s.w.org/images/core/emoji/17.0.2/72x72/1f50e.png" alt="🔎" class="wp-smiley" style="height: 1em; max-height: 1em;" /></h2>
<p>Which table contains the data you need? How does it connect to the rest of the application?</p>
<p>Those are often the first questions when you inherit a project.</p>
<p>Start broad in <strong>Object Explorer</strong>. Look for schema names, naming patterns, and tables or views that match the feature you&#8217;re working on. Then use schema visualization to trace how the relevant objects connect.</p>
<p>With the connected database context available in Copilot Chat, ask a focused question. For a film rental application, that might be:</p>
<ul>
<li>Which tables contain actor and film data?</li>
<li>How are they connected?</li>
<li>Where is rental status stored?</li>
</ul>
<p>Treat the answer as a starting point, not a verified map. Check the objects in the tree, review the relationships in the visualizer, and run a small read-only query to confirm that you&#8217;re following the right data path.</p>
<blockquote><p><img src="https://s.w.org/images/core/emoji/17.0.2/72x72/1f4a1.png" alt="💡" class="wp-smiley" style="height: 1em; max-height: 1em;" /> <strong>Try this:</strong> Ask one discovery question, then verify the answer against the database before asking the next.</p></blockquote>
<p><iframe title="YouTube video player" src="https://www.youtube.com/embed/19ghd8izkug" width="800" height="450" frameborder="0" allowfullscreen="allowfullscreen"></iframe></p>
<h2>4. Turn a performance issue into a decision you can review <img src="https://s.w.org/images/core/emoji/17.0.2/72x72/1f4ca.png" alt="📊" class="wp-smiley" style="height: 1em; max-height: 1em;" /></h2>
<p>Connection drops and rising latency tell you something is wrong. Figuring out why usually means gathering logs, comparing metrics, and building a timeline.</p>
<p>Before changing anything, look for a useful chain of evidence: <strong>what changed, when it changed, which workload is involved, and why a particular action might help.</strong></p>
<p>For example, repeated customer-history lookups on a large transactions table might lead to a composite-index recommendation. The next step isn&#8217;t simply to create the index. Review the supporting metrics, expected outcome, and rollback guidance first.</p>
<p><strong>Database Agent</strong> brings these pieces together in the Azure PostgreSQL experience shown in VS Code. You can also ask Copilot for help understanding a recommendation before deciding what to do.</p>
<blockquote><p><img src="https://s.w.org/images/core/emoji/17.0.2/72x72/1f4dd.png" alt="📝" class="wp-smiley" style="height: 1em; max-height: 1em;" /> <strong>Availability note:</strong> The accompanying video introduces Database Agent as an upcoming experience, with actions applied manually. Check current availability and Azure requirements; automated remediation is discussed as a future capability.</p>
<p><img src="https://s.w.org/images/core/emoji/17.0.2/72x72/1f4a1.png" alt="💡" class="wp-smiley" style="height: 1em; max-height: 1em;" /> <strong>Before acting:</strong> Can you explain why the change should help, how you&#8217;ll check the result, and what you&#8217;ll do if it doesn&#8217;t?</p></blockquote>
<p><iframe title="Troubleshoot and Optimize PostgreSQL Performance in VS Code" width="500" height="281" src="https://www.youtube.com/embed/TyXd761Ye2s?feature=oembed" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe></p>
<h2>Try it <img src="https://s.w.org/images/core/emoji/17.0.2/72x72/2705.png" alt="✅" class="wp-smiley" style="height: 1em; max-height: 1em;" /></h2>
<ol>
<li><strong>Build with the PostgreSQL extension for VS Code:</strong> <a href="https://aka.ms/pg-pro-vs-code-repo">https://aka.ms/pg-pro-vs-code-repo</a></li>
<li><strong>Explore one table your application uses.</strong> Inspect its structure and relationships before writing a query.</li>
<li><strong>Write and review a read-only query.</strong> Check the tables, joins, and filters, then run it and confirm the results match your requirement.</li>
</ol>
<blockquote><p><strong><img src="https://s.w.org/images/core/emoji/17.0.2/72x72/1f4ac.png" alt="💬" class="wp-smiley" style="height: 1em; max-height: 1em;" /> Which database task takes the most detective work for you: understanding the schema, writing queries, or investigating performance? Let us know in the comments.</strong></p></blockquote>
<p>Happy coding! <img src="https://s.w.org/images/core/emoji/17.0.2/72x72/1f499.png" alt="💙" class="wp-smiley" style="height: 1em; max-height: 1em;" /></p>
<p>The post <a href="https://devblogs.microsoft.com/blog/postgresql-like-a-pro-vscode/">PostgreSQL Like A Pro: Build, query, and troubleshoot without leaving VS Code</a> appeared first on <a href="https://devblogs.microsoft.com">Microsoft for Developers</a>.</p>

---
*Artikel ini otomatis diambil dari [Microsoft Developer](https://devblogs.microsoft.com/blog/postgresql-like-a-pro-vscode/). Baca artikel selengkapnya di sumber asli.*
