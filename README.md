
# Premier League Match & Player Prediction using Poisson Models

This project builds a probabilistic football prediction system for the English Premier League using Poisson regression, combining:
- historical match data (goals, shots, corners, cards)
- time-decayed team strength features
- rigorous train / validation / test evaluation
- player-level allocation using the Fantasy Premier League (FPL) API

The model predicts:
- expected goals for home & away teams
- match outcome probabilities (Home / Draw / Away)
- clean sheet probabilities
- expected team stats (shots, shots on target, corners, yellow & red cards)
- top 3 most likely players to score, assist, shoot, hit the target, or receive a yellow card in a given match

Project Goals:
==================
- Build a transparent, classical ML baseline for football prediction
- Avoid data leakage with strict chronological splits
- Evaluate models honestly using Poisson deviance
- Combine team-level models with player-level data for interpretable outputs
- Create a framework extensible to xG, lineups, and betting comparisons

Data Sources: 
==================

Match Level Data:

Premier League match history (2010/11 → present), including:
- Goals (FTHG, FTAG)
- Shots & shots on target
- Corners
- Yellow & red cards
- Home / Away identifiers
- Match dates & seasons

This is converted into a team-level dataset (one row per team per match).

Player Level Data:

Pulled from the Fantasy Premier League API:
- xG / xA per 90
- minutes & starts per 90
- disciplinary records
- ICT metrics (threat, creativity, influence)
- availability probabilities

Modelling Approach:
========================

Team-level models

Separate Poisson regression models are trained for:
- Home goals
- Away goals
- Corners
- Shots
- Shots on target
- Yellow cards
- Red cards

Key features include:
- rolling attacking strength
- rolling defensive weakness of the opponent
- home advantage
- interaction terms (attack × defence)

Time decay:
==================

Older matches receive less weight using an exponential half-life:

weight = 0.5^(age in years/half-life)

This allows the model to adapt to tactical and squad changes over time.

Primary metric: Poisson deviance
====================================

Measures how well predicted means (λ) match observed counts
Evaluated per match and in aggregate
Analysed using:
- deviance distributions
- cumulative mean deviance
- rolling deviance over time
- learning curves

Typical performance:
- Home goals deviance ≈ 1.15–1.20
- Away goals deviance ≈ 1.00–1.05

These values are strong for classical football models.

Match-level Predictions
====================================

For a future fixture, the model outputs:
- Expected goals (λ_home, λ_away)
- Home / Draw / Away probabilities
- Clean sheet probabilities
- Most likely scoreline
- Expected:
  + shots
  + shots on target
  + corners
  + yellow & red cards

Player-level Predictions
====================================

Team-level expectations are allocated to players using FPL metrics.

Allocation logic
- Goals → xG/90 × expected minutes
- Assists → xA/90 × expected minutes
- Shots → threat × expected minutes
- Shots on target → xG-weighted minutes
- Yellow cards → historical YC/90 × minutes

Expected counts are converted into probabilities using a Poisson assumption.

Output
====================================

For each fixture:
- Expected goals (λ_home, λ_away)
- Home / Draw / Away probabilities
- Clean sheet probabilities
- Most likely scoreline
- Expected:
  + shots
  + shots on target
  + corners
  + yellow & red cards
- Top 3 likely scorers
- Top 3 likely assisters
- Top 3 likely shooters
- Top 3 likely shots on target
- Top 3 most likely to receive a yellow card

Tech Stack
====================================

- Python
- pandas / numpy
- scikit-learn (PoissonRegressor)
- matplotlib
- requests (FPL API)

Limitations
====================================

- No confirmed lineups (minutes are probabilistic)
- No in-game state modelling (e.g. red cards)
- Uses goals, not true event-level xG
- Does not incorporate bookmaker odds (yet)

These are deliberate trade-offs to keep the model interpretable and reproducible.

Future Improvements
====================================

-	Incorporate lineup confirmation and substitutions
-	Replace goals with event-level xG
-	Model red-card states explicitly
-	Compare against bookmaker probabilities
-	Extend to rolling season-by-season backtests
-	Add Bayesian updating for player minutes
