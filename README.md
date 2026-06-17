# AI Nadu — The LLM Kingdom Simulator 

> *What if the world's major AI models were South Indian Tamil kingdoms — with kings, queens, crop fields, temples, armies, and tiny walking people?*

Built in a single evening using Claude. No frameworks. No backend. Just one HTML file and a conversation that kept getting more interesting.

---

## What is this?

This is a real-time strategy simulation where five major AI language models — Claude, GPT-4, Gemini, Llama, and Mistral — exist as rival Tamil kingdoms on a procedurally generated South Indian map.

Each kingdom has its own:
- **King and Queen** (the real-world founders, Tamil-styled)
- **Territory** that expands across paddy fields, forests, rivers and mountains
- **Strategy** — some are diplomats, some are raiders, some are builders
- **Resources** — Tokens, Revenue, Parameters, and Trust
- **Tiny animated people** walking around as farmers, soldiers, priests, and merchants
- **Temples and villages** that get built as kingdoms grow

Seasons change. Monsoons flood the land. Training runs complete. Regulatory storms hit. Kingdoms rise, sign API treaties, go to war, and fall.

You watch it all happen in real time.

---

## Live Demo

👉 **[Play the simulator here]( https://ganesanrashikaa-arch.github.io/AI_Tamil_kingdom_Simulator/)**

No installation. No login. Opens directly in your browser.

---

## The Five Kingdoms

| Kingdom | Symbol | Founders | Capital | Strategy |
|---|---|---|---|---|
| **Claude Nadu** | 🔶 | Raja Dario · Rani Daniela | Anthropolis | Expansionist |
| **GPT Nadu** | ⚫ | Raja Sam Altman · Rani Mira Murati | OpenAI Puram | Trader |
| **Gemini Nadu** | ♊ | Raja Sundar · Rani Demis Hassabis | DeepMind Nagari | Defender |
| **Llama Nadu** | 🦙 | Raja Mark Zuckerberg · Rani Yann LeCun | Meta Mandapam | Builder |
| **Mistral Nadu** | 🌬️ | Raja Arthur Mensch · Rani Sophia Yang | Paris Nagaram | Raider |

---

## How to Use It

**Controls**
- `▶ PLAY` — starts the simulation
- `⚡ FAST` — speeds it up (great for watching long campaigns)
- `↺ RESET` — generates a fresh random world
- **Click any kingdom** on the map to see its full stats, allies, wars, and traits

**Tabs (right sidebar)**
- `LLM KINGDOMS` — live stats for all five kingdoms sorted by strength
- `CHRONICLES` — a running log of every battle, alliance, flood, and harvest

**What to watch for**
- Paddy fields turn golden as they approach harvest
- Tiny people animate differently by role — priests have gold dots, soldiers march faster
- Alliance lines (dashed gold) and war lines (dashed red) connect kingdoms
- The HP arc ring around each capital shows health at a glance
- Temples appear on the map as kingdoms build faith

---

## The Story Behind It

This started from a LinkedIn post about a Claude hackathon that I scrolled past without thinking much about. It sat in the back of my mind.

A few days later an idea clicked — what if I built an imaginary world where AI models had to actually survive? Not just exist, but compete, build territory, fight wars, and outlast each other. A god-mode simulation. Like WWE but for LLMs.

The first version was surprising. Claude was losing. Grok was dominating. I assumed it was about capability. It wasn't — it was about responsibility. Grok had a raider strategy, aggressive and attack-first. Claude was a diplomat, sharing resources and trying to make peace. Values that look like weakness when the rules reward aggression.

So I flipped it. Made Claude the raider. Claude won.

Then I wanted better visuals. A recently trending Tamil game had stunning aesthetics that stuck in my mind — paddy fields, temples, rivers. I brought that whole visual language into the simulator.

And in a slightly cringe but honestly charming moment, Claude added the actual CEOs and founders as kings and queens without being asked. Sam Altman as Raja. Mark Zuckerberg ruling Llama Nadu. Nobody planned that.

The whole thing happened in one evening.

---

## Technical Details

### Stack
- **Pure HTML/CSS/JavaScript** — single file, zero dependencies, zero build step
- **HTML5 Canvas** — all rendering is done via the Canvas 2D API
- **No frameworks** — no React, no Vue, no libraries at all

### How the world is generated

Every time you hit Reset, the map is procedurally generated from scratch using layered value noise. The terrain types — plains, paddy fields, forests, mountains, rivers, coastline, sea — are determined by noise thresholds combined with a geographic bias that roughly approximates South India's shape (Western Ghats on the left, coastline to the east and south).

```
noise value → terrain type
> 0.85 or edge         → SEA
> 0.72                 → COAST
< 0.28 (interior)      → RIVER
> 0.62 (west band)     → MOUNTAIN
> 0.55 (north/west)    → FOREST
> 0.45                 → PADDY
else                   → PLAIN
```

Three main rivers are then drawn as organic paths using linear interpolation with random drift, simulating the Kaveri, Vaigai, and Periyar river systems.

### Kingdom strategies

Each kingdom runs a strategy function every tick:

| Strategy | Behaviour |
|---|---|
| `expansionist` | Grows territory aggressively, builds army, attacks weaker neighbours |
| `trader` | Accumulates revenue, forms alliances, grows trade routes |
| `defender` | Fortifies when threatened, seeks peace deals, prioritises faith |
| `builder` | Converts resources into structures (temples, villages), steady expansion |
| `raider` | Attacks the weakest target each tick, fast resource gain |

### Resource system

Each kingdom tracks four resources that map to real AI industry concepts:

| Simulation | Real world equivalent |
|---|---|
| Tokens | Training data / inference capacity |
| Revenue | Commercial success / funding |
| Parameters | Model size / raw capability |
| Trust | User trust / safety reputation |

Resources are gained by harvesting from owned territory (paddy = tokens, coast = revenue) and lost through army upkeep and metabolism each tick. Starvation damages HP.

### Crop growth

Paddy cells have a `cropGrowth` float from 0 to 1. Each tick it increments based on season multiplier and cell fertility. When it reaches 1 it resets and adds to the owning kingdom's food. The visual colour transitions through five stages from bare earth (`#c8a878`) through green growth to golden harvest (`#e8d840`).

### People (agents)

Each kingdom spawns 8 micro-agents on initialisation, typed as farmer, soldier, merchant, priest, or builder. They wander randomly within a radius of their kingdom's home cell, moving toward a new random target every 60–140 ticks. Their leg animation uses a sine wave keyed to tick count and x-position, so they look like they're walking. Priests render with a gold dot on their head.

### World events

A random world event fires approximately every 16 ticks. Events include:

- Training run completion (food boost)
- Data contamination (food penalty)
- API treaty (forced alliance between two kingdoms)
- Enterprise customers arriving (gold boost)
- New model version consecrated (temple built, faith up)
- AI regulatory flood (all kingdoms lose food)
- GPU cluster unlock (army boost)

### Seasons

Four seasons cycle every 90 days (ticks):
- **Monsoon** — 1.5× crop growth multiplier
- **Winter** — 0.7× crop growth
- **Spring** — 1.0× baseline
- **Summer** — 1.0× baseline

### Rendering loop

The canvas redraws every simulation tick. Drawing order:
1. Terrain base colours
2. Territory ownership tint (kingdom colour at low opacity)
3. Paddy row lines and forest tree dots
4. Mountain peaks with snow caps
5. Temple structures and village huts
6. Grid overlay (very subtle)
7. Alliance lines (dashed gold)
8. War lines (dashed red)
9. Micro-agent people with leg animation
10. Particles (battle sparks, harvest bursts, alliance trails)
11. Kingdom capitals with glow, HP arc ring, symbol, name label

---

## File Structure

```
ai-tamil-kingdom-simulator/
│
└── tamil_kingdom.html      # The entire simulator — open this in any browser
└── README.md               # This file
```

That's it. One file.

---

## Running Locally

No server needed. Just download `tamil_kingdom.html` and open it in any modern browser (Chrome, Firefox, Safari, Edge).

```bash
# If you prefer cloning
git clone https://github.com/yourusername/ai-tamil-kingdom-simulator.git
cd ai-tamil-kingdom-simulator
open tamil_kingdom.html   # macOS
# or just double-click the file on Windows/Linux
```

---

## What I Learned

The question that stuck with me after building this:

Aravind Srinivas once said that in the future, the skill won't be answering questions correctly — it will be asking better ones.

AI didn't replace my thinking here. It just gave it somewhere bigger to go. A whole evening, one conversation, and something I genuinely couldn't have built alone came out of it.

---

## Contributing

Feel free to fork and extend it. Some ideas worth exploring:

- Add Grok, DeepSeek, and Perplexity as additional kingdoms
- Add a diplomacy panel where you can manually trigger alliances or wars
- Add a speed slider instead of just fast/slow toggle
- Persist world state to localStorage so it survives page refresh
- Add sound — harvest bells, battle drums, monsoon rain

---

---

*Built with Claude (Anthropic)
