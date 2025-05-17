# 📘 Deno Forge Brand Guide (v0.1)

## 🧭 Brand Identity

- **Name:** Deno Forge  
- **Tagline (WIP):** *“Forged with intent. Tempered with tests.”*  
- **Tone:** Confident, practical, a bit mythic. Slightly witty. Never shouty.

## 🎭 Character System: *The Deno Smiths*

- Each Deno Smith is:
  - A dinosaur
  - A specialist or metaphor for a module (e.g., testing, formatting)
  - Drawn in a consistent, friendly style
  - An avatar of craftsmanship

- Each module may feature:
  - A unique Deno Smith variant
  - An inline avatar in the README or CLI
  - Optional lore in the docs (e.g., “This module was first forged by Flint the Swift”)

## 🔨 Product Principles: FORGE

**F**ocus — One tool, one purpose  
**O**penness — Transparent and human-first  
**R**eliability — Tested and production-grade  
**G**rit — Code doesn’t write itself  
**E**cosystem-first — Plays well with others

Include these (optionally) in every module README.

## 🎨 Visual Style

| Element        | Style Recommendation                      |
|----------------|--------------------------------------------|
| **Colors**     | Fire orange, charred gray, molten yellow   |
| **Fonts**      | Cinzel (titles), IBM Plex Sans (body)      |
| **Emojis**     | 🦖 ⛏️ 🔥 🧱 ⚒️ 🛠️                         |
| **Theme**      | Black background, orange or gold accent    |
| **Icons**      | Modules = tools (hammer, tongs, etc)       |

## 📦 Module Naming Guidelines

All modules are published under the `@deno-forge` scope.

Module names are **thematically aligned with forgework**, and may be either:

- **Nouns** that represent tools or materials (e.g., `steel`, `furnace`, `tongs`)
- **Verbs** commonly used as CLI commands (e.g., `scaffold`, `quench`)

This allows for expressive, purpose-driven names while preserving clarity and command-line ergonomics.

**Examples:**
- `@deno-forge/scaffold`
- `@deno-forge/quench`
- `@deno-forge/steel`
- `@deno-forge/format`

## 🧠 Voice & Tone Guidelines

| Situation           | Voice Example                                  |
|---------------------|------------------------------------------------|
| CLI Welcome         | “🦖 Welcome to Deno Forge.”                     |
| Error Message       | “⚠️ That input’s not quite forged right.”       |
| Help Section        | “Use `--init` to strike the first blow.”       |
| README Tone         | Confident, instructional, with a spark of humor|

Avoid corporate stiffness. Avoid meme-speak. Think: *skilled craftsperson in a forge who also writes clean docs.*

## 💡 Future Additions

- CLI splash screens per Smith
- Auto-generator for Smith avatars
- JSR badge with rotating forge principles
- Static site: **deno-forge.dev** or **hall-of-smiths.dev**