# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Pearl is a single-file web application for composition classrooms. Students enter words or phrases tracing their brainstorming progression; the app renders them as concentric iridescent rings forming a pearl, with the seed idea at the center.

## Running the app

Open `index.html` directly in a browser — there is no build step, server, or package manager.

To preview quickly from the terminal:

```bash
open index.html
```

## Architecture

Everything lives in `index.html` as one self-contained file: HTML structure, CSS, and JavaScript.

**State** — a single `ideas` array. Index 0 is the seed; each subsequent index is a new layer.

**Canvas rendering** (`draw()`) — three-pass approach:
1. Ring fills painted outer → inner so each fill covers the one beneath it
2. Pearl luster highlight gradient (sits above fills, below text)
3. Text painted outer → inner (`arcText` for rings, `centerText` for the seed)

The canvas resizes on every `draw()` call: `size = (SEED_R + (n-1) * RING_W) * 2 + PAD * 2`, so the pearl always fills the canvas at consistent proportions regardless of layer count.

**Color palette** — `COLORS` array (8 entries) cycles by layer index. Each entry has five values: highlight, two radial-gradient fill stops, edge stroke, and ink (text) color.

**`arcText`** — draws characters one at a time along an arc, centered at −π/2 (12 o'clock). Font size auto-shrinks if the text span would exceed 300°.

**Student-facing copy** — the two prompt strings come directly from `Pearl idea.docx` and are hardcoded in `updatePrompt()`. Edit there to change the language students see.
