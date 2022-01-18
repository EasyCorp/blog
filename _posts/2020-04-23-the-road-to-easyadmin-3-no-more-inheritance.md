---
layout: post
title: 'The Road to EasyAdmin 3: No More Inheritance'
permalink: posts/the-road-to-easyadmin-3-no-more-inheritance
---

[EasyAdmin](https://github.com/EasyCorp/EasyAdminBundle) is an admin generator
for Symfony applications. EasyAdmin 3 is the new version that will be released
soon. This article is part of a series that explains some of its biggest changes
and the reasoning behind them.

-----

Some programming languages allow to define [non-subclassable classes][1] using
keywords such as `sealed` and `final`. Compiled languages (Java, C#, etc.) do
this mainly to allow aggressive optimizations while compiling code. In PHP this
is mostly done to minimize the backward-compatibility breaks.

Any class that can be extended creates an "extension point" in your application.
Users will assume that they can extend your classes to modify some of their
methods. This provides lots of freedom to your uses, but will make your project
more difficult to evolve because you can't change those classes without breaking
users code.

Should all classes be final?
----------------------------

Marco Pivetta published an article titled [When to declare classes final][2]
where he defends that most/all classes should be final by default if they define
interfaces. However, in the PHP world there are two main philosophies.

Taylor Otwell, creator of the Laravel framework, prefers to never use `final`
and advocates against it. His reasoning is that if you use some non-final class
and the project changes it, it probably won't take you much time to fix your code:

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">I really hope <a href="https://twitter.com/symfony?ref_src=twsrc%5Etfw">@symfony</a> does not continue a trend of making things &quot;final&quot;. Absolutely *zero* benefit besides assuaging the superstitions of PHP developers. We are all consenting adults. Mark it as <a href="https://twitter.com/Internal?ref_src=twsrc%5Etfw">@internal</a> or something if you want to indicate you should proceed with caution.</p>&mdash; Taylor Otwell 🪐 (@taylorotwell) <a href="https://twitter.com/taylorotwell/status/1237053249703854080?ref_src=twsrc%5Etfw">March 9, 2020</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

Symfony follows the opposite philosophy and prefers to make everything `final`
by default (unless there are some reasons to make something an "extension point").
However, Nicolas Grekas, the most active Symfony code contributor, agreed with
Taylor and argued that using the `@final` annotation instead of the `final`
keyword may be enough to warn users without taking their freedom to extend:

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">I personally prefer the `<a href="https://twitter.com/final?ref_src=twsrc%5Etfw">@final</a>` annotation because like you said ppl are grown up. Expressing the intend is way enough: &quot;BC not guaranteed if you extend this, but you can still do it&quot;.<br>We already did some switches back from keywork to annotation. Some as bug fixes. ;)</p>&mdash; Nicolas Grekas (@nicolasgrekas) <a href="https://twitter.com/nicolasgrekas/status/1237058162617966594?ref_src=twsrc%5Etfw">March 9, 2020</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

What should we do in EasyAdmin?
-------------------------------

My position in the *"final vs non-final"* debate is very clear: **I don't know what to do**.
Both sides have strong points. This reminds me of Microsoft vs Apple: Microsoft
keeps backward compatibility for decades whereas Apple breaks it often. None is
wrong and both companies are wildly successful.

A thing to keep in mind before making a decision is that EasyAdmin 3 is a
complete rewrite of EasyAdmin 2. We may have introduced serious problems or
shortcomings in the new architecture, so giving complete freedom to users too
soon may be counterproductive for the project.

Considering all this, I opted for the most conservative decision. If you start
with "non-final", you cannot make it "final" later without making people angry.
If you start with "final", you can keep it or turn some/all classes into "non-final"
later if needed. That's why **all classes are final in EasyAdmin 3** (for now).

The only exceptions are a few abstract classes defined to simplify the creation
of dashboard and CRUD controllers (and they are optional, because you can
implement the related interface instead of extending the abstract class). In the
coming months, after EasyAdmin 3 is released as stable and developers start
using it in real applications, we'll revisit this _"all classes are final"_ decision.

Contracts
---------

Symfony introduced a [Contracts component][3] to define a set of abstractions
extracted out of the Symfony components. Most of these abstractions are PHP
interfaces implemented by other Symfony components.

Following the same idea, EasyAdmin 3 defines multiple contracts in the
`EasyCorp\Bundle\EasyAdminBundle\Contracts` namespace. This way we can group all
our interfaces in a single place, making it easier to find and use them.
Hopefully these contracts will be enough to make the project usable and extensible
even if all classes are "final".

[1]: https://en.wikipedia.org/wiki/Class_(computer_programming)#Non-subclassable
[2]: https://ocramius.github.io/blog/when-to-declare-classes-final/
[3]: https://symfony.com/doc/current/components/contracts.html
