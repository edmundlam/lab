# AGENTS.md

This file provides guidance to AI Agents when working with code in this folder.

## Project Purpose

Parent-child coding collaboration for creating simple HTML/CSS/JavaScript games. The focus is on learning together through hands-on coding rather than production software.

**Target audience:** Kids (young to middle ages, ~5-12) learning to code with a parent.

**Goals:**
- Create simple, playable games that can run locally and be shared via GitHub Pages
- Keep code approachable for pair programming sessions
- Start with single-file games, evolve to shared components only when patterns emerge naturally
- Prioritize learning and exploration over architecture purity

## Game Types of Interest

- **Typing games** (first game built: Flower Typing Garden)
- Educational games
- Puzzle games
- Simple arcade/action games

Each game will have its own visual theme and style, driven by the kids' ideas.

## Technical Approach

- **Stack:** Pure HTML/CSS/JavaScript to start
- **No build tools** initially — games should open directly in a browser
- **Single-file games** per folder — `index.html` is the final playable artifact, with embedded CSS/JS
- **Companion docs** (spec.md, design.md, etc.) are fine for planning but not published — only `index.html` needs to be deployable
- **Evolution:** Refactor to shared components only when repetitive patterns emerge during coding sessions
- **GitHub Pages:** Should be deployable when a game feels "done"
- **localStorage:** Use browser localStorage for saving progress (stars, unlocks, etc.) when appropriate

## Coding Style for Kids

- **Keep it simple:** Avoid over-architecting. A 100-line working game is better than a perfectly structured 300-line one.
- **Comment for learning:** Explain what things do in kid-friendly terms when complex
- **Pair programming focus:** Code should be readable and explainable in a coding session
- **Working > perfect:** Get something playable fast, then iterate together

## Folder Structure

```
kids/                         # This folder
├── CLAUDE.md                  # Entry point (references this file)
├── AGENTS.md                  # This file — guidance for AI agents
├── typing/
│   └── flower-game/
│       ├── index.html         # Self-contained playable game
│       ├── spec.md            # Game spec (not published)
│       └── design.md          # Design doc (not published)
└── [category]/
    └── [game-name]/
        └── index.html         # The essential artifact for each game
```

**Don't add structure prematurely.** Let the games and patterns dictate organization.

## Development

- **Run games:** Open `index.html` directly in a browser
- **Deploy:** Push to GitHub, enable GitHub Pages on the repo
- **No tests required:** This is exploratory learning, not production software
- **No linters/formatters initially:** Keep the barrier to entry low for kids

## When Adding New Games

1. Create a category folder if one doesn't fit an existing category
2. Create a subfolder named after the game
3. Add a single `index.html` with embedded CSS/JS — this is the must-have artifact
4. Optionally add `spec.md` or `design.md` for planning (not required)
5. Make it playable as soon as possible
6. Focus on fun and learning moments over code quality

The kids should drive the game ideas — your role is to help make them real and explain concepts as you go.