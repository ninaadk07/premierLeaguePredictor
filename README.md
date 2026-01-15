
# Premier League Match & Player Prediction using Poisson Models

This project builds a probabilistic football prediction system for the English Premier League using Poisson regression, combining:
- historical match data (goals, shots, corners, cards)
- time-decayed team strength features
- rigorous train / validation / test evaluation
- player-level allocation using the Fantasy Premier League (FPL) API

The model predicts:
	•	expected goals for home & away teams
	•	match outcome probabilities (Home / Draw / Away)
	•	clean sheet probabilities
	•	expected team stats (shots, shots on target, corners, yellow & red cards)
	•	top 3 most likely players to score, assist, shoot, hit the target, or receive a yellow card in a given match
