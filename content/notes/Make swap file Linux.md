---
title: "Make swap file Linux"
tags:
- Linux
weight: 0
draft: 
---

First create the swap ile on disk
```
sudo dd if=/dev/zero of=/swapfile bs=1024 count=1048576
```