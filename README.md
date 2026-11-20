# CMPE 255 Data Mining Process Assignment

Three projects apply CRISP-DM, SEMMA, and KDD to three distinct Kaggle datasets. Each notebook opens with a methodology map and includes stage-level checks, explanations, and saved outputs.

| Project | Colab | Medium article | YouTube |
| --- | --- | --- | --- |
| CRISP-DM · International soccer | [Open in Colab](https://colab.research.google.com/github/isaackimmi/cmpe255-data-mining-process/blob/main/crisp-dm-soccer/notebook.ipynb) | [From Match History to Kickoff Predictions](https://medium.com/@isaac.kim_79645/from-match-history-to-kickoff-predictions-a91fd30be450) | [Watch walkthrough](https://youtu.be/Kjj3Ffcz_Gg) |
| SEMMA · NBA | [Open in Colab](https://colab.research.google.com/github/isaackimmi/cmpe255-data-mining-process/blob/main/semma-nba/notebook.ipynb) | [Can Recent Form Predict an NBA Home Win?](https://medium.com/@isaac.kim_79645/can-recent-form-predict-an-nba-home-win-2fff7bbd18a8) | [Watch walkthrough](https://youtu.be/Y2aXxSJtipY) |
| KDD · Formula 1 | [Open in Colab](https://colab.research.google.com/github/isaackimmi/cmpe255-data-mining-process/blob/main/kdd-formula1/notebook.ipynb) | [Where Do Formula 1 Drivers Gain Places?](https://medium.com/@isaac.kim_79645/where-do-formula-1-drivers-gain-places-9be7c9ed6aa5) | [Watch walkthrough](https://youtu.be/5WD5Zyfi7KQ) |

## Run the projects

Open a notebook in Colab and use a CPU runtime. Select **Runtime → Run all**. Each notebook downloads a versioned archive from Kaggle and checks the selected CSVs against the recorded SHA-256 fingerprints. If automatic download fails, download the indicated archive from Kaggle, upload the ZIP to the runtime, and set `PROJECT_DATA_ZIP` to its path before the setup cell runs.

| Project | Dataset snapshot | Supporting files |
| --- | --- | --- |
| CRISP-DM | [International Football Results, v138](https://www.kaggle.com/datasets/martj42/international-football-results-from-1872-to-2017/versions/138) | [Detailed report](crisp-dm-soccer/report.md), [evaluation](crisp-dm-soccer/artifacts/evaluation.json), [saved prediction model](crisp-dm-soccer/artifacts/prediction_model.json) |
| SEMMA | [NBA Games Data, v10](https://www.kaggle.com/datasets/nathanlauga/nba-games/versions/10) | [Held-out assessment](semma-nba/artifacts/assessment.json) |
| KDD | [Formula 1 World Championship, v24](https://www.kaggle.com/datasets/rohanrao/formula-1-world-championship-1950-2020/versions/24) | [Scope counts](kdd-formula1/artifacts/scope.json), [pattern comparison](kdd-formula1/artifacts/pattern_comparison.csv), [race counts](kdd-formula1/artifacts/later_race_support.csv) |

For the soccer inference demo in a fresh runtime, run the standalone scoring cell in the Deployment section. It loads the saved prediction model without training. The model uses ratings through December 31, 2025.
