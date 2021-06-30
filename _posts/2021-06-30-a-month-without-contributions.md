---
layout: post
title: 'A month without contributions'
permalink: posts/a-month-without-contributions
---

[EasyAdmin](https://github.com/EasyCorp/EasyAdminBundle) is a popular admin
generator for [Symfony](https://symfony.com) applications. It's an open source
project with [millions of downloads](https://packagist.org/packages/easycorp/easyadmin-bundle/stats)
but it hasn't allowed a single contribution for the past 30 days. Nobody could
create an issue or pull request during the past month. Keep reading to know why.

Open Source is asymmetric
-------------------------

In Open Source projects there's usually 2 to 4 orders of magnitude in the number
of consumers and contributors (1 person will contribute for every 100 to
10,000 people who use the project).

This difference happens in all kinds of projects. Symfony for example is the
backend project [with most contributors](https://symfony.com/blog/symfony-was-the-backend-framework-with-the-most-contributors-in-2019)
in the entire world and it just has a few hundred [contributors](https://symfony.com/contributors),
compared to the [hundreds of thousands](https://symfony.com/what-is-symfony) of
developers who use it.

This difference is specially big in EasyAdmin. While the number of downloads
keeps rising significantly, contributions are lacking. In addition, most
contributions are just bug reports instead of pull requests fixing those bugs.
Sadly, many bug reporters expect me to fix everything without their help.

Don't get me wrong, I'm lucky because EasyAdmin community, like the rest of the
Symfony community, is very kind and polite, specially
[our sponsors](https://github.com/sponsors/javiereguiluz). However, too many
people demand me to fix their bugs and too few people offer any help.

EasyAdmin is not my work
------------------------

I store all my coding projects in two folders: `/Users/javier/work/` and
`/Users/javier/projects/`. This helps me better differentiate my paid work and
my side-projects and Open Source contributions. EasyAdmin code is stored in
`projects/`, so it's not my work. I spend time on it (several thousands of hours
so far) because I enjoy it and I'm very glad that it's useful to others.

However, the fact that most "contributions" are just bug reports means that
most of my contributions are fixes for those issues. This is making my EasyAdmin
experience miserable. Instead of a fun project, it feels like a boring
(and unpaid) job.

Also, the stream of bug reports and feature requests is not sustainable. When I
fix a bug, we quickly receive 5 new bug reports. And when I fix 5 bugs, we
receive 10 more, and so on. This must stop.

You need silence to focus
-------------------------

Have you ever tried to do some work which required to focus while some kids are
shouting, fighting and screaming in the background? You can't. You need silence
(or some specific kind of generated noise or music) to focus on your work.

That's why I decided to stop all contributions in EasyAdmin repository for 30
days (you can do that in GitHub -> Your Project -> Settings -> Moderation Settings
-> Interaction limits). I needed that long period of silence for my own mental health.

During this time, I've fixed many issues and completed some important tasks such
as the migration to Bootstrap 5 (and the removal of jQuery). All this was possible
because I no longer had to read things like "I have this bug, fix it!",
"Please take a look at my problem", "Any updates about this?", etc.

I don't work for you
--------------------

This "no contributions" experience has been a success for me and the project.
However, this is an open source project, so you need to allow user
contributions at some point. That's why **contributions are open again** for
EasyAdmin, but with some changes.

I will no longer fix bugs that don't affect me. I will fix bugs that affect my
applications in addition to obvious bugs and security bugs. That's all. This is
important because **I don't work for you** or your company. I can **work with you**
to solve the issues that you are experiencing, but you need to do most of the work.

I hope these changes help us move the project forward in a sustainable way.
I also thank all of you who contribute in one way or another to this project.
If you or your company are looking for a quick way of contributing, consider
[sponsoring me on GitHub](https://github.com/sponsors/javiereguiluz). Thanks!
