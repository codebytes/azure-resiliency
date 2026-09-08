# Ensuring Azure Resiliency

This repository contains the slide deck, demos, and additional resources for the "Ensuring Azure Resiliency" talk by Chris Ayers, Senior Risk SRE at Microsoft. The talk provides an in-depth look at Azure resiliency, design principles, and best practices.

## Slides

- [View the HTML slide deck](https://chris-ayers.com/azure-resiliency/)
- [Download `Slides.pdf`](https://chris-ayers.com/azure-resiliency/Slides.pdf)

Merging to `main` triggers the Pages workflow, which builds and publishes both
artifacts together.

## Authoring and Building Slides

The tooling follows [marp-slides-template](https://github.com/codebytes/marp-slides-template),
with the Azure presentation, images, table styling, and footer settings preserved.

| Component | Version | Configuration |
|-----------|---------|---------------|
| Marp CLI | 4.5.0 | Pages Docker image and local/review commands |
| Mermaid | 11.17.2 | Optional runtime; add only when a deck contains Mermaid source |
| Font Awesome Free | 7.3.1 | Versioned CSS import in the custom themes |

Open `slides/Slides.md` with the Marp for VS Code extension, or reopen the
repository in its Debian Bookworm devcontainer. The container includes Node.js
24 and the Marp, Markdown, Mermaid, and draw.io extensions. VS Code is configured
for HTML export, live overflow diagnostics, and smart editable-PPTX export
(editable export requires LibreOffice and may fall back to non-editable output).

### Local Preview and Export

Use Node.js 18.3+; Node.js 24 LTS is recommended. No global Marp installation or
package manifest is required:

```bash
# Start Marp's local slide server.
npx --yes @marp-team/marp-cli@4.5.0 --server --html --theme-set slides/themes --input-dir slides

# Build the same HTML and assets as GitHub Pages.
mkdir -p build
cp -R slides/img slides/themes build/
npx --yes @marp-team/marp-cli@4.5.0 --theme-set slides/themes --html -o build/index.html -- slides/Slides.md

# Export PDF (requires Chrome/Chromium, Edge, or Firefox locally).
npx --yes @marp-team/marp-cli@4.5.0 --theme-set slides/themes --html --allow-local-files -o build/Slides.pdf -- slides/Slides.md
```

Use `-o build/Slides.pptx` instead for PowerPoint export. The Pages workflow uses
the pinned Marp Docker image, which includes a browser, to publish HTML, PDF,
images, and themes. Generated `build/` output is ignored by Git.

### Themes, Icons, and Diagrams

The current deck uses `custom-default`. The optional `vsl` theme is adapted
from the Visual Studio Live San Diego speaker template and retains its embedded
title/content backgrounds. Other alternatives are `custom-gaia`,
`custom-uncover`, and the minimal `custom`. All include Font Awesome, centered
images (`![center](img/example.png)`), two/three-column wrappers
(`<div class="columns">` / `columns3`), and a smaller-text slide class
(`<!-- _class: small -->`). Keep using wrappers for slides with mixed headings,
lists, and images rather than turning the entire slide into a grid.

Use icons such as `<i class="fa-brands fa-github"></i>`. Font Awesome's imports
must remain before ordinary CSS rules, with terminating semicolons.

The current talk uses image-based diagrams and does not load Mermaid at runtime.
For a deck that needs Mermaid, use version 11.17.2 and add the initializer once,
then define diagrams with:

```html
<pre class="mermaid">
flowchart LR
    Detect --> Recover --> Learn
</pre>
```

Plain fenced Mermaid blocks are not automatically rendered by the Marp CLI.
CDN access is required for Mermaid and Font Awesome; use pre-rendered local
diagrams when offline or when a diagram must render identically in every export.

The talk contains both editable `.drawio.svg` / `.drawio.png` diagrams and
regular image exports. Use the shared `drawio-diagrams` skill when a diagram
must remain editable, and store diagrams and any source specifications in
`slides/img/`.

### Slide Review and Copilot Guidance

Use the shared `marp-slide-review` skill before presenting or publishing. Follow
the installed skill's setup and command instructions for overflow checks, then
inspect the rendered HTML/PDF for footer overlap, padding, and readability.

The [repository instructions](.github/copilot-instructions.md) document the
authoring conventions and shared skills used by this deck.

## Demos

The repository includes a variety of demos that demonstrate the principles of Azure resiliency in different scenarios. These demos can help you understand how to implement resilient architectures effectively.

## Resources

For more information on Azure resiliency and best practices, you can refer to the following resources:

- [Azure Well-Architected Framework](https://learn.microsoft.com/en-us/azure/well-architected/)
- [Reliability design principles](https://learn.microsoft.com/en-us/azure/well-architected/reliability/principles)
- [Reliability maturity model](https://learn.microsoft.com/en-us/azure/well-architected/reliability/maturity-model)
- [Well-Architected design guides (Design essentials)](https://learn.microsoft.com/en-us/azure/well-architected/design)
- [Azure Well-Architected Review assessment](https://learn.microsoft.com/en-us/assessments/azure-architecture-review/)
- [Azure Availability Zones](https://learn.microsoft.com/en-us/azure/reliability/availability-zones-overview)
- [Azure Resiliency Checklist](https://learn.microsoft.com/en-us/azure/reliability/)

## Connect with Chris Ayers

Feel free to connect with Chris Ayers on social media and visit his blog for more information on Azure resiliency and other topics:

- BlueSky: [@chris-ayers.com](https://bsky.app/profile/chris-ayers.com)
- LinkedIn: [chris-l-ayers](https://linkedin.com/in/chris-l-ayers/)
- Blog: [https://chris-ayers.com/](https://chris-ayers.com/)
- GitHub: [Codebytes](https://github.com/codebytes)
- Mastodon: [@Chrisayers@hachyderm.io](https://hachyderm.io/@Chrisayers)

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for more information.