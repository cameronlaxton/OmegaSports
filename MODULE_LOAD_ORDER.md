# Module Loading Order Reference

This document provides a quick reference for the exact module loading order required by the OMEGA system.

## Loading Order (19 Modules)

Modules must be loaded in this exact sequence:

### Foundation (Load First)
1. `modules/foundation/model_config.md` - Configuration and thresholds
2. `modules/foundation/league_config.md` - League-specific configuration parameters
3. `modules/foundation/core_abstractions.md` - League-agnostic core abstractions (Team, Player, Game, State)

### Analytics
4. `modules/analytics/universal_analytics.md` - Universal models (Pythagorean, Elo/Glicko)
5. `modules/analytics/league_baselines.md` - League-specific baselines (Four Factors, EPA, RE, xG)
6. `modules/analytics/market_analysis.md` - Market intelligence

### Modeling
7. `modules/modeling/realtime_context.md` - Context normalization
8. `modules/adjustments/injury_adjustments.md` - Injury impact modeling
9. `modules/modeling/projection_model.md` - Baseline and contextual projections
10. `modules/modeling/probability_calibration.md` - Probability calibration system

### Simulation
11. `modules/simulation/simulation_engine.md` - Monte Carlo simulations
12. `modules/simulation/correlated_simulation.md` - Correlated simulation for SGP support

### Betting
13. `modules/betting/odds_eval.md` - Odds conversion and EV calculation
14. `modules/betting/parlay_tools.md` - Correlation and parlay evaluation
15. `modules/betting/kelly_staking.md` - Bankroll management

### Utilities
16. `modules/utilities/model_audit.md` - Performance tracking
17. `modules/utilities/data_logging.md` - Data logging
18. `modules/utilities/sandbox_persistence.md` - Persistent bet tracking and backtesting
19. `modules/utilities/output_formatter.md` - Standardized output generation

## Loading Instructions

For each module:
1. Open the file
2. Read metadata header
3. Extract every ```python``` block
4. Execute them using the available sandbox execution method

## Critical Rules

- **NEVER skip modules** - All 19 modules must be loaded
- **NEVER change the order** - Dependencies require this exact sequence
- **If a module fails to load**: HALT ANALYSIS and report the failure
- **NEVER improvise** - Do not fabricate fallback calculations

## Module Dependencies

- Foundation modules (1-3) have no dependencies
- Analytics modules (4-6) depend on foundation
- Modeling modules (7-10) depend on foundation and analytics
- Simulation modules (11-12) depend on modeling
- Betting modules (13-15) depend on simulation
- Utility modules (16-19) depend on all previous modules

## Quick Copy-Paste List

```
modules/foundation/model_config.md
modules/foundation/league_config.md
modules/foundation/core_abstractions.md
modules/analytics/universal_analytics.md
modules/analytics/league_baselines.md
modules/analytics/market_analysis.md
modules/modeling/realtime_context.md
modules/modeling/injury_adjustments.md
modules/modeling/projection_model.md
modules/modeling/probability_calibration.md
modules/simulation/simulation_engine.md
modules/simulation/correlated_simulation.md
modules/betting/odds_eval.md
modules/betting/parlay_tools.md
modules/betting/kelly_staking.md
modules/utilities/model_audit.md
modules/utilities/data_logging.md
modules/utilities/sandbox_persistence.md
modules/utilities/output_formatter.md
```

