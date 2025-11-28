# OMEGA Sports Betting Simulation

A modular, quantitative sports analytics engine designed for sports betting analysis. The system uses a Python-in-LLM pattern where instruction modules (`.md` files) contain executable Python code blocks that are extracted and run in a sandbox environment.

## Overview

OMEGA identifies +EV (positive expected value) wagers by comparing internal simulation probabilities against market-implied probabilities. The system supports multiple sports leagues (NFL, NBA, MLB, NHL, NCAAF, NCAAB) and various bet types (spreads, totals, moneylines, props, parlays).

## Key Features

- **Modular Architecture**: Self-contained modules with clear responsibilities
- **League-Agnostic Core**: Common abstractions support multiple sports
- **Monte Carlo Simulations**: ≥10,000 iterations per market with league-specific mechanics
- **Probability Calibration**: Fixes unrealistic probability extremes
- **Correlated Simulation**: SGP-style correlated market support
- **Data Logging**: File-based storage for simulation outputs and bet recommendations
- **Persistent Bet Tracking**: Swappable storage backend (default: `data/logs/` and `data/exports/`) with backtesting and audit capabilities
- **Standardized Output**: Mandatory tables following Required Output Format protocol

## Quick Start

1. **Read Instructions**: Start with `config/CombinedInstructions.md` for complete system documentation
2. **Load Modules**: Modules must be loaded in the exact order specified in `MODULE_LOAD_ORDER.md`
3. **Run Analysis**: Follow the execution workflow in `config/CombinedInstructions.md`
4. **Review Output**: All outputs follow the required output format with specified tables

## Project Structure

```
omega-betting-agent/
├── modules/                    # Core execution modules
│   ├── foundation/            # Foundation modules (load first)
│   ├── analytics/             # Analytics modules
│   ├── modeling/              # Modeling modules
│   ├── simulation/            # Simulation modules
│   ├── betting/               # Betting logic modules
│   ├── adjustments/           # Adjustment modules
│   └── utilities/             # Utility modules
├── config/                    # Configuration files
│   ├── CombinedInstructions.md
│   └── AGENT_SPACE_INSTRUCTIONS.md
├── docs/                      # Documentation
│   ├── ARCHITECTURE.md
│   ├── bet_log_template.md
│   └── SETUP.md
├── data/                      # Data files (gitignored)
│   ├── logs/                  # Generated logs
│   ├── exports/               # CSV exports
│   └── audits/                # Audit reports
├── examples/                  # Example data files
├── .gitignore
├── README.md
└── MODULE_LOAD_ORDER.md
```

### Core Documentation
- `config/CombinedInstructions.md` - Primary instruction file
- `docs/ARCHITECTURE.md` - System architecture and module documentation
- `docs/SETUP.md` - GitHub setup and Perplexity Space integration guide
- `docs/CHANGELOG.md` - Version history and changes
- `docs/bet_log_template.md` - Bet logging template and schema reference
- `MODULE_LOAD_ORDER.md` - Quick reference for module loading order
- `README.md` - This file

### Module Organization
- **Foundation**: `modules/foundation/` - Configuration and core abstractions
- **Analytics**: `modules/analytics/` - Universal and league-specific analytics
- **Modeling**: `modules/modeling/` - Projections, calibration, context
- **Simulation**: `modules/simulation/` - Monte Carlo and correlated simulation
- **Betting**: `modules/betting/` - Odds evaluation, staking, parlays
- **Adjustments**: `modules/adjustments/` - Injury and context adjustments
- **Utilities**: `modules/utilities/` - Logging, persistence, auditing, formatting

## Module Loading Order

Modules must be loaded in this exact order (see `MODULE_LOAD_ORDER.md` for quick reference):

1. `modules/foundation/model_config.md`
2. `modules/foundation/league_config.md`
3. `modules/foundation/core_abstractions.md`
4. `modules/analytics/universal_analytics.md`
5. `modules/analytics/league_baselines.md`
6. `modules/analytics/market_analysis.md`
7. `modules/modeling/realtime_context.md`
8. `modules/adjustments/injury_adjustments.md`
9. `modules/modeling/projection_model.md`
10. `modules/modeling/probability_calibration.md`
11. `modules/simulation/simulation_engine.md`
12. `modules/simulation/correlated_simulation.md`
13. `modules/betting/odds_eval.md`
14. `modules/betting/parlay_tools.md`
15. `modules/betting/kelly_staking.md`
16. `modules/utilities/model_audit.md`
17. `modules/utilities/data_logging.md`
18. `modules/utilities/sandbox_persistence.md`
19. `modules/utilities/output_formatter.md`

## Usage

### Basic Workflow

1. **Context Normalization**: Normalize weather, rest, travel, pace inputs
2. **Injury Adjustments**: Apply injury impacts to team and player projections
3. **Baseline Projections**: Compute baseline team and player performance
4. **Contextual Adjustments**: Apply context multipliers (pace, efficiency, variance)
5. **Simulation**: Run Monte Carlo simulations (≥10,000 iterations)
6. **Probability Calibration**: Calibrate probabilities to fix extremes
7. **Edge Calculation**: Calculate edge and EV vs market odds
8. **Market Analysis**: Assess market confidence and sharp action
9. **Stake Recommendation**: Calculate Kelly-based stake recommendations
10. **Persistent Logging**: Log bets and simulation results to `data/logs/` and `data/exports/` using `OmegaCacheLogger` (swappable storage backend)
11. **Output Formatting**: Generate required tables per Required Output Format protocol

### Example

```python
# Load modules (in order)
# ... module loading code ...

# Normalize context
context = realtime_context.normalize_realtime_inputs(raw_context, league="NBA")

# Apply injury adjustments
adjusted = injury_adjustments.apply_injury_adjustments(metrics, injuries, league)

# Compute projections
baseline = projection_model.compute_baseline(matchup_data, league)
contextualized = projection_model.compute_context(baseline, context_modifiers, league)

# Simulate
sim_results = simulation_engine.run_game_simulation(projection, n_iter=10000)

# Calibrate probability
calibrated = probability_calibration.calibrate_probability(
    sim_results["true_prob_a"],
    method="combined"
)

# Calculate edge
implied_prob = odds_eval.implied_probability(odds_american)
edge = odds_eval.edge_percentage(calibrated["calibrated"], implied_prob)

# Format output
output = output_formatter.format_full_output(bet_data, context_data, sim_results)
```

## Output Protocol

**CRITICAL**: All analysis outputs MUST follow this exact structure. 

**Required Output:**
1. Full Suggested Summary Table (all markets)
 1a. Game Bets Table (spread/total/ML only/Game Props)
 1b. Player Props Table
2. SGP Mentionables *if applicable for the day*
3. Context Drivers Narrative Briefing
4. Risk & Stake Table (if staking recommendations made)
5. Game/Performance/Metrics/Context Narrative Breakdown Analysis for ending.

## Data Storage

### Storage Strategy (Perplexity Spaces)

**Swappable Storage Backend**: `OmegaCacheLogger` uses configurable paths in GitHub (default: `data/logs/` and `data/exports/`)

- **Session Storage**: `data/logs/` and `data/exports/` directories in GitHub (default paths)
  - `data/logs/bet_log.json` - All bet recommendations and results
  - `data/logs/simulation_log.json` - All simulation results
  - `data/logs/audit_log.json` - Audit history
  - `data/exports/BetLog.csv` - Cumulative bet log export
  - `data/exports/omega_bets_*.csv` - Daily bet summaries

- **Persistence**: Files persist in Spaces if uploaded/synced with GitHub
  - Files written to `data/logs/` and `data/exports/` persist if uploaded/synced to Space
  - Primary persistence: File attachments delivered at end of each task
  - Files can be retrieved from previous thread attachments or Space files

### Fallback Storage

- **Logs**: `logs/` directory (for compatibility)
  - `simulations_YYYY-MM-DD.json` - Simulation outputs
  - `bets_YYYY-MM-DD.json` - Bet recommendations
  - `bets_YYYY-MM-DD.csv` - CSV exports


## Persistent Bet Tracking & Backtesting

### Overview

The `sandbox_persistence.md` module provides persistent bet tracking and backtesting capabilities specifically designed for Perplexity Spaces sandbox environment.

### Storage Location

**Swappable Storage Backend**: `OmegaCacheLogger` accepts configurable paths

**Default Paths** (Recommended for Spaces/Repo):
- `data/logs/` - JSON log files (bet_log.json, simulation_log.json, audit_log.json)
- `data/exports/` - CSV export files (BetLog.csv, daily summaries)
- Files persist in Spaces if uploaded/synced to the Space

**Alternative Paths** (Session-Only):
- Use `/tmp/omega_storage/` if you need strict session-only storage
- Example: `logger = OmegaCacheLogger(base_path="/tmp/omega_storage")`

**Persistence Strategy**:
1. **Session**: Write to `data/logs/` and `data/exports/` during task execution
2. **End of Session**: Export files as attachments for long-term storage
3. **Future Sessions**: Load from previous task attachments or Space files

**Data Retrieval Method**: Previous Task Attachments or Space Files
- Review threads within the Space from previous task sessions
- Locate file attachments (BetLog.csv, bet_log.json, simulation_log.json) from previous tasks
- Or load from explicitly uploaded Space files
- Extract content from those files and load using `load_from_thread_fallback()`

### Daily Task Workflow

**Execution Method**: Extract Python code blocks from `.md` files and execute in Perplexity Spaces sandbox using get-execute.

1. **Extract and Execute Code from sandbox_persistence.md**:
   - Extract `OmegaCacheLogger` class from `modules/utilities/sandbox_persistence.md`
   - Execute the code to make the class available
   - Instantiate: `logger = OmegaCacheLogger()` (defaults to "data/logs" and "data/exports")

2. **Log Bet Recommendations**:
   ```python
   bet_id = logger.log_bet_recommendation({
       "date": "2025-11-24",
       "league": "NBA",
       "matchup": "Pistons vs Pacers",
       "pick": "DET -9.5",
       "bet_type": "spread",
       "odds": "-110",
       "implied_prob": 0.485,
       "model_prob": 0.54,
       "edge_pct": 5.5,
       "confidence": "High",
       "predicted_outcome": "DET 123, IND 110",
       "factors": "Pistons home court advantage"
   })
   ```

3. **Log Simulation Results**:
   ```python
   sim_id = logger.log_simulation_result({
       "date": "2025-11-24",
       "game_id": "NBA_DET_IND",
       "league": "NBA",
       "matchup": "Pistons vs Pacers",
       "sim_results": {...},
       "n_iterations": 10000
   })
   ```

4. **Create Result Files** (for fallback):
   - At end of session, export bet log: `logger.export_to_csv("omega_bets_2025-11-24.csv")`
   - These files can be attached to session output for future thread access

### Audit/Backtesting Task Workflow

**Execution Method**: Extract Python code blocks from `.md` files and execute in Perplexity Spaces sandbox.

1. **Extract and Execute Code from sandbox_persistence.md**:
   - Extract `OmegaCacheLogger` class
   - Execute and instantiate: `logger = OmegaCacheLogger()`

2. **Load Stored Bets**:
   ```python
   # Method A: Read from data/logs/bet_log.json (if file exists and was persisted)
   bets = logger.get_bets_by_date_range("2025-11-20", "2025-11-24")
   
   # Method B: Primary - Load from thread attachments or Space files
   # Review threads from last 7 days, extract JSON content
   thread_files = [
       {"filename": "bet_log.json", "content": <json_content_from_thread>}
   ]
   bets = logger.load_from_thread_fallback(thread_files)
   ```

2. **Update Bet Results** (when games finish):
   ```python
   logger.update_bet_result(
       bet_id="2025-11-24_NBA_1",
       result="Win",
       final_score="DET 125, IND 115",
       closing_odds="-112"
   )
   ```

3. **Run Backtest Audit**:
   ```python
   audit = logger.run_backtest_audit(
       start_date="2025-11-20",
       end_date="2025-11-24"
   )
   # Returns: win_rate, ROI, CLV, Brier score, breakdowns by league/confidence/bet_type
   ```

### Key Features

- **Swappable Storage**: Default paths are `data/logs/` and `data/exports/` (persist in Spaces if uploaded/synced)
- **File Operations**: Use standard Python `open()`, `json.dump()`, `json.load()` to read/write files
- **Persistence**: Files persist in Spaces if uploaded/synced; also deliver as attachments at end of task
- **Data Retrieval**: Load from previous task attachments or Space files by reviewing threads
- **Automatic CLV Calculation**: Closing Line Value computed when closing odds provided
- **Comprehensive Metrics**: Win rate, ROI, CLV, Brier score, breakdowns by league/confidence/bet_type
- **CSV Export**: Export bet log to CSV for external analysis
- **Cumulative Bet Log**: BetLog.csv contains complete history, updated daily

### File Operations

**Storing Files (Default Paths)**:
```python
import json
import os
os.makedirs("data/logs", exist_ok=True)
os.makedirs("data/exports", exist_ok=True)
with open("data/logs/bet_log.json", "w") as f:
    json.dump(data, f, indent=2)
# NOTE: Files persist in Spaces if uploaded/synced; also deliver as attachment
```

**Fetching Files (From Previous Task Attachments or Space Files)**:
```python
# Method 1: During session (if file exists in data/logs/ and was persisted)
import json
if os.path.exists("data/logs/bet_log.json"):
    with open("data/logs/bet_log.json", "r") as f:
        data = json.load(f)

# Method 2: From previous task attachments or Space files (primary method)
# Review threads, extract file content from attachments, then:
thread_files = [{"filename": "bet_log.json", "content": <extracted_content>}]
data = logger.load_from_thread_fallback(thread_files)
```

### Bet Record Schema

Each bet record includes:
- `bet_id`: Unique identifier (format: `YYYY-MM-DD_LEAGUE_N`)
- `date`, `league`, `matchup`, `pick`, `bet_type`
- `odds`, `implied_prob`, `model_prob`, `edge_pct`
- `confidence`, `predicted_outcome`, `factors`
- `stake_units`, `stake_amount` (optional)
- `result`, `final_score`, `closing_odds`, `clv_pct` (updated after game)
- `logged_at`, `updated_at` (timestamps)

See `sandbox_persistence.md` for complete schema documentation.

### Task Instructions

See `TASK_INSTRUCTIONS.md` for simplified, paragraph-style task wording:
- **Task 1 (Morning)**: Generate bets, run simulations, log results, deliver files
- **Task 1B (Early Next Day)**: Update pending bet results, export cumulative BetLog.csv
- **Task 2 (Weekly)**: Fetch data, run audit, generate calibration recommendations

## League Support

### Currently Supported
- **NFL**: Drive-based simulation, EPA metrics, weather impact
- **NBA**: Possession-based, Four Factors, pace adjustments
- **MLB**: Inning-based, run expectancy, weather critical
- **NHL**: Shift-based, xG metrics, goalie adjustments
- **NCAAF/NCAAB**: High-variance, tempo-based

## Key Concepts

### Probability Calibration
Fixes unrealistic probability extremes using:
- Shrinkage toward 0.5
- Reasonable caps (max 0.85, min 0.15)
- Historical performance mapping (when available)

### Correlated Simulation
Supports SGP-style correlated markets by:
1. Simulating team-level outcomes first
2. Deriving player stats from team outcomes via allocation rules
3. Maintaining correlation between team and player outcomes


## Limitations & TODOs

- League-specific baselines use heuristic approximations (TODOs for regression-based implementations)
- EPA, RE, and xG interfaces are simplified (TODOs for data-driven models)
- No automatic data fetching (agent-controlled)

## References

- `config/CombinedInstructions.md` - Complete instruction set
- `docs/ARCHITECTURE.md` - System architecture details
- `docs/SETUP.md` - GitHub setup and integration guide
- `MODULE_LOAD_ORDER.md` - Module loading reference
- Module-specific `.md` files - Detailed function documentation

