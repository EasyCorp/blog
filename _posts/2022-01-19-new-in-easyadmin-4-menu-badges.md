---
layout: post
title: 'New in EasyAdmin 4: Menu Badges'
permalink: posts/new-in-easyadmin-4-menu-badges
---

[EasyAdmin 4.0.2][1] has been released and it includes one of the most requested
features by the community: **menu badges**. These badges are commonly used to
display some numeric value next to menu items (e.g. number of notifications,
number of new clients, number of invoices pending to be paid, etc.)

As explained in the docs about [menu item configuration options][2], these badges
are configured with the `setBadge()` method. For example, to show the number of
comments pending to be reviewed:

```php
namespace App\Controller\Admin;

use EasyCorp\Bundle\EasyAdminBundle\Config\MenuItem;
use EasyCorp\Bundle\EasyAdminBundle\Controller\AbstractDashboardController;

class DashboardController extends AbstractDashboardController
{
    public function __construct(private CommentRepository $comments)
    {
    }

    public function configureMenuItems(): iterable
    {
        // ...

        // get the number of pending comments somehow; this is a regular Symfony
        // controller, so you can for example call to your entity repositories
        $numPendingComments = $this->comments->getNumPendingReview();
        yield MenuItem::linkToCrud('Comments', 'far fa-comments', Comment::class)
            ->setBadge($numPendingComments);
    }

    // ...
}
```

The default design of menu badges is similar to what you can find in other apps
and websites:

<a href="{{site.url}}/images/easyadmin-4-menu-badges.png" target="_blank">
    <img src="{{site.url}}/images/easyadmin-4-menu-badges.png" alt="Default design of menu badges in EasyAdmin 4" />
</a>

Even if badges usually display numeric values, you can pass to `setBadge()`
any numeric or string variable (including *stringable* objects). In addition,
you can fully customize the design of menu badges:

```php
// the second argument can be any of the predefined Bootstrap styles:
// `primary`, `secondary`, `success`, `danger`, `warning`, `info`, `light`, `dark`
yield MenuItem::linkToCrud('...', '...', '...')
    ->setBadge($numPendingComments, 'warning');

// if none of the default styles fits your backend, pass the contents of the
// HTML `style` attribute as the second argument:
yield MenuItem::linkToCrud('...', '...', '...')
    ->setBadge('NEW', 'background: transparent; color: red; outline: 2px solid red');
```

The following screenshot shows a backend with several menu badges with custom design:

<a href="{{site.url}}/images/easyadmin-4-menu-badges-custom.png" target="_blank">
    <img src="{{site.url}}/images/easyadmin-4-menu-badges-custom.png" alt="Menu badges in EasyAdmin 4 with customized design" />
</a>

[1]: https://github.com/EasyCorp/EasyAdminBundle/releases/tag/v4.0.2
[2]: https://symfony.com/bundles/EasyAdminBundle/4.x/dashboards.html#menu-item-configuration-options
