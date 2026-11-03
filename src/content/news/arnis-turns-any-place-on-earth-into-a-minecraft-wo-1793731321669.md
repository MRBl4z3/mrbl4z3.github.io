---
title: "Arnis turns any place on Earth into a Minecraft world"
description: "Louis Erbkamm built the first prototype in his free time. Now GitHub Copilot helps him maintain it alone. The post Arnis turns any place on Earth into a Minecraft world appeared first on The GitHub Bl..."
pubDate: "2026-11-03T16:00:00.000Z"
authors: ["e-learning"]
organisations: ["github"]
category: "web-development"
tags: ["github", "web-development"]
thumbnail: "https://images.unsplash.com/photo-1573804633927-bfcbcd909acd?auto=format&fit=crop&w=800&h=450&q=80"
heroImage: "https://images.unsplash.com/photo-1573804633927-bfcbcd909acd?auto=format&fit=crop&w=800&h=450&q=80"
keywords: ["github", "tech news", "developer"]
canonicalUrl: "https://github.blog/open-source/maintainers/arnis-turns-any-place-on-earth-into-a-minecraft-world/"
---

<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN" "http://www.w3.org/TR/REC-html40/loose.dtd">
<html><body><p class="wp-block-paragraph">Louis Erbkamm knows his way around Arnis, the smallest town in Germany, but he&rsquo;s never set foot in it.</p>



<blockquote class="wp-block-quote is-layout-flow wp-block-quote-is-layout-flow">
<p class="wp-block-paragraph">I know where the shops are. Where the boats are&hellip; I know the city pretty well, but I haven&rsquo;t been there yet.</p>
<cite>Louis Erbkamm, maintainer of Arnis</cite></blockquote>



<p class="wp-block-paragraph">Arnis is also the name of his open source project, which takes real-world data from OpenStreetMap and other sources to turn places you know into Minecraft worlds you can freely explore. Since Louis started Arnis in 2022, it&rsquo;s reached close to 900,000 downloads from GitHub and about 70 contributors.</p>



<h2 id="h-two-weeks-and-a-slow-prototype" class="wp-block-heading">Two weeks and a slow prototype</h2>



<p class="wp-block-paragraph">Louis grew up playing Minecraft and started coding at 13, always looking for ways to bring code into the game. Arnis began as a fun diversion combining Minecraft with geospatial data, and it went on GitHub, where he already kept his side projects. He finished the first prototype within two weeks while on a break from school.</p>



<p class="wp-block-paragraph">That first version was Python written by hand, built only on OpenStreetMap, and generating a town took a while. He needed somewhere real to test it, so he looked up towns in Germany, where he lives, and found Arnis. A small town helped while early generation was slow.</p>



<blockquote class="wp-block-quote is-layout-flow wp-block-quote-is-layout-flow">
<p class="wp-block-paragraph">Arnis has a lot of buildings, it has streets, it has waters, it has all kinds of different elements that are useful to test with.</p>
<cite>Louis</cite></blockquote>



<p class="wp-block-paragraph">The project didn&rsquo;t have a name yet, so it borrowed the town&rsquo;s unique moniker.</p>



<h2 id="h-a-community-that-brings-its-own-ideas" class="wp-block-heading">A community that brings its own ideas</h2>



<p class="wp-block-paragraph">When Arnis added elevation, its worlds stopped being flat, and geospatial specialists from all over showed up with ideas on how best to integrate it. They started discussions on their own, and Louis says that&rsquo;s exactly what he needed.</p>



<p class="wp-block-paragraph">Bigger calls happen in the open too: before porting Arnis from Python to Rust, Louis opened discussions on GitHub to hear different perspectives. Contributors have since translated Arnis into about 20 languages and built major features of their own.</p>



<blockquote class="wp-block-quote is-layout-flow wp-block-quote-is-layout-flow">
<p class="wp-block-paragraph">The best thing that has happened to me is essentially this big community on GitHub to see actually people from all kinds of different fields coming together to work on something that I started as a side project.</p>
<cite>Louis</cite></blockquote>



<h2 id="h-how-a-trash-bin-gets-into-minecraft" class="wp-block-heading">How a trash bin gets into Minecraft</h2>



<p class="wp-block-paragraph">Some features start on a walk. Louis noticed how many trash bins there were in his city, and there were none in his generated worlds.</p>



<p class="wp-block-paragraph">Minecraft has no dedicated bins, so he took the question to the community, and together they found a block that fit. Placing one follows the same path as everything else: for the area a user selects, Arnis queries OpenStreetMap for every mapped element, then looks up each element&rsquo;s coordinates and type.</p>



<p class="wp-block-paragraph">If an element isn&rsquo;t mapped, Arnis can&rsquo;t place it. For structures too detailed for the map to describe, like the Statue of Liberty in New York, or Christ the Redeemer in Rio de Janeiro, Arnis converts 3D models into Minecraft blocks.</p>



<h2 id="h-one-maintainer-a-stack-of-pull-requests" class="wp-block-heading">One maintainer, a stack of pull requests</h2>



<p class="wp-block-paragraph">As the sole maintainer, Louis reviews every pull request himself, and they stack up. He usually spends evenings or weekends working on Arnis, which means the community has to wait for him to be done with work for the day.</p>



<p class="wp-block-paragraph">He uses GitHub Copilot to get an overview of open issues and bugs from the latest release, see how many people are running into them, trace where a bug came from in the codebase, and create a fix. Since he brought AI into the project, he says, code reviews and documentation have both increased.</p>



<blockquote class="wp-block-quote is-layout-flow wp-block-quote-is-layout-flow">
<p class="wp-block-paragraph">I could delegate those annoying and boring tasks to Copilot, while I could still have a look at those really cool topics that are related to the development of it.</p>
<cite>Louis</cite></blockquote>



<h2 id="h-sub-agents-that-look-for-regressions" class="wp-block-heading">Sub-agents that look for regressions</h2>



<p class="wp-block-paragraph">Big features arrive as big pull requests.</p>



<p class="wp-block-paragraph">Louis opens a large pull request in the GitHub Copilot app for a first overview and a performance review, with Copilot running in the background looking for small changes that would cut memory usage or generation time.</p>



<blockquote class="wp-block-quote is-layout-flow wp-block-quote-is-layout-flow">
<p class="wp-block-paragraph">It takes just one wrong calculation in street lights, for example, to make the generation time of an entire city much slower.</p>
<cite>Louis</cite></blockquote>



<p class="wp-block-paragraph">He also keeps a mental list of the areas of the codebase that need extra care whenever someone changes them, and he has written the same warnings out by hand over and over.</p>



<blockquote class="wp-block-quote is-layout-flow wp-block-quote-is-layout-flow">
<p class="wp-block-paragraph">I&rsquo;m starting a few sub agents with the GitHub Copilot app to have a look specifically at those cases, and to come up with corner cases to test to make sure that we do not introduce any regressions into new releases.</p>
<cite>Louis</cite></blockquote>



<p class="wp-block-paragraph">He likes that the terminal, his files, the code changes and the pull request all sit in one app next to the chat.</p>



<h2 id="h-an-unexpected-classroom" class="wp-block-heading">An unexpected classroom</h2>



<p class="wp-block-paragraph">Louis&rsquo;s favorite use of Arnis is in schools, where teachers have generated their city or a neighborhood park.</p>



<p class="wp-block-paragraph">Some turn it into a contest: every student starts from the same generated area, improves a nearby park or playground using Arnis and Minecraft, and the class looks at the results together.</p>



<h2 id="h-where-arnis-goes-next" class="wp-block-heading">Where Arnis goes next</h2>



<p class="wp-block-paragraph">Recent releases have gone beyond the bounds of Earth, adding the Moon and Mars. Louis wants Arnis to stay easy to use while getting closer to reality, with more detail in less time, and he would like to see it in more schools.</p>



<h2 id="h-start-with-your-hometown" class="wp-block-heading">Start with your hometown</h2>



<p class="wp-block-paragraph">Download Arnis from github.com/louis-e/arnis, pick a place on the map, choose where to spawn, and click Start Generation: within a few minutes, the world is ready to open in Minecraft. Louis suggests a place you know, like your hometown or somewhere memorable you&rsquo;ve been, such as the Grand Canyon or Niagara Falls. Then look for what&rsquo;s missing.</p>



<blockquote class="wp-block-quote is-layout-flow wp-block-quote-is-layout-flow">
<p class="wp-block-paragraph">So maybe you see a bench that was not generated even though it is there.</p>
<cite>Louis</cite></blockquote>



<p class="wp-block-paragraph">Map it on OpenStreetMap, or tell Louis if it&rsquo;s a bug and he will fix it.</p>



<p class="wp-block-paragraph"></p>
</body></html>
<p>The post <a href="https://github.blog/open-source/maintainers/arnis-turns-any-place-on-earth-into-a-minecraft-world/">Arnis turns any place on Earth into a Minecraft world</a> appeared first on <a href="https://github.blog">The GitHub Blog</a>.</p>

---
*Artikel ini otomatis diambil dari [GitHub Blog](https://github.blog/open-source/maintainers/arnis-turns-any-place-on-earth-into-a-minecraft-world/). Baca artikel selengkapnya di sumber asli.*
