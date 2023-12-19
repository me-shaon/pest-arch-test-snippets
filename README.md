# PEST 'Architecture test' code snippets
Useful ['Architecture test'](https://pestphp.com/docs/arch-testing) code snippets for [PEST](https://pestphp.com/) which you can use in your Laravel projects.

PR and ideas are welcome! 🙌

## Table of contents

- [Do not leave debug statements](#do-not-leave-debug-statements)
- [Do not use `env()` outside of config files](#do-not-use-env-outside-of-config-files)
- [Use strict type checking](#use-strict-type-checking)
- [Invokable classes should have __invoke method](#invokable-classes-should-have-__invoke-method)
- [Classes should contain a specific method](#classes-should-contain-a-specific-method)
- [Classes should implement an interface](#classes-should-implement-an-interface)
- [Classes should have proper suffix](#classes-should-have-proper-suffix)
- [Ensure cross domain boundaries are respected](#ensure-cross-domain-boundaries-are-respected)

---

### Do not leave debug statements

It's easy to leave debug statements like `dd` or `dump` in the code and push them to production.

But now we can avoid that by adding the following test.

```php
arch('Do not leave debug statements')
    ->expect(['dd', 'dump', 'var_dump'])
    ->not->toBeUsed();
```

If you are using [Ray](https://myray.app/) you should add `ray` to the expectation list as well.


### Do not use `env()` outside of config files

If we use `config:cache` in production ([which we should](https://laravel.com/docs/master/deployment#optimizing-configuration-loading)), `env()` helper will always return `null`.  
But it's very easy to forget while writing code!

Add the following test to make sure we are not doing that.

```php
arch('Do not use env helper in code')
    ->expect(['env'])
    ->not->toBeUsed();
```


### Use strict type checking

We might prefer to use [Strict type checking](https://ashallendesign.co.uk/blog/using-declare-strict_types-1-for-more-robust-php-code) in our code. 

To ensure that, we can add the following test:

```php
arch('Use string type check')
    ->expect('App')
    ->toUseStrictTypes();
```

### Invokable classes should have __invoke method

Sometimes we want to make sure that some classes should be invokable. For example, some developers prefer that `Action` classes should be invokable. 

To ensure that, we can add the following test:

```php
arch('Action classes should be invokable')
    ->expect('App\Actions')
    ->toBeInvokable();
```

### Classes should contain a specific method

A lot of Laravel developers prefer to use[Laravel actions](https://laravelactions.com/). This classes must have a `handle` method. Also, [Laravel Jobs] should have a `handle` method as well.

Using the following test, we can make sure that these classes must have the `handle` method implemented:

```php
arch('Job classes should have handle method')
    ->expect('App\Jobs')
    ->toHaveMethod('handle');
```

### Classes should implement an interface

When implementing the Repository pattern, it's a common practice to have a `RepositoryInterface` and all the Repository classes implement it.

How to make sure no Repository classes missed this requirement? It's pretty easy:

```php
arch('app')
    ->expect('App\Repositories')
    ->toImplement('App\Repositories\RepositoryInterface');
```

### Classes should have proper suffix

Service pattern is also a popular choice by the Laravel developers. In this case, we might want to make sure that all the 'Service' classes have the 'Service' suffix in it.

To confirm this, we can add this test:

```php
arch('Services classes should have proper suffix')
    ->expect('App\Services')
    ->toHaveSuffix('Service');
```

We do the same for 'Controller' classes, right?
```php
arch('Services classes should have proper suffix')
    ->expect('App\Controllers')
    ->toHaveSuffix('Controller');
```

### Ensure cross-domain boundaries are respected

When implementing 'Domains' or 'Modules' in our code, we might want to make sure that one domains/modules code is not directly used in other domains/modules. 
This will help us to keep the domains/modules independent of each other. 

For example, let's say we have two Modules in our project called `RideSharing` and `FoodDelivery`. We want to make sure that one modules code is not used in other module.
To ensure this constraint, we can just add this:

```php
arch('Modules should be independent')
    ->expect('Modules\RideSharing')
    ->not->toBeUsed('Modules\FoodDelivery');
```
