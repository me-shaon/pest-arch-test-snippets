# PEST 'Architecture test' code snippets
Useful 'Architecture test' code snippets for [PEST](https://pestphp.com/) which you can use in your Laravel projects.

PR and ideas are welcome! 🙌

## Table of contents

- [Do not leave debug statements](#do-not-leave-debug-statements)
- [Do not use `env()` outside of config files](#do-not-use-env-outside-of-config-files)
- [Use strict type checking](#use-strict-type-checking)

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
