Test Execution Logging Design - generated artifacts

- rep_result.qmd   Quarto source; contains Mermaid diagrams and HTML/DOCX/PDF configuration.
- rep_result.html  Rendered HTML fallback.
- rep_result.docx  Rendered DOCX fallback.
- rep_result.pdf   Rendered PDF fallback.
- my_template.docx Reference DOCX used for the DOCX render.
- gtp_result_final.md Original source.
- prompt_source.md Processing prompt.
- diagram_assets/  Compact SVG diagram fallbacks used by the non-Quarto renders.

Quarto was not installed in the execution environment. Therefore the QMD was generated exactly as the
requested source artifact, while the three preview/output formats were rendered with Pandoc and compact
SVG fallbacks for the Mermaid visuals. Native Quarto rendering of rep_result.qmd will replace those
fallbacks with Quarto's Mermaid renderer.
