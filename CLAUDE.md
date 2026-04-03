# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

PIXEL BLASTER is a self-contained retro top-down shooter game implemented as a single `index.html` file (~865 lines). No build tools, bundler, package manager, or external dependencies — just vanilla HTML5 Canvas and JavaScript.

## Running

Open `index.html` directly in a browser. No server required (though `python3 -m http.server` works if needed for AudioContext restrictions).

## Architecture

The entire game lives in `index.html` inside a single `<script>` tag, organized into numbered sections:

- **CONFIG / PAL** (constants): Game tuning values and color palette — change gameplay feel here
- **SFX** (audio engine): Procedural sound via Web Audio API oscillators and noise buffers — no audio files
- **Sprite system**: Character art defined as string arrays with single-char color keys mapped through `SPRITE_COLORS`; rendered pixel-by-pixel via `drawSprite()`
- **Game state machine**: `STATE.MENU → PLAYING → LEVEL_COMPLETE → GAME_OVER`, driven by the `game` object
- **Entity classes**: `Player`, `Bullet`, `Enemy`, `Particle` — each has `update()` and `draw()` methods
- **Wave/level system**: `generateLevel()` procedurally creates wave definitions based on level number; enemy types unlock at level 2 (fast), 4 (tank), every 5th (boss)
- **Game loop**: Fixed-timestep (60fps) with accumulator pattern in `gameLoop()`; `update()` and `render()` are separate

## Key Design Patterns

- All rendering uses raw Canvas 2D (`fillRect`, no images/sprites loaded externally)
- Collision is circle-vs-circle (`circlesCollide`)
- Screen shake, damage flash, and muzzle flash are frame-counted timers on the `game` object
- Input state is polled each frame via the `input` singleton (keyboard + mouse)
- Particles are capped at 200 to prevent performance degradation

## Workflow

After completing a unit of work, commit to Git with a clean, descriptive commit message and push to GitHub. Do this incrementally — don't let work accumulate uncommitted. This ensures we never lose progress.
