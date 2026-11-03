# Agent instructions: Ensuring Azure Resiliency

This repository holds the slides for Chris Ayers' talk **Resilient by Design / Ensuring Azure Resiliency**. The deck is published to <https://chris-ayers.com/azure-resiliency/>.

## Repository layout

- `slides/`: Marp deck, images and diagram sources in `slides/img/`, themes in `slides/themes/`.
- `docs/authoring.md`: slide authoring and demo notes.

## Slides

- Write the deck in Markdown with [Marp](https://marp.app/). `slides/Slides.md` is the source; `build/` is generated output and is never edited or committed.
- Keep the frontmatter at the top of the deck:

  ```yaml
  ---
  marp: true
  theme: custom-default
  ---
  ```

- Store themes in `slides/themes/` and images and diagram sources in `slides/img/`. Prefer theme classes and CSS variables over inline styles.
- Write speaker notes as HTML comments (`<!-- -->`).
- Render locally with the Marp CLI version the Pages workflow uses (v4.5.0):

  ```bash
  npx @marp-team/marp-cli@4.5.0 --theme-set slides/themes --html -o build/index.html -- slides/Slides.md
  npx @marp-team/marp-cli@4.5.0 --theme-set slides/themes --html --pdf --allow-local-files -o build/Slides.pdf -- slides/Slides.md
  ```

- `.github/workflows/marp-pages.yml` builds the HTML deck and `Slides.pdf` and publishes both to GitHub Pages. If you add slide assets outside `slides/img/` and `slides/themes/`, add them to the workflow's copy step.

## Shared skills

Use the shared skills from [codebytes/skills](https://github.com/codebytes/skills) (for Copilot CLI: `copilot plugin install https://github.com/codebytes/skills`). Don't copy their instructions or scripts into this repository.

| Task                                                                          | Skill               |
| ----------------------------------------------------------------------------- | ------------------- |
| Write or revise slides, notes, order, or layouts                              | `marp-authoring`    |
| Charts, Mermaid diagrams, or other visuals                                    | `marp-visuals`      |
| Editable `.drawio.svg` diagrams (stored in `slides/img/`)                     | `drawio-diagrams`   |
| Check overflow, clipping, and HTML/PDF parity before presenting or publishing | `marp-slide-review` |

## Talk-specific rules

- Always keep the speaker bio slide and the closing resources slide with contact details. Preserve their active contact links when shortening or restructuring the deck.
- Keep the existing talk content and generic `custom-default` styling when updating tooling. Use the CSS variables and reusable layout classes in `slides/themes/` for new slides.
- Font Awesome 7.3.1 is loaded by the themes; do not add a duplicate CDN import to the deck.
- The current deck uses image-based diagrams; do not add an unused Mermaid runtime to it. If Mermaid is needed later, include this once and put definitions in `<pre class="mermaid">` blocks (SVG labels prevent clipping in PDF; the print rule removes an extra tooltip page):

  ```html
  <script type="module">
    import mermaid from "https://cdn.jsdelivr.net/npm/mermaid@11.17.2/dist/mermaid.esm.min.mjs";
    const printStyle = document.createElement("style");
    printStyle.textContent =
      "@media print { body > .mermaidTooltip { display: none !important; } }";
    document.head.append(printStyle);
    mermaid.initialize({ startOnLoad: true, htmlLabels: false });
  </script>
  ```

## Readme

- Keep the readme sections in this order: Slides, Recordings, Resources, Contact.
- When a new recording of the talk is published, add it at the top of `## Recordings` as `- [Event Year](https://www.youtube.com/watch?v=VIDEO_ID)`. When the video's title differs from the talk's, add an em dash and the recorded title after the event. Keep the list newest first.
- The talk also appears on <https://chris-ayers.com/speaking/> (source: `content/speaking.md` in [codebytes/codebytes.github.io](https://github.com/codebytes/codebytes.github.io)); new events and recordings belong there too.

## General

- Keep changes scoped to the request; don't reformat unrelated slides or code.
- Never commit secrets, keys, or connection strings. Demo placeholders must be obviously fake.
