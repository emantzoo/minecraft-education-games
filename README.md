# Teaching Algorithms Through Minecraft Education Edition

Two educational games built with **MakeCode Python** for Minecraft Education Edition that make classification, search strategies, and optimization tangible through gameplay.

**[Live Interactive Demo](https://emantzoo.github.io/minecraft-education-games/)** — try the algorithms in your browser, no Minecraft license needed.

---

## Overview

These games use Minecraft's agent-based environment as a laboratory for algorithmic thinking. Students interact with concepts by watching an AI agent make decisions in real time — sorting animals into pens, navigating reward grids, and comparing search strategies.

### Game 1: Animal Classification Sorter

An agent patrols a field of 50 randomly spawned animals and sorts them into pens using two classification strategies:

- **`lucky1` (Fine-grained)** — each animal type gets its own pen (5 types, 5 pens: Cow, Pig, Chicken, Llama, Panda)
- **`lucky2` (Coarse)** — animals are grouped into 2 broad categories: FARM (Cow, Pig, Chicken) and EXOTIC (Llama, Panda)

Animals spawn with a **weighted probability distribution**: 70% farm, 30% exotic.

**Concepts taught:** Classification granularity, probability distributions, data categorization tradeoffs.

### Game 2: Gold Hunt — Search Strategy Comparison

An agent navigates a procedurally generated 10×10 grid where each block carries a reward (Stone +3, Diamond +5, Lava -5, Gold +10). Four search strategies compete:

| Strategy | Command | Description |
|---|---|---|
| Random Walk | `rand` | Moves in a random direction each step — baseline with no reward awareness |
| Greedy | `greedy` | Evaluates all 4 neighbors, picks highest immediate reward |
| Greedy + No Backtrack | `greedy_noback` | Greedy with backtracking prevention via XOR on direction indices |
| Greedy + Avoid Visited | `greedy_avoid_wool` | Greedy, preferring unvisited cells (marked with wool), falls back when trapped |

Additionally, `test_paths` runs 5 trials from a fixed start with random detours, computing reward/step ratios to identify the optimal path.

**Concepts taught:** Search algorithms, greedy optimization, state tracking, algorithm comparison, reward maximization.

---

## Repository Structure

```
minecraft-education-games/
├── README.md
├── worlds/
│   ├── game1-sorting.mcworld        # Minecraft world with animal pens
│   └── game2-goldhunt.mcworld       # Minecraft world with reward grid
├── code/
│   ├── game1-sorting.mkcd           # MakeCode project — sorting game
│   └── game2-goldhunt.mkcd          # MakeCode project — gold hunt
├── demo/
│   └── index.html                   # Interactive browser demo (GitHub Pages)
└── docs/
    └── setup-instructions.md        # Installation guide
```

---

## Getting Started

### Requirements

- **Minecraft Education Edition** (requires a school/organization license)
- No additional software needed — MakeCode is built into Minecraft Education

### Installing the Worlds (.mcworld)

1. Download the `.mcworld` file from the `worlds/` folder
2. Double-click the file (or right-click → Open with → Minecraft Education Edition)
3. The world imports automatically and appears in your world list

### Importing the Code (.mkcd)

1. Open the imported world and press **C** to open Code Builder
2. Select **MakeCode** as the coding platform
3. Click **Import** → **Import File**
4. Browse to the `.mkcd` file from the `code/` folder
5. The code loads in the editor — run it or edit it

### Playing Game 1 (Animal Sorting)

1. Type `clear` in chat to remove any existing animals
2. Type `spawn` to generate 50 animals (70% farm, 30% exotic)
3. Type `lucky1` for fine-grained sorting (5 pens) or `lucky2` for broad sorting (2 categories)
4. Watch the agent patrol and sort animals into pens
5. Clear and respawn between runs for clean results

### Playing Game 2 (Gold Hunt)

1. Type `fillframe` to generate a new random 10×10 grid with rewards
2. Type `setup` to place the agent at a random starting position
3. Choose a search strategy: `rand`, `greedy`, `greedy_noback`, or `greedy_avoid_wool`
4. Watch the agent navigate the grid — visited cells change to wool for visual tracking
5. Compare scores and step counts across strategies
6. Type `test_paths` for an automated 5-trial comparison

---

## Key Technical Highlights

### Fisher-Yates Shuffle

Adapted for MakeCode Python's constraints (no `len()` or `range()`):

```python
def shuffle_list(lst, count):
    i = count - 1
    while i > 0:
        j = Math.random_range(0, i)
        temp = lst[i]
        lst[i] = lst[j]
        lst[j] = temp
        i -= 1
```

### Backtracking Prevention via XOR

Directions are indexed 0–3 (N, S, E, W). XOR with 1 gives the opposite direction:

```python
# 0^1=1 (N↔S), 2^1=3 (E↔W)
if last_move_idx != -1 and index == (last_move_idx ^ 1):
    continue  # Skip opposite direction
```

### Visual State Tracking

Visited blocks transform into colored wool, giving students a real-time visual trail of the agent's path:

```python
def mark_visited(x, y, z):
    block_below = positions.create_world(x, y - 1, z)
    if blocks.test_for_block(STONE, block_below):
        blocks.place(GRAY_WOOL, block_below)
    elif blocks.test_for_block(DIAMOND_BLOCK, block_below):
        blocks.place(LIGHT_BLUE_WOOL, block_below)
    elif blocks.test_for_block(LAVA, block_below):
        blocks.place(ORANGE_WOOL, block_below)
```

---

## Transferable Skills

- **Algorithm Design** — Fisher-Yates shuffle, greedy search with tiebreaking, backtracking prevention via bitwise XOR, multi-trial path evaluation
- **Data & Probability** — Weighted probability distributions, procedural world generation, cumulative scoring systems
- **Educational Design** — Tangible algorithm comparison, visual state tracking, progressive complexity across game modes
- **Technical Execution** — MakeCode Python within Minecraft Education constraints (limited stdlib), spatial coordinate systems, agent-based simulation

---

## Interactive Demo

Can't access Minecraft Education? The **[live demo](https://emantzoo.github.io/minecraft-education-games/)** faithfully recreates both games in your browser:

- Watch the Gold Hunt agent navigate with all 4 strategies side by side
- See animals spawn and get sorted into pens with lucky1 vs lucky2
- Adjust speed, regenerate grids, and compare algorithm performance

---

## Built With

- **MakeCode for Minecraft** v2.0.30 (Python)
- **Minecraft Education Edition**
- Interactive demo: React

---

## Author

**Eirini Mantzouni** — Data Analyst & Statistical Researcher

---

## License

This project is shared for educational and portfolio purposes. The Minecraft Education worlds and MakeCode code are provided as-is for learning use.
