# Color Chain Reaction 🎮

So I built this game called **Color Chain Reaction** — it's a browser puzzle game where you click colored tiles, blow them up in chains, and try to clear the board before you run out of moves. Sounds simple, and it kind of is at first. Then level 4 happens and suddenly you're staring at the screen wondering where it all went wrong.

No frameworks, no libraries, no npm install. Just one HTML file. Open it in a browser and you're playing.

---

## What's in the box

```
color-chain-reaction/
├── color_chain_reaction.html   ← the whole game lives here
└── README.md                   ← you're reading it
```

That's it. One file. Drop it anywhere.

---

## How to actually play

Click a colored cell. If it's connected to 2 or more cells of the same color, they all explode. The cells above fall down (gravity), which sometimes creates new groups you can chain into. That's kind of the whole game — but planning those chains is where it gets interesting.

Each level gives you a set of **targets** — specific color groups you need to destroy before your moves run out. Clear all the targets, move to the next level. Clear the *entire* board, get a bonus 500 points. Run out of moves without clearing targets... restart.

**Keyboard shortcuts (because clicking buttons is slow):**
- `N` → New game
- `R` → Restart level
- `Esc` → Close the popup

---

## Scoring

Bigger explosions are *way* more rewarding than small ones. The formula is:

```
Points = (number of cells)² × 10 × level multiplier
```

So popping 3 cells gives you 90 points. Popping 8 cells gives you 640. Chaining into a massive group is almost always worth waiting for rather than clicking impatiently.

The level multiplier starts at 1.0 and goes up by 0.15 each level, so the same move is worth more the further you get.

---

## How it gets harder

Early levels give you fewer colors and bigger groups, so finding chains is easy. As you level up, more colors get added, the board gets more fragmented, and the targets get trickier. By level 5 or 6 you're genuinely having to think a move or two ahead.

| Level | Colors on board | Moves given |
|-------|----------------|-------------|
| 1     | 3              | 10          |
| 2     | 4              | 10          |
| 3     | 5              | 11          |
| 4     | 6              | 11          |
| 5+    | 6 (capped)     | keeps going up |

---

## The technical stuff (for the project report)

Built with plain **HTML, CSS, and JavaScript** — ES6+, no dependencies.

### The two algorithms that make it work

**Flood fill** — When you click a cell, the game needs to find every connected cell of the same color instantly. It does this with a simple BFS (breadth-first search) that fans out from the clicked cell, checking all four neighbours, and keeps going until it hits cells of a different color or the grid edge.

```js
function flood(g, start, color) {
  const stack = [start];
  const seen  = new Set([start]);
  while (stack.length) {
    const cur = stack.pop();
    const row = Math.floor(cur / GRID_SIZE);
    const col = cur % GRID_SIZE;
    [[row-1,col],[row+1,col],[row,col-1],[row,col+1]].forEach(([r,c]) => {
      if (r >= 0 && r < GRID_SIZE && c >= 0 && c < GRID_SIZE) {
        const idx = r * GRID_SIZE + c;
        if (!seen.has(idx) && g[idx] === color) {
          seen.add(idx);
          stack.push(idx);
        }
      }
    });
  }
  return [...seen];
}
```

**Gravity** — After an explosion, empty gaps need to collapse. The gravity function works column by column — it collects all non-empty cells from the bottom up, then redistributes them back from the bottom, leaving empty slots at the top.

```js
function applyGravity() {
  for (let col = 0; col < GRID_SIZE; col++) {
    const cells = [];
    for (let row = GRID_SIZE - 1; row >= 0; row--) {
      const v = grid[row * GRID_SIZE + col];
      if (v !== -1) cells.push(v);
    }
    for (let row = GRID_SIZE - 1; row >= 0; row--) {
      grid[row * GRID_SIZE + col] = cells.length ? cells.shift() : -1;
    }
  }
}
```

### State management

The game state is just a flat array of numbers (the grid), a few counters, and a couple of flags. No classes, no frameworks — just variables and functions. It's deliberately simple so everything is easy to follow.

Best score is saved to `localStorage` so it survives page refreshes.

---

## Want to tweak it?

Everything you'd want to change is right at the top of the script block:

```js
const GRID_SIZE = 8;        // make it bigger or smaller

const COLORS = [            // add more colors, change hex values
  { name: 'Red',    bg: '#E24B4A' },
  { name: 'Blue',   bg: '#378ADD' },
  // ...
];

function levelConfig(l) {
  return {
    colors: Math.min(2 + l, COLORS.length),  // how many colors per level
    moves:  10 + Math.floor(l / 2) * 2,      // how many moves per level
  };
}
```

Want a harder game? Lower the starting moves. Want a chill experience? Raise them. It's all right there.

---

## Running it

**Easiest way:** just open the HTML file in your browser. That's genuinely all you need to do.

**If you want a local server for development:**
```bash
# Python
python -m http.server 8000

# Node
npx serve .
```

Then go to `http://localhost:8000/color_chain_reaction.html`.

---

## What's included in the game

- 8×8 grid with up to 6 colors
- Chain explosion mechanic with gravity
- Level targets system
- Move counter with visual dot indicators
- Hover preview — hover over a cell to see the whole connected group highlight before you click
- Smooth CSS animations for explosions
- Win/lose overlay popups
- Score, level, and all-time best tracking
- Best score saved across sessions (localStorage)
- Keyboard shortcuts
- Fully responsive — works on mobile too
- Zero external dependencies

---

*Made with HTML · CSS · JavaScript — nothing else needed.*
