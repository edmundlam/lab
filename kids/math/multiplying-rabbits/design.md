# Multiplying Rabbits - Design Document

## Overview
A single-file HTML/JS multiplication game where a bunny poops multiplication problems. Players answer correctly to make the bunny poop again. Goal: 10 correct answers in 10 minutes per level.

**Target Audience:** 9-year-old learning multiplication tables 1-8.

## Game Mechanics

### Answer Input
- **On-screen numpad:** Clickable number buttons (0-9, Enter, Clear)
- **Physical keyboard:** Type numbers and press Enter
- Both methods work simultaneously for accessibility

### Wrong Answers
- **No penalty:** Poop stays until the correct answer is entered
- Visual feedback: Shake animation or color flash to indicate wrong
- Player can try again immediately
- Sound: Gentle "try again" sound (not harsh)

### Timer
- **10 minutes per level** to complete 10 poops
- Timer displays visibly (MM:SS format)
- When time runs out: Show "Time's up!" with options

### Progression
- **10 Levels total:**
  - Levels 1-8: Single tables (1x, 2x, 3x, 4x, 5x, 6x, 7x, 8x)
  - Level 9: Mixed tables 1-5
  - Level 10: Mixed tables 1-8
- **Multiplication range:** 1x1 to 8x12 per spec
- **Random order:** Problems never sequential
- **Sequential unlocking:** Must complete Level N before accessing Level N+1
- **Persistence:** localStorage saves completed levels only

### Win Condition
Complete all 10 levels → View Diploma button appears on Level 10 complete message.

## Screens & Navigation

### Screen Flow
```
Start Screen → Game Screen → (Level Complete OR Time's Up) → Start Screen
                                              ↓
                                         Diploma (from Level 10 only)
```

### Start Screen

**Layout:** Centered vertical stack
```
┌─────────────────────────┐
│     Mute Button 🔊      │ ← Top-right
├─────────────────────────┤
│   Multiplying Rabbits    │ ← Title
├─────────────────────────┤
│ Help the bunny poop     │ ← Instructions
│ 10 times by solving     │
│ math problems! 🐰💩      │
├─────────────────────────┤
│  [1] [2] [3] [4] [5]    │ ← Level Grid
│  [6] [7] [8] [9] [10]   │   (🔒 locked, 🐰 unlocked, ⭐ completed)
├─────────────────────────┤
│     [Reset Progress]    │ ← Bottom buttons
│     (with confirmation) │
└─────────────────────────┘
```

**Level Grid Behavior:**
- Tap a level to start (if unlocked)
- Locked levels: Grey padlock 🔒, not tappable
- Unlocked levels: Bunny icon 🐰, tappable
- Completed levels: Gold star ⭐, tappable (replay)

**Instructions Text:** "Help the bunny poop 10 times by solving math problems! 🐰💩"

**Reset Flow:**
1. Tap "Reset Progress"
2. Confirmation: "Really reset? You'll lose your completed levels!"
3. Buttons: [Yes] [No]
4. If Yes: Clear localStorage, return to Level 1 unlocked

**Mute Button:** Top-right corner, toggles sound on/off

### Game Screen

**Layout:** Responsive (see Responsive Design section)
```
┌─────────────────────────┐
│     Mute Button 🔊      │ ← Top-right
├─────────────────────────┤
│   Timer: 09:45          │ ← Timer display
│   Level 3 - 3× Table    │ ← Level indicator
│   Poops: 3/10           │ ← Progress
├─────────────────────────┤
│                         │
│    🐰(3)     💩         │ ← Meadow area
│           3 × 4 = ?     │   (bunny with table number)
│                         │   (poop with problem)
│                         │
├─────────────────────────┤
│  [7][8][9][↵]           │ ← On-screen numpad
│  [4][5][6][⌫]           │
│  [1][2][3][0]           │
└─────────────────────────┘
```

### Level Complete Message

**Displays when:** Player answers 10th poop correctly

**Message Text:** "You finished the 3x table! 🐰💩"

**Buttons:**
- [Next Level] — Starts the next unlocked level (or disabled if Level 10)
- [Return to Menu] — Goes back to start screen

### Time's Up Message

**Displays when:** Timer reaches 0:00

**Message Text:** "Time's up!"

**Buttons:**
- [Retry Level] — Restart current level with fresh problems
- [Go to Menu] — Return to start screen

**Behavior:** Level progress is NOT saved on time's up

### Diploma Screen

**Access:** Only from Level 10 complete message → [View Diploma] button

**To reaccess:** Must replay and complete Level 10 again

**Layout:**
```
┌─────────────────────────┐
│                         │
│      💩 GIANT POOP 💩   │ ← Centerpiece (placeholder: large poop emoji)
│                         │
├─────────────────────────┤
│  Name: [__________]     │ ← Input field
│  Date: 2025-08-22       │ ← Pre-filled
├─────────────────────────┤
│      Good job! 🐰💩      │ ← Message
│                         │
│  Levels: ⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐ │ ← Completion badge
│                         │
│      [Print]            │ ← Print button
│      [Menu]             │ ← Return to start
└─────────────────────────┘
```

**Print Behavior:**
- CSS `@media print` hides non-diploma elements
- Centers diploma on page
- Ensures colors print well
- Triggered by Print button

**Persistence:**
- Saves player name to localStorage
- Can return to diploma by replaying Level 10

## Visual Design

### Scene (Meadow)
- **Background:** Plain meadow with flowers
- **Placeholder:** CSS gradient sky + grass with simple flower shapes
- **Future:** Replace with detailed illustrations in separate session

### Bunny Character
- **Displays table number** on its body (e.g., "3" for 3x table)
- **Placement:** Center-left or center-top of meadow
- **Placeholder:** Large emoji 🐰 or simple CSS/SVG bunny shape with number overlay
- **Animation:** Subtle bounce or breathing when idle

### Poop
- **Appearance:** Brown pile shape with multiplication problem displayed
- **Format:** "3 × 4 = ?" text centered on poop
- **Placement:** Appears near bunny, scattered slightly
- **Placeholder:** CSS poop shape (brown oval/rounded) with white text
- **Future:** Detailed illustration

### Poop Disappearance Animation
When answered correctly:
1. Poop morphs/fades into purple and green bag shape
2. Bag shrinks and fades out
3. New poop appears nearby

### On-Screen Numpad
- Grid layout: 1-9 in 3x3, 0 below, Enter and Clear to the right
- Kid-friendly buttons: Large, colorful, easy to tap
- Visual feedback: Highlight current input

## Asset Rendering System

### Design Principle
**Separate "what to draw" from "how to draw it"** — this makes swapping emoji placeholders → real images trivial later.

### Object Pattern
Each visual element is an object with either `emoji` (placeholder) or `img` (real asset) property:

```javascript
// Example: Bunny character
const bunny = {
  x: 100,
  y: 100,
  emoji: '🐰',              // Placeholder
  img: null,                // Will be populated later with Image object
  tableNumber: 3
};

// Example: Level grid icon
const levelIcon = {
  level: 1,
  state: 'unlocked',        // 'locked' | 'unlocked' | 'completed'
  emoji: '🐰',              // Changes based on state
  img: null
};
```

### Rendering Function
Wrap all drawing in one function per element type:

```javascript
function drawBunny(ctx, bunny) {
  if (bunny.img) {
    ctx.drawImage(bunny.img, bunny.x, bunny.y, bunny.w, bunny.h);
  } else {
    ctx.font = '48px serif';
    ctx.fillText(bunny.emoji, bunny.x, bunny.y);
  }
  // Draw table number overlay (always text)
  ctx.fillText(bunny.tableNumber, bunny.x + 20, bunny.y + 10);
}

function drawIcon(ctx, icon) {
  if (icon.img) {
    ctx.drawImage(icon.img, icon.x, icon.y, icon.w, icon.h);
  } else {
    ctx.font = '32px serif';
    ctx.fillText(icon.emoji, icon.x, icon.y);
  }
}
```

### Asset Preloading
Load images at game start, not mid-play:

```javascript
const assets = {
  bunny: null,
  poop: null,
  bag: null,
  lock: null,
  star: null,
  // ... more
};

function preloadAssets() {
  const toLoad = [
    { key: 'bunny', src: 'images/bunny.png' },
    { key: 'poop', src: 'images/poop.png' },
    { key: 'bag', src: 'images/bag.png' },
    { key: 'lock', src: 'images/lock.png' },
    { key: 'star', src: 'images/star.png' }
  ];

  toLoad.forEach(item => {
    const img = new Image();
    img.onload = () => { assets[item.key] = img; };
    img.src = item.src;
  });
}

// Call at game initialization
preloadAssets();
```

### Visual Elements Catalog
| Element | Placeholder Emoji | Future Image File | Usage Location |
|---------|------------------|-------------------|----------------|
| Bunny | 🐰 | `images/bunny.png` | Meadow (game screen) |
| Poop | 💩 | `images/poop.png` | Meadow (game screen) |
| Bag | 🟣🟢 (CSS shapes) | `images/bag.png` | Poop disappearance animation |
| Locked level | 🔒 | `images/lock.png` | Start screen level grid |
| Unlocked level | 🐰 | `images/bunny-small.png` | Start screen level grid |
| Completed level | ⭐ | `images/star.png` | Start screen level grid |
| Mute on | 🔊 | `images/speaker-on.png` | Top-right corner (all screens) |
| Mute off | 🔇 | `images/speaker-off.png` | Top-right corner (all screens) |
| Flower | 🌸 | `images/flower.png` | Meadow background |
| Diploma poop | 💩 (large) | `images/poop-giant.png` | Diploma screen |

### Migration Path
When real art is ready:
1. Add image files to `images/` folder
2. Update `preloadAssets()` with new paths
3. Populate `img` properties on objects during initialization
4. Rendering function automatically switches to `drawImage`
5. Can swap incrementally — some elements use emoji while others use images
6. No gameplay logic needs to change

### Level State Icons
Icon selection based on level state:

```javascript
function getLevelIcon(levelNum, state) {
  const icons = {
    locked: { emoji: '🔒', img: assets.lock },
    unlocked: { emoji: '🐰', img: assets.bunny },
    completed: { emoji: '⭐', img: assets.star }
  };
  return icons[state];
}
```

### CSS vs Canvas Rendering
For DOM-based elements (level grid, buttons), use CSS classes with emoji content:

```css
.level-btn.locked::before { content: '🔒'; }
.level-btn.unlocked::before { content: '🐰'; }
.level-btn.completed::before { content: '⭐'; }
```

Swap to background images later:

```css
.level-btn.locked { background-image: url('images/lock.png'); }
.level-btn.unlocked { background-image: url('images/bunny-small.png'); }
.level-btn.completed { background-image: url('images/star.png'); }
```

## Audio Design

### Sound Effects
- **Correct answer:** Happy/chime sound
- **Wrong answer:** Gentle "try again" sound (not harsh)
- **Level complete:** Victory/fanfare sound
- **Time's up:** Calm "let's try again" sound
- **Poop appears:** Subtle "pop" sound
- **Diploma:** Celebration sound

### Implementation
- Use simple audio files (short, kid-friendly)
- Fallback: No audio if files fail to load
- **Mute button:** Top-right corner, toggles all sounds
- Persist mute state to localStorage

## Responsive Design

### Breakpoints
1. **Phone:** < 600px
2. **Tablet:** 600px - 900px
3. **Desktop:** > 900px

### Layout Changes
**Stacking adjustments at each breakpoint:**
- Phone: Single column, smaller fonts, numpad buttons larger for touch
- Tablet: 2-column grid where appropriate, medium fonts
- Desktop: Full spacing, optimal font sizes

**Mobile-specific:**
- Touch target sizes: Minimum 44x44px for buttons
- Numpad: Larger buttons on phone for easier tapping
- Portrait/landscape: Both supported, layout adapts

## Technical Implementation

### File Structure
```
multiplying-rabbits/
├── index.html          # Single-file game (HTML + CSS + JS)
├── spec.md            # Original spec
└── design.md          # This design document
```

### HTML Structure
```html
<body>
  <!-- Start Screen -->
  <div id="start-screen" class="screen">
    <button id="mute-btn">🔊</button>
    <h1>Multiplying Rabbits</h1>
    <p class="instructions">Help the bunny poop 10 times by solving math problems! 🐰💩</p>
    <div id="level-grid">
      <!-- 10 level buttons with icons -->
    </div>
    <button id="reset-btn">Reset Progress</button>
  </div>

  <!-- Game Screen -->
  <div id="game-screen" class="screen hidden">
    <button id="mute-btn-game">🔊</button>
    <div id="timer">10:00</div>
    <div id="level-info">Level 1 - 1× Table</div>
    <div id="score">Poops: 0/10</div>

    <div id="meadow">
      <div id="bunny">
        <span class="table-number">1</span>
      </div>
      <div id="poop-container">
        <!-- Poops appear here -->
      </div>
    </div>

    <input type="text" id="answer" readonly>
    <div id="numpad">
      <!-- Buttons: 0-9, Enter, Clear -->
    </div>
  </div>

  <!-- Message Overlay -->
  <div id="message-overlay" class="hidden">
    <h2 id="message-title"></h2>
    <p id="message-body"></p>
    <div id="message-buttons"></div>
  </div>

  <!-- Diploma Screen -->
  <div id="diploma-screen" class="screen hidden">
    <!-- Diploma content -->
  </div>

  <!-- Reset Confirmation Modal -->
  <div id="reset-modal" class="hidden">
    <p>Really reset? You'll lose your completed levels!</p>
    <button id="reset-confirm">Yes</button>
    <button id="reset-cancel">No</button>
  </div>
</body>
```

### State Management
```javascript
const state = {
  // Current game state
  currentLevel: 1,
  currentPoop: 0,           // 0-9 (10 poops to complete)
  timeRemaining: 600,       // seconds
  currentProblem: null,     // {a: 3, b: 4, answer: 12}
  timerInterval: null,
  isMuted: false,

  // Persistent state (saved to localStorage)
  levelsCompleted: [],      // array of completed level numbers [1, 2, 3...]
  playerName: ''
};
```

**Important:** `currentLevel`, `currentPoop`, `timeRemaining`, and `currentProblem` are **NOT saved** during gameplay. Refreshing mid-level returns to start screen with fresh level.

### localStorage Keys
- `multiplying-rabbits-completed`: JSON array of completed level numbers
- `multiplying-rabbits-name`: Player name for diploma
- `multiplying-rabbits-muted`: Boolean for mute state

**Graceful Degradation:**
- All localStorage calls wrapped in try/catch
- If storage fails, game works normally without persistence
- Silent failure — no error messages shown to player

### Screen Management
```javascript
const screens = ['start-screen', 'game-screen', 'diploma-screen'];

function showScreen(screenId) {
  screens.forEach(id => {
    document.getElementById(id).classList.add('hidden');
  });
  document.getElementById(screenId).classList.remove('hidden');
}
```

### Level State Logic
```javascript
function getLevelState(levelNum) {
  if (state.levelsCompleted.includes(levelNum)) {
    return 'completed'; // ⭐
  } else if (levelNum === 1 || state.levelsCompleted.includes(levelNum - 1)) {
    return 'unlocked'; // 🐰
  } else {
    return 'locked'; // 🔒
  }
}

function isLevelUnlocked(levelNum) {
  const state = getLevelState(levelNum);
  return state === 'unlocked' || state === 'completed';
}
```

### Problem Generation Logic
```javascript
function generateProblem(level) {
  // Determine which tables to use based on level
  let tables;
  if (level <= 8) {
    tables = [level];
  } else if (level === 9) {
    tables = [1, 2, 3, 4, 5];
  } else {
    tables = [1, 2, 3, 4, 5, 6, 7, 8];
  }

  const table = tables[Math.floor(Math.random() * tables.length)];
  const multiplier = Math.floor(Math.random() * 12) + 1;

  return {
    a: table,
    b: multiplier,
    answer: table * multiplier
  };
}
```

### Game Loop

#### Start Level
1. Reset `currentPoop` to 0
2. Reset `timeRemaining` to 600
3. Generate first problem
4. Start timer interval (decrement every second)
5. Show game screen

#### Each Problem
1. Display poop with "A × B = ?"
2. Wait for player input (Enter key or numpad Enter)
3. Check answer against `currentProblem.answer`

#### Correct Answer
1. Increment `currentPoop`
2. Play correct sound (if not muted)
3. Animate poop → bag → fade
4. If `currentPoop < 10`: Generate new problem
5. If `currentPoop === 10`: Trigger Level Complete

#### Wrong Answer
1. Play wrong sound (if not muted)
2. Shake poop or flash color
3. Poop stays, player tries again

#### Level Complete
1. Stop timer
2. Add level to `levelsCompleted`
3. Save to localStorage
4. Play victory sound (if not muted)
5. Show message overlay: "You finished the Xx table! 🐰💩"
6. Show buttons:
   - If level < 10: [Next Level] [Return to Menu]
   - If level === 10: [View Diploma] [Return to Menu]

#### Time's Up
1. Stop timer
2. **Do NOT save** any progress (level restarts fresh)
3. Show message overlay: "Time's up!"
4. Show buttons: [Retry Level] [Go to Menu]

#### View Diploma
1. Check if all 10 levels completed (redundant, button only shows on Level 10)
2. Show diploma screen
3. Pre-fill today's date
4. Load saved player name if exists

### Code Organization Principles
**Simplest, most modular, easy to maintain/improve:**
- Organize into clear functions: `renderStartScreen()`, `startLevel()`, `checkAnswer()`, etc.
- Separate drawing logic from game logic
- Keep functions small and focused
- Use comments to explain complex sections (kid-friendly when possible)
- Configuration objects for easy tweaking (timers, level definitions, etc.)

## Accessibility Considerations
- Keyboard navigation for numpad
- Large, readable fonts
- High contrast for text
- Clear visual feedback for all actions
- No time pressure on individual problems (only level timer)
- Touch-friendly button sizes (44x44px minimum)
- Screen reader friendly labels where appropriate

## Edge Cases Handled
| Case | Behavior |
|------|----------|
| localStorage disabled | Game works, no persistence, silent fail |
| Refresh mid-level | Return to start screen, restart level fresh |
| All levels completed | Level 10 remains replayable, diploma accessible via Level 10 |
| Reset progress | Confirmation dialog, then clear localStorage |
| Mute toggle | Persists across sessions, works immediately |

## Testing Checklist
- [ ] Timer counts down correctly
- [ ] Correct answer advances to next poop
- [ ] Wrong answer allows retry without penalty
- [ ] All 10 levels generate appropriate problems
- [ ] Level 9 uses tables 1-5, Level 10 uses 1-8
- [ ] Time's up shows message with Retry/Menu buttons
- [ ] Level complete shows message with Next/Menu buttons
- [ ] Level grid shows correct lock/unlock/complete states
- [ ] Tap level to start works
- [ ] Reset progress shows confirmation and clears data
- [ ] Mute button toggles and persists
- [ ] Diploma shows after Level 10 complete
- [ ] Diploma requires replaying Level 10 to reaccess
- [ ] localStorage saves/loads completed levels
- [ ] Keyboard and numpad both work
- [ ] Print layout looks correct
- [ ] Sound effects play (if implemented) and mute works
- [ ] Works on mobile/touch devices
- [ ] Responsive layout at all 3 breakpoints
- [ ] Graceful degradation without localStorage

## Future Enhancements (Out of Scope for MVP)
- Detailed artwork (bunny, poop, meadow illustrations) — separate session
- Background music toggle
- Multiple difficulty levels
- Star ratings per level
- Share diploma feature
- More table options (9x, 10x, 11x, 12x)
- Skip ahead to specific tables (practice mode)
- Hint system for stuck problems