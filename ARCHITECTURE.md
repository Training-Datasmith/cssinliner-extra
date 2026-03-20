# Architecture: cssinliner-extra

## Purpose

A Twig extension that integrates the `pelago/emogrifier` CSS inliner into Twig templates. It provides a `csstoinlinestyles` filter and `inlinecss` function so email templates can inline CSS directly within Twig rendering.

## Directory Structure

```
CssInlinerExtension.php      — Twig extension: registers the filter/function and delegates to Emogrifier
Resources/
  functions.php              — Legacy global PHP functions for backward compatibility
Tests/
  IntegrationTest.php        — Tests verifying the filter produces correctly inlined HTML
  LegacyFunctionsTest.php    — Tests for the legacy function wrappers
```

## Key Design Decisions

- **Thin wrapper** — the extension contains no CSS parsing logic; it simply wires Twig's filter/function API to `Pelago\Emogrifier\CssInliner`.
- **Backward-compatible functions** — `Resources/functions.php` provides global function wrappers for projects using the pre-Twig-extension API.

## Extension Points

- Register additional Twig filters in a custom extension that extends or composes `CssInlinerExtension`.

## Dependency Flow

```
Twig template: {{ content|csstoinlinestyles }}
  └── CssInlinerExtension::filter(html, css?)
        └── Pelago\Emogrifier\CssInliner::inlineStylesOf(html)
```
