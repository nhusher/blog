---
title: What's next?
kerned_title: What&rsquo;<span class="k1-1">s</span> next?
layout: post
---

A few days ago, the development team behind YUI [announced that development on the project would be halted](http://yahooeng.tumblr.com/post/96098168666/important-announcement-regarding-yui). I didn't think I had anything to say about it&mdash;I assumed that I would feel some sadness for a few weeks or months until I moved onto whatever is next. But it's been a week, and I think I'm itching for something to say.

I've also been looking for a reason to start blogging again, and this is a good a reason as any. In the wake of the "death" of YUI, what now? What's good out there? What are the bad ideas? I wonder if there's anything I can contribute to the conversation.

The world of JS has changed a lot in the last five years. The old-and-busted IE world is fading and the future looks like a bright forest of evergreen browsers. Node.js came out of left field; it now completely dominates the frontend build-tooling world and has a growing place as the [frontend of the backend](http://www.nczonline.net/blog/2013/10/07/node-js-and-the-new-web-front-end/). There are now enough popular compile-to-JS languages that whole communities have sprouted up around each. It's now possible to compile a C++ game engine to something that can run on the browser. The modern web is a weird place, I hope to make a little bit of sense out of it in the process.

I suspect that I'll be writing about three different topics in the coming months:

1. Exploring how to build large applications using new frontend technologies.
2. Demonstrations of interesting new or "fringe" programming constructs
3. Looking at the behavior and implications of language features

Before I start looking forward, I want to begin by looking back.

#### A retrospective on YUI

I've been building applications in YUI for my entire professional career. YUI2 was the first library I picked for ajax, animations, and widgets. I tracked the early development of YUI3 with great interest. Learning YUI3 threw me into entrprise-quality javascript with composable primitives, great documentation, comprehensive testing, and readable source code.

YUI was never a very sexy tool to work with. For most of the last 5 years, it was owned by Yahoo, which despite its [great](https://twitter.com/juandopazo) [in-house](http://tilomitra.com/) [talent](http://new.davglass.com/), doesn't project an aura of developer credibility, especially in the last few years. The YUI branding was boring and didn't clearly advertise what made it special. The value of the framework was also hard to articulate to fellow UI developers; In 2010 extensibility, composibility, and maintainability took a back seat to simple APIs and fast DOM mutation. For almost everyone, the features of the library appeared monolithic and over-engineered. I believe history has validated YUI's ideas, though&mdash;in 2014, most of the ideas first pioneered by YUI exist in current client-side frameworks.

To its fans, YUI felt like being in a near-future world where browser inconsistencies were gone, the event API was robust and sane, modules could be isolated and tested, and building applications with complicated state wasn't a nightmare of DOM smashing. If a component didn't do exactly what you wanted it to, almost anything could be extended and modified. Compared to black-box jQuery plugins that were its contemporary, YUI is a revelation in composability.

The community was similarly execellent. I used to monitor the message board, then the mailing list, and still spend much of my day lurking on IRC. I feel strongly that the quality of a community can be measured by the responses to open-ended design questions ("What's the best way of doing *x*?"). There was always a constructive discussion about the most-maintainable way to achive a certain goal, even if it wasn't in the "YUI happy path". Asking a question about a new feature like promises or application routing was met with helpful examples, usually by the designers of those modules.

Unlike many modern JS frameworks, YUI isn't "opinionated"<sup>[1]</sup> about application structure or design. It makes a few recommendations, but the components were loosely-coupled and could be used in many arrangements. Prior to the annancouement, there was discussion of breaking the framework up into ES6 modules that could be used outside the YUI context with limited dependencies. This would have been a significant-but-worthwhile project.

There's still a lot of code that I'll be working with that sits on top of YUI, but has been marked for refactoring to Angular<sup>[2]</sup> since late last year. Eventually, the dependency will be removed entirely, and I'll have few enough reasons to hit up the documentation. Still, I wouldn't be the developer I am today without the experience I've had with YUI; the great ideas embedded in it will inform my decisions about writing software for a long time.

<ol class="footnotes">
  <li id="1-1">
    "Opinionated" feels like a spin-word for a much uglier reality&mdash;an opinionated system is tightly-coupled or very complex, and usually both. It's telling that systems that could reasonably be called opinionated, but also have well-justified reasons for their decisions, do not claim that adjective. On the other hand, systems that make design decisions without clearly-stated rationales are eager to use it.
  </li>
  <li>
    I assume I'll be writing a lot about Angular in the coming months. I'm sure know too little about it, and I'm sure dislike it too much.
  </li>
</ol>

[1]: #1-1
[2]: #1-2
