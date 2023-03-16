---
layout: post
title:  "Building the JustNotSorry MVP"
author: Steve Brudz
date:   2016-03-13
description: "JustNotSorry is a free extension that helps you send more confident emails by warning you when you use words which undermine your message.  Learn about how we built the minimum viable product (MVP)."
---
![JustNotSorry logo](/img/justnotsorry/justnotsorry-logo.webp)

[JustNotSorry](https://justnotsorry.com/) is a free app that underlines words in your email that might be undermining your message. You can read about the back story [here](https://medium.com/cyrusite-chatter/just-not-sorry-the-backstory-33f54b30fe48#.82135epsz), but this post is about how we built the minimum viable product (MVP).

[Tami Reiss](http://tamireiss.com/) came to me with the idea for it while I was between clients — on the beach, as we say at Cyrus. I had limited time because I'd be starting at a new client soon. We also weren't sure how interested people would be in it, so we set out to build a lean MVP — just enough functionality to be useful. It had to highlight words like "just" and "sorry" as you were composing your email. We'd make it for Gmail since it's a common email platform and it's the one we use at Cyrus.

I started with research. I had created a prototype Gmail extension a couple years earlier that added information to a sidebar but I'd never done anything with the compose email features. Plus, things change quickly in the software world, so I needed to see what was currently supported. Some googling found that Gmail still had the concept of extensions but it didn't have any other sort of client-side API. I spent some time prototyping an extension to see if I could hook into the compose window. The documentation was out of date, though, and in some places seemed to indicate that extensions were being deprecated. I found myself checking stack overflow just to get a "Hello World" running. This was a dead end.

I looked at other Gmail extensions, such as [Crystal](https://www.crystalknows.com/), and found that they had built separate extensions for Chrome, Firefox, and Safari. A quick survey of the Chrome Extension docs showed that they were clear, straightforward, and seemingly up-to-date. After a quick discussion with Tami, we decided to limit our MVP to just support Gmail on Chrome.

Chrome extensions allow you to run JavaScript on top of a loaded web page. I realized that I could modify the Gmail compose window using a library like [jQuery](https://jquery.com/). This is the same technique that tools like [Optimizely](https://www.optimizely.com/) use for creating A/B tests, where they show different button text to different users. In order to do this, you need to understand a bit about the HTML page that you are modifying — you need to find a class or an id that you can hook into with jQuery. I logged in to Gmail and used the Chrome developer tools to poke around in the page's [document object model](https://en.wikipedia.org/wiki/Document_Object_Model) (DOM). Gmail's DOM is very complex. The class names and ids are generated and opaque. Many of them seem to change each time you load the page. It seemed like a lot of work to reverse engineer this logic, so I went looking for open source libraries. Fortunately, [Kartik Talwar](https://twitter.com/TheRealKartik) had built [Gmail.js](https://github.com/KartikTalwar/gmail.js) to solve just this problem. He even had a [Chrome extension boilerplate project](https://github.com/KartikTalwar/gmail-chrome-extension-boilerplate).

I used the boilerplate to load Gmail.js and tried out the library functions using the Chrome developer console. There was a way to trigger a callback function whenever a compose window was created. Just what I needed.

The Gmail compose window uses an HTML 5 feature called [contenteditable](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/contenteditable), which allows for rich text editing, such as formatting, bold, italics, etc. Under the hood, this means that the DOM for the compose window is a mini HTML tree. If you write an email that looks like this:

> Hello,
> 
> I'm **sorry** that I was late.

then the underlying HTML looks something like this:

```html
<div contenteditable="true">
Hello,
<div>
I'm <b>sorry</b> that I was late.
</div>
```

and the DOM for it would be a tree structure that looks like this:

![DOM tree structure of an email](/img/justnotsorry/jns-dom.webp)

To find the phrase "I'm sorry" requires searching the tree and combining text at different levels. I started to write my own way of doing this, but then I came across James Padolsey's [findAndReplaceDOMText](https://github.com/padolsey/findAndReplaceDOMText) library, which solved the problem.

One of the key requirements of the MVP was to provide feedback as you were composing your email. To do this, I used a feature of the DOM called a [MutationObserver](https://developer.mozilla.org/en-US/docs/Web/API/MutationObserver). This enables your JavaScript code to watch a particular part of the DOM for any changes and to trigger a function when certain conditions are met. For JustNotSorry, I set it up to watch for any changes in text or any additions or removals of DOM elements from the compose window. This allows it to check for warnings each time a character is typed into the compose window.

The first prototype of JustNotSorry used these features to turn itself on when a compose window was opened, run checking logic each time a character was typed, and highlight the word "just" if it found it by wrapping that word in a span tag that was styled with CSS.

Another key requirement was that the highlighting should not be included in the email that was sent. My initial efforts found that this was harder to do than it seemed. I tried to hook onto the send button, so that clicking send would first remove the highlighting, but Gmail didn't wait, it triggered an AJAX request immediately to send the email. I needed more brain power. Fortunately, my colleague Manish Kakwani had an afternoon free, so he and I spent it trying out different ideas.

We found that Gmail.js had a way for us to hook into the AJAX request after it was created but before it was sent. We used this to strip out the highlighting.

Perfect. We were done with the MVP in 3 days. We told our slackbot Gary to "ship it" and he replied with [Tyler Edlin's epic picture of Chewbacca riding a giant squirrel fighting nazis](https://www.deviantart.com/tyleredlinart/art/comission-fur-on-fur-172151625):

![Chewbacca riding a giant squirrel fighting nazis](/img/justnotsorry/tyler.webp)

Maybe that was a sign.

I published a test version of the plugin on the Chrome Web Store so that Tami and others at Cyrus could try it out. Quickly, Tami's response came back: "the plugin crashes my Gmail."

Some investigation found that the [Salesforce IQ](https://web.archive.org/web/20160312035305/http://www.salesforce.com/salesforceiq/overview/) Chrome extension that she also had installed had a conflict. The issue was the Gmail.js feature that allowed us to hook into the AJAX request. To do this, Gmail.js modifies the [XmlHttpRequest](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) object to use some of its own custom functions. Salesforce IQ seemed to be trying to do the same thing and the two were not playing nicely.

Back to the drawing board for the functionality for stripping out the highlighting. I changed the code to use the other approach that Manish and I had come up with but rejected as being less reliable: remove the highlighting when the compose window loses the focus. I tested the new approach and it seemed to work well.

At about the same time, Kerri, our director of finance, reported another issue: if she edited a highlighted word, for example changing "just" to "justify", the highlighting would remain and extend to the entire word.

The simplest solution was to be more aggressive about clearing the highlighting, clearing and re-adding the highlighting each time a character was typed. This worked great, except it was inefficient and had the side effect of suddenly moving the cursor position to earlier in the email text when a highlighted word was edited.

Here's why: The cursor position in an html web page is stored on the [selection](https://developer.mozilla.org/en-US/docs/Web/API/Window/getSelection) property of the window. It holds a [reference to a DOM node](https://developer.mozilla.org/en-US/docs/Web/API/Selection/anchorNode) and a [number of characters offset](https://developer.mozilla.org/en-US/docs/Web/API/Selection/anchorOffset). When we cleared the highlighting, we were deleting `SPAN` nodes in the DOM and this caused the browser to recalculate the selection property to use a parent node which reset the cursor position.

![The JustNotSorry cursor bug](/img/justnotsorry/jns-bug.webp)

To fix this issue, I changed JustNotSorry to keep track of the cursor position and then each time the highlighting was cleared, to set the position to the correct value. Getting and setting the cursor position in a contenteditable is complicated but some googling led me to the [Caret.js](https://ichord.github.io/Caret.js/) library by Harold Luo. Caret.js supported getting the cursor position for contenteditable but setting the cursor position hadn't been implemented yet. There was some discussion about how to do it on the project's issue tracker, so I started with those ideas, implemented the missing functionality, and submitted a [pull request](https://github.com/ichord/Caret.js/pull/44) to add the functionality to the project.

Sorting this last issue out took a few days, but now the JustNotSorry MVP was good enough to release to the public.

It wasn't perfect. I wasn't happy with the clearing and re-adding of the highlighting each time a character was typed or with the way the selection had to be saved and reset.

But an MVP doesn't have to be perfect, it needs to work well enough for you to learn whether there's enough interest in your idea to merit investing more time and money into it.

---

On a related note, I've sometimes heard people say that they're moving fast and the MVP is probably going to be throw away code, so they don't need to spend time writing tests. This sort of blanket thinking is dangerous and can end up hurting your speed and your customer's experience. When you're building an MVP, you want to write tests where the benefit outweighs the cost. Some types of code are faster to write when you test-drive them and test-driven code has fewer bugs, which makes for a better user experience.

In the case of JustNotSorry, there were two areas where tests made sense: the logic to highlight particular words and the logic to set the cursor position. Both of these areas had to cover many different scenarios and edge cases, making them hard to test manually.

On the other hand, the logic to monitor when a compose window opened or when you're typing was straightforward and easy to test in the browser. Further down the road, it might make sense to invest in tests for these areas, especially if more logic needed to be added, but for the MVP, I chose to test these areas manually.

---

We launched JustNotSorry at the end of December 2015, with a call to action for people to write better emails in 2016. It quickly found an audience and got great media coverage. We had over 100k downloads in the first month.

![JustNotSorry press](/img/justnotsorry/jns-press.webp)

There's more work to be done to address the feedback from our users, but it was definitely a successful MVP.

If you'd like help building a lean MVP for your product, come talk to Cyrus.

*Update: As of 2016, [Cyrus Innovation](https://web.archive.org/web/20150319174934/http://www.cyrusinnovation.com/) became part of [Def Method](https://defmethod.com) and the folks at Def Method have continued to maintain JustNotSorry.*

*Originally published on Medium [here](https://medium.com/@sbrudz/building-the-justnotsorry-mvp-d1be01ce2f22).*