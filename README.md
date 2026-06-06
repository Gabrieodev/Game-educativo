<div align="center">

# EcoGames

**Gamified environmental education platform — three independent minigames, one modular fullstack architecture.**

[![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)](https://developer.mozilla.org/en-US/docs/Web/HTML)
[![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white)](https://developer.mozilla.org/en-US/docs/Web/CSS)
[![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
[![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)](https://nodejs.org/)
[![Express](https://img.shields.io/badge/Express.js-000000?style=for-the-badge&logo=express&logoColor=white)](https://expressjs.com/)

</div>

---

## Overview

**EcoGames** is a web platform that transforms sustainability and environmental education into gamified, progressive experiences. A central lobby connects three independent minigames — each addressing a distinct environmental theme — backed by a single shared Node.js/Express server with fully decoupled game modules.

The architecture is designed for modularity: adding a new game requires no changes to existing modules.

---

## Platform Architecture

```
┌─────────────────────────────────────────────────────┐
│                     index.html                      │
│                  (entry point / redirect)           │
└──────────────────────┬──────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────┐
│                 lobby/lobby.html                    │
│                  (platform hub)                     │
└──────────┬─────────────────┬──────────────┬─────────┘
           │                 │              │
           ▼                 ▼              ▼
┌──────────────────┐ ┌──────────────┐ ┌───────────────────┐
│   greenmemo/     │ │   ecohero/   │ │    trashdash/     │
│   (frontend)     │ │  (fullstack) │ │   (fullstack)     │
└──────────────────┘ └──────┬───────┘ └────────┬──────────┘
                            │                   │
                            └─────────┬─────────┘
                                      ▼
                         ┌────────────────────────┐
                         │       backend/         │
                         │  Node.js + Express.js  │
                         │    database.json       │
                         └────────────────────────┘
```

---

## Modularisation Philosophy

Each game is treated as an independent module. There is no global player, ranking, progress or score entity — every game defines its own data model and rules, evolving without impacting the others.

| Aspect | GreenMemo | EcoHero | TrashDash |
|---|---|---|---|
| Type | Frontend standalone | Fullstack | Fullstack |
| Persistence | In-memory | JSON Database | JSON Database |
| Ranking | — | ✓ | ✓ |
| Shared backend | — | ✓ | ✓ |
| Audio | ✓ | ✓ | ✓ |
| Dedicated ranking screen | — | ✓ | ✓ (`ranking.html`) |

---

## Folder Structure

```plaintext
ecogames/
│
├── index.html                          # Entry point → redirects to lobby
├── .gitignore
│
├── lobby/                              # Platform navigation hub
│   ├── css/
│   └── lobby.html
│
├── greenmemo/                          # Game 1: Educational memory game (frontend only)
│   └── frontend/
│       ├── audio/
│       ├── css/
│       ├── js/
│       └── greenmemo.html
│
├── ecohero/                            # Game 2: Environmental management (fullstack)
│   └── frontend/
│       ├── audio/
│       ├── css/
│       ├── js/
│       └── ecohero.html
│
├── trashdash/                          # Game 3: Recycling conveyor (fullstack)
│   ├── audio/
│   ├── css/
│   ├── js/
│   ├── ranking.html
│   └── trashdash.html
│
└── backend/                            # Shared backend
    ├── controllers/
    │   ├── ecohero.city.controller.js
    │   ├── ecohero.player.controller.js
    │   ├── ecohero.ranking.controller.js
    │   └── trashdash.controller.js
    ├── routes/
    │   ├── ecohero.city.routes.js
    │   ├── ecohero.player.routes.js
    │   ├── ecohero.ranking.routes.js
    │   └── trashdash.routes.js
    ├── services/
    │   ├── ecohero.city.service.js
    │   └── ecohero.ranking.service.js
    ├── database.json
    ├── package.json
    └── server.js
```

---

## Tech Stack

| Layer | Technologies |
|---|---|
| Frontend | HTML5, CSS3, JavaScript ES6+ (Vanilla) |
| Audio | Web Audio API — native, no external dependencies |
| Backend | Node.js, Express.js |
| Persistence | JSON flat file (`database.json`) |

---

## Games

### GreenMemo — Sustainable Memory Game

Educational memory game built entirely with HTML, CSS and Vanilla JavaScript — no frameworks. Five progressive phases, each with its own visual theme driven by CSS custom properties swapped at runtime.

**Core mechanics:**
- Real CSS 3D flip via `rotateY`, `backface-visibility` and `transform-style: preserve-3d`
- `shake` animation on mismatch, reduced opacity on matched pairs
- HUD system tracking lives, score and current phase
- Native audio effects per phase

**Phase system:**

| Phase | Theme |
|---|---|
| 1 | Water Guardians |
| 2 | Living Forest |
| 3 | Clean Energy |
| 4 | Smart Recycling |
| 5 | Living Oceans |

**Rules:** 5 lives per phase · +10 points per matched pair · life lost on mismatch · game over at zero lives · phase 5 completion triggers trophy screen with star rating and badges.

**Game engine — 15 organised sections:**

```
1. Phase data          6. Loading            11. Restart
2. Global state        7. Board construction  12. HUD
3. DOM selectors       8. Initial preview     13. Utilities
4. Navigation          9. Click logic         14. Event listeners
5. Theme application  10. Events             15. Init
```

Fonts: Righteous + Nunito | Responsive down to 380px

---

### EcoHero — Environmental Management Game

Fullstack game with complete backend integration, JSON persistence and a global ranking system.

**Features:**
- Player and profile management
- City progression system
- Persistent global ranking
- Dedicated REST APIs per domain
- Native audio effects

**Backend modules:**

```plaintext
controllers/
├── ecohero.city.controller.js       # City logic
├── ecohero.player.controller.js     # Player management
└── ecohero.ranking.controller.js    # Ranking system

routes/
├── ecohero.city.routes.js
├── ecohero.player.routes.js
└── ecohero.ranking.routes.js

services/
├── ecohero.city.service.js
└── ecohero.ranking.service.js
```

Each layer (controller → service → persistence) maintains strict separation of responsibilities.

---

### TrashDash — Interactive Recycling Conveyor

Fullstack game with a dedicated ranking screen (`ranking.html`). Players operate a moving conveyor belt, classifying waste items into the correct recycling bins before they leave the screen.

**Core mechanics:**
- Dynamically spawned items on a scrolling conveyor
- Distinct recycling bins per waste category
- Correct classification scores points; incorrect penalises
- Native audio effects and soundtrack

**Backend modules:**

```plaintext
controllers/
└── trashdash.controller.js          # Score and ranking logic

routes/
└── trashdash.routes.js              # TrashDash REST endpoints
```

---

## Lobby

The lobby serves as the platform homepage and central navigation hub.

| Section | Content |
|---|---|
| Header | EcoGames name and tagline |
| Hero | Welcome message and call-to-action |
| Mission | Educational purpose statement |
| Stat cards | 3 games · 100% interactive · Sustainable impact |
| Game grid | Clickable cards for each minigame |
| Footer | Copyright 2026 |

---

## Architectural Decisions

**Single backend, independent modules** — A single Express server handles all game APIs, avoiding the complexity of multiple processes. Independence between games is enforced by a prefix-based naming convention across all backend files (`ecohero.*`, `trashdash.*`), which scales naturally as new games are added.

**No global state between games** — There is no shared `player`, `ranking` or `score` entity. Each game defines its own data model entirely, meaning one game's rules can evolve without any risk of affecting the others.

**JSON as the persistence layer** — `database.json` eliminates external database setup, making the project immediately runnable and straightforward for an educational and portfolio context.

**GreenMemo as a fully standalone frontend** — GreenMemo has zero backend dependency and runs entirely in the browser. EcoHero and TrashDash use the shared backend only for ranking persistence, keeping all gameplay logic client-side.

**Web Audio API throughout** — All three games use their own `audio/` folder with native browser audio — no external libraries, no CDN dependency, consistent immersive experience across the platform.

**Real CSS 3D flip — no JavaScript animation library** — GreenMemo's card flip uses only CSS transform properties, demonstrating CSS mastery and keeping the animation performant without runtime overhead.

---

## Technical Highlights

- CSS custom properties swapped dynamically at runtime for per-phase visual themes
- Game engine structured in 15 named sections for maintainability
- Two independent fullstack games sharing a backend without coupling between them
- TrashDash has a dedicated `ranking.html` as an independent page
- Responsive layout down to 380px across all games
- Zero frontend frameworks — Vanilla HTML, CSS and JS throughout

---

## Roadmap

- [x] Central lobby with navigation between games
- [x] GreenMemo — memory game with 5 themed phases
- [x] EcoHero — fullstack game with ranking and city progression
- [x] TrashDash — fullstack recycling conveyor with dedicated ranking
- [ ] Global authentication system (unified login and profile)
- [ ] Teacher administration panel
- [ ] PWA — offline support for frontend-only games
- [ ] Internationalisation (i18n) — English and Spanish support

---

## How to Run

### Prerequisites

- Node.js v18 or higher
- npm v9 or higher

### 1. Clone the repository

```bash
git clone https://github.com/gabrieodev/ecogames.git
cd ecogames
```

### 2. Install backend dependencies

```bash
cd backend
npm install
```

### 3. Start the server

```bash
npm start
```

The backend will be available at `http://localhost:3000`.

### 4. Open the platform

Open `index.html` directly in the browser or use a static server from the project root:

```bash
npx serve .
```

Access: `http://localhost:5000`

---

## Available Scripts

Run from the `/backend` directory:

| Script | Command | Description |
|---|---|---|
| Start server | `npm start` | Runs Express in production mode |
| Development mode | `npm run dev` | Auto-restart with nodemon |

---

## Contributing

To add a new minigame or improve an existing one:

1. Fork the repository
2. Create a branch named after your game: `git checkout -b feat/game-name`
3. Follow the established modular structure:
   - Frontend with `audio/`, `css/`, `js/` folders and a main `.html` file
   - If backend is needed, add modules with the `game-name.*` prefix
   - Integrate the game card into the lobby
4. Open a Pull Request with the game description and mechanics

---

## License

This project is licensed under the [MIT License](LICENSE).

---

<div align="center">

**EcoGames** — Environmental education through play.

3 games · modular architecture · real-world purpose · Node.js · Express · Vanilla JS

</div>
