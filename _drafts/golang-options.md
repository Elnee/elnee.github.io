---
title: "Golang patterns to initialize a complex struct"
categories:
  - Programming
tags:
  - Golang
---

There is a lot of different ways to initialize a Golang struct with a big set
of configuration options. The most popular pattern I know is the functional
options pattern, but as for me it's a bit cumbersome in some cases.

So, I decided to write this blog post about different ways that I know, some
from other internet articles, some was just discovered in open source repos or
written by me while experimenting with the code.

Draft list of patterns:
- functional options pattern
  - with & w/o builder?
- options struct pattern
