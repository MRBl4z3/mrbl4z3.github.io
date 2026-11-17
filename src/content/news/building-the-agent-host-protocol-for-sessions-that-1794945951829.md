---
title: "Building the Agent Host Protocol for sessions that continue across devices"
description: "Discover how we built the open Agent Host Protocol to keep your agent sessions accessible across editors, apps, phones, and the cloud. The post Building the Agent Host Protocol for sessions that conti..."
pubDate: "2026-11-17T18:20:40.000Z"
authors: ["e-learning"]
organisations: ["github"]
category: "web-development"
tags: ["github", "web-development"]
thumbnail: "https://images.unsplash.com/photo-1573804633927-bfcbcd909acd?auto=format&fit=crop&w=800&h=450&q=80"
heroImage: "https://images.unsplash.com/photo-1573804633927-bfcbcd909acd?auto=format&fit=crop&w=800&h=450&q=80"
keywords: ["github", "tech news", "developer"]
canonicalUrl: "https://github.blog/engineering/engineering-principles/building-the-agent-host-protocol-for-sessions-that-continue-across-devices/"
---

<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN" "http://www.w3.org/TR/REC-html40/loose.dtd">
<html><body><p class="wp-block-paragraph">Models are now good enough to work for hours or even days on their own. You don&rsquo;t have to watch over every thing they do, but you still need to be able to check in, no matter where you are. An agent can&rsquo;t live only inside one application. The work should continue even If you close a window or switch devices.</p>



<p class="wp-block-paragraph">In GitHub Copilot you can start an agent session in one place and pick it up in another. This session continuity runs on the Agent Host Protocol (AHP), an open standard built first in VS Code and adopted across GitHub. Here&rsquo;s how it works, what we learned building it, and how you can use it to bring your agents into your own apps and workflows.</p>



<h2 id="h-why-one-integration-per-client-stopped-working" class="wp-block-heading">Why one integration per client stopped working</h2>



<p class="wp-block-paragraph">GitHub Copilot was first built in VS Code as an extension that bundled its own harness. This gave you access to your agents in VS Code, but it tied their lifetime to the editor. Extensions are attached to a single VS Code window. Close that window, and the agent is gone. Update VS Code, and every agent stops as the application restarts. Connect to a remote machine, and you get an entirely new extension host commit-locked against the version of the VS Code workbench you have.</p>



<p class="wp-block-paragraph">At the same time, the <a href="https://github.com/features/ai/github-app">Copilot app</a> was being developed, alongside the existing GitHub mobile apps for iOS and Android. Each product was doing its own distinct integration with <a href="https://docs.github.com/copilot/concepts/copilot-surfaces/copilot-on-github">cloud-based agents</a> to work towards delivering session continuity.</p>



<p class="wp-block-paragraph">Each of these integrations solved the problem for one product, but developers also still want to be able to pull their agents into their own workflows&ndash;whether that&rsquo;s an editor, a terminal, a phone, or a tool they built themselves&ndash;and they can&rsquo;t do that if every client needs its own one-off integration. That&rsquo;s why developers needed a protocol: a standard, independently versioned language across versions of VS Code, the GitHub Copilot app, mobile apps, and any other client that might host an agent. A shared protocol means every client sees the same agents, history, and state without bespoke integrations, and you aren&rsquo;t tied to one app&rsquo;s idea of what an agent session is.</p>



<h2 id="h-designing-a-protocol-for-agents-on-every-client" class="wp-block-heading">Designing a protocol for agents on every client</h2>



<p class="wp-block-paragraph">The protocol needed to allow multiple clients to interact with the same set of agents seamlessly. Start a session in the cloud from your phone, for example, then continue it from your laptop. It should tolerate network interruptions, reconnections, and concurrent client mutations. And it needed to be full-featured enough for you to do real work from each of your clients.</p>



<p class="wp-block-paragraph">At the same time, the protocol needed to stay out of agent <em>behavior</em>. Unlike the domains that produced the <a href="https://microsoft.github.io/language-server-protocol/">Language Server Protocol</a> and <a href="https://microsoft.github.io/debug-adapter-protocol/">Debug Adapter Protocol</a>, the world of agents is evolving quickly. That&rsquo;s why AHP avoids encoding how agents work, and instead focuses on how you want to interact with them.</p>



<p class="wp-block-paragraph">You also need the protocol to be simple enough to implement on your own client or server. While the full feature set would necessarily be extensive, the core protocol needed to be minimal.</p>



<p class="wp-block-paragraph">The result of these requirements is the <a href="https://microsoft.github.io/agent-host-protocol/">Agent Host Protocol</a> (AHP). It is oriented around URI-identified &ldquo;channels&rdquo;, which are the coarsest unit of functionality in the protocol. Every server must implement the root, session, and chat channels, but can also implement more channels that provide terminal support, MCP additions, or OTel reporting, for example.</p>



<p class="wp-block-paragraph">Within each channel is a reducer-based state object, which drives most of the logic for clients. The server is the arbiter of the state, and each client sends a sequence of actions that either mutate the state or trigger side effects, like sending a chat message. This provides a few very nice properties. Reconnection and replay in poor network conditions are easy: a client sends all queued actions, presents the right &lsquo;loading&rsquo; state, and waits for the server to reconcile and acknowledge the actions before confirming them. Clients and servers are straightforward to debug. And reducer-based state also plays nicely with popular UI frameworks like React.</p>



<figure class="wp-block-image size-large"><img data-recalc-dims="1" loading="lazy" decoding="async" height="512" width="1024" src="https://github.blog/wp-content/uploads/2026/11/ahp-protocol-state.png?resize=1024%2C512" alt="Three clients (VS Code, the Copilot app, and a phone) send dispatchAction messages to an agent host and receive action messages back. Inside the host, required root, session, and chat channels each hold reducer-based state; the chat channel's ordered action log feeds a reducer that produces the next state. Optional terminal, MCP, and telemetry channels sit alongside." class="wp-image-100314" srcset="https://github.blog/wp-content/uploads/2026/11/ahp-protocol-state.png?w=2400 2400w, https://github.blog/wp-content/uploads/2026/11/ahp-protocol-state.png?w=300 300w, https://github.blog/wp-content/uploads/2026/11/ahp-protocol-state.png?w=768 768w, https://github.blog/wp-content/uploads/2026/11/ahp-protocol-state.png?w=1024 1024w, https://github.blog/wp-content/uploads/2026/11/ahp-protocol-state.png?w=1536 1536w, https://github.blog/wp-content/uploads/2026/11/ahp-protocol-state.png?w=2048 2048w" sizes="auto, (max-width: 1000px) 100vw, 1000px" /></figure>



<h2 id="h-shipping-five-clients-and-two-hosts" class="wp-block-heading">Shipping five clients and two hosts</h2>



<p class="wp-block-paragraph">Adopting the Agent Host Protocol in VS Code required building a new harness and agentic stack underneath it. The Copilot Chat extension, which used to provide all of VS Code&rsquo;s AI and agentic capabilities, had been growing for over three years, reaching over 400,000 lines of TypeScript, not counting tests. But, as a VS Code extension, it was structured around the lifecycle of a VS Code window and depended entirely on the VS Code extension API. Neither that lifetime nor those APIs would be available in a standalone agent host process.</p>



<p class="wp-block-paragraph">As we started building the Agent Host Protocol in early 2026, the <a href="https://aka.ms/copilot-sdk/blog-2.0">Copilot SDK</a> was also being developed, and it made sense to leverage it as the foundation for the new agentic stack. This was a big transition. It&rsquo;s easy to build a harness implementation that says hello and can edit files&mdash;we were using the agent host to develop the agent host after the first couple weeks&mdash;but it&rsquo;s another thing to build one that covers the long tail of features developers had come to expect from Copilot Chat.</p>



<figure class="wp-block-image size-large"><img data-recalc-dims="1" loading="lazy" decoding="async" height="512" width="1024" src="https://github.blog/wp-content/uploads/2026/11/copilot-loc-history.png?resize=1024%2C512" alt="Line chart of non-test lines of code since 2023. Copilot Chat grows steadily to about 415,000 lines of TypeScript as it moves from microsoft/vscode-copilot to microsoft/vscode-copilot-chat in mid-2025 and into microsoft/vscode in April 2026. Starting in spring 2026, the VS Code agent host grows to about 369,000 lines of TypeScript and the GitHub agent host to about 305,000 lines of Rust." class="wp-image-100315" srcset="https://github.blog/wp-content/uploads/2026/11/copilot-loc-history.png?w=2400 2400w, https://github.blog/wp-content/uploads/2026/11/copilot-loc-history.png?w=300 300w, https://github.blog/wp-content/uploads/2026/11/copilot-loc-history.png?w=768 768w, https://github.blog/wp-content/uploads/2026/11/copilot-loc-history.png?w=1024 1024w, https://github.blog/wp-content/uploads/2026/11/copilot-loc-history.png?w=1536 1536w, https://github.blog/wp-content/uploads/2026/11/copilot-loc-history.png?w=2048 2048w" sizes="auto, (max-width: 1000px) 100vw, 1000px" /></figure>



<p class="wp-block-paragraph">GitHub has had <a href="https://docs.github.com/copilot/concepts/copilot-surfaces/copilot-on-github">Copilot cloud agents</a> for a couple of years, allowing developers to run tasks in the cloud from any device. While VS Code was quietly developing the Agent Host Protocol, GitHub was looking to expand that capability to coordinate sessions running on your own devices via the Copilot app, or in cloud sandboxes. This was exactly the <code>N:M</code> problem that AHP was built to solve, so adopting the same technology made perfect sense.</p>



<p class="wp-block-paragraph">From the outset, the GitHub Agent Host had different requirements than VS Code&rsquo;s implementation. VS Code runs the host locally and can drop its host on other compute over SSH, containers, WSL, or Dev Tunnels, and it is actually included as part of the <a href="https://code.visualstudio.com/docs/remote/vscode-server">VS Code Server</a> bundle. The GitHub Agent Host needed to run in the cloud and across a much wider variety of infrastructure, environments, and security constraints.</p>



<p class="wp-block-paragraph">Those differing requirements are why they&rsquo;re separate implementations written in different languages: VS Code&rsquo;s in TypeScript and GitHub&rsquo;s in Rust. For now, having two agent hosts has been a good acid-test for the protocol itself. How well does it work when two entirely different teams and sets of agents implement the same protocol independently?</p>



<p class="wp-block-paragraph">Pretty well, it turns out. Like MCP, the Agent Host Protocol provides escape hatches via custom methods and <code>_meta</code>data on objects that consumers can use for out-of-band signaling. After several months heads-down on our respective implementations, our first task as we worked toward unification was inventorying the ways each implementation had extended the protocol. Many of these extensions reasonably stay GitHub-specific, such as the GitHub Agent Host using metadata to link to project boards. In other cases, variance was a sign of a gap in the protocol; early versions, for example, could not accurately represent shell-type tool output that wasn&rsquo;t backed by a PTY. And some were bad assumptions, like VS Code&rsquo;s client assuming a particular structure for the session URIs it received from the agent host. A protocol with only one implementation isn&rsquo;t truly portable; as we taught our clients and servers to talk to each other, AHP became portable in practice, not just in theory.</p>



<figure class="wp-block-image size-large"><img data-recalc-dims="1" loading="lazy" decoding="async" height="512" width="1024" src="https://github.blog/wp-content/uploads/2026/11/session-across-surfaces.png?resize=1024%2C512" alt='A single session, "Fix the flaky login test," runs continuously on an agent host. It is started from the Copilot app at 9:02 AM, a tool call is approved from a phone at 9:40 AM, the diff is reviewed in VS Code at 11:15 AM, and a cloud agent lands the pull request at 11:32 AM.' class="wp-image-100316" srcset="https://github.blog/wp-content/uploads/2026/11/session-across-surfaces.png?w=2400 2400w, https://github.blog/wp-content/uploads/2026/11/session-across-surfaces.png?w=300 300w, https://github.blog/wp-content/uploads/2026/11/session-across-surfaces.png?w=768 768w, https://github.blog/wp-content/uploads/2026/11/session-across-surfaces.png?w=1024 1024w, https://github.blog/wp-content/uploads/2026/11/session-across-surfaces.png?w=1536 1536w, https://github.blog/wp-content/uploads/2026/11/session-across-surfaces.png?w=2048 2048w" sizes="auto, (max-width: 1000px) 100vw, 1000px" /></figure>



<p class="wp-block-paragraph">Today, the Agent Host Protocol provides that same continuity across all VS Code and GitHub surfaces. Agent sessions running locally in the GitHub Copilot app, in cloud sandboxes, and in VS Code all share the same underlying protocol. To see how it&rsquo;s used, read about how you can <a href="https://github.blog/changelog/2026-10-28-work-from-anywhere-with-a-new-connected-copilot-cloud-experience/">work from anywhere with the new connected Copilot cloud experience</a>.</p>



<h2 id="h-using-the-agent-host-protocol" class="wp-block-heading">Using the Agent Host Protocol</h2>



<p class="wp-block-paragraph">AHP was designed to be easy to use in clients and servers. <a href="https://microsoft.github.io/agent-host-protocol/">Agent Host Protocol</a> libraries are available in Rust, TypeScript, Kotlin, Swift, .NET, and Go, and agents are able to build clients and servers with little prompting. I have a macropad sitting on my desk that reflects my agent&rsquo;s state in real time, and others have built everything from smart watch apps to Kanban-style task management tools against the protocol. Whether you want a companion app for your own agents, a way to bring agent sessions into your team&rsquo;s tools, or AHP support in your own editor or agent, the protocol is open for you to build on.</p>



<p class="wp-block-paragraph">You can connect to an agent host you implement in the Copilot app (enable <strong>Experimental &gt; Direct Connections</strong>, then go to <strong>Environments &gt; Add Agent Host Protocol Host</strong>) and in VS Code (via <strong>Add Remote Agent Host</strong> in the Agents window). You can spawn a standalone version of VS Code&rsquo;s agent host by running <code>code agent host</code> in your terminal, or see the endpoints for the one in your editor by running <code>code agent endpoints</code>, and <a href="https://github.com/microsoft/agent-host-protocol-samples/">samples</a> can help you and your agent get started.</p>



<p class="wp-block-paragraph">The Agent Host Protocol is an open standard, and questions and suggestions are welcome <a href="https://github.com/microsoft/agent-host-protocol">on GitHub</a>.</p>



<p class="wp-block-paragraph">Happy coding!</p>



<div class="wp-block-group post-content-cta has-global-padding is-layout-constrained wp-block-group-is-layout-constrained">
<p class="wp-block-paragraph"><a href="https://github.com/microsoft/agent-host-protocol">Explore the Agent Host Protocol on GitHub &gt;</a></p>
</div>
</body></html>
<p>The post <a href="https://github.blog/engineering/engineering-principles/building-the-agent-host-protocol-for-sessions-that-continue-across-devices/">Building the Agent Host Protocol for sessions that continue across devices</a> appeared first on <a href="https://github.blog">The GitHub Blog</a>.</p>

---
*Artikel ini otomatis diambil dari [GitHub Blog](https://github.blog/engineering/engineering-principles/building-the-agent-host-protocol-for-sessions-that-continue-across-devices/). Baca artikel selengkapnya di sumber asli.*
