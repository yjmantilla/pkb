---
title: "Markdown conversion with uv"
source: "https://chatgpt.com/c/69e151f8-89f8-83ea-af40-ab4deaea5703"
author:

created: 2026-04-22
description: "ChatGPT conversation with 2 messages"
tags:
  - "clippings"
  - "tools"
---
**You said**

read this page, https://github.com/microsoft/markitdown make a command easily runnable with uv to convert something to markdown

---

**ChatGPT said**

I’m checking the repo page and its usage so I can give you a command that works cleanly with `uv`.

Yep. Based on the repo README, the CLI usage is:

`markitdown path-to-file.pdf -o document.md` [GitHub](https://github.com/microsoft/markitdown)

And installation with all format support is:

`pip install 'markitdown[all]'` [GitHub](https://github.com/microsoft/markitdown)

So the easiest `uv` one-liner is:

```python
Bashuvx --from 'markitdown[all]' markitdown input.pdf -o output.md
```

Examples:

```csharp
Bashuvx --from 'markitdown[all]' markitdown report.docx -o report.md
uvx --from 'markitdown[all]' markitdown slides.pptx -o slides.md
uvx --from 'markitdown[all]' markitdown sheet.xlsx -o sheet.md
```

If you want a version with shell variables:

```bash
BashINPUT="file.pdf"; OUTPUT="${INPUT%.*}.md"; uvx --from 'markitdown[all]' markitdown "$INPUT" -o "$OUTPUT"
```

If you will use it often, install it into your project env instead:

```lua
Bashuv add 'markitdown[all]'
uv run markitdown input.pdf -o output.md
```

The repo also notes that optional extras exist by format, such as `[pdf]`, `[docx]`, and `[pptx]`, so you can use narrower installs if you do not want everything. [GitHub](https://github.com/microsoft/markitdown)

A tiny reusable shell function would be:

```bash
Bashmdown () {
  local input="$1"
  local output="${2:-${input%.*}.md}"
  uvx --from 'markitdown[all]' markitdown "$input" -o "$output"
}
```

Then run:

```
Bashmdown myfile.pdf
mdown myfile.docx notes.md
```