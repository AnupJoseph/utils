---
title: Find Magic
description: All find related tricks and tips
slug: bash-tricks
date: 23-03-2024 00:00:00+0000
draft: false
categories:
    - Shell
tags:
    - shell
    - bash

---


### Filter by type
```sh
find path/to/folder -type f -name "*.type" 
```

### Filter by type and date
```sh
find path/to/folder -type f -name "*.type" ! -newermt "2023-06-01 00:00:00" 
```