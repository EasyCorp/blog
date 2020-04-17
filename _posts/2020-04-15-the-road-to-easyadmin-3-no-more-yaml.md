---
layout: post
title: 'The Road to EasyAdmin 3: No More YAML'
permalink: posts/the-road-to-easyadmin-3-no-more-yaml
---

[EasyAdmin][1] is an admin generator for Symfony applications. EasyAdmin 3 is
the new version that will be released soon. This article is part of a series
that explains some of its biggest changes and the reasoning behind them.

-----

One of my favorite excerpts from the famous "Clean Code" book by Robert C. Martin
states that:

> The ratio of time spent reading code vs. writing code is well over 10:1.
> We are constantly reading old code as part of the effort to write new code.
> [That's why] we want the reading of code to be easy, even if it makes the writing harder.

Following the same analogy, our biggest mistake with EasyAdmin 2 was to optimize
it for creating backends quickly, instead of optimizing it for backend updates
and maintenance. **EasyAdmin 3 fixes this mistake** making a bit harder to
create backends but much easier to update and maintain them.

In EasyAdmin 2 backends were generated dynamically using some configuration
written in YAML. You could add some PHP code for advanced and dynamic features,
but most of the backend was YAML config transformed into PHP code in runtime.

**In EasyAdmin 3 backends are defined exclusively using PHP code**. You can't
use YAML in any part of EasyAdmin. We even removed the `symfony/yaml` dependency
from `composer.json`.

This may scare some of you and think that EasyAdmin 3 will force you to write
long and boring PHP code. Don't worry about that. The needed code is not verbose
and everything is autocompleted by your IDE, so you don't really need to
memorize any config option or method name.

Backwards Coding
----------------

Unlike most developers, for complex features **I prefer to code backwards**.
That means defining how the end-user code will look like and once I'm happy with
that, going backwards to implement the code that makes it possible.

I took this idea to the extreme for EasyAdmin 3. The development process was as
follows:

* First, I wrote the entire docs for it ([see the doc commit](https://github.com/EasyCorp/EasyAdminBundle/pull/2967/commits/384a82639b6a17679f0021e9efac3730e7cd819d));
* Then, I wrote the code needed to make it work like that (you can see the (hundreds of) commits in the [master branch](https://github.com/EasyCorp/EasyAdminBundle/commits/master));
* Once the code was ready, I used it to build a real application. This allowed me to find some things that didn't work nice for end users;
* Finally, I updated the code for those changes and updated the docs too ([see the doc commits](https://github.com/EasyCorp/EasyAdminBundle/pull/2967/commits)).

Simple Upgrading
----------------

The idea of having to transform a long and complex EasyAdmin 2 YAML config into
the PHP files needed by EasyAdmin 3 may seem boring and daunting. That's why
EasyAdmin 3 includes a command called `make:admin:migration` which does that for
you.

The command can't convert 100% of your current config, but it will convert most
of it, so you'll save a lot of time when upgrading.

[1]: https://github.com/EasyCorp/EasyAdminBundle
