---
layout: post
title: 'A new EasyAdmin Demo application'
permalink: posts/a-new-easyadmin-demo-application
---

[Symfony introduced its Demo application][1] in 2015 as a learning resource to
showcase the main features of the Symfony project. We've been maintaining it
since then and today it's still a great resource to learn Symfony, to prepare
some quick proof of concept and to benchmark new features.

Some time later, and inspired by Symfony Demo, we introduced the EasyAdmin Demo
application to showcase the main [EasyAdmin][2] features. Sadly, I couldn't
find the time and energy to maintain the project. This damages the reputation of
the entire EasyAdmin project and causes frustration to some users.

That's why we've decided to **drop the entire EasyAdmin Demo application** and
archive [its repository][3]. However, we still need to showcase EasyAdmin
somehow because we want users to see it in action. If only there was a sample
Symfony application that we could use for this...

Forking Symfony Demo
--------------------

Symfony Demo is the perfect application to solve this problem. It's a simple
but realistic Symfony application that follows the latest and greatest best
practices defined by Symfony. That's how we created **the new EasyAdmin Demo**:
forking Symfony Demo and creating an EasyAdmin backend on top of it.

This is how it looks:

<img src="{{site.url}}/images/easyadmin-demo-index.png" alt="Homepage of the EasyAdmin Demo application" />

Symfony Demo includes its own custom-made backend, so you can compare how to
create backends with EasyAdmin and with pure Symfony code.

Besides providing an entire backend to manage the Symfony Demo application,
we've included other resources, such as a "form field reference". This renders
in a single page all the available form fields in EasyAdmin backends, so you
can check how they look and feel:

<img src="{{site.url}}/images/easyadmin-demo-field-reference.png" alt="Full reference of form fields available in EasyAdmin" />

The new EasyAdmin Demo project is available at [github.com/EasyCorp/easyadmin-demo][4].

Doing Less to Do More
---------------------

Using Symfony Demo as the base of the EasyAdmin Demo will allow us to offload
most of the maintenance burden to the Symfony community. This will liberate us
to work on adding and improving EasyAdmin features.

Finally, in case you are wondering if we're going to submit these changes to the
Symfony Demo repository: **we'll never do that**. Symfony Demo must stay
agnostic and not favor any of the available admin generators for Symfony
applications (mostly, [SonataAdmin][5] and [EasyAdmin][2]).

<div class="message">
    👋 If EasyAdmin is useful for you or your company, please consider
    <a href="https://github.com/sponsors/javiereguiluz">sponsoring me on GitHub</a>.
</div>

[1]: https://symfony.com/blog/introducing-the-symfony-demo-application
[2]: https://github.com/EasyCorp/EasyAdminBundle/
[3]: https://github.com/javiereguiluz/easy-admin-demo
[4]: https://github.com/EasyCorp/easyadmin-demo
[5]: https://github.com/sonata-project/SonataAdminBundle
