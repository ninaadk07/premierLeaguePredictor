
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
- Build a transparent, classical ML baseline for football prediction
- Avoid data leakage with strict chronological splits
- Evaluate models honestly using Poisson deviance
- Combine team-level models with player-level data for interpretable outputs
- Create a framework extensible to xG, lineups, and betting comparisons

Data Sources: 

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

Older matches receive less weight using an exponential half-life:

weight = 0.5^(age in years/half-life)

This allows the model to adapt to tactical and squad changes over time.

Primary metric: Poisson deviance

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
