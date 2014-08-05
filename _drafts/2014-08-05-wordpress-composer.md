---
layout: post-no-feature
title: "Managing WordPress with Composer"
description: "Install and update WordPress and your themes and plugins with PHP's package manager."
category: articles
tags: [composer, php, wordpress, package manager, software development]
comments: true
published:false
---

This article will discuss a few strategies that allow for installing and update WordPress, any theme and any plugin you want through the Composer package manager. We'll also look at how you can utilize Composer packages in WordPress plugin development.

WordPress has built in installers and updaters for core, themes and plugins. In a mission-critical environment, we don't want any of that. So forget they exist.

Forgotten?

Good.

I wrote last week about what I see as [the sorry state of WordPress development tooling](http://blog.johnpedrie.com/articles/wordpress-tooling/). I believe that ditching the built in installers and using a package manager is a huge step towards fixing that.

### Tools Needed

* Composer offers a multitude of different [installers](https://github.com/composer/installers) that allow us to install packages to different paths. We'll be using `wordpress-theme`, `wordpress-plugin`, and `wordpress-mu-plugin` in this article.
* The guys over at [Outlandish](http://outlandish.com/) maintain a mirror of the WordPress theme and plugin repository called [WordPress Packagist](http://wpackagist.org). By adding WordPress Packagist to our `composer.json` file, we can install any theme or plugin found in the WordPress repository.











