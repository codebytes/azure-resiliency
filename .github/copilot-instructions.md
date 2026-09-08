- All slides must be written in Markdown format and use the [Marp](https://marp.app/) framework.
- frontmatter Each slide deck must include the following frontmatter:
   ```yaml
   ---
   marp: true
   theme: custom-default
   ---
   ```
- speaker notes - Use HTML comments (`<!-- -->`) for speaker notes.
- Mermaid diagrams - Include this script once per deck when Mermaid diagrams are used:
   ```html
   <script type="module">
     import mermaid from 'https://cdn.jsdelivr.net/npm/mermaid@11.17.2/dist/mermaid.esm.min.mjs';
     const printStyle = document.createElement('style');
     printStyle.textContent = '@media print { body > .mermaidTooltip { display: none !important; } }';
     document.head.append(printStyle);
     mermaid.initialize({ startOnLoad: true, htmlLabels: false });
   </script>
   ```
   SVG labels prevent clipping in PDF exports; the print-only rule prevents an extra tooltip page.
   Put diagram definitions in `<pre class="mermaid">` blocks. The current talk uses image-based diagrams; do not add an unused Mermaid runtime to it.
- Slide decks should be stored in the `slides` directory.
- Custom themes or assets should be stored in appropriate subdirectories under `slides`.
- Follow Marp best practices for layout and styling.
- Ensure compatibility with Marp CLI 4.5.0 for HTML, PDF, and presentation generation.
- Keep the existing talk content and generic `custom-default` styling when updating tooling.
- Use CSS variables and the reusable layout classes in `slides/themes/` for new decks.
- Font Awesome 7.3.1 is loaded by the themes; do not add a duplicate CDN import to a deck.
- Use the shared skills from [codebytes/skills](https://github.com/codebytes/skills), installed through the agent's plugin or skill support. Do not duplicate their definitions or scripts in this repository.
- Use `marp-authoring` for deck content and layouts, and `marp-visuals` for charts and Mermaid assets.
- Use `marp-slide-review` to check for overflow, clipping, and rendering problems before presenting or publishing. Follow the installed skill's instructions for its scripts and dependencies.
- Use `drawio-diagrams` for editable `.drawio.svg` figures and store presentation diagrams and source specifications in `slides/img/`.
