# Setup Instructions

## Requirements

- **Minecraft Education Edition** — requires a school or organization Microsoft 365 license
- Works on Windows 10/11, macOS, iPad, and Chromebook
- No additional software needed — MakeCode is built into Minecraft Education

---

## Installing Minecraft Worlds (.mcworld)

The `.mcworld` files contain the pre-built environments for each game (the animal pens for Game 1, the reward grid arena for Game 2).

### Steps

1. Download the `.mcworld` file from the `worlds/` folder in this repository
2. Locate the file on your computer
3. **Double-click** the file — Minecraft Education Edition will open and import the world automatically
4. Alternatively: right-click the file → **Open with** → **Minecraft Education Edition**
5. Once imported, the world appears in your world list when you launch the game

### Troubleshooting

- If double-clicking doesn't open Minecraft, make sure `.mcworld` files are associated with Minecraft Education Edition in your OS settings
- On iPad: download the file, tap it, and select "Open in Minecraft Education"
- Worlds are saved locally — you only need to import once

---

## Importing MakeCode Code (.mkcd)

The `.mkcd` files contain the Python code that powers the game logic. You import these into a world using MakeCode's Code Builder.

### Steps

1. Open the imported world in Minecraft Education Edition
2. Press **C** on your keyboard to open **Code Builder**
3. Select **MakeCode** as your coding platform (if prompted)
4. In the MakeCode editor, click **Import** (top right area)
5. Select **Import File**
6. Browse to the `.mkcd` file from the `code/` folder and open it
7. The code loads in the editor — you can view, edit, or run it immediately

### Notes

- The code is written in MakeCode Python (not standard Python — it uses Minecraft-specific APIs)
- You can switch between Python view and Blocks view in the MakeCode editor
- Changes you make in the editor are saved with the world

---

## Game 1: Animal Classification Sorter

### Setup

1. Import `game1-sorting.mcworld` and `game1-sorting.mkcd` following the steps above
2. Enter the world and open Code Builder (press **C**)

### How to Play

Run these commands in the Minecraft chat (press **T** to open chat):

| Order | Command | What it does |
|-------|---------|-------------|
| 1 | `clear` | Removes all animals from the field. **Always run this first.** |
| 2 | `spawn` | Generates 50 animals with a 70/30 distribution (70% farm, 30% exotic) |
| 3a | `lucky1` | **Fine-grained sorting** — each animal goes to its own pen (5 pens) |
| 3b | `lucky2` | **Broad sorting** — animals grouped into FARM or EXOTIC (2 pens) |

### Workflow

1. Type `clear` to start fresh
2. Type `spawn` to populate the field
3. Type `lucky1` or `lucky2` to watch the agent sort
4. To compare strategies: `clear` → `spawn` → `lucky1`, then `clear` → `spawn` → `lucky2`

### What to observe

- How many patrol rounds does each strategy need?
- Does fine-grained sorting (lucky1) take longer than coarse sorting (lucky2)?
- Are there animals the agent misses? Why?

---

## Game 2: Gold Hunt — Search Strategy Comparison

### Setup

1. Import `game2-goldhunt.mcworld` and `game2-goldhunt.mkcd` following the steps above
2. Enter the world and open Code Builder (press **C**)

### How to Play

| Order | Command | What it does |
|-------|---------|-------------|
| 1 | `fillframe` | Generates a new random 10×10 grid (50% stone, 30% lava, 20% diamond, 1 gold) |
| 2 | `setup` | Places the agent at a random starting position |
| 3 | Choose a strategy | `rand`, `greedy`, `greedy_noback`, or `greedy_avoid_wool` |

### Search Strategies

- **`rand`** — Random Walk. The agent moves in a random direction each step. This is the baseline — no intelligence, no reward awareness.

- **`greedy`** — Greedy Search. The agent looks at all 4 neighboring blocks, picks the one with the highest reward. Ties are broken randomly.

- **`greedy_noback`** — Greedy + No Backtracking. Same as greedy, but the agent cannot reverse direction (e.g., if it went North, it can't immediately go South). This prevents oscillation.

- **`greedy_avoid_wool`** — Greedy + Avoid Visited. The agent prefers blocks it hasn't visited yet (unvisited blocks keep their original appearance; visited blocks turn to wool). If all neighbors are visited, it falls back to the best available wool block.

- **`test_paths`** — Automated Comparison. Runs 5 trials from the same starting point, each taking a different random detour before heading to gold. Reports step counts, total rewards, and reward/step ratios.

### Reward System

| Block Type | Reward | Visual |
|-----------|--------|--------|
| Stone | +3 | Gray block |
| Diamond | +5 | Light blue block |
| Lava | -5 | Orange/red block |
| Gold | +10 | Yellow block (the goal) |

When visited, blocks change to colored wool (gray, light blue, orange) so you can see the agent's trail.

### What to observe

- Which strategy finds gold fastest?
- Which strategy has the highest total score?
- Does avoiding visited cells (wool) actually help?
- What happens when the greedy agent gets stuck in a cluster of lava blocks?
- How does the no-backtrack rule change the path shape?

### Suggested Exercises

1. Run all 4 strategies on the **same grid** (use `setup` between runs, don't `fillframe` again) and compare results
2. Try `fillframe` several times to see how grid composition affects strategy performance
3. Use `test_paths` and discuss why some random detours produce better reward/step ratios
4. Modify the code: change the reward values or block probabilities and observe what happens

---

## No Minecraft License?

If you don't have access to Minecraft Education Edition, the **interactive browser demo** faithfully recreates both games. Visit the [live demo](https://emantzoo.github.io/minecraft-education-games/) to:

- Watch the Gold Hunt agent navigate with all 4 strategies
- See animals spawn and get sorted with lucky1 vs lucky2
- Adjust speed, regenerate environments, and compare performance

The demo runs entirely in your browser — no installation needed.
