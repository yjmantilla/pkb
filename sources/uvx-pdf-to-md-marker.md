---
title: "UVX PDF to MD"
source: "https://chatgpt.com/c/69e95ebc-de60-83ea-8873-ee95cf22c42b"
author:

created: 2026-04-22
description: "ChatGPT conversation with 2 messages"
tags:
  - "clippings"
  - "tools"
---
**You said**

make a uvx to convert a pdf to md from this https://github.com/datalab-to/marker\\

---

**ChatGPT said**

I’m checking the repo’s current CLI/package usage so I can give you a `uvx` command that matches how Marker is actually meant to be run today.

Use:

```bash
uvx --from marker-pdf marker_single /path/to/file.pdf --output_format markdown --output_dir ./marker_out
```

That matches the repo’s current package name (`marker-pdf`) and single-file CLI (`marker_single`), and `markdown` is one of the supported output formats. [GitHub+1](https://github.com/datalab-to/marker)

A slightly more practical version is:

```css
Bashuvx --from marker-pdf marker_single /path/to/file.pdf --output_format markdown --output_dir ./marker_out
```

Useful variants:

```css
Bash# force OCR
uvx --from marker-pdf marker_single input.pdf --output_format markdown --force_ocr

# only selected pages
uvx --from marker-pdf marker_single input.pdf --output_format markdown --page_range "0,5-10"

# better accuracy with LLM mode
uvx --from marker-pdf marker_single input.pdf --output_format markdown --use_llm
```

Marker’s README also notes Python 3.10+ and PyTorch are needed. [GitHub](https://github.com/datalab-to/marker)

For non-PDF formats, use the full extra:

```csharp
Bashuvx --from 'marker-pdf[full]' marker_single /path/to/file.docx --output_format markdown
```

The repo says the `[full]` extra is needed for documents other than PDFs. [GitHub](https://github.com/datalab-to/marker)

If you want, I can also give you a tiny shell function like `pdf2md input.pdf` using `uvx`.