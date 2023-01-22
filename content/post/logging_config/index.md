---
title: Set up a simple logging config
description: Method to add a quick logging config to the application
slug: logging_config
date: 22-01-2023 00:00:00+0000
# image: table.png
categories:
    - Logging
tags:
    - python
---


Here's a snippet to have a basic logging config setup in Python in a couple of lines of code.
```python
import logging
logging.basicConfig(
    filename="output.log",
    filemode="w",
    level=logging.DEBUG,
    format="%(asctime)s:%(levelname)s:%(message)s",
    datefmt="%Y-%m-%d %I:%M:%S%p",
)
```