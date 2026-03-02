# Barney's Bins — A Vibe Coding Learning Guide
### How an AI Built a Complete Browser Game From Scratch

---

> **What is Vibe Coding?**
> Vibe coding is the practice of describing what you want in plain English to an AI assistant, then letting the AI write the actual code. You guide the *vision*, the AI handles the *implementation*. You don't need to know how to code — you just need to know what you want to build. This document walks through exactly how Barney's Bins was created, explaining every technical choice in plain language.

---

## Table of Contents

1. [The Big Picture — What Are We Building?](#1-the-big-picture)
2. [The Single File Approach](#2-the-single-file-approach)
3. [Setting the Stage — HTML Structure](#3-html-structure)
4. [Making It Look Right — CSS Styling](#4-css-styling)
5. [The Colour Palette — The Paint Box](#5-the-colour-palette)
6. [Pixel Art Sprites — Drawing With Numbers](#6-pixel-art-sprites)
7. [The Sprite Renderer — Turning Numbers Into Pictures](#7-the-sprite-renderer)
8. [Sound Effects — Making Noise Without Audio Files](#8-sound-effects)
9. [Game Constants — The Rules Written In Stone](#9-game-constants)
10. [Game State — The Game's Memory](#10-game-state)
11. [The Entity System — A Template For Everything That Moves](#11-the-entity-system)
12. [Drawing the World — Layers Like a Stage Set](#12-drawing-the-world)
13. [Drawing the Wagon — Barney's Big Green Truck](#13-drawing-the-wagon)
14. [Obstacles and Collectibles — The Stuff on the Road](#14-obstacles-and-collectibles)
15. [The Particle System — Sparks, Smoke and Stars](#15-the-particle-system)
16. [The HUD — The Dashboard at the Top](#16-the-hud)
17. [Spawning — How Obstacles Appear](#17-spawning)
18. [Collision Detection — Did We Hit Something?](#18-collision-detection)
19. [The Game Loop — The Heartbeat of the Game](#19-the-game-loop)
20. [Input Handling — Listening to the Player](#20-input-handling)
21. [The Three Screens — Menu, Game, Game Over](#21-the-three-screens)
22. [How The AI Was Asked To Build This](#22-how-the-ai-was-asked-to-build-this)

---

## 1. The Big Picture

**What was built:** A fully playable retro-style arcade game that runs in any web browser — no downloads, no installs. You drive Barney's bin lorry down a road, collect wheelie bins, dodge potholes and angry neighbours, and try to get the highest score.

**The technology:** The entire game is built using three things:
- **HTML** — the structure (like the walls of a building)
- **CSS** — the presentation (like the paint and wallpaper)
- **JavaScript** — the behaviour (like the electricity and plumbing)

All three live inside a single file called `index.html`. Open it in a browser and the game runs instantly.

**The key technology inside JavaScript is the HTML Canvas.** Think of it like a digital whiteboard. JavaScript draws rectangles, fills them with colour, and does this 60 times per second — so fast that it looks like animation.

---

## 2. The Single File Approach

Most websites are spread across dozens of files. This game deliberately lives in **one file** — about 1,280 lines. The reasons for this:

- **Simplicity** — you can share the game by sending one file
- **No server needed** — just double-click and it opens
- **Easier to understand** — everything is in one place

The vibe coding prompt that led to this decision was essentially: *"Make it a self-contained single HTML file, no external libraries."*

---

## 3. HTML Structure

```
<!DOCTYPE html>          ← Tells the browser "this is a modern webpage"
<html lang="en">         ← The outer wrapper; lang="en" means English
<head>                   ← Setup info (not visible on screen)
  <meta charset="UTF-8"> ← Support for all characters/emoji
  <meta name="viewport"> ← Makes it behave on phones
  <title>...</title>     ← The browser tab text
  <style>...</style>     ← CSS lives here
</head>
<body>                   ← Everything the player sees
  <div id="wrap">        ← A box that holds the canvas and controls
    <canvas id="game">   ← THE DIGITAL WHITEBOARD (800×450 pixels)
    <div id="controls">  ← The key guide text shown below the game
  </div>
  <script>...</script>   ← All the game code lives here
</body>
```

**What to notice:** The `<canvas>` tag is just a blank rectangle. JavaScript will draw everything inside it — every cloud, every house, every spinning wheel. The canvas is defined as 800 wide and 450 tall (these are game pixels, not screen pixels).

---

## 4. CSS Styling

CSS is responsible for just four things here:

| What | Why |
|------|-----|
| `background: #0d0d1a` | Very dark navy background — matches the night-sky game feel |
| `display: flex; align-items: center` | Centres the game perfectly in the browser window, on any screen size |
| `border: 4px solid #FFD700` | Gold border around the canvas — gives it that arcade cabinet feel |
| `image-rendering: pixelated` | Tells the browser NOT to blur the pixels when the canvas is scaled up — essential for pixel art to look sharp |

The `font-family: 'Courier New', monospace` is used throughout to give the game a retro computer terminal aesthetic.

---

## 5. The Colour Palette

```javascript
const COL = {
  skin:   '#FFCC99',
  hivis:  '#FFD700',
  wagMd:  '#2E7D32',
  ...
};
```

**What this is:** A master list of every colour used in the game, all named and stored in one place.

**Why it's done this way:** Instead of scattering colour codes like `#FFD700` all over the code (impossible to remember or change), they're all stored here with descriptive names. If Barney's hi-vis jacket needed to change colour, you'd only change it in one place.

**The naming system:**
- `wagDk`, `wagMd`, `wagLt` = wagon Dark, Medium, Light (the three shades of green on the truck)
- `sky1`, `sky2`, `sky3` = three shades of blue sky from dark at the top to lighter at the horizon
- `uiGold`, `uiRed`, `uiGreen` = colours used in the User Interface (HUD/score display)

**Hex colour codes** like `#FFCC99` are how computers describe colours. The six characters describe Red, Green, Blue values. The AI chose all these colours deliberately to create a cohesive, warm, slightly retro palette.

---

## 6. Pixel Art Sprites

This is one of the most clever parts of the whole game. Each character — Barney's face, Mrs Miggins, Mr Grumpy, the rat, the wheelie bin — is defined as a **grid of numbers**.

```javascript
const S_BARNEY_FACE = {
  pal: {                          // ← The "palette" - what each number means
    1: '#FFCC99',  // skin
    2: '#E0A070',  // darker skin
    3: '#5C3317',  // dark brown hair
    ...
  },
  d: [                            // ← The "data" - the pixel grid
    [0,0,3,3,4,3,3,3,3,4,3,3,0,0],  // ← Row 0: hair top
    [0,3,4,3,3,3,3,3,3,3,3,4,3,0],  // ← Row 1: hair
    ...
  ]
};
```

**How to read it:**
- `0` = transparent (empty, skip this pixel)
- `1` = draw a pixel using colour #1 from the palette (skin tone)
- `3` = draw a pixel using colour #3 (dark brown hair)

Think of it like a **paint-by-numbers grid**. The grid is 14 columns wide and 17 rows tall. Each number tells the renderer what colour to fill that tiny square.

**Barney has three face variants:**
- `S_BARNEY_FACE` — normal happy expression
- `S_BARNEY_BLINK` — eyes closed (rows 5-7 replaced with eyelid pixels)
- `S_BARNEY_OOF` — grimace when hit (mouth rows changed to a straight line)

The blink and grimace variants are created by **copying** the normal face and changing just a few rows — smart reuse of existing work:

```javascript
const S_BARNEY_BLINK = JSON.parse(JSON.stringify(S_BARNEY_FACE)); // exact copy
S_BARNEY_BLINK.d[6] = [0,1,3,3,3,3,1,1,3,3,3,3,1,0]; // replace eyes with closed lids
```

---

## 7. The Sprite Renderer

```javascript
function drawSprite(spr, x, y, scale, flipX) {
```

This single function handles drawing **every sprite** in the game. Here's how it works:

1. **Loop through every row** in the sprite grid (top to bottom)
2. **Loop through every column** in each row (left to right)
3. **Look up the number** at that position
4. **Skip it if it's 0** (transparent)
5. **Find the colour** for that number in the palette
6. **Draw a filled rectangle** at the correct screen position

The rectangle size is controlled by `P * scale`, where `P = 3`. So one "pixel" in the sprite grid becomes a 3×3 block on screen — giving that chunky pixel art look.

The `flipX` option mirrors the sprite horizontally, so Mrs Miggins and Mr Grumpy (who walk from right to left on the pavement) face the correct direction without needing separate reversed sprite data.

---

## 8. Sound Effects

The game has no audio files at all — every beep, honk, and jingle is **generated mathematically** in real time.

```javascript
function playTone(freq, dur, type, vol) {
  const o = audioCtx.createOscillator();  // a sound wave generator
  const g = audioCtx.createGain();        // a volume controller
  o.type = 'square';                      // square waves = 8-bit retro sound
  o.frequency.setValueAtTime(freq, ...);  // pitch (Hz)
  g.gain.exponentialRampToValueAtTime(0.001, ...); // fade out
}
```

**The Web Audio API** is a built-in browser feature that can generate sound waves. It's like a tiny synthesiser.

**Sound types used:**
- `'square'` wave — classic 8-bit video game sound
- `'sawtooth'` wave — harsher, used for the "hit" sound

**The sound effects:**
| Function | What it sounds like | How it works |
|----------|---------------------|--------------|
| `sfxCollect()` | Rising three-note ding | Plays 440Hz, then 660Hz, then 880Hz with tiny delays |
| `sfxHit()` | Low thud/crunch | Low sawtooth wave, getting lower |
| `sfxHonk()` | Beep-boop horn | Two square waves, slightly different pitches |
| `sfxLevelUp()` | Ascending fanfare | Five notes played 70ms apart, rising in pitch |

**Why no audio files?** Pure pragmatism — no files to load, no format compatibility issues, the game works instantly everywhere.

---

## 9. Game Constants

```javascript
const ROAD_TOP  = 155;   // pixels from top where road starts
const ROAD_H    = 270;   // road is 270 pixels tall
const LANE_H    = 90;    // each of the 3 lanes is 90 pixels tall
const LANE_Y    = [200, 290, 380]; // vertical centre of each lane
const WAGON_X   = 140;   // Barney never moves left/right, stays at X=140
const HIT_COOL  = 120;   // 120 frames of invincibility after being hit (2 seconds at 60fps)
```

**Constants are values that never change.** By naming them and keeping them at the top, the AI ensured:
- If the road needed moving, change `ROAD_TOP` in one place
- Everything that depends on it automatically recalculates
- The numbers are self-documenting — you know what they mean

**The lane system:** Instead of free movement, Barney snaps between three fixed lanes. This is a classic arcade mechanic — simple to control, easy to understand. The Y positions (200, 290, 380) are the vertical centres of these lanes.

---

## 10. Game State

```javascript
let state = 'menu';       // the game is currently on the menu screen
let score = 0, lives = 3, level = 1, hiScore = 0;
let speed = 2.5;          // how fast obstacles move (pixels per frame)
let barneyLane = 1;       // which lane Barney is currently in (0, 1, or 2)
let barneyTargetLane = 1; // which lane Barney is moving TOWARDS
let barneyY = LANE_Y[1];  // Barney's actual Y position (smoothly animated)
let invTimer = 0;         // countdown for post-hit invincibility
let barneyFace = 'normal';// which face expression to show
```

**Game state is the game's working memory.** Every piece of information the game needs to know at any moment is stored in these variables.

**The `state` variable** is particularly important — it controls which screen is shown:
- `'menu'` — show the title screen
- `'playing'` — run the game
- `'gameover'` — show the game over screen

**The two lane variables** (`barneyLane` and `barneyTargetLane`) are a clever trick. When you press up or down:
- `barneyTargetLane` changes immediately
- `barneyY` (the actual screen position) smoothly glides towards the target lane
- Once it arrives, `barneyLane` updates to match

This creates the smooth sliding movement between lanes rather than a jarring instant jump.

---

## 11. The Entity System

```javascript
class Entity {
  constructor(type, lane, x) {
    this.type = type;      // 'bin', 'pothole', 'miggins', 'rat', etc.
    this.lane = lane;      // which lane (0, 1, or 2)
    this.x = x;            // starting X position (off the right edge)
    this.y = LANE_Y[lane]; // Y position = the lane's centre
    this.active = true;    // still alive? if false, remove it
    this.frame = 0;        // counts up each tick, used for animation
  }
  update() {
    this.x -= speed * speedMult; // move left across the screen
    this.frame++;
    if (this.x < -200) this.active = false; // gone off screen? mark for removal
  }
}
```

**A class is a template.** Rather than writing separate code for every obstacle and collectible, one `Entity` template covers them all. Every bin, pothole, Mrs Miggins, rat, and coffee cup is created from this same blueprint.

**The `active` flag** is how things get removed. Rather than deleting mid-loop (which causes bugs), entities are simply marked as `active = false`. After each frame, the game filters out all inactive entities in one sweep.

**The `frame` counter** increases by 1 every game tick. Animation code uses this — for example, the rat alternates between two leg positions every 4 frames: `Math.floor(frame / 4) % 2 === 0`.

---

## 12. Drawing the World

The game world is drawn in **layers**, back to front — exactly like a stage set with backdrops, middle pieces, and foreground props:

```
Layer 1: Sky (solid colour bands, darkest blue at top)
Layer 2: Clouds (fluffy white rectangles, drifting slowly)
Layer 3: Buildings (terraced UK houses in the background)
Layer 4: Lamp posts (regularly spaced, scrolling)
Layer 5: Pavement (grey strip with slab lines)
Layer 6: Road (dark grey with lane markings)
Layer 7: Collectibles (bins, coffee cups)
Layer 8: Obstacles (potholes, characters)
Layer 9: Barney's wagon
Layer 10: Particles (smoke, sparks)
Layer 11: Score popups (+50, OUCH!, etc.)
Layer 12: UI / HUD (score, lives, level)
```

**Parallax scrolling** creates the illusion of depth. Things further away appear to move more slowly:
- Buildings scroll at 30% of road speed
- Lamp posts scroll at 80% of road speed
- Road markings scroll at 100% speed

This single trick makes a flat 2D game feel three-dimensional.

**The buildings** are procedurally generated — the code describes *rules* for drawing a house (where the walls, roof, windows, and doors go), then applies those rules repeatedly across the screen. Four house "templates" are mixed and tiled to create a believable street.

**Window lighting** uses a hash trick to decide if each window is lit or dark:
```javascript
const lit = ((Math.round(wx) * 7 + Math.round(wy) * 13) % 37) > 16;
```
This looks random but is actually consistent — the same window is always lit or dark, avoiding flickering as the building scrolls.

---

## 13. Drawing the Wagon

Barney's lorry is drawn entirely with rectangles — no sprite data needed because the vehicle is large and geometric enough to work well with basic shapes.

The wagon is assembled from parts, drawn in order (back to front):

```
1. Bin body section (dark green, with vertical panel ridges)
2. "BARNEY'S BINS" text + hi-vis stripe
3. Bin lift mechanism (grey hydraulic arms at the back)
4. Cab section (lighter green)
5. Windscreen (blue-grey glass with white glare highlight)
6. Barney's face sprite (inside the window)
7. Front grille (dark bars)
8. Headlight
9. Front bumper
10. Two wheels (with rotating lug bolts)
```

**The wheels** are particularly clever. A wheel is drawn as:
- A dark circle (approximated with many tiny rectangles placed around a centre point)
- A grey hub cap (solid rectangle)
- Five lug bolts (small rectangles rotated using trigonometry)

The bolt rotation is tied to `frameCount * 0.08` — so as the frame counter increases, the bolts rotate, giving the impression of a spinning wheel.

**The bounce effect:** When Barney hits a pothole, `wagonBounce` is set to 12. Every frame it decays (`wagonBounce -= 1.5`), and the wagon's Y position is offset by `Math.sin(wagonBounce * 0.8) * wagonBounce * 0.8`. This creates a damping oscillation — the wagon bounces and settles naturally.

---

## 14. Obstacles and Collectibles

**Potholes** are drawn procedurally — irregular dark ovals made from overlapping rectangles at slightly different sizes and offsets, with a crumbling edge effect using brown pixels.

**Rubbish piles** use sprite data — a 16×10 grid of overlapping black bags with splashes of orange, blue, and green (representing different bits of rubbish).

**Mrs Miggins** and **Mr Grumpy** each have full sprite character art and speech bubbles. The speech bubbles appear/disappear on a timer (`Math.floor(animFrame / 30) % 2 === 0` = visible for 30 frames, hidden for 30 frames, repeating).

**Collectibles bob up and down** to attract the player's attention:
```javascript
const bob = Math.sin(frameCount * 0.08 + c.x * 0.01) * 4;
```
`Math.sin` produces a smooth wave. The `+ c.x * 0.01` means each collectible bobs at a slightly different phase, so they don't all move in unison — more natural looking.

---

## 15. The Particle System

Particles are tiny floating squares used for visual feedback. Three types:

| Type | Used For | Behaviour |
|------|----------|-----------|
| `smoke` | Exhaust from wagon | Grey, drifts upward and to the right, grows as it fades |
| `spark` | Hit effects | Bright orange/red/yellow, burst outward in all directions |
| `star` | Collect effects | Gold, burst upward in a starburst pattern |

Each particle is an object with:
- Position (`x`, `y`)
- Velocity (`vx`, `vy`) — how fast and in what direction
- `life` — countdown to deletion
- `maxLife` — used to calculate how faded it is (`alpha = life / maxLife`)

Every frame, all particles are updated: position += velocity, life decreases by 1, velocity slows down slightly (`*= 0.97`). When `life` reaches 0, the particle is removed.

**Exhaust smoke** is generated regularly from the truck's exhaust pipe. `exhaustTimer` counts down; when it hits 0, a new smoke particle is spawned and the timer resets to a random value between 18 and 28 frames.

---

## 16. The HUD

HUD stands for "Heads-Up Display" — the score panel at the top of the screen.

```javascript
// Semi-transparent dark bar across the top
ctx.fillStyle = 'rgba(0,0,0,0.6)';
ctx.fillRect(0, 0, W, 36);
```

`rgba(0,0,0,0.6)` is black at 60% opacity — it darkens the game behind it without hiding it completely.

**The score display** pads with leading zeros:
```javascript
String(score).padStart(7, '0')  // 1234 becomes "0001234"
```
This is a classic arcade aesthetic — the score always shows 7 digits.

**The speed bar** is a mini bar chart:
```javascript
ctx.fillRect(400, 18, Math.min(80, (speed / 8) * 80), 12);
```
At speed 0, the bar is 0px wide. At max speed (8), it's 80px wide. The colour changes from green → orange → red as speed increases.

**Hearts for lives:** Each life is drawn as a pixel-art heart shape assembled from 7 rectangles. Filled hearts are red; lost hearts are dark grey.

---

## 17. Spawning

The spawn system controls the flow of the game — what appears and when.

```javascript
function getSpawnInterval() {
  return Math.max(40, 100 - level * 8 - Math.floor(score / 500) * 3);
}
```

This formula means:
- At level 1, a new entity spawns roughly every 92 frames (about 1.5 seconds)
- By level 5, it's every 60 frames (1 second)
- Never faster than every 40 frames (the minimum)

**Obstacle vs collectible ratio:**
```javascript
const obsChance = 0.45 + level * 0.06;
```
- Level 1: 45% chance of obstacle, 55% collectible (player-friendly start)
- Level 5: 75% chance of obstacle (game getting hard)

**Obstacle types** are chosen by rolling a random number and comparing ranges:
- 0.00 – 0.25 → pothole
- 0.25 – 0.50 → rubbish pile
- 0.50 – 0.75 → Mrs Miggins
- 0.75 – 1.00 → Mr Grumpy
- 15% additional chance to override with a rat (a faster-moving obstacle)

---

## 18. Collision Detection

Collision is the system that asks: "Has Barney hit something?"

**How it works:** The game defines a rectangular hitbox around the wagon, then checks if any obstacle or collectible's X position falls within that range AND is in the same lane.

```javascript
const wagLeft  = WAGON_X - 95;   // left edge of wagon
const wagRight = WAGON_X + 75;   // right edge
const wagTop   = barneyY - 40;   // top edge
const wagBot   = barneyY + 20;   // bottom edge
```

For each obstacle:
1. Skip it if it's in a different lane (and not the lane Barney is moving towards)
2. Skip it if its X position doesn't overlap with the wagon's X range
3. If it passes both checks — HIT!

**The target lane check** (`barneyTargetLane`) prevents a frustrating edge case: if Barney is halfway through moving into lane 2 and something is already there, the game correctly counts it as a collision even though `barneyLane` still shows lane 1.

**After a hit:**
- Life count decreases
- `invTimer` is set to 120 (2 seconds of invincibility — the wagon flickers)
- `flashTimer` triggers a red screen flash
- `wagonBounce` triggers the bounce animation
- Barney's face changes to the grimace expression
- A score popup appears with a contextual message (OUCH! / OI!! / SQUEAK!)
- Hit sound plays
- Sparks particles burst

---

## 19. The Game Loop

```javascript
function loop() {
  frameCount++;

  if (state === 'menu')     { bgScroll += 0.5; drawMenu(); }
  else if (state === 'playing') { update(); draw(); }
  else if (state === 'gameover') { bgScroll += 0.2; drawGameOver(); }

  requestAnimationFrame(loop);
}
```

This is the **heartbeat of the entire game**. It runs approximately 60 times per second.

**`requestAnimationFrame`** is the browser's way of saying "call this function before the next screen refresh." This keeps the game perfectly synchronised with the monitor — smooth, efficient, and the browser can pause it when you switch tabs.

**Every frame:**
1. `frameCount` increases by 1 (the game's internal clock)
2. The current screen is determined by `state`
3. If playing: `update()` runs all game logic, then `draw()` renders everything
4. The function schedules itself to run again next frame

**`update()` and `draw()` are kept separate** — a good practice:
- `update()` only changes numbers (positions, timers, scores)
- `draw()` only reads those numbers and paints the screen
- This makes it easy to understand, debug, and modify either half independently

---

## 20. Input Handling

```javascript
document.addEventListener('keydown', e => {
  if (keys[e.code]) return;  // ignore if key already held
  keys[e.code] = true;
  ...
  if (e.code === 'ArrowUp' || e.code === 'KeyW') barneyTargetLane--;
  if (e.code === 'ArrowDown' || e.code === 'KeyS') barneyTargetLane++;
  if (e.code === 'Space' || e.code === 'KeyH') sfxHonk();
});
```

**Key input is event-driven.** The browser tells JavaScript when a key is pressed (`keydown`) and when it's released (`keyup`). The game stores which keys are currently held in the `keys` object.

**The `if (keys[e.code]) return`** check prevents "key repeat" — without it, holding down arrow up would fire the lane-change many times.

**Touch controls** work on the same principle but use swipe distance:
- Swipe up more than 20px → move lane up
- Swipe down more than 20px → move lane down
- Tap (swipe less than 15px) → honk

**`e.preventDefault()`** stops the browser's default behaviour for space bar (scrolling the page) and arrow keys (scrolling the page).

---

## 21. The Three Screens

The game has three distinct screens controlled by the `state` variable:

**Menu Screen (`state = 'menu'`)**
- Draws the animated background (clouds, buildings) — the world slowly scrolls past even on the menu
- Displays the title card (dark panel with gold border)
- Shows a mini version of Barney's face sprite
- Instructions text
- A blinking "PRESS ENTER OR SPACE TO START" message (visible for 30 frames, hidden for 30 frames)

**Game Screen (`state = 'playing'`)**
- Calls `update()` then `draw()` every frame
- Full gameplay

**Game Over Screen (`state = 'gameover'`)**
- Calls `draw()` first (the frozen game state is visible underneath)
- Overlays a semi-transparent dark panel
- Shows final score, whether it's a new best, and the grimace-face Barney sprite

**`startGame()`** resets every variable to its starting value — score to 0, lives to 3, speed to 2.5, all arrays emptied. This ensures each playthrough starts fresh.

---

## 22. How The AI Was Asked To Build This

Vibe coding works through a conversation. Here's how the Barney's Bins project came together:

**The initial prompt was something like:**
> *"Build me a single-file HTML5 canvas game. The player drives a green bin lorry called Barney's Bins down a road. Three lanes. Collect wheelie bins, dodge obstacles. Pixel art style. Retro. UK setting."*

From that description, the AI made dozens of decisions:
- Chose the 800×450 canvas size (classic arcade aspect ratio)
- Designed the sprite system using numbered grids
- Invented the character cast (Mrs Miggins in her dressing gown, Mr Grumpy with his newspaper, the rat)
- Chose Web Audio API for sound (no files needed)
- Designed the parallax scrolling system
- Set the game balance (lives, speed curve, spawn rates)

**Refinements through conversation:**
> *"Give Barney a big friendly face visible through the cab window"*
> *"Add a blinking animation so he feels alive"*
> *"Make Mrs Miggins angry with a speech bubble"*
> *"Add exhaust smoke from the truck"*
> *"The wheels should look like they're spinning"*

Each of these prompts resulted in the AI adding or modifying specific sections of code, building up the game piece by piece.

**What made vibe coding work here:**
1. **Clear vision** — knowing what feel you want ("charming retro UK arcade game")
2. **Incremental building** — adding one feature at a time rather than demanding everything at once
3. **Feedback** — trying each version, then asking for changes
4. **Trusting the AI on technical decisions** — sprite grid sizes, audio frequencies, collision maths

---

## Key Takeaways for Aspiring Vibe Coders

| Concept | What It Means | Why It Matters |
|---------|---------------|----------------|
| **Single Responsibility** | Each function does one thing | `drawWagon()` only draws. `update()` only updates. Easier to change. |
| **Constants at the top** | All magic numbers named and stored | Change one number, fix everything |
| **State machine** | One variable controls which screen shows | Clear, predictable program flow |
| **Layer-based drawing** | Draw back to front, like a stage | Correct depth ordering without complexity |
| **Entity template** | One class covers all moving objects | Write once, use everywhere |
| **Separate update/draw** | Game logic and rendering never mix | Clean, debuggable, modifiable |
| **Palette object** | All colours named in one place | Consistent look, easy theme changes |

---

*This game was built through conversation with Claude, an AI assistant by Anthropic. The human provided the creative vision — the characters, the setting, the feel. The AI provided the technical implementation. Together: a complete, playable game in a single file.*

---
**Total lines of code:** ~1,280
**External dependencies:** None
**Files needed to run:** 1
**Programming languages:** HTML, CSS, JavaScript
**Time to open and play:** Instant — just open `index.html` in any browser
