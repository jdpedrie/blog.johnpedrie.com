---
layout: post-no-feature
title: "It's Time to Fix WordPress Tooling"
description: "WordPress development is a mess. Everyone still uses WordPress. Where do we go from here?"
category: articles
tags: [composer, php, wordpress, package manager, software development]
comments: true
---

The last several years have seen something of a revolution in the world of PHP application development. The PHP community has professionalized itself immensely through better development practices, improved tooling, and language improvements. The advent of PHP-FIG brought about an unprecedented level of standardization and interoperability. Composer brought package management on a scale we'd witnessed in other languages but often saw as little more than a pipe dream in the world of PHP.

Despite these improvements, or perhaps highlighted by them, many legacy applications have lagged behind. Resistance to changes seen as damaging to backwards compatibility has held back many well-known open source projects. WordPress has been no exception.

No one can dispute the success WordPress has seen over the last decade. But with this success has come challenges. The team charged with maintaining a project which is used by over 20% of the internet is understandably concerned with preserving backwards compatibility. Simply for this reason, it is hard to greatly fault the WordPress team for the state of its codebase and the slow rate at which it has improved and modernized.

The goals of the WordPress core team are different from mine. They are concerned with maintenance and iterative improvement in features and user experience. I am far more interested in building a solid foundation on which to build fun stuff. I want access to modern development tools. I want to use Composer packages. I want to store my project in source control in an effective manner than allows for pain-free updates. I want to follow [a code style guide that is accepted by the majority of the community](https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-2-coding-style-guide.md) rather than [a proprietary one that is used only in the WordPress world](http://make.wordpress.org/core/handbook/coding-standards/php/).

Over the next [undetermined period of time], I'll be documenting some of the work I've been doing in attempting to achieve this goal. I hope some of it may be useful to others.

I found myself in a position where I was working on WordPress at work, and felt incredibly constrained by software that shows its age quite acutely. My project over these past months has been an attempt to make WordPress accessible to professional, enterprise PHP developers. People who want to use modern tooling, but who also don't want to close themselves off from the huge market for bespoke WordPress development.

In my next article, I'll be discussing installing and maintaining WordPress through Composer. Feel free to check out what I'm going to talk about at the [github repository](https://github.com/jdpedrie/wp-composer).