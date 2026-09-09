
# Quarto Supported Output Formats

> Quarto supports virtually all Pandoc output formats and provides first-class support for the formats below. 【1-75eef0】

## Documents

### HTML

```yaml
format: html
```

### PDF

```yaml
format: pdf
```

### Microsoft Word

```yaml
format: docx
```

### OpenDocument Text (ODT)

```yaml
format: odt
```

### ePub

```yaml
format: epub
```

### Typst PDF

```yaml
format: typst
```

### ConTeXt PDF

```yaml
format: context
```

---

## Presentations

### Reveal.js

```yaml
format: revealjs
```

### PowerPoint

```yaml
format: pptx
```

### Beamer (LaTeX Slides)

```yaml
format: beamer
```

---

## Markdown Variants

### GitHub Flavored Markdown

```yaml
format: gfm
```

### CommonMark

```yaml
format: commonmark
```

### Hugo

```yaml
format: hugo
```

### Docusaurus

```yaml
format: docusaurus
```

### Markua

```yaml
format: markua
```

---

## Wiki Formats

### MediaWiki

```yaml
format: mediawiki
```

### DokuWiki

```yaml
format: dokuwiki
```

### ZimWiki

```yaml
format: zimwiki
```

### Jira Wiki

```yaml
format: jira
```

### XWiki

```yaml
format: xwiki
```

---

## Technical & Publishing Formats

### JATS XML

```yaml
format: jats
```

### Jupyter Notebook

```yaml
format: ipynb
```

### reStructuredText

```yaml
format: rst
```

### AsciiDoc

```yaml
format: asciidoc
```

### Org Mode

```yaml
format: org
```

### DocBook

```yaml
format: docbook
```

### RTF

```yaml
format: rtf
```

### Plain Text

```yaml
format: plain
```

### Muse

```yaml
format: muse
```

---

## Common Render Commands

Render HTML:

```powershell
quarto render document.qmd --to html
```

Render PDF:

```powershell
quarto render document.qmd --to pdf
```

Render Typst PDF:

```powershell
quarto render document.qmd --to typst
```

Render Word:

```powershell
quarto render document.qmd --to docx
```

Render PowerPoint:

```powershell
quarto render document.qmd --to pptx
```

Render RevealJS slides:

```powershell
quarto render document.qmd --to revealjs
```

Render ePub:

```powershell
quarto render document.qmd --to epub
```

Render Jupyter Notebook:

```powershell
quarto render document.qmd --to ipynb
```

Render GitHub Markdown:

```powershell
quarto render document.qmd --to gfm
```

Render Plain Text:

```powershell
quarto render document.qmd --to plain
```

---

## Most Common Formats in Practice

```text
html
pdf
typst
docx
pptx
revealjs
epub
ipynb
gfm
```

Source: Quarto "All Formats" reference. 【1-75eef0】【2-3539f0】