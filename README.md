# Predicting Spotify Track Popularity

This project predicts Spotify track `popularity` using audio features and
lightweight metadata from the Kaggle dataset **Spotify Tracks Dataset | Audio
Features**.

The full project workflow is in:

[spotify_popularity_prediction.ipynb](/Users/deidei/Documents/Codex/2026-06-04/files-mentioned-by-the-user-project/spotify_popularity_prediction.ipynb)

## Research Question

Can Spotify track popularity be predicted from audio characteristics and
metadata, and which features contribute most to popularity?

## Dataset

- Source: `saichaitanyareddyai/spotify-tracks-dataset-audio-features`
- File: `spotify-tracks-dataset-detailed.csv`
- Rows: 114,000 tracks
- Genres: 114
- Artists: 31,437 unique artists
- Target variable: `popularity`, a score from 0 to 100

The notebook excludes raw identifier and text fields:

- `track_id`
- `artists`
- `album_name`
- `track_name`

These fields are excluded because they may encourage memorization of specific
songs or artists. This project focuses on audio features and simple metadata,
not full text analysis.

## Predictive Task

This is a regression task. Given a track's audio features and metadata, predict
its numeric Spotify popularity score.

Evaluation metrics:

- MAE
- RMSE
- R2

## Models

The notebook compares:

- Mean baseline
- Genre mean baseline
- Ridge Regression
- Random Forest Regressor
- HistGradientBoosting Regressor

The train/test split is grouped by `track_id`, so the same track cannot appear
in both training and testing sets.

## Results from the Current Notebook Run

| Model | MAE | RMSE | R2 |
|---|---:|---:|---:|
| HistGradientBoosting | 13.030 | 17.782 | 0.369 |
| Ridge Regression | 14.167 | 19.270 | 0.259 |
| Genre mean baseline | 14.234 | 19.356 | 0.253 |
| Random Forest | 16.495 | 20.589 | 0.154 |
| Global mean baseline | 18.965 | 22.389 | ~0.000 |

HistGradientBoosting performs best. It reduces RMSE by about 21% compared with
the mean baseline.

## Ablation Study

| Feature Set | MAE | RMSE | R2 |
|---|---:|---:|---:|
| Audio + metadata + simple engineered features | 13.134 | 17.867 | 0.363 |
| Audio + metadata | 13.266 | 18.072 | 0.348 |
| Audio features only | 16.633 | 20.576 | 0.155 |

The ablation study shows that audio features alone provide useful signal, but
genre and metadata substantially improve prediction.

## Key Findings

The strongest predictors under permutation importance are:

- `track_genre`
- `track_name_length`
- `acousticness`
- `danceability`
- `loudness`
- `valence`
- `energy`
- `duration_ms`

The major takeaway is that Spotify popularity cannot be explained by acoustic
features alone. Genre and lightweight metadata capture important market context.

## EDA Included in the Notebook

The notebook includes:

- Data quality, missing-value, and duplicate `track_id` checks
- Popularity target distribution
- Genre-level popularity patterns, including top and bottom genres
- Audio feature distributions
- Audio feature correlations with popularity
- Correlation heatmap among audio features
- Explicit vs non-explicit popularity comparison
- Low / medium / high popularity bucket profiles
- EDA summary that motivates the model and ablation design

## How to Run

Open `spotify_popularity_prediction.ipynb` in Cursor or Jupyter and run the
cells from top to bottom.

The notebook will recreate any figures or CSV outputs if needed.
