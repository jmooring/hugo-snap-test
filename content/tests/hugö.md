+++
title = 'Hugö'
date = 2023-07-22T17:44:39-07:00
draft = false
+++

The purpose of this page is to test a content file with a path that contains bytes > 0x80. See <https://github.com/gohugoio/hugo/issues/9810>.

You should see a non-zero last modified date. If not, disable `core.quotepath` in your Git configuration:

```text
git config --global --add core.quotepath false
```
