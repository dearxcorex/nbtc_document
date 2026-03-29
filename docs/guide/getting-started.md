---
title: Getting Started
tags:
  - guide
  - setup
---

# Getting Started

## Prerequisites

!!! warning "Requirements"

    Make sure you have the following installed before proceeding.

- Python 3.11+
- [uv](https://docs.astral.sh/uv/) package manager
- [Obsidian](https://obsidian.md) (for editing)

## Installation

### 1. Install dependencies

```bash
uv sync
```

### 2. Start local server

```bash
uv run zensical serve
```

The site will be available at `http://localhost:8000`.

## Editing with Obsidian

1. Open Obsidian
2. Select **Open folder as vault**
3. Navigate to `nbtc_document/docs/`
4. Start editing!

!!! tip "Markdown Links"

    Use standard Markdown links like `[Page Title](path/to/page.md)` for cross-page navigation.

## Building for Production

```bash
uv run zensical build
```

The output will be in the `site/` directory.
