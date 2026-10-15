# Serverless (FaaS) vs. Containers: When to Pick Which?

Lecture slides for CSCI 40500/77100 (Software Engineering), CUNY Hunter College, Fall 2026. Compares serverless computing (Backend as a Service and Functions as a Service) with containers: what each is, examples, the features and limits of FaaS, pros and cons, how the two have converged, and how to choose between them, with the 2023 Prime Video case study.

## Viewing

Open `serverless_containers.html` in any web browser. The deck uses [Slidy](https://www.w3.org/Talks/Tools/Slidy2/): advance with the arrow keys (or space), or by clicking; press `c` or the "Contents" button for the table of contents. The distributed `serverless_containers.html` is a single self-contained file and works fully offline.

## Building from Source

The slides are written in Pandoc Markdown (`serverless_containers.md`) and built with [Pandoc](https://pandoc.org) (version 2.11 or newer, which bundles the `slidy` writer, so no extra installs are needed):

```bash
make                  # build serverless_containers.html
make self-contained   # single-file HTML with resources inlined (for distribution)
```

See the `Makefile` for the exact Pandoc invocation and the other targets.

Source files:

- `serverless_containers.md`—the slide content (edit this)
- `header.html`—CSS and JavaScript injected into the document `<head>`
- `graphics/`—figures

The `make deploy` target is for the author's own web host and relies on an ssh alias (`compsci`) defined in `~/.ssh/config`; adopters can ignore it.

## Attribution

Based on the "[Serverless (FaaS) vs. Containers](https://s3.amazonaws.com/files.commons.gc.cuny.edu/wp-content/blogs.dir/2880/files/2021/05/Serverless_vs_containers.pdf)" student presentation from Software Engineering (CSCI 40500/77100), Hunter College, Spring 2021. The pet store and ad click examples are adapted from "[Serverless Architectures](https://martinfowler.com/articles/serverless.html)" by Mike Roberts (martinfowler.com, 2018) and redrawn as `graphics/pet-store-traditional.svg`, `graphics/pet-store-serverless.svg`, and `graphics/ad-clicks.svg`. The pros and cons follow "[Serverless (FaaS) vs. Containers—When to Pick Which?](https://www.serverless.com/blog/serverless-faas-vs-containers)" by Philipp Müns (Serverless, Inc., 2017). Added since the original: the essential qualities, limits, and costs of serverless from "[What Serverless Computing Is and Should Become](https://doi.org/10.1145/3406011)" by Schleier-Smith et al. (*Communications of the ACM*, 2021), and the case study from "[Scaling Up the Prime Video Audio/Video Monitoring Service and Reducing Costs by 90%](https://web.archive.org/web/2023/https://www.primevideotech.com/video-streaming/scaling-up-the-prime-video-audio-video-monitoring-service-and-reducing-costs-by-90)" by Marcin Kolny (Prime Video Tech, 2023).
