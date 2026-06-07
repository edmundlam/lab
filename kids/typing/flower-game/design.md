# Learn to Type Flower Game — Design Document

## Overview

A typing game where players pick flowers by typing the letters shown on them. Flowers grow from the ground in a sunny garden setting. As levels progress, the typing challenges get harder.

## Game Mechanics

### Typing System
- **One letter per flower (Levels 1-6)**: Each flower shows a single letter to type
- **Multiple letters per flower (Levels 7-10)**: Flowers show words or letter combinations
- **3-4 flowers on screen** at any given time, growing from the ground
- **First letter auto-targets**: Typing the first letter automatically selects the matching flower
- **All flowers must start with different letters** (no two flowers on screen share the same starting letter)
- **Backspace to retarget**: If you backspace until empty, you can target a different flower
- **When picked**: A new flower appears to replace it (until time runs out)

### Progression System
- **10 levels total**
- **Must earn 3 stars** on a level to unlock the next one
- **Can replay** any completed level
- **Cannot skip ahead** to locked levels

### Level Progression

| Level | Content | Letters/Word Length |
|-------|---------|---------------------|
| 1 | F, G | 1 letter |
| 2 | F, G, D, K | 1-2 letters |
| 3 | F, G, D, K, S, L, A, ; | 2 letters |
| 4 | + T, U | 2-3 letters |
| 5 | + R, I | 3 letters |
| 6 | All alpha except excluded keys | Varies |
| 7 | Little words | Short words |
| 8 | Easy words | Simple words |
| 9 | Medium words | Moderate words |
| 10 | All words | Any words |

**Excluded keys (never used):** Q, W, Y, O, P, Z, X, C, V, B

### Scoring & Stars
Each level can earn up to 3 stars:
- **1 star**: Pick 5 flowers
- **2 stars**: Pick 10 flowers
- **3 stars**: Pick 15 flowers

### Time Limits
- **5 minutes per level**
- **Can restart anytime** (resets current level score to zero)
- Timer visible on screen

## Visual Design

### Theme & Colors
- **Sunny garden theme**
- Sky blue background
- Green grass at the bottom
- Bright, colorful flowers

### Flower Style
- **Simple shapes** drawn with code (circles and petals)
- **Flower petal colors**: Red, Blue, Light Purple, Pink (randomly assigned)
- **Flower centers**: Always yellow
- Grow from the ground (stems visible)

### Flower Picking Animation
- **Visible rockets/flames** appear under the flower
- Flower flies upward into the air
- "Total picked" counter increments by one
- Counter displayed on screen

### Wrong Key Feedback
- Flower shakes when wrong key pressed
- No other penalty

### Screens

**Level Selection Grid:**
- Shows all 10 levels
- Each level shows star rating (0-3 stars)
- Locked levels show a lock icon or are grayed out
- Click a level to play

**Game Screen:**
- Garden background
- 3-4 flowers growing from ground
- Timer display
- Flowers picked counter
- Current level indicator
- Sound toggle button

**Diploma Screen:**
- Prompts for player's name
- Shows diploma with:
  - Player's name
  - Date completed
  - Bouquet of flowers
- Print button

## Progress Saving

- **Browser localStorage** saves progress
- Saves:
  - Stars earned for each level
  - Which levels are unlocked
- Survives closing and reopening browser

## Sound Effects

Sounds that can be toggled on/off:
- Flower picked (rocket sound)
- Wrong key (shake sound)
- Level complete
- Diploma earned

**Sound toggle button** on game screen

## Win Condition

Complete all 10 levels (earn 3 stars on each) → Receive printable diploma

## Technical Notes

- **Single HTML file** (index.html) with embedded CSS and JavaScript
- **No build tools** — opens directly in browser
- **LocalStorage** for progress persistence
- **Canvas or DOM elements** for flowers (to be determined during implementation)