---
layout: post
title: 'EasyAdmin 4 Released'
permalink: posts/easyadmin-4-released
---

EasyAdmin 4, the latest version of the fast and beautiful admin generator for
Symfony applications, is now available. Even if it's a major release, most
projects can upgrade from EasyAdmin 3.x without making a single change.

EasyAdmin 4.x doesn't include any new feature different from the ones included
in the recent 3.x releases. However, it increases the minimum requirements to
PHP 8.0 and Symfony 5.4. We had to do this because we couldn't make EasyAdmin 3.x
compatible with Symfony 4.x, 5.x and 6.x.

In summary:

* If you can't upgrade to PHP 8 or Symfony 6.x, keep using EasyAdmin 3.x.
  You won't get new features, but you'll keep getting most bug fixes.
* If you are already using PHP 8 or Symfony 5.4/6.0, upgrade to EasyAdmin 4.x
  as soon as possible. The upgrade will be smooth and you'll get all the great
  new features that we'll start adding to EasyAdmin 4.x.

A minor but noticeable change in EasyAdmin 4.x is the new "Welcome Page", which
is what you see by default when generating a new EasyAdmin dashboard with the
command `php bin/console make:admin:dashboard`

![EasyAdmin 4 Welcome Page](images/easyadmin-4-welcome-page.jpg)

A major feature removed in this version is the command that upgraded the YAML
files used to configure EasyAdmin 2 into the PHP files used by EasyAdmin 3. If
you are upgrading from EasyAdmin 2, upgrade first to 3.x version, generate the
PHP files automatically and then, keep upgrading to 4.x.

Read the [EasyAdmin 4 documentation][1] to learn more about the changes and
features of this version.

[1]: https://symfony.com/bundles/EasyAdminBundle/4.x/index.html
