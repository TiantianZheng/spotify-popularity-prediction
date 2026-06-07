# Predicting Spotify Track Popularity

This repository contains the DSC148 final project **Predicting Spotify Track
Popularity Using Audio Features and Genre Metadata**.

## Project Files

- [Final report PDF](reports/DSC148_Final_Project.pdf)
- [Project notebook](spotify_popularity_prediction.ipynb)
- [Report draft / companion notes](reports/project_report_draft.md)
- [Results summary](reports/spotify_results_summary.md)
- [Requirements](requirements.txt)

## Research Question

Can Spotify track popularity be predicted from audio characteristics and
lightweight metadata, and which features contribute most to popularity?

## Dataset

- Source: `saichaitanyareddyai/spotify-tracks-dataset-audio-features`
- File: `spotify-tracks-dataset-detailed.csv`
- Rows: 114,000 tracks
- Original columns: 20
- Columns after feature engineering: 23
- Unique track IDs: 89,741
- Genres: 114
- Artists: 31,437 unique artists
- Target variable: `popularity`, a score from 0 to 100

The notebook excludes raw identifier and high-cardinality text fields:

- `track_id`
- `artists`
- `album_name`
- `track_name`

These fields are excluded because they may encourage memorization of specific
songs or artists. The project focuses on audio features and lightweight
metadata rather than full text analysis.

## Project Components

The notebook and report are organized around the project instruction
components:

| Required component | Location |
|---|---|
| Dataset | Notebook Section 1; report Section 2 |
| Predictive Task | Notebook Section 2; report Section 3 |
| Model | Notebook Section 3; report Section 5 |
| Literature | Notebook Section 4; report Section 4 |
| Results | Notebook Section 5; report Section 6 |

## Predictive Task

This is a regression task. Given a track's audio features and metadata, predict
its numeric Spotify popularity score.

Evaluation metrics:

- MAE
- RMSE
- R2

The train/test split is grouped by `track_id`, so the same track cannot appear
in both training and testing sets.

## Models

The notebook compares:

- Global mean baseline
- Genre mean baseline
- Ridge Regression
- Random Forest Regressor
- HistGradientBoosting Regressor

## Results

| Model | MAE | RMSE | R2 |
|---|---:|---:|---:|
| HistGradientBoosting | 13.030 | 17.782 | 0.369 |
| Ridge Regression | 14.167 | 19.270 | 0.259 |
| Genre mean baseline | 14.234 | 19.356 | 0.253 |
| Random Forest | 16.495 | 20.589 | 0.154 |
| Global mean baseline | 18.965 | 22.389 | ~0.000 |

HistGradientBoosting performs best. It reduces RMSE by about 21% compared with
the global mean baseline.

## Ablation Study

| Feature Set | MAE | RMSE | R2 |
|---|---:|---:|---:|
| Audio + metadata + simple engineered features | 13.134 | 17.867 | 0.363 |
| Audio + metadata | 13.266 | 18.072 | 0.348 |
| Audio features only | 16.633 | 20.576 | 0.155 |

The ablation study shows that audio features alone provide useful signal, but
genre and metadata substantially improve prediction.

## Key Findings

- `track_genre` is the strongest feature under permutation importance.
- Audio-only features are useful but limited.
- Genre metadata captures important market and audience context.
- High-popularity tracks are often underpredicted, suggesting that missing
  external factors such as artist fame, playlists, marketing, and social trends
  matter.

## EDA Included in the Notebook

The notebook includes:

- Data quality, missing-value, and duplicate `track_id` checks
- Popularity target distribution
- Genre-level popularity patterns, including top and bottom genres
- Audio feature distributions
- Audio feature correlations with popularity
- Correlation heatmap among audio features
- EDA summary that motivates the model and ablation design

## How to Run

Open `spotify_popularity_prediction.ipynb` in Cursor or Jupyter and run the
cells from top to bottom.

The notebook will recreate any ignored figure or CSV output directories if
needed.
