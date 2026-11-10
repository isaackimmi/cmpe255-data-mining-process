# CRISP-DM report · International soccer match predictions

## 1. Business understanding

A match preview writer needs a simple way to compare two national teams before kickoff. The output gives probabilities for a win, draw, or loss from the listed home team's perspective. The main success criterion is lower log loss than the training outcome frequencies. Macro F1 and the confusion matrix show where the hard predictions fail.

## 2. Data understanding

The [Kaggle snapshot](https://www.kaggle.com/datasets/martj42/international-football-results-from-1872-to-2017/versions/138) contains 49,945 rows before filtering. Each row is a men's international match. The final scores define the outcome before any penalty shootout. A neutral flag distinguishes the first-listed team from a team that is actually at home.

The input fields used for the analysis had no missing values in this snapshot. Across the 1990–2025 analysis window, listed home teams won 50.7% of matches at non-neutral venues and 42.7% at neutral venues. Teams and competitions differ across those groups, so this comparison does not isolate a causal venue effect.

## 3. Data preparation

Remove exact duplicate rows across the six fields used for the analysis. Keep 1990–2025 and calculate each match result from the two scores. Start both teams at an Elo rating of 1,500. Use the matches from 1990–1999 to warm up the ratings. Elo measures relative team strength using earlier results. Each match uses the Elo difference and the neutral flag. Calculate all features for a given date before applying any results from that date.

| Role | Dates | Rows |
| --- | --- | ---: |
| Train | 2000–2018 | 18,184 |
| Validation | 2019–2022 | 3,597 |
| Test | 2023–2025 | 3,297 |

The test simulation updates team ratings with completed matches from earlier dates. Model coefficients stay frozen. This mirrors a preview writer who learns scores after each day of matches.

## 4. Modeling

The baseline predicts the same outcome frequencies for every matchup. The learned model scales the Elo difference and venue flag, then uses multinomial logistic regression to estimate three probabilities. Compare three values of the regularization parameter C using validation log loss. Select C = 10 and refit both estimators with the combined training and validation rows.

## 5. Evaluation

| Method | Test log loss | Test macro F1 |
| --- | ---: | ---: |
| Outcome frequencies | 1.0540 | 0.2134 |
| Elo difference and neutral flag | 0.8765 | 0.4423 |

The model improved both metrics on this split. Its paired log-loss difference was −0.1775. A 95% interval from 1,000 resamples of match dates was −0.1975 to −0.1582. This interval describes the test cohort. Its log loss was 0.8528 for 2,219 non-neutral matches and 0.9254 for 1,078 neutral matches. A draw was never the model's highest probability class, even though 753 matches ended in a draw. The probabilities are more informative for this use case than the most likely label alone. The dataset does not capture lineups, injuries, or other context that changes shortly before a match.

## 6. Deployment

The [prediction model](artifacts/prediction_model.json) contains the model coefficients, scaling values, class labels, and team ratings through December 31, 2025. In a fresh Colab runtime, run the standalone scoring cell. When a local copy is absent, it downloads the JSON from a fixed GitHub commit and checks its SHA-256 fingerprint. It needs only the file, two distinct team names, and the venue flag. It can score the match without a dataset download or retraining.

The neutral-venue Argentina versus France example produces about 48.4% for Argentina to win, 25.4% for a draw, and 26.3% for France to win. The notebook tests that the exported artifact reproduces the trained estimator's probabilities. Values can appear to sum to slightly above or below 100% due to rounding.

For a future update, acquire a new snapshot and run the same pipeline. Record the rating date, compare log loss and draw performance on a new held-out period, and replace the release artifact only after that check. [IBM CRISP-DM deployment reference](https://www.ibm.com/docs/en/spss-modeler/19.0.0?topic=deployment-overview)
