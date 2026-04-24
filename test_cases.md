# Test Cases — Color Chain Reaction

---

## Grid

| # | Test | Steps | Expected |
|---|------|-------|----------|
| 1 | Grid is 8×8 | Open the game | 64 colored cells appear |
| 2 | Level 1 has 3 colors | Start new game | Only 3 different colors on board |
| 3 | Level 3 has 5 colors | Reach level 3 | 5 colors shown in legend |
| 4 | Restart brings back original board | Make moves → press R | Board looks exactly like it did at start of level |

---

## Clicking & Explosions

| # | Test | Steps | Expected |
|---|------|-------|----------|
| 5 | Can't click a lone cell | Click a cell with no same-color neighbours | Nothing happens, no move lost |
| 6 | 2 connected cells explode | Click a pair of same-color cells | Both disappear |
| 7 | Entire connected group explodes | Click inside a large group | All connected cells of that color disappear |
| 8 | Separate same-color cells don't affect each other | Click one group | Other unconnected same-color cells stay |
| 9 | Empty cells are unclickable | Click a dark/empty cell | Nothing happens |
| 10 | Can't click while animation is playing | Click, then immediately click again | Only first click registers |

---

## Gravity

| # | Test | Steps | Expected |
|---|------|-------|----------|
| 11 | Cells fall after explosion | Explode a group in the middle | Cells above drop down |
| 12 | Only affected columns change | Explode cells in 2 columns | Other columns stay the same |
| 13 | Tops of columns go empty | Explode bottom cells | Top of that column becomes empty |
| 14 | Gravity can create new groups | Explode cells between two same-color regions | The two regions merge and become one group |

---

## Scoring

| # | Test | Steps | Expected |
|---|------|-------|----------|
| 15 | Score formula works | Explode 5 cells on Level 1 | Score goes up by 250 (5² × 10) |
| 16 | Higher levels score more | Explode 5 cells on Level 3 | Score goes up by 325 (5² × 10 × 1.30) |
| 17 | Score adds up over moves | Make multiple explosions | Total score equals sum of all individual scores |
| 18 | Board clear gives +500 | Remove every single cell | Extra 500 points added |
| 19 | Best score updates live | Beat your previous best | Best counter updates immediately |
| 20 | Best score saved after reload | Get a high score → refresh page | Best score still shows the same number |

---

## Moves

| # | Test | Steps | Expected |
|---|------|-------|----------|
| 21 | Valid click costs 1 move | Click a valid group | Move counter goes down by 1 |
| 22 | Invalid click costs no move | Click isolated or empty cell | Move counter unchanged |
| 23 | Running out of moves restarts level | Use all moves without clearing targets | "Out of Moves" popup, then level resets |
| 24 | Move dots match counter | Make a few moves | Number of lit dots equals moves remaining |

---

## Targets

| # | Test | Steps | Expected |
|---|------|-------|----------|
| 25 | Targets exist on the board | Check targets vs board at level start | Each target color group actually exists |
| 26 | Target ticks off when cleared | Destroy a target group | That target shows ✓ and goes grey |
| 27 | Partial clear doesn't tick target | Destroy a smaller group of same color | Target stays active |
| 28 | All targets cleared = level up | Clear every target | Level complete popup appears |
| 29 | Max 4 targets per level | Check target bar on any level | Never more than 4 targets shown |

---

## Level Progression

| # | Test | Steps | Expected |
|---|------|-------|----------|
| 30 | Level number goes up on win | Complete a level | Level counter increments by 1 |
| 31 | New Game resets everything | Play to level 5 → click New Game | Level back to 1, score back to 0 |
| 32 | Best score survives New Game | Get a high score → click New Game | Best score stays the same |

---

## UI & Feedback

| # | Test | Steps | Expected |
|---|------|-------|----------|
| 33 | Hovering highlights a group | Hover over any cell | All connected same-color cells glow |
| 34 | Moving mouse off clears highlight | Hover then move away | Glow disappears |
| 35 | Message shows points earned | Explode a group | "+X pts (Y cells)" message appears |

---

## Keyboard

| # | Test | Steps | Expected |
|---|------|-------|----------|
| 36 | N key = New Game | Press N | Resets to level 1 |
| 37 | R key = Restart level | Press R | Level restarts with original board |
| 38 | Esc key = close popup | Open any popup → press Esc | Popup closes |

---

## Popups

| # | Test | Steps | Expected |
|---|------|-------|----------|
| 39 | Win popup looks right | Clear all targets | Shows 🎉, "Level Complete!", score, "Next Level" button |
| 40 | Board clear popup looks right | Clear entire board | Shows 🏆, "Board Cleared!", mentions +500 bonus |
| 41 | Lose popup looks right | Run out of moves | Shows 💔, "Out of Moves!", "Try Again" button |
| 42 | "Next Level" button works | Click it on win popup | Goes to next level |
| 43 | "Try Again" button works | Click it on lose popup | Restarts current level |

---

## Edge Cases

| # | Test | Steps | Expected |
|---|------|-------|----------|
| 44 | Full board same color | Set all cells to same color, click any | Entire board clears, +500 bonus |
| 45 | Only 1 cell left | Engineer a board with 1 remaining cell | Click does nothing, no move lost |
| 46 | Score never drops | Play several levels including losses | Score is always 0 or higher |
| 47 | Fast clicking doesn't break game | Click rapidly for 10 seconds | No errors, game state stays valid |
| 48 | Works in incognito (no localStorage) | Open in private window, play | Game works fine, best score just resets on reload |

---

**Total: 48 test cases across 9 areas**
