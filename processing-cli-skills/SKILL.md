---
name: processing-cli-skills
description: Processing 4 CLI commands (build, run, export, format) and Processing Java programming API reference. macOS (Apple Silicon and Intel). Processing 4.5.2.
---

# Processing Skill

Processing 4.5.2 on macOS at `/Applications/Processing.app/Contents/MacOS/Processing`.

## When to use what

| User asks about | Read this |
|---|---|
| Build, run, export, format, sketchbook, contributions, lsp | `references/cli.md` |
| Shapes, colors, images, text, math, PVector, data, input, output, transforms, 3D, rendering | `references/java-api.md` |
| Example sketches, demos, how to do X (e.g. "particles", "noise", "shaders", "GUI") | `references/examples.md` |
| Multiple | Read multiple |

## Core rules (always apply)

- Processing binary: `/Applications/Processing.app/Contents/MacOS/Processing`
- `processing-java` removed in Processing 4 — always use `Processing cli`
- Sketch = folder with matching `.pde` file: `MySketch/MySketch.pde`
- `--sketch` points to folder, not `.pde` file
- `--run`/`--build`/`--present`/`--export` must be the last Processing argument
- Arguments after those pass through to the sketch's `args[]`
- `size()` must be first line in `setup()` (or use `settings()` for variable sizes)
- Version check: `Processing --version` → `processing-4.5.2-1313`
