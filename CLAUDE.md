# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Interactive world map showing visited countries. Static site with vanilla JavaScript, no build step required. Forked from aslushnikov/been and localized to Portuguese (pt-BR).

## Development Commands

```bash
npm start    # Starts light-server at http://localhost:8000
```

No build process - this is a static site served directly.

## Architecture

**Three core classes in script.js:**

- **Country** (lines 1-38): Manages individual country state, connects map SVG paths to sidebar list items via Symbol property for bidirectional lookup
- **Region** (lines 40-45): Groups countries by geographic region (Europa, Ásia, África, etc.)
- **Map** (lines 65-132): Main orchestrator - loads compressed SVG, parses countries.md, handles zoom/pan animations with anime.js

**Data flow:**
1. `Map.create()` fetches and decompresses `world.svg.gz` using DecompressionStream API
2. Parses `countries.md` markdown to get country list and visited status
3. Creates Country instances that link SVG `<path>` elements to sidebar `<li>` elements
4. Click handlers toggle visited state and trigger zoom animations

**Key files:**
- `countries.md` - Data source; format: `- [x] CC CountryName` where `[x]` = visited, `[ ]` = not visited
- `world.svg.gz` - Compressed world map SVG with country paths identified by 2-letter ISO codes
- `style.css` - CSS Grid layout (3-column desktop, 1-column mobile at 600px breakpoint)

## Browser Requirements

Uses modern APIs: `DecompressionStream`, ES6 classes. Includes Safari-specific CSS prefixes and `scrollIntoViewIfNeeded` polyfill.

## Customization

To mark countries as visited, edit `countries.md` and change `[ ]` to `[x]` for the relevant country entries.
