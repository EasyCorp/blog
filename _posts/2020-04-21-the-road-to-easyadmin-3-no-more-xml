---
layout: post
title: 'The Road to EasyAdmin 3: No More XML'
permalink: posts/the-road-to-easyadmin-3-no-more-xml
---

[EasyAdmin](https://github.com/EasyCorp/EasyAdminBundle) is an admin generator
for Symfony applications. EasyAdmin 3 is the new version that will be released
soon. This article is part of a series that explains some of its biggest changes
and the reasoning behind them.

-----

EasyAdmin 2 followed [Symfony Best Practices][1] strictly: YAML for config, XML
for defining services, Twig for templates, XML for translations, etc. EasyAdmin
3 is a complete rewrite of the project, so we could revisit all previous
decisions. As explained in previous posts, in EasyAdmin 3 we [removed all YAML][2]
and we also [removed all PHP arrays][3]. The next step was to **remove all XML config**.

The problem of XML
------------------

[XML][4] is a nice markup language when used to transfer information between
machines. Related technologies such as [XSLT][5] make it even more powerful.
Nowadays there are some contenders such as [Protobuf][6], but you can't go wrong
with something as battle-tested as XML.

**Using XML for humans is a different story entirely**. Most developers I know
strongly dislike XML. I do too. I can't stand XML verbosity and ugliness.
Moreover, I find it more difficult to use on a daily basis.

Consider this simple YAML config from a Symfony application:

```yaml
framework:
    request:
        formats:
            jsonp: 'application/javascript'
```

What would be the equivalent config in PHP? As simple and obvious as you may think:

```php
$container->loadFromExtension('framework', [
    'request' => [
        'formats' => [
            'jsonp' => 'application/javascript',
        ],
    ],
]);
```

Now, what would be the equivalent config in XML? Let's forget for a moment about
the `container.framework.request` wrapper and let's focus on the `formats`
option. How would you define that in XML? Maybe like this?

```xml
<formats>
    <!-- ... -->
    <format name="jsonp" mime-type="application/javascript"/>
</formats>
```

Or maybe like this?

```xml
<formats>
    <!-- ... -->
    <format name="jsonp">application/javascript</format>
</formats>
```

Or maybe like this?

```xml
<formats>
    <!-- ... -->
    <format>
        <name>jsonp</name>
        <mime-type>application/javascript"</mime-type>
    </format>
</formats>
```

This is one of the most important problems I find when working with XML. To me
it's never obvious when something should be an element/tag or an attribute (or
an element + attribute or a nested element, etc.) That rarely happens to me in
YAML or PHP or JSON, where I find data structures more intuitive.

In case you were wondering, the previous config is written like this in XML
(it's probably even worse than you had imagined!):

```xml
<?xml version="1.0" encoding="UTF-8" ?>

<container xmlns="http://symfony.com/schema/dic/services"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:framework="http://symfony.com/schema/dic/symfony"
    xsi:schemaLocation="http://symfony.com/schema/dic/services
        https://symfony.com/schema/dic/services/services-1.0.xsd
        http://symfony.com/schema/dic/symfony
        https://symfony.com/schema/dic/symfony/symfony-1.0.xsd">

    <framework:config>
        <framework:request>
            <framework:format name="jsonp">
                <framework:mime-type>application/javascript</framework:mime-type>
            </framework:format>
        </framework:request>
    </framework:config>
</container>
```

I know that XML can provide validation and autocompletion when using XSD files,
but that requires creating those files and keeping them in sync forever. Even in
Symfony, which is [the backend framework with the most contributors in the world][7],
we've suffered from this out-of-sync problem, so you'll probably face it too.

For all these reasons we decided that enough is enough and that we would no
longer use XML in the project. What did we use instead? **PHP**.

PHP in Translations
-------------------

If you are going to hire an external company to translate your project, then you
*may* need to use XLIFF in your translation files (you may not because
translation companies support many formats besides XLIFF, so you should ask first).

If you are translating the project yourself, **don't use XLIFF**. You won't need
any of its advanced features and its verbosity will make your translation files
harder to manage.

Consider for example this XLIFF excerpt from the original EasyAdmin 2 translation.
You need 27 lines and 1,242 characters just to translate the page titles
(note also the duplicated contents in ``<source>`` and ``id``):

```xml
<!-- Resources/translations/EasyAdminBundle.en.xlf -->
<?xml version="1.0" encoding="utf-8"?>
<xliff xmlns="urn:oasis:names:tc:xliff:document:1.2" version="1.2">
    <file source-language="en" target-language="en" datatype="plaintext" original="file.ext">
        <body>
            <trans-unit id="new.page_title">
                <source>new.page_title</source>
                <target>Create %entity_label%</target>
            </trans-unit>
            <trans-unit id="show.page_title">
                <source>show.page_title</source>
                <target>%entity_label% (#%entity_id%)</target>
            </trans-unit>
            <trans-unit id="edit.page_title">
                <source>edit.page_title</source>
                <target>Edit %entity_label% (#%entity_id%)</target>
            </trans-unit>
            <trans-unit id="list.page_title">
                <source>list.page_title</source>
                <target>%entity_label%</target>
            </trans-unit>
            <trans-unit id="search.page_title">
                <source>search.page_title</source>
                <target><![CDATA[{0} No results found|{1} <strong>1</strong> result found|]1,Inf] <strong>%count%</strong> results found]]></target>
            </trans-unit>
        </body>
    </file>
</xliff>
```

This is the same translation excerpt in EasyAdmin 3 using PHP (it takes 10 lines
and 340 characters):

```php
// Resources/translations/EasyAdminBundle.en.php
<?php
return [
    'page_title' => [
        'show' => '%entity_label% (#%entity_id%)',
        'edit' => 'Edit %entity_label% (#%entity_id%)',
        'list' => '%entity_label%',
        'new' => 'Create %entity_label%',
        'search' => '{0} No results|{1} <strong>1</strong> result|]1,Inf] <strong>%count%</strong> results',
    ],
];
```

PHP translations are concise, easy to manage, can contain comments (and even a
bit of PHP logic), they can optionally group related translations using nested
keys, they can include HTML contents without having to escape them with `CDATA`
sections, etc.

Best of all is that you don't have to update anything to make it work. Just
create the PHP translation file and Symfony will use it as it did with the XLIFF
file. This is true for both Symfony apps and stand-alone bundles.

PHP in Services Config
----------------------

The YAML/XML configuration needed to define services is not that bad. Even XML
config is not as verbose as in other cases. Take this example from EasyAdmin 2:

```xml
<?xml version="1.0" ?>
<container xmlns="http://symfony.com/schema/dic/services"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://symfony.com/schema/dic/services http://symfony.com/schema/dic/services/services-1.0.xsd">

    <services>
        <service id="easyadmin.command.make_admin_migration"
                 class="EasyCorp\Bundle\EasyAdminBundle\Command\MakeAdminMigrationCommand"
                 public="true">
            <argument type="service" id="easyadmin.migrator" />
            <argument>%kernel.project_dir%</argument>
            <tag name="console.command" />
        </service>

        <!-- ... -->
    </services>
</container>
```

However, using XML to configure PHP services feels wrong because we're adding
an unnecessary layer. There's nothing you can do in XML that you can't do (much
easily) in PHP. Why use XML to "write" PHP code? It doesn't make any sense.

That's why we think it's better to use PHP code to configure PHP services. This
is the equivalent code in EasyAdmin 3:

```php
use EasyCorp\Bundle\EasyAdminBundle\Command\MakeAdminMigrationCommand;
use EasyCorp\Bundle\EasyAdminBundle\Maker\Migrator;

return static function (ContainerConfigurator $container) {
    $services
        ->set(MakeAdminMigrationCommand::class)->public()
            ->arg(0, ref(Migrator::class))
            ->arg(1, '%kernel.project_dir%')
            ->tag('console.command');
};
```

Just being able to use the `::class` constants makes this format better than XML.
If you add the conciseness of PHP, the fully autocompleted fluent interface and
a syntax that is familiar to you, the decision is a no-brainer.

When using a full Symfony application, there's nothing to change or configure.
Just create the `config/services.php` file and Symfony will use it. However, in
stand-alone bundles you need to change a bit your bundle extension class:

```php
use Symfony\Component\Config\FileLocator;
use Symfony\Component\DependencyInjection\Loader\PhpFileLoader;
use Symfony\Component\DependencyInjection\Loader\XmlFileLoader;
use Symfony\Component\HttpKernel\DependencyInjection\Extension;

class EasyAdminExtension extends Extension
{
    public function load(array $configs, ContainerBuilder $container)
    {
        // Before
        $loader = new XmlFileLoader($container, new FileLocator(__DIR__.'/../Resources/config'));
        $loader->load('services.xml');

        // After
        $loader = new PhpFileLoader($container, new FileLocator(__DIR__.'/../Resources/config'));
        $loader->load('services.php');

        // ...
    }
}
```

Conclusion
----------

XML is a powerful markup language and provides autocompletion when using XSD.
It's great to exchange information between machines, but **we don't recommend to
use XML in config intended for humans** because of its verbosity and complexity.

**PHP provides all the benefits of XML in a much more concise way**. It also
avoids problems such as `CDATA` escaping and the use of "magic strings" for
fully-qualified class names (solved in PHP using `::class` constants).

[2]: /blog/posts/the-road-to-easyadmin-3-no-more-yaml
[3]: /blog/posts/the-road-to-easyadmin-3-no-more-arrays
[4]: https://en.wikipedia.org/wiki/XML
[5]: https://en.wikipedia.org/wiki/XSLT
[6]: https://en.wikipedia.org/wiki/Protocol_Buffers
[7]: https://symfony.com/blog/symfony-was-the-backend-framework-with-the-most-contributors-in-2019
