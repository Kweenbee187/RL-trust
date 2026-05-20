# FT-KGRL · Live Network Intelligence

> A real-time reinforcement learning simulation for trust-based packet routing and unsupervised malicious node detection in a dynamic network topology.

[![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
![HTML](https://img.shields.io/badge/Built%20with-HTML%2FJS-orange)
![Nodes](https://img.shields.io/badge/Nodes-60-cyan)
![Episodes](https://img.shields.io/badge/Episodes-1200-purple)

---

## Overview

FT-KGRL is a fully self-contained browser simulation that demonstrates how a **Q-learning agent** learns to route packets through a network while simultaneously detecting malicious nodes — without any labels. Trust scores evolve per edge over 1200 episodes, and an unsupervised scoring formula identifies suspicious nodes based on trust instability alone.

The simulation runs three concurrent views: live RL routing, random baseline comparison, and a split compare mode.

---

## Features

### RL-Trust Routing
An epsilon-greedy Q-learning agent selects next-hop neighbors based on accumulated Q-values. The reward function balances packet delivery success, trust score, node energy, and routing risk:

```
reward = WS·success + WT·trust_high + WE·energy − WR·risk − CP·consistency_penalty
```

### Unsupervised Malicious Node Detection
No ground truth labels are used during detection. Nodes are scored by trust instability:

```
suspicion_score = (1 − mean_trust) + 1.2 × variance
```

Top-scored nodes are flagged. Precision and recall are computed against hidden ground truth for evaluation.

### Trust Propagation
Every 50 episodes, trust diffuses across the graph:

```
trust[u][k] = 0.97 × old + 0.03 × neighbor_trust[v][k]
```

Purple edge flashes visualize active diffusion.

### Compare Mode
A draggable split divider lets you compare the random baseline (left) against the RL-Trust policy (right) side by side, with live delivery averages shown for each.

### Speed Modes
Three simulation speeds are available — Normal (step-by-step with animation), Fast, and Turbo — for either watching the learning process unfold or reaching convergence quickly.

---

## Simulation Phases

| Phase | Episodes | Description |
|-------|----------|-------------|
| INIT | 0–9 | All nodes equal trust (0.5), no routing history |
| EXPLORE | 10–59 | Epsilon-greedy random exploration, Q-values forming |
| Q-LEARN | 60–199 | Reward accumulation, faster paths emerge |
| TRUST DIV | 200–399 | Malicious nodes cause drops, variance builds |
| PROPAGATE | 400–599 | Graph-wide trust diffusion activates |
| DETECTION | 600–799 | Unsupervised suspicion scores identify bad nodes |
| CONVERGE | 800–999 | Policy stabilizes, Precision ≈ 0.9, Recall ≈ 0.9 |
| ADAPT | 1000–1200 | Edge removal (~4%/ep) tests topology robustness |

---

## Usage

No installation required. Single-file HTML application.

```bash
git clone https://github.com/Kweenbee187/RL-trust.git
cd RL-trust
# Open RL-trust.html in any modern browser
```

**Controls:**
- **PLAY / PAUSE** — start or pause the simulation
- **RESET** — restart from episode 0 with the same seed
- **COMPARE ◄►** — split-screen view against random baseline
- **FOCUS** — dim non-adjacent nodes on hover
- **Step Delay** — adjust animation speed in Normal mode
- **Reveal Ground Truth** — show actual malicious nodes (hidden by default)
- **Node Size** — adjust visual node radius

Hover any node for Q-values, trust average, suspicion score, energy, and degree.

---

## Tech Stack

| Library | Purpose |
|---------|---------|
| [D3.js v7](https://d3js.org/) | Force layout (not used here — custom layout) |
| Vanilla JavaScript | RL engine, Q-table, trust graph, detection |
| HTML5 Canvas | All rendering (nodes, edges, packets, charts) |
| CSS3 | UI layout and animations |

---

## File Structure

```
RL-trust/
├── RL-trust.html   # Full simulation (single file, self-contained)
├── LICENSE         # Apache 2.0
└── README.md       # This file
```

---

## License

Copyright © 2026 **Kweenbee187**

Licensed under the [Apache License, Version 2.0](./LICENSE).

---

## Author

**Kweenbee187**
