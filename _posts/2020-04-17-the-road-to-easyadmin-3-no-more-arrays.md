---
layout: post
title: 'The Road to EasyAdmin 3: No More Arrays'
permalink: posts/the-road-to-easyadmin-3-no-more-arrays
---

[EasyAdmin](https://github.com/EasyCorp/EasyAdminBundle) is an admin generator
for Symfony applications. EasyAdmin 3 is the new version that will be released
soon. This article is part of a series that explains some of its biggest changes
and the reasoning behind them.

-----

Arrays are the most used structures for data storage in PHP. The main reason is
that they are easy to use and provide lots of utilities. Their biggest problem
is that they are a generic data storage, so they don't provide any semantics.

In EasyAdmin 2, the entire config was defined using YAML and then it was
transformed into a big associative array. Managing everything through this array
is not easy because keys are not autocompleted (although the latest versions of
the great PhpStorm IDE are improving that).

We could use constants to define the keys, but the resulting code would be
cumbersome (e.g. `$config[CONFIG_KEYS::DESIGN][CONFIG_KEYS::CSS_CLASSES]`).
That's why we ended up using regular associative arrays, which makes it harder
to develop the application (is it `$config['design']['css_classes']` or
`$config['design']['cssclasses']` or `$config['design']['cssClasses']` ?).

EasyAdmin 3 already [removes all YAML config][1], so we decided to also **remove
all arrays from our code**. We left some arrays in some parts used by end users
(because arrays are much easier to use than other alternatives), but we removed
all arrays from our internal code. What did we use instead of arrays? **DTOs**.

DTOs
----

[DTOs][2] or "Data Transfer Objects" are objects that carry data between
processes. In its simplest form, a DTO is a PHP object with public properties.
More generally, DTOs can include *getters*, *setters* and serialize/unserialize
logic. They shouldn't contain any business logic.

At first we tried to create "pure" DTOs which only included *getters*. However,
during runtime we must update some values of some DTOs, so we needed some kind
of *setter*. Instead of adding regular *setters* we first tried defining
*wither* methods:

```php
$originalObject = new SomeDto(['foo' => 'bar']);
$aDifferentObject = $originalObject->with('foo', 'not bar');

// the with() method looks like this
class SomeDto
{
    // ...

    public function with($propertyName, $propertyValue): self
    {
        $newObject = clone $this;
        $newObject->{$propertyName} = $propertyValue;

        return $newObject;
    }
}
```

This was too convoluted and felt like a *hack* (the *wither* is just a *setter*
acting under a different name). Ultimately we made a trade-off between
*"real-world code convenience"* and *"academic code purity"* and that's the
reason why our DTOs have both *getters* and *setters*.

Config Objects
--------------

DTOs don't provide a great DX (*developer experience*) because they are too low
level. Instead of forcing end-users work with DTOs to configure backends, we
created high-level "config objects" with semantic *setters* and fluent
interfaces.

For example, when you configure an action, you can choose if it's displayed as
a link or as a button. This is how you configure that:

```php
use EasyCorp\Bundle\EasyAdminBundle\Config\Action;

$action = Action::new('name', 'Some Label')->displayAsLink();
                                     // or ->displayAsButton();
```

Internally, this config object calls to the related DTO object (`ActionDto`
in this example):

```php
namespace EasyCorp\Bundle\EasyAdminBundle\Config;

use EasyCorp\Bundle\EasyAdminBundle\Dto\ActionDto;

final class Action
{
    private $dto;

    private function __construct(ActionDto $actionDto)
    {
        $this->dto = $actionDto;
    }

    public static function new(string $name, $label = null, ?string $icon = null): self
    {
        $dto = new ActionDto();
        $dto->setName($name);
        // ...

        return new self($dto);
    }

    // ...

    public function displayAsLink(): self
    {
        $this->dto->setHtmlElement('a');

        return $this;
    }

    public function displayAsButton(): self
    {
        $this->dto->setHtmlElement('button');

        return $this;
    }
}
```

In other cases, the config object validates the given data or performs some logic
before passing the data to the DTO (because DTO don't contain any validation or
business logic):

```php
// validating the given data before trying to store it in the DTO
public function setDefaultSort(array $sortFieldsAndOrder): self
{
    $sortFieldsAndOrder = array_map('strtoupper', $sortFieldsAndOrder);
    foreach ($sortFieldsAndOrder as $sortField => $sortOrder) {
        if (!\in_array($sortOrder, ['ASC', 'DESC'])) {
            throw new \InvalidArgumentException('...');
        }

        if (!\is_string($sortField)) {
            throw new \InvalidArgumentException('...');
        }
    }

    $this->dto->setDefaultSort($sortFieldsAndOrder);

    return $this;
}

// performing some operations with the DTO methods
public function addMenuItems(array $items): self
{
    $this->dto->setItems(array_merge($this->dto->getItems(), $items));

    return $this;
}
```

The resulting code provides benefits for all parties:

* End-users configure the backend using semantic and fluent methods via
  config objects (`Action`, `MenuItem`, `Filter`, `Assets`, `Dashboard`, etc.);
* DTOs are only used internally (`ActionDto`, `MenuItemDto`, `AssetsDto`, etc.)
  and they provide getters/setters to simplify our code.

Collection Objects
------------------

Sometimes we need to pass or return a group of DTOs, such as a group of
`ActionDto` to define all the actions of the page or a group of `FieldDto` to
list all fields displayed on the page. Although it's tempting to use an
associative array for this, we decided to define specific collection objects.

Our collection objects are similar Doctrine's `ArrayCollection` but simpler,
because they only provide the methods we really need in our code. This change
may look minor, but improves the code significantly. Instead of a generic
``array`` with no specific semantics, we can type-hint arguments and return
types with these objects, making the code easier to read and write:

```php
public function processFields(FieldCollection $fields, EntityDto $entityDto): void
{
    // ...
}

public function processGlobalActions(ActionConfigDto $actionsDto): ActionCollection
{
    // ...
}
```


Generic DTOs
------------

In some cases, defining a specific DTO felt like overengineering, so we created
a generic and multi-purpose storage called `KeyValueStore`. It's similar to
Symfony's `ParameterBag` but we added support for a feature that simplifies our
code significantly: "dot notation".

Thanks to the "dot notation", we don't need to perform checks for array keys
(`if (isset($config['foo']['bar']) { ... }`) and we can quickly get/set/check
nested elements: `$config->get('foo.bar')`, `$config->set('foo.bar.baz', 'value')`,
`$config->has('foo.qux.bar')`, etc.

We use a `KeyValueStore` object for example to manage the options passed to
Symfony form types:

```php
$field->setFormTypeOptionIfNotSet('row_attr.class', $field->getCssClass());
$field->setFormTypeOption('attr.align', $field->getTextAlign());
```

In Summary
----------

Using arrays is a quick and simple way of storing information, data and config
in your PHP applications. However, as soon as your application grows in size or
complexity, arrays are not an optimal solution.

Using objects made our code much easier to read and manage. Thanks to IDE
autocompletion, code is much easier to write too. Finally,
[objects consume less memory than arrays][3] (at least in PHP 7) so they also
improve your application performance.

[1]: /blog/posts/the-road-to-easyadmin-3-no-more-yaml
[2]: https://en.wikipedia.org/wiki/Data_transfer_object
[3]: https://steemit.com/php/@crell/php-use-associative-arrays-basically-never
