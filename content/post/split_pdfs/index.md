---
title: Split pdfs with python
description: A utility function to split a mulit-page pdf into separate pages
slug: split-pdfs
date: 28-01-2023 00:00:00+0000
categories:
    - Python
tags:
    - pdfs
    - python
    - document-processing
---

This is a function using `pypdf` to split a multi-page PDF into individual pages.

```python
import os
from pypdf import PdfReader, PdfWriter

def split_pdfs(input_file_path):
    inputpdf = PdfReader(open(input_file_path, "rb"))

    out_paths = []
    if not os.path.exists("outputs"):
        os.makedirs("outputs")

    for i, page in enumerate(inputpdf.pages):
        output = PdfWriter()
        output.add_page(page)

        out_file_path = f"outputs/{input_file_path[:-4]}_{i}.pdf"
        with open(out_file_path, "wb") as output_stream:
            output.write(output_stream)

        out_paths.append(out_file_path)
    return out_paths
```

