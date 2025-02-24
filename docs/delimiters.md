---
id: delimiters
title: Automatic delimiters
---

> This chapter doesn't contain `PHP` code snippets, because PHP PCRE require delimiters. There is no way to omit them
> in Vanilla PHP.

Thanks to automatic delimiters, one can use regular expressions without [brain strain](overview.md#brain-strain)
and without coming up with a suitable delimiter.

```php
pattern('[A-Z][a-z]+')->match($subject)->first();
```

Automatic delimiters work perfectly fine, regardless of whether you passed a delimited pattern or not.

```php
pattern('[A-Z][a-z]+')->match($subject)->first();
pattern('#[A-Z][a-z]+#')->match($subject)->first();
```

The code snippets above are equal.

## Is pattern delimited

You can check whether a pattern is delimited with `is()->delimited()` method.

<!--DOCUSAURUS_CODE_TABS-->
<!--T-Regx-->
```php
pattern('[A-Z][a-z]+')->is()->delimitered();
pattern('#[A-Z][a-z]+#')->is()->delimitered();
```
<!--END_DOCUSAURUS_CODE_TABS-->
<!--T-Regx:{multiline-return}-->
<!--Result-Value-->

```php
false
true
```
<!--Result-Value:{multiline-return}-->

## Adding delimiters

To change undelimited pattern into a delimited one - use `delimiter()` method;

<!--DOCUSAURUS_CODE_TABS-->
<!--T-Regx-->
```php
pattern('Welcome/Or not')->delimiter();
```
<!--END_DOCUSAURUS_CODE_TABS-->
<!--T-Regx:{echo-at(0)}-->
<!--Result-Output-->

```text
#Welcome/Or not#
```

### Ambiguity

How does T-Regx decide whether a pattern is already delimited, or whether it's not and needs to be delimited again?

The rule is simple.

If a pattern *can* be thought of as delimited - T-Regx assumes it's delimited.

## Flags

There are two ways of passing flags:

Either pass a second argument to the [`pattern()`](introduction.md#entry-points)/[`Pattern::of()`](introduction.md#entry-points):

```php
pattern('[A-Z][a-z]+', 'i')->match($subject)->first();
```

or use a regular delimited pattern with a flag:

```php
pattern('/[A-Z][a-z]+/i')->match($subject)->first();
```

## I want to break it

T-Regx has a set of predefined, suitable delimiters (like `/`, `#`, `~`, etc.) and simply uses the first one, 
that doesn't occur in your pattern. If you exhaust each of them; if you use every possible, predefined, suitable delimiter - 
T-Regx will throw `ExplicitDelimiterRequiredException`.

In that case, you simply have to use an explicit, regular delimiter and automatic delimiter won't be used.

If you think another automatic delimiter can be used, 
please create [a github issue](https://github.com/T-Regx/T-Regx/issues/new/choose).
