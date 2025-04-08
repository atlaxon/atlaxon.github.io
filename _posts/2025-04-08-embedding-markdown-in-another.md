---
title: "Embedding Markdown in Another"
date: 2025-04-08
---

## Embedding Markdown in Another

### Introduction

The advantage of Markdown is its simplicity; with just a few types of markup, it can present document content to readers in a clear and organized manner, making it very easy for users to get started. In contrast, markup languages like AsciiDoc and reStructuredText, while offering more features, are not as easy to master—not to mention heavyweight typesetting tools like LaTeX. However, "what makes it succeed can also lead to its failure." Markdown lacks a crucial file inclusion function, which limits its ability to build complex documents. The project [markdown-include](https://github.com/atlaxon/markdown-include) on GitHub utilizes text processing programs (such as Perl, Awk, Sed, etc.) and GNU Make to implement file inclusion for Markdown. It can automatically generate the inclusion dependencies of .md files and then use GNU Make to build a single, unified .md file.

### Prerequistes

- Text processing utilities: perl, awk, sed
- GNU make
- If you want to convert Markdown to PDF, you must install TeX Live and Pandoc.

### Embedded Markdown Syntax

The embeded markdown syntax is comptiable with [Obsidian Flavored Markdown](https://help.obsidian.md/obsidian-flavored-markdown), but adding more restrictions. All files of the document should be placed in the `docs` directory, and all Markdown files should have the `.md` suffix. If file inclusion is involved （including images etc）, the file inclusion path should be relative to `docs/`. Place the `Makefile` in the same level directory as `docs`, and simply run the `make` command to generate the files in the `dist` directory.

The file inclusion syntax is as follows:

```
![[ filename.md ]]
```

Note:

- The file inclusion directives must stand alone on their own lines.
- `![[` must start at the beginning of the line.
- Spaces before or after `filename.md` are allowed.
- After `]]`, only whitespace is permitted—no other characters are allowed.

Beset practice: To ensure content between files does not interfere with each other, each directive in a file should be preceded and followed by a blank line.

## Converting Markdown to PDF with Pandoc

The rule in Makefile is as follows:

```
%.pdf: %.md
	pandoc $(OPTIONS) $< -o $@
```

You need to reset variable FONT in Makefile according to your system and run `make pdf`.

When automatically generated PDFs have formatting issues, you can generate a .tex file instead by running `make tex`, edit it manually, and then compile it to the final PDF.

The rule in Makefile is as follows:

```
%.tex: %.md
	pandoc $(OPTIONS) $< -o $@
```
