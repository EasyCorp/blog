---
layout: post
title: 'EasyAdmin 3 is released'
permalink: posts/easyadmin-3-is-released
---

**EasyAdmin 3 has just been released** as a stable version. After several months
of work, the new version is ready for new and existing projects.

EasyAdmin is an admin generator for Symfony applications. It helps you generate
the backend of your projects by solving all the repetitive stuff (listing data,
pagination, sorting, creating/updating entities, etc.)

A total revamp
--------------

EasyAdmin 2 was optimized for RAD (*rapid application development*). You
configured the backend using a YAML file and EasyAdmin used that config to
generate the backend on the fly.

EasyAdmin 3 is a complete rewrite of EasyAdmin 2. The entire code has been
written from scratch. EasyAdmin 3 is optimized for maintenance and everything is
configured in PHP (YAML is no longer used in any part of the bundle). Creating
backends take more time than in EasyAdmin 2 but in exchange, maintenance is much
simpler.

Using PHP exclusively brings in many other benefits. Your backend code is now
highly dynamic and can use all Symfony native features. Many of the previous
limitations no longer exist in EasyAdmin 3.

How to install
--------------

EasyAdmin 3 requires PHP 7.2 or higher and works with Symfony 4.4, 5.0 and 5.1.
Run this command to add it to your project:

```
$ composer require easycorp/easyadmin-bundle:^3.0
```

Then [read EasyAdmin 3 docs](https://symfony.com/doc/master/bundles/EasyAdminBundle/index.html).

How to upgrade
--------------

EasyAdmin 2 used just a YAML file whereas EasyAdmin 3 uses several PHP files.
Upgrading seems impossible, but it's actually pretty easy. Both EasyAdmin 2 and
3 include a command called `make:admin:migration` which transforms the YAML config
into PHP code.

The upgrade command is not perfect and cannot update 100% of your previous YAML
config, but it will save you a lot of time. [Read the upgrading article](https://symfony.com/doc/master/bundles/EasyAdminBundle/upgrade.html)
to learn more about the upgrade process.

Thank you
---------

Now it's time to celebrate 🎉 and thank to the [EasyAdmin contributors](https://symfony.com/doc/master/bundles/EasyAdminBundle/upgrade.html)
and to all the developers who use it and recommend it to others.
