# ChessGuardian Skill for OpenClaw

An [OpenClaw](https://github.com/kenken64/openclaw) skill that enables AI agents to play chess via the [ChessGuardian](https://chessguardian-production.up.railway.app) platform. Start games, make moves, run autoplay bots (Stockfish or Minimax), and watch live games — all through natural language.

## Features

- **Start & manage chess games** against Stockfish AI or in player-vs-player mode
- **Make moves** in SAN (`e4`, `Nf3`, `O-O`) or UCI (`e2e4`) format
- **Stockfish autoplay bot** — plays as Black using a local Stockfish engine with configurable search depth
- **Minimax autoplay bot** — plays as Black using a custom minimax engine with alpha-beta pruning, piece-square tables, mobility scoring, and king safety evaluation
- **Live game viewing** — shareable browser link and QR code for any game
- **Board snapshots** — render board positions as images via chessboard.js + Playwright

## Project Structure

```
.
├── SKILL.md                          # OpenClaw skill definition & API docs
├── references/
│   └── api.md                        # Detailed API reference (modes, status values, analysis)
└── scripts/
    ├── autoplay_stockfish.py          # Stockfish autoplay bot
    ├── autoplay_minimax.py            # Minimax alpha-beta autoplay bot
    ├── run_stockfish.sh               # Shell wrapper for Stockfish bot
    └── run_minimax.sh                 # Shell wrapper for Minimax bot
```

## Prerequisites

- Python 3 with `chess` and `requests` packages
- [Stockfish](https://stockfishchess.org/) binary (for the Stockfish bot)
- Playwright + Chromium (optional, for board screenshots)

```bash
pip install chess requests
```

## Configuration

Set the ChessGuardian server URL via environment variable (defaults to the production instance):

```bash
export CHESSGUARDIAN_URL=https://chessguardian-production.up.railway.app
```

## Usage

### Stockfish Autoplay Bot

Plays as Black using a local Stockfish engine against the server's Stockfish (White).

```bash
# Start a new game with default settings (depth 20, 5s delay)
./scripts/run_stockfish.sh new

# Start a new game with custom depth and delay
./scripts/run_stockfish.sh new 15 3

# Join an existing game
./scripts/run_stockfish.sh <game_id>

# Or run the Python script directly
python3 -u scripts/autoplay_stockfish.py new --depth 20 --delay 5
```

### Minimax Autoplay Bot

Plays as Black using a custom minimax engine with alpha-beta pruning.

```bash
# Start a new game with default settings (depth 4, 5s delay)
./scripts/run_minimax.sh new

# Start a new game with custom depth and delay
./scripts/run_minimax.sh new 5 3

# Join an existing game
./scripts/run_minimax.sh <game_id>

# Or run the Python script directly
python3 -u scripts/autoplay_minimax.py new --depth 4 --delay 5
```

### Bot Options

| Flag | Description | Default |
|------|-------------|---------|
| `--depth` | Search depth (Stockfish: 1-30, Minimax: 1-6 recommended) | Stockfish: 20, Minimax: 4 |
| `--delay` | Seconds between moves | 5 |
| `--url` | ChessGuardian server URL | Production URL |
| `--max-moves` | Maximum moves before stopping | 200 |

## API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/api/live/start` | Start a new game (`{"mode": "ai"}` or `{"mode": "pvp"}`) |
| `GET` | `/api/live/<game_id>` | Get current game state |
| `POST` | `/api/live/<game_id>/move` | Make a move (`{"move": "e5"}`) |
| `GET` | `/api/live/<game_id>/qr` | Get QR code image (PNG) |

See [references/api.md](references/api.md) for full API documentation including response formats, game status values, and analysis fields.

## Live Viewing

Games can be watched in real-time in the browser:

```
https://chessguardian-production.up.railway.app/live/<game_id>
```

## License

MIT
