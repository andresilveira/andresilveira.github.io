---
title: How to upgrade Elixir to 1.6 on MacOSX
date: 2018-01-10 09:45:28
categories: code
tags: code functional-programming elixir
---

I've installed Elixir via Homebrew and when trying to upgrade Elixir with `brew upgrade elixir`, I only got the patch version upgraded, that is, from 1.5 to 1.5.3.

If you want to get the latest version available try running

```
brew update
brew upgrade elixir --HEAD
```

This will bring the latest release from Elixir's git repository.
