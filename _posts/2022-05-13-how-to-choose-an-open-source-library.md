---
layout: post
title:  "How to choose an open-source library"
author: Steve Brudz
date:   2022-05-13
---
![A person confused by having so many libraries from which to choose](/img/choosing-a-library.png)

Open-source libraries are essential to creating today's software efficiently. Most applications are built upon a core framework and multiple open-source libraries, each of which might depend upon multiple other open-source libraries. Most languages have an associated repository of open-source libraries. [npm](https://www.npmjs.com/) alone has over 1 million packages published to it. With so many options to choose from, how do you pick the best one for your project?

### The Problem Statement

First and foremost, define what problem you're trying to solve by using a library.

If you're working as part of a team and the choice will impact a significant portion of the application, such as a UI component library (do I choose [Bootstrap](https://getbootstrap.com/) or [Ant Design](https://ant.design/)?) or an authentication library ([Auth0](https://auth0.com/), [Okta](https://www.okta.com/), or [Firebase](https://firebase.google.com/)?), then take the time to write up a short document that states the problem you need to solve and lists out any constraints. Then evaluate at least three different options and discuss the results with your teammates.

For lower impact decisions that are isolated to a particular layer of your application or development stack, such as test helpers like [jest-dom](https://github.com/testing-library/jest-dom), then you probably don't need to be so formal as to write up a document, but many of the criteria noted below will still be useful to consider.

If the task is something small, such as a library for splitting an array into pairs, ask yourself whether you really need to be using a library in the first place.

Each outside library that you incorporate into your project brings a maintenance burden with it. It'll increase the size of your application. Other developers working on the project will need to learn how to use it. You'll need to update it regularly to incorporate security fixes. The library itself will likely have its own dependencies, which your project now inherits.

### Searching for a Library

To find good open-source libraries, I recommend starting with a keyword search on Google. There are other search options out there, such as DuckDuckGo for the privacy conscious, but I've found that for programming-related searches, Google tends to return the best results.

Pick out some keywords that are applicable to your problem domain and your language/technology stack. For example, "node auth middleware" or "flutter chart library". You'll generally get a mix of stack overflow posts, blog tutorials, and libraries. There might also be some garbage results, such as scraper sites that repost stack overflow content--avoid these. You might need to try a few different keyword combinations to get the results you're looking for.

Open the links for the search results that look promising. Usually, the first page or two of results is enough to review. Skim the content and look for trends. Often the same few libraries will be mentioned in multiple places. Put these on your shortlist to evaluate further.

Another option is to use the search functionality for the package repository associated with the language you're using, such as npm for JavaScript or [rubygems](https://rubygems.org/) for Ruby. Searching package repositories will often return large numbers of results with limited context. I find this sort of search to be helpful if I know what I'm looking for, but not as good as Google for exploring possibilities.

If there don't seem to be many choices, try using Google's autocomplete feature to find other options. To do this, type a library name into the search box and then add "vs" after it, such as "flutter vs", and then look at the autocomplete results. This can often reveal other similar libraries to look at. Choosing one of the autocomplete results will usually articles that compare the technologies to each other, which can be helpful.

### Evaluating a Library

#### Does it do what you need?

Compare the features of the library against the requirements for the problem you're trying to solve. How does it stack up? Does it check off the must-haves?

Some libraries (e.g., [prettier](https://prettier.io/), [semantic-release](https://github.com/semantic-release/semantic-release)) are very opinionated in the way they do things, often times for good reason. If an opinionated library feature conflicts with one of your requirements, it's worth considering if there's any flexibility in that requirement.

Other times a library could present a different approach to solving a problem than you'd considered originally. On one project a few years back, I remember searching for a library that would provide workflow management functionality and instead coming across a library that provided an easy way to add finite state machine functionality, which turned out to be a better way to approach the problem at hand.

#### How good is the documentation?

Skim the documentation. Can you easily tell the purpose of the library? How easy does it seem to install and use it? Are there guides and examples? Is there a separate detailed API section to the documentation? Is there a way to view previous versions of the documentation so that you can use the library if you're on an older release?

Keep an eye out for type-o's or inconsistencies in the documentation. These are a potential sign that the documentation is outdated or inaccurate, which can make it frustrating to work with the library.

#### How active is the project?

Not all open-source libraries are created equal. Some are well-crafted and maintained by experienced professionals (e.g., [React](https://reactjs.org/)); others are experiments that a developer tried out while learning; and still others were once heavily used but have now been sunset (e.g, [PhantomJS](https://github.com/ariya/phantomjs/issues/15344)).

Check the last commit date and the number of recent commits to see how actively maintained the library is. Some libraries are so stable they don't need many changes (e.g., [lodash](https://github.com/lodash/lodash)), but most should be regularly updated with security patches, fixes, and additions. If there's not much recent activity, this is a sign that the library may be abandoned.

Look at the open issues in the bug tracker. Are there a lot? Are people complaining about a lack of updates? This can be a major warning sign. Similarly, many open pull requests can be a signal that the library isn't receiving much attention.

The converse can also be true: some libraries evolve so rapidly that it can be challenging to keep up with them. This is especially relevant in the JavaScript world, where every week there seems to be a hot new library.

A great example is the [Mantine](https://mantine.dev/) React component library, which started in early 2021 and just over a year later hit its version 4.0 release with 10k stars on GitHub. I tried it on a side project after it hit the 1.0 release and liked working with it, but within a month the 2.0 release was out with major new features and breaking changes. All the new functionality was great, but I struggled to keep up--I wanted to spend my time extending the project; not upgrading my UI library every week.

My advice for libraries like this is to assess the cost-benefit balance. If the library provides the features you need and the development is being done in a professional manner with releases that use [semantic versioning](https://semver.org/), then it might be worth the cost of upgrading frequently. Otherwise, consider holding off until the library stabilizes and the release rate slows down.

#### Other criteria to look at

How popular is the package? The number of GitHub stars or the number of downloads can be indicators that signal that a library is well-maintained or high quality.

Are there code quality measures in place? There should be automated tests that are run via a continuous integration build. If not, there's a high risk that the library will be buggy.

Is the library backed by a company (like Meta/Facebook or Google) or does it use [Tidelift](https://tidelift.com/) or [Open Collective](https://opencollective.com/) for funding? These are all promising signs that the library will be around for a while.

Is there a code of conduct & instructions for contributors? Do they honor their contributors by including them in the readme (or elsewhere)?

Is there a roadmap?

Do they tag each release with a version? Do they include release notes? Upgrade notes?

What open-source license is it using? Check to make sure that it's [compatible](https://en.wikipedia.org/wiki/License_compatibility) with the code that you're working on. This can be a tricky subject area, so if you're in doubt, ask someone with more experience. As a general rule of thumb, MIT licensed libraries are safe to use most places. Apache licensed libraries are similar with some additional restrictions. For GPL and LGPL licenses, you'll need to evaluate your specific circumstances.

If you're working in a larger, enterprise environment, make sure to find out what standards are in place for use of open-source software. Some organizations require open-source libraries to be reviewed by an architecture group before they can be used. Others don't allow the use of open-source software at all.

#### Try them out

Once you've come up with a list of viable candidates, give each of them a try. Install the library and try out some example code.

Does it install without errors? Does it behave in the way you expected? If you try an example from their documentation, does it work as described? If the answer to any of these questions is "No", then it's a potential red flag to keep in mind.

Note, when installing a library, make sure that you're using the official version as noted in the documentation--be careful of [type-o squatting](https://arstechnica.com/information-technology/2020/04/725-bitcoin-stealing-apps-snuck-into-ruby-repository/).

### Final Considerations

If you're building an application that needs to last, make sure that the building blocks you're using are stable and long-lasting as well. There's no guarantee with open-source software, so it's important to do your research before committing to a library.

Most open-source projects are a labor of love that people work on in their spare time. For popular projects it can be [difficult](https://nolanlawson.com/2017/03/05/what-it-feels-like-to-be-an-open-source-maintainer/) for volunteer maintainers to keep up with the workload. If you do find a library useful and especially if you're making money from it, donating some money or your time to the library will help ensure that it will stay around.

All that said, sometimes you just want to experiment with something new. In that case, ignore the points above and try it out. Maybe that new library with 2 GitHub stars is good enough to bloom into something that many people will want to use.

*Thank you to [Rob Bridges](https://medium.com/u/31528dfdca2f) for providing feedback on this article.*

*Originally published on Medium [here](https://medium.com/@sbrudz/how-to-choose-an-open-source-library-c19f260e06ad).*
