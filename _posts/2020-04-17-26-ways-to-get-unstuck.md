---
layout: post
title:  "26 Ways to Get Unstuck in Software Development"
author: Steve Brudz
date:   2020-04-17
description: "Everyone gets stuck sometimes. As a senior engineer and consultant, part of my job is helping others to get unstuck. This article pulls together and shares techniques that I’ve learned over the years for getting unstuck."
image: /img/stuck-in-web.jpeg
---
![A fly stuck in a spiderweb](/img/stuck-in-web.jpeg)
*Photo by [zibik](https://unsplash.com/@zibik) on [Unsplash](https://unsplash.com)*

Anyone who's written software has been there: The code you've written looks correct, it should work and yet it doesn't. You've been pulling your hair out and cursing for three days, trying everything you can think of until you try that one thing that makes it work. The fix itself is often simple, sometimes as little as a type-o or a missing semicolon. Other times, it's due to a bug in a library you're using, so it's not even your fault and all it took was upgrading to a new version. But you still just wasted days of your time and your project manager and teammates are annoyed at you for taking so long.

Everyone gets stuck sometimes. As a senior engineer and consultant, part of my job is helping others to get unstuck. This article pulls together and shares techniques that I've learned over the years for getting unstuck. I hope it helps other developers spend less time being frustrated. It's not intended to be a comprehensive list, so feel free to share techniques that have helped you in the comments.

#### 1. Search Google and/or Stack Overflow

[Google](https://www.google.com/), [Stack Overflow](https://stackoverflow.com/questions), and various public searchable forums can be great resources for finding the answer to a technical problem. There's so much information out there, though, that the key is to find the right search terms to get a relevant answer. Sometimes you can paste an error message into the Google search box, remove the ids specific to your environment, and get helpful results. Other times, using a few different keywords together, such as the language, library, OS, and function name can lead you to useful posts. If you find solutions on Stack Overflow, be cautious about copy-pasting whole blocks of code--sometimes the solutions offered work but don't follow best practices.

#### 2. Go for a walk

A short walk will give you some distance from the problem and help get your creative juices flowing. A [2014 study](https://www.ncbi.nlm.nih.gov/pubmed/24749966) by Oppezzo and Schwartz at Stanford showed that walking significantly increased participants' "creative divergent thinking," which is important for creative problem solving.

#### 3. Ask someone for help

Talk to your team lead, manager, or another developer. If you're working on legacy code, use a tool like [git blame](https://git-scm.com/docs/git-blame) to find out who wrote the code you're working on and ask them if they can help troubleshoot the issue. Consider posting a question on [Stack Overflow](https://stackoverflow.com/), but read the [guidelines](https://stackoverflow.com/help/how-to-ask) first and don't expect a fast answer.

#### 4. Talk to a duck

Also called [rubber duck debugging](https://en.wikipedia.org/wiki/Rubber_duck_debugging), the idea is that act of explaining the problem to someone else, or even to an inanimate object like a rubber duck, will help you to see the solution to the problem.

#### 5. Pair program on the problem

This technique overlaps with asking someone for help and talking to a duck, but it is useful enough that it deserves to be mentioned separately. Show someone else the code. Explain the problem to them. Walk them through what you've done so far. Sometimes the act of explaining the problem will help lead to the solution. Sometimes having another set of eyes or a different perspective will open up new possibilities that you hadn't considered.

#### 6. Get some sleep

If you're stuck, don't stay up all night working on the problem. It's tempting to keep plugging away and try "just one more thing". But much like free rounds of Jagermeister shots offered to college students at the end of the night, most people who try it aren't happy the next morning. Late at night you're more likely to introduce new bugs and the next day you'll be so tired your productivity will be much lower. It's much wiser to get some sleep and wake up early to work on it. A [2009 study](https://www.pnas.org/content/106/25/10130) by Denise Cai et al. at U.C. San Diego showed that REM sleep "enhances the integration of unassociated information for creative problem solving." There have been many times where I've gone to sleep with a knotty problem in the back of my mind and woken up the next morning with a fresh idea for how to approach it that turned out to be the key to solving it.

#### 7. Pare the problem down to its essentials

If you're working in a code base that has significant complexity or technical debt, sometimes the root cause of your problem can be obscured by the surrounding code. Try extracting the code that's relevant to your problem into a small example project. If you can get things working there, it's usually easy to port the change into your main code base.

#### 8. Try an experiment or alternate approach

There are usually many ways to approach a problem. Try putting your current attempt on a back-burner and starting again with a different approach. Make sure to use a [separate branch]({% post_url 2015-11-17-git-workflow-for-code-experiments %}) for your experiment.

#### 9. Write a test

Write a failing test that reproduces the problem and then change the code to fix the problem and make the test pass. This gets you improved code coverage and, once the test is green, confidence that you've actually fixed the problem.

#### 10. Add log statements to the code

Sometimes a few console.log or println statements are enough to find your issue.

#### 11. Read the source code

If you're using an open source library and the problem seems to be related to that library, open up the source code and read through it. This is the great power of open source code. Make sure to look at the version of the code that your app is using, though.

#### 12. Search for bug reports

Most libraries have bug or issue trackers; many times people will have reported a similar issue to what you're seeing. Even if the bug isn't resolved, you can often find a work-around in the comments.

#### 13. Read the documentation

Sometimes the solution is right there in the documentation, as an option or an example. For libraries that have evolved rapidly (such as [react-router](https://reacttraining.com/react-router/)), make sure you're looking at the docs for the correct version. For larger libraries, I like to skim the entire API document--sometimes the information you're looking for is there but called by a different name than you're expecting.

#### 14. Read the release notes

Most libraries document what's changed between versions. Some also include important notes about compatibility or bug fixes. Worth a look.

#### 15. Read the book

People tend to look online these days, but books often put techniques and best practices into a larger context. This can be especially useful when you're working with a set of technologies that are new to you.

#### 16. Try a different computer or environment

My co-worker Kevin and I once spent days tracking down the source of a test that failed on our CI server but didn't fail locally. It turned out that a difference between the Unix and Mac file systems was the culprit. [Docker](https://www.docker.com/) is great for eliminating environmental differences but even then remember that running a Docker container on your Mac or PC is still using the underlying file system.

#### 17. Use an existing library

If someone else has already solved the same problem or even a similar problem, try their solution out. Most languages have large repositories of open source libraries available for use ([npm](https://www.npmjs.com/) for JavaScript, [RubyGems](https://rubygems.org/) for Ruby, [PyPI](https://pypi.org/) for Python, etc.). Before investing too heavily in trying out a library, make sure to note what open source license it uses and determine if you're able to use it on your project. Some companies, especially those in heavily regulated industries, are very strict about what external libraries they allow developers to use.

#### 18. Write your own library

There are times I've struggled with integrating a large library into my code base, and I only needed to use a small part of the functionality provided by it. In these cases, it can be less work in the long run to write just the necessary logic from scratch. Sometime you don't need a massive library to solve your problem.

#### 19. Use git bisect

[git-bisect](https://git-scm.com/docs/git-bisect) is an incredibly useful tool for finding the specific commit in which a bug was introduced. First, find a commit where things fail and then find a commit where things work. Run git bisect and it will help you narrow down to exactly which commit introduced the problem.

#### 20. Use a debugger

A debugger allows you to stop your code's execution at a specific point and then step through the code line by line, inspecting the values of the variables. This can be hugely helpful, particularly if you're working in a dynamically typed language like [JavaScript](https://charlesagile.com/debug-series-nodejs-browser-javascript).

#### 21. Use a profiler

If you're trying to address a performance or memory issue, it can be tempting to tweak areas of the code that seem like obvious issues--nested loops, file system access. Without the data a profiler can provide, you're groping in the dark.

#### 22. Solve for the goal, not what you're given

Ever been assigned a ticket that specifies how to implement a solution and then when you go to implement it, you realize that what was specified just doesn't work? When that happens, it's time to take a step back, talk to the person who wrote the ticket, and find out what the goal of that ticket was. Then come up with your own solution that accomplishes the goal.

#### 23. Draw a sequence diagram

As Sandi Metz says, an object-oriented program is defined by the messages that objects pass between each other. Sequence diagrams help you see these messages. They can be useful both when writing new code and when trying to understand complex legacy code. [Sandi Metz's Practical Objected Oriented Design](https://www.poodr.com/) is a great resource for learning how to use sequence diagrams effectively.

#### 24. Look for a design pattern that fits

Design patterns are powerful and can be overused, but they're documented as patterns precisely because many people have found them useful. It's worth having them in your toolkit. Some useful resources include the classic [GoF Design Patterns](https://en.wikipedia.org/wiki/Design_Patterns) book, Martin Fowler's [articles on architecture](https://martinfowler.com/architecture/), Robert Nystrom's [Game Programming Patterns](https://gameprogrammingpatterns.com/), and Alexander Shvets' [refactoring guru](https://refactoring.guru/design-patterns).

#### 25. Use an APM solution

Application Performance Monitoring tools like [New Relic](https://newrelic.com/) and [Data Dog](https://www.datadoghq.com/) are great for diagnosing production issues. When properly configured, they allow you to see when errors occurred, slow queries, disk I/O, and memory usage across your entire stack.

#### 26. Look at the log files

Hopefully, the application is logging information with timestamps. This can be enough to pinpoint problems sometimes.

* * *

My final bit of advice is to be patient with yourself and don't get discouraged. For those who are just starting out in the craft, it can seem like really skilled developers never get stuck. Based on observations over the past 20+ years of my professional software development career, I can tell you that's not the case--everyone gets stuck sometimes. Skilled developers have just learned a variety of techniques for getting unstuck quickly in a variety of situations. Getting stuck doesn't make you a bad developer--it's a part of the craft, part of the joy and pain of turning ideas into tangible, working software. And when you do figure out how to solve that knotty problem, it feels great and motivates you to keep developing.

*Originally published on Medium [here](https://medium.com/@sbrudz/26-ways-to-get-unstuck-in-software-development-5836a49d765).*
