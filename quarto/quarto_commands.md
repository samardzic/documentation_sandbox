

## Render the files

```bash
quarto render test_report.qmd
C:\opt\quarto\bin> .\quarto.exe render C:\Build\ST_automation\documents\test.qmd
quarto render test_report.qmd --to html
quarto render test_report.qmd --to pdf

quarto --version
quarto preview /Users/nenads/Build/test_quarto/test1.qmd --no-browser --no-watch-inputs
quarto --help
quarto install TinyTex
```
<br/><br/>




## Tools
- You can find the exact location of `tools` with:
```bash
quarto list tools
quarto check
```

- If TinyTeX was installed by Quarto, tlmgr.bat is usually somewhere under:
```bash
%LOCALAPPDATA%\Quarto\tools\tinytex\
```

- Or locate the TeX binaries directly:
```bash
where.exe pdflatex
where.exe tlmgr
```
<br/><br/>




## Preview While Editing 

- Start live preview:
```bash
quarto preview report.qmd
```
<br/><br/>





## Project Creation

- Website:
```bash
quarto create-project mysite --type website
```

- Book:
```bash
quarto create-project mybook --type book
```

- Blog:
```bash
quarto create-project myblog --type website:blog
```




# Quarto CLI - Most Useful Commands Reference

## General

Show installed Quarto version:

```powershell
quarto --version
```

Check installation status, dependencies, TinyTeX, Python, R, Jupyter, etc.:

```powershell
quarto check
```

Show all available commands:

```powershell
quarto help
```

---

## Render Documents

Render a single document:

```powershell
quarto render report.qmd
```

Render all documents in a project:

```powershell
quarto render
```

Render to HTML:

```powershell
quarto render report.qmd --to html
```

Render to PDF:

```powershell
quarto render report.qmd --to pdf
```

Render to Word:

```powershell
quarto render report.qmd --to docx
```

---

## Live Preview

Preview a document with auto-refresh:

```powershell
quarto preview report.qmd
```

Preview an entire project:

```powershell
quarto preview
```

---

## Create Projects

Create a website:

```powershell
quarto create-project mysite --type website
```

Create a book:

```powershell
quarto create-project mybook --type book
```

Create a blog:

```powershell
quarto create-project myblog --type website:blog
```

---

## TinyTeX Management

Install TinyTeX:

```powershell
quarto install tinytex
```

Install TinyTeX and update PATH:

```powershell
quarto install tinytex --update-path
```

Update TinyTeX:

```powershell
quarto update tinytex
```

Remove TinyTeX:

```powershell
quarto uninstall tinytex
```

List Quarto-managed tools:

```powershell
quarto list tools
```

---

## Extensions

Install an extension:

```powershell
quarto add quarto-ext/fontawesome
```

List installed extensions:

```powershell
quarto list extensions
```

Update extensions:

```powershell
quarto update
```

Remove an extension:

```powershell
quarto remove quarto-ext/fontawesome
```

---

## Publishing

Publish to GitHub Pages:

```powershell
quarto publish gh-pages
```

Publish to Posit Connect:

```powershell
quarto publish connect
```

Publish to Quarto Pub:

```powershell
quarto publish quarto-pub
```

---

## Project Maintenance

Clean generated files:

```powershell
quarto clean
```

Clean specific document artifacts:

```powershell
quarto clean report.qmd
```

---

## Diagnostics & Troubleshooting

Render with debug logging:

```powershell
quarto render report.qmd --log-level debug
```

Save render log:

```powershell
quarto render report.qmd --log output.log
```

Comprehensive environment check:

```powershell
quarto check all
```

---

## Windows-Specific Utilities

Find Quarto executable:

```powershell
where.exe quarto
```

Find TinyTeX PDF compiler:

```powershell
where.exe pdflatex
```

Find TeX package manager:

```powershell
where.exe tlmgr
```

Find Quarto-managed tools:

```powershell
quarto list tools
```

---

## Daily Workflow Commands

```powershell
quarto check
quarto preview
quarto render
quarto list tools
quarto install tinytex
quarto update
quarto clean
```