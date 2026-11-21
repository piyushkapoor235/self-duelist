# Self Duelist

A 2D fighting game in which AI learns a player's fighting style using behavior cloning.

## Running the Game in the Godot Editor

1. Install [Godot 4.3+](https://godotengine.org/download).
2. Open the `game/` folder in Godot using **Import Project**.
3. Press **F5** to start the main scene at `scenes/main.tscn`. Player matches are automatically logged at 10 Hz, whenever the character is actionable, into `data/raw/human_self.csv`.
4. Controls:
   - **A/D** or **←/→**: Move · **W/Space**: Jump
   - **J**: Light punch (`light`) · **K**: Heavy punch (`heavy`) · **L**: Kick (`kick`)
   - **S/↓**: Block (`block`) · **Q**: Dodge (`dodge`) · **E**: Dash forward (`dash_fwd`)
   - **1 / 2 / 3 / 0**: Change the bot opponent (`Rusher` / `Turtle` / `Poker` / `Random`)
   - **4 / 5 / 6 / 7 / 8**: Fight a `Mirror` opponent (`Default` / `Mirror-Rusher` / `Mirror-Turtle` / `Mirror-Poker` / `Mirror-Self`)
   - **9**: Visual **Mirror-X vs. Bot-X** mode, cycling through `Rusher`, `Turtle`, and `Poker`
   - **R/Enter**: Start a new match · **Esc**: Open or close the mode-selection menu
   - **Tab**: Toggle the `AI Brain` probability-distribution table (MLP at 10 Hz) · **F2**: Toggle hitbox and hurtbox frames.

## Running Headless Tests, Smoke Tests, and the Baseline

```powershell
# From the repository root (after installing Godot):
$env:GODOT_EXE = "C:\path\to\Godot_v4.x-stable_win64.exe"
.\scripts\run_headless.ps1

# Run the complete Week 3 smoke test (generate data -> train -> export JSON -> verify parity < 1e-4):
powershell -ExecutionPolicy Bypass -File .\scripts\run_smoke_test.ps1

# Run the complete Week 4 pipeline (generate >=10,000 samples per bot + 15 human_self sessions -> train four Mirror models -> evaluate and export reports to results/):
powershell -ExecutionPolicy Bypass -File .\scripts\run_week4_baseline.ps1

# Run N headless matches between two bots and write the results to CSV:
.\scripts\run_matches.ps1 -P1 rusher -P2 rotate -Matches 500 -ResultsCsv data/raw/matches.csv -LogCsv data/raw/rusher.csv
```

Or run the scenes directly:

```text
godot --headless --path game --scene res://scenes/headless_test.tscn
godot --headless --path game --scene res://scenes/headless_parity_test.tscn
```

## Folder Structure

```text
self-duelist-ai/
├── game/                 # Godot 4 project (characters, controllers, MLP, data logger, match runner)
├── ml/                   # Python/PyTorch pipeline (dataset, model, training, JSON export, smoke tests, baseline)
├── scripts/              # PowerShell runners (headless, matches, smoke test, baseline)
└── data/raw, data/models # CSV logs and trained JSON/PT models
```
