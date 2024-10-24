---
title: Web Components are not Framework Components — and That’s Okay
date: 2024-10-01
image: images/component-types.png
social_posts:
  - twitter: https://x.com/twitter/status/1841183386024755660
  - mastodon: https://front-end.social/@leaverou/113233583191981935
---

_**Disclaimer:** This post expresses my opinions, which do not necessarily reflect consensus by the whole Web Components community._

A [blog post by Ryan Carniato](https://dev.to/ryansolid/web-components-are-not-the-future-48bh)
titled _“Web Components Are Not the Future”_ has recently stirred a lot of controversy.
A few other JS framework authors pitched in, expressing [frustration](https://x.com/youyuxi/status/1839833110164504691) and [disillusionment](https://x.com/Rich_Harris/status/1839885047349788720) around Web Components.
Some Web Components [folks](https://www.linkedin.com/posts/kreitlow_as-someone-who-builds-complex-custom-elements-activity-7246381911470682113-dPoI/) [wrote](https://nolanlawson.com/2024/09/28/web-components-are-okay/) [rebuttals](https://www.abeautifulsite.net/posts/web-components-are-not-the-future-they-re-the-present/),
while others [repeatedly](https://x.com/Mr__Disagree/status/1839487412797350267) [tried](https://x.com/mr__disagree/status/1839846994040283344) to get to the bottom of the issues,
so they could be addressed in the future.

When you are on the receiving end of such an onslaught,
the initial reaction is to feel threatened and become defensive.
However, these kinds of posts can often end up shaking things up and pushing a technology forwards in the end.
I have some personal experience:
after I published my [2020 post titled “The failed promise of Web Components”](../../2020/09/the-failed-promise-of-web-components) which also made the rounds at the time,
I was approached by a bunch of folks (Justin Fagnani, Gray Norton, Kevin Schaaf) about teaming up to fix the issues I described.
The result of these brainstorming sessions was the [Web Components CG](https://web-components-cg.netlify.app/) which now has a life of its own
and has become a vibrant Web Components community that has helped move several specs of strategic importance forwards.

<!-- more -->

As someone who deeply cares about Web Components,
[my initial response was also to push back](https://x.com/LeaVerou/status/1840134654852247765).
I was reminded of how many times I have seen this pattern before.
**It is common for new web platform features to face pushback and resistance for many years**;
we tend to compare them to current userland practices, and their ergonomics often fare poorly at the start.
Especially when there is no immediately apparent [80/20 solution](https://en.wikipedia.org/wiki/Pareto_principle), making things _possible_ tends to precede making them _easy_.

Web platform features operate under a whole different set of requirements and constraints:
- They need to last _decades_, not just until the next major release.
- They need to not only cater to the current version of the web platform, but _anticipate_ its future evolution and be compatible with it.
- They need to be _backwards compatible_ with the web as it was 20 years ago.
- They need to be compatible with a slew of accessibility and internationalization needs that userland libraries often ignore at first.
- They are developed in a distributed way, by people across many different organizations, with different needs and priorities.

Usually, the result is **more robust, but takes a lot longer**.
That’s why I’ve often said that web standards are _"product work on hard mode"_ — they include most components of regular product work (collecting user needs, designing ergonomic solutions, balancing impact over effort, leading without authority, etc.), but with the added constraints of a distributed, long-term, and compatibility-focused development process that would make most PMs pull their hair out in frustration and run screaming.

**I’m old enough to remember this pattern playing out with CSS itself**:
huge pushback when it was introduced in the mid 90s.
It was clunky for layout and had terrible browser support — _“why fix something that wasn’t broken?”_ folks cried.
Embarrassingly, I was one of the last holdouts:
I liked CSS for styling, but was among the last to switch to floats for layout — tables were just so much more ergonomic!
The majority resistance lasted until the mid '00s when it went from _“this will never work”_ to _“this was clearly the solution all along”_ almost overnight.
And the rest, as they say, is history. 🙂

But the more I thought about this, the more I realized that — as often happens in these kinds of heated debates — **the truth lies somewhere in the middle**.
Having used both several frameworks, and several web components,
and having authored both web components (most of them experimental and unreleased) and [even one framework](https://mavo.io/) over the course of [my PhD](https://phd.verou.me), both sides do have some valid points.

Frankly, if framework authors were sold the idea that web components would be a compile target for their frameworks, and then got today’s WC APIs, I understand their frustration.
Worse yet, if every time they tried to explain that this sucks as a compile target they were told "no you just don’t get it", heck I’d feel gaslit too!
**Web Components are still far from being a good compile target for all framework components,
but that is not a prerequisite for them being useful**.
They simply solve different problems.

Let me explain.

<figure>

<img src="images/component-types.svg" alt="Venn diagram illustrating general vs project-specific components">

<figcaption>

Not all component use cases are the same.
</figcaption>
</figure>

I think the crux of this debate is that **the community has mixed two very different categories of use cases**,
largely because frameworks do not differentiate between them;
"component" has become the hammer with which to hammer every nail.
Conceptually, there are two core categories of components:
1. **Generalizable elements** that extend what HTML can do and can be used in the same way as native HTML elements across a wide range of diverse projects.
Things like tabs, rating widgets, comboboxes, dialogs, menus, charts, etc.
Another way to think about these is _"if resources were infinite, elements that would make sense as native HTML elements"_.
2. **Reactive templating**: UI modules that have project-specific purposes and are not required to make sense in a different project.
For example, a font foundry may have a component to demo a font family with child components to demo a font family style,
but the uses of such components outside the very niche font foundry use case are very limited.

Of course, it’s a spectrum; few things in life fit neatly in completely distinct categories.
For example an `<html-demo>` component may be somewhat niche, but would be useful across any site that wants to demo HTML snippets
(e.g. a web components library, a documentation site around web technologies, a book teaching how to implement UI patterns, etc.).
But the fact that it’s a spectrum does not mean the distinction does not exist.

WCs primarily benefit the use case of generalizable elements that extend HTML,
and are still painful to use for reactive templating.
Fundamentally, **it’s about the ratio of potential consumers to authors**.

The huge benefit of Web Components is **interoperability**:
you write it once, it works forever, and you can use it with any framework (or none at all).
It makes no sense to fragment efforts to reimplement e.g. tabs or a rating widget separately for each framework-specific silo,
it is simply duplicated busywork.

As a personal anecdote, a few weeks ago I found this [amazing JSON viewer component](https://carlosnz.github.io/json-edit-react/), but I couldn’t use it because I don’t use React (I prefer Vue and Svelte).
To this day, I have not found anything comparable for Vue, Svelte, or vanilla JS.
This kind of fragmentation is sadly an everyday occurrence for most devs.

But when it comes to project-specific components, the importance of interop decreases:
you typically pick a framework and stick to it across your entire project.
Reusing project-specific components across different projects is not a major need,
so the value proposition of interop is smaller.

Additionally, the **ergonomics** of consuming vs authoring web components are vastly different.
_Consuming_ WCs is already pretty smooth, and the APIs are largely there to demystify most of the magic of built-in elements and expose it to web components (with a few small gaps being actively plugged as we speak).
However, _authoring_ web components is a whole different story.
Especially without a library like [Lit](https://lit.dev/), authoring WCs is still painful, tedious, and riddled with footguns.
For generalizable elements, this is an acceptable tradeoff, as their potential consumers are a much larger group than their authors.
As an extreme example of this, nobody complains about the ergonomics of implementing native elements in browsers using C++ or Rust.
But when using components as a templating mechanism, authoring ergonomics are _crucial_,
since the overlap between consumers and authors is nearly 100%.

This was the motivation behind [this Twitter poll I posted a while back](https://x.com/LeaVerou/status/1697245010650148924).
I asked if people mostly _consumed_ web components, _used_ WCs that others have made, or both.
Note that many people who use WCs are not aware of it, so the motivation was not to gauge adoption,
but to see if the community has caught on to this distinction between use cases.
**The fact that > 80% of people who knowingly use web components are also web components _authors_ is indicative of the problem.**
WCs are meant to empower folks to do more, not to be consumed by expert web developers who can also write them.
Until this number becomes a lot smaller, Web Components will not have reached their full potential.
This was one of the reasons [I joined the Web Awesome project](../awesome);
I think that is the right direction for WCs:
encapsulating complexity into beautiful, generalizable, customizable elements that give people superpowers by extending what HTML can do:
they can be used by developers to author gorgeous UIs,
designers to do more without having to learn JS,
or even hobbyists that struggle with both (since HTML is the most approachable web platform language).

So IMO making it about frameworks vs web components is a false dichotomy.
Frameworks already use native HTML elements in their components.
**Web components extend what native elements can do, and thus make crafting project-specific components easier across *all* frameworks** (as well as *no* frameworks).
I wonder if this narrative could resonate across both sides and reconcile them.
Basically _“yes, we may still need frameworks for nontrivial apps, but web components make their job easier”_
rather than pitting them against each other in a pointless comparison where everyone loses.

We will certainly eventually get to the point where web components are more ergonomic to author,
but we first need to get the low-level foundations right.
At this point **the focus is still on making things _possible_ rather than making them _easy_**.
The last remaining pieces of the puzzle are things like
[Reference Target](https://github.com/WICG/webcomponents/blob/gh-pages/proposals/reference-target-explainer.md) for cross-root ARIA
or [`ElementInternals.type`](https://github.com/openui/open-ui/issues/1088#issuecomment-2366092981) to allow custom elements to become popover targets or submit buttons,
both of which saw a lot of progress at [W3C TPAC](https://www.w3.org/2024/09/TPAC/Overview.html) last week.

After that, perhaps eventually web components will even become viable for reactive templating use cases;
things like the [`open-stylable` shadow roots](https://github.com/WICG/webcomponents/issues/909) proposal,
declarative elements, or [DOM Parts](https://github.com/WICG/webcomponents/blob/gh-pages/proposals/DOM-Parts-Declarative-Template.md)
are some early beginnings in that direction,
and [declarative shadow DOM](https://www.konnorrogers.com/posts/2023/what-is-declarative-shadow-dom) paved the way for SSR (among other things).
**Then, and only then, they may make sense as a compile target for frameworks.**
However, that is quite far off.
And even if we get there, frameworks would still be useful for complex use cases,
as they do a lot more than let you use and define components.
Components are not even the best reuse mechanism for every project-specific use case — e.g. for list rendering, components are overkill compared to something like [`v-for`](https://vuejs.org/guide/essentials/list).
And by then frameworks will be doing even more.
It is by definition that frameworks are always a step ahead of the web platform,
not a failing of the web platform.
As Cory [said](https://www.abeautifulsite.net/posts/web-components-are-not-the-future-they-re-the-present/), _“Frameworks are a testbed for new ideas that may or may not work out.”_.

The bottom line is, web components reduce the number of use cases where we need to reach for a framework,
but complex large applications will likely still benefit from one.
**So how about we conclude that frameworks are useful, web components are also useful, stop fighting and go make awesome sh!t using whatever tools we find most productive?**

_Thanks to Michael Warren, Nolan Lawson, Cory LaViska, Steve Orvell, and others for their feedback on earlier drafts of this post._
