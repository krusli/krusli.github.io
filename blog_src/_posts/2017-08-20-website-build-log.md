---
layout: post
title: Angular 4 personal website - a "build log"
post_description: The journey of one bored dev's personal site. A first of many posts.
---

![Old version](https://krusli.github.io/assets/website-old.jpg)
*Old version of the website*

Insomnia is hitting me hard - and after hours of trying to (and failing at) falling asleep, I decided that I might as well make my time worthwhile while I'm at it.

I've had some ideas at the top of my head for what I wanted in my website. In particular, I wanted my website to:
- tell my _story_ better - in a format that reads more like a narrative,
- be more maintainable,
- have reusable common elements - through Components,
- as a side benefit, implement the Single Page Application model,
- let me try my hand at web design and development again,
- at the very least, have no dead links (right now, the _About me_ and _My projects_ links are pretty much dead) 😅, and
- implement the design ideas I sketched and had floating around my head for weeks but never got around to finally implementing.

I used Angular before for several projects: [_HookTurns_](http://hookturns.info) and  [_OpenElement_](https://openelement.herokuapp.com), and although the learning curve for Angular was quite steep, I found Angular's rich feature-set especially rewarding for me.

Alas that's enough of an introduction - I should get started with the project.

### Setup
[`@angular/cli`](https://www.npmjs.com/package/@angular/cli) - this utility lets you easily create a project, run it locally, build the project, run tests and even reduces the amount of boilerplate code you have to write for Components, Directives, Services. Just use this - it makes your life easier, and makes me wish I started with this when I first worked with Angular[^1].

I've used [Bootstrap](https://getbootstrap.com) several times before (including my old site), and I'm coming back to it for this version of the website too. If you're not familiar with it (unlikely), Bootstrap is a front-end framework for responsive design websites, providing a grid system, UI elements and components, and styling (which can still be customised via CSS).

[^1]: Google's [Angular guides](https://angular.io/guide/quickstart) had me configuring the dev environment myself - downloading the libraries, configuring scripts for the webserver and transpiler, taking too much effort and even refusing to do a Webpack build for me in the end. The guide has since been updated to use `@angular/cli`.

### Design
Instead of simply building off a Bootstrap template, I wanted to use Bootstrap elements (and design "patterns") with some code to help with the UI behaviours I want:

![](https://krusli.github.io/assets/top-bar1.jpg)
![](https://krusli.github.io/assets/top-bar2.jpg)
*A top bar that collapses when users scroll down on the page.*

<br>
![](https://krusli.github.io/assets/nav-indicator1.jpg)
![](https://krusli.github.io/assets/nav-indicator2.jpg)
*A navigation indicator (on the right-hand side of the pages here) to indicate the user's current location and lets the user jump between sections. I plan to have my navigation indicator on the left hand side of the page though.*

<br>
![](https://krusli.github.io/assets/album.jpg)
*Album-style view, for a projects showcase.*
