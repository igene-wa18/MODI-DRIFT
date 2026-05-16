# MODI DRIFT — Cinematic Obstacle Dodger (Single-File HTML Game)

**MODI DRIFT** is a **self-contained** browser game built with **HTML + CSS + JavaScript** (Canvas 2D). It features neon visuals, animated obstacles (static / moving / rotating / pulsing), optional lasers, a portal-based objective, lives, scoring, sound effects + background music, and a **match-result scoreboard overlay** with embedded win/lose videos.

> File: `index.html`

---

## How to Run

1. Double-click `index.html`.
2. Or open it in your browser (Chrome/Edge recommended).

No build step is required.

---

## Gameplay Overview

You control a neon “ball” that:

- **Flies upward** when you press/tap (space / tap / W).
- **Falls with gravity** when released.
- Must **dodge pillars (obstacles)** and survive with **3 lives**.
- Must **reach the portal** to clear the current level.
- When you reach the portal on the final level, a **WIN** overlay is shown.
- When you run out of lives (hit an obstacle), a **LOSE** overlay is shown.

---

## Controls

### Fly / Thrust
- **Space** / **↑ Arrow** / **W**
- **Mouse down** (click/tap on the canvas)

### Pause / Resume
- **Esc** / **P**

---

## UI Screens

### 1) Start Menu
- Title: **MODI DRIFT**
- Buttons:
  - **Start Game**
  - **How to Play**

### 2) How to Play (Overlay)
Lists controls and the goal:
- Fly to dodge obstacles.
- Reach the **portal**.
- Have **3 lives**.
- Near-miss bonus system.

### 3) Level Intro (Transition)
A short **LEVEL intro overlay** before gameplay begins.

### 4) HUD (During Gameplay)
- **Level indicator**
- **Lives** (3 neon orbs)
- **Score**
- **Progress bar**: “Distance to Portal”

### 5) Pause Menu
- **Resume**
- **Quit to Menu**

### 6) Scoreboard Overlay (Win/Lose)
A full-screen overlay that shows:
- Left: **reel-style vertical video panel**
- Right: **match result**
- Score counters for **MODI** and **RAHUL**
- Buttons:
  - **Play Again**
  - **Main Menu**

---

## Scoring & Mechanics

### Lives
- Player starts with **3 lives**.
- Hitting an obstacle (pillars) reduces lives and triggers:
  - camera shake
  - hit flash
  - floating text: **“-1 LIFE”**
  - collision sound

### Near Miss Bonus
- Obstacles detect a “near miss” window around their bounds.
- If you pass very close without colliding, you gain:
  - **+50 score**
  - **combo** increments
  - on-screen “NEAR MISS!”
  - optional **combo text** when combo > 1

### Portal Win
- The portal has an opening animation.
- When the ball reaches the portal:
  - score increases (**+200**) 
  - slow motion effect briefly triggers
  - after a short delay, the game advances to the next level

---

## Levels

The game defines **8 levels** in `LevelData`:

1. **The Beginning**
2. **Narrow Paths**
3. **Shifting Winds** (moving obstacles)
4. **Spinning Doom** (rotating hazards)
5. **Laser Grid** (lasers on/off + moving/rotating)
6. **Pulse Chamber** (pulsing walls)
7. **The Gauntlet** (everything at once)
8. **Void's End** (final test)

Each level configures:
- length (time/distance)
- speed
- gravity
- flap strength
- obstacle gap / width
- obstacle behaviors: `movingObstacles`, `rotatingObstacles`, `lasers`, `pulsing`
- density affects obstacle spacing

---

## Visual Design

### Canvas Rendering
The game draws everything on `#gameCanvas` using Canvas 2D:
- Neon background grid and starfield
- Obstacle pillars with gradients + glow
- Rotating obstacles use a rotating arm-like gradient and a central hub
- Pulsing obstacles expand/contract via sinusoidal scaling
- Lasers: flickering red beams with dashed warning section
- Portal: radial glow + rotating arcs + particle sparks
- Ball: outer neon glow + optional face image clipped into a circular region

### Images / Assets (Optional)
The script supports optional embedded images:
- `BALL_IMAGE_SRC` — image clipped on the ball
- `PILLAR_IMAGE_SRC` — circular badge on pillars
- `PORTAL_IMAGE_SRC` — image in the portal center
- `GATE_IMAGE_SRC` — floating badge above the portal gate

If these are empty/invalid, the game uses glowing shapes instead.

---

## Audio

### Background Music
- Audio element: `#bgMusic`
- Current source is set to the local mp3 file name in this directory:
  - `Dope Shope Song by Narendra Modi Yo Yo honey Singh Narendra Modi voice dubbed songs mp3.mp3`

The code starts playback on **Start Game**.

### Hit Sound
- Audio element: `#hitSound`
- Intended to play on collisions.
- The code also generates synthesized collision tones.

---

## Embedded Win / Lose Videos (Scoreboard)

Two HTML `<video>` elements are embedded:
- `#winVideo` — uses: `VID_20260510_043400_737.mp4`
- `#loseVideo` — uses: `VID_20260510_073715_973.mp4`

If video sources aren’t available, the UI shows a **“Embed video in code”** placeholder.

---

## Code Structure (Within the Single HTML File)

### DOM Elements
- `#gameCanvas`
- Overlays:
  - `#startMenu`, `#howToPlay`, `#levelIntro`
  - `#hud`, `#pauseMenu`
  - `#scoreboard`
- Reusable components inside scoreboard:
  - `#winVideo`, `#loseVideo`, player score counters

### Key JavaScript Parts
- **Audio helpers**: `playBgMusic`, `playHitSound`, `AudioSys.*`
- **Game state machine**: `Game.state` = `menu | intro | playing | paused | dying | gameover | victory`
- **Entities**:
  - `Player` (physics + draw + damage)
  - `Obstacle` (static/moving/rotating/pulsing)
  - `Laser` (on/off cycle + collision)
  - `Portal` (opening animation + collision)
- **Level generation**: `generateLevel(levelNum)` using `LevelData`
- **Main loop**: `gameLoop(timestamp)` via `requestAnimationFrame`

---

## Customization Guide

### 1) Replace Background Music
Edit the `src` attribute inside:
```html
<audio id="bgMusic" ...>
  <source src="YOUR_MUSIC_FILE.mp3" type="audio/mpeg">
</audio>
```

### 2) Replace Hit Sound
Edit:
```html
<audio id="hitSound" ...>
  <source src="YOUR_HIT_SOUND.mp3" type="audio/mpeg">
</audio>
```

### 3) Replace Win / Lose Videos
Edit the `<source>` tags:
- `#winVideo` → `YOUR_WIN_VIDEO.mp4` / `webm`
- `#loseVideo` → `YOUR_LOSE_VIDEO.mp4` / `webm`

### 4) Replace Ball/Pillar/Portal Images
In the JS “Embed configuration” section at the top:
- `const BALL_IMAGE_SRC = '...'
- `const PILLAR_IMAGE_SRC = '...'
- `const PORTAL_IMAGE_SRC = '...'
- `const GATE_IMAGE_SRC = '...'

You can set them to local filenames (if placed next to the HTML) or to remote URLs.

---

## Notes / Compatibility

- Runs in modern browsers supporting Canvas, Web Audio, and MP4 video playback.
- Some browsers require user interaction before autoplaying audio (the game starts music after clicking “Start Game”).

---

## Credits / Branding

The scoreboard and UI text refer to **MODI** vs **RAHUL** and uses the title **MODI DRIFT**.

---

## File Summary

- `neon-drift-upgraded-2.html` — full game implementation (UI + game engine + overlays)
- `VID_20260510_043400_737.mp4` — win video
- `VID_20260510_073715_973.mp4` — lose video
- `Dope Shope Song by Narendra Modi Yo Yo honey Singh Narendra Modi voice dubbed songs mp3.mp3` — background music (referenced by the audio tag)

