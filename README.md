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
- Ridge Regression
- Random Forest Regressor
- HistGradientBoosting Regressor

## Results from the Current Notebook Run

| Model | MAE | RMSE | R2 |
|---|---:|---:|---:|
| HistGradientBoosting | 12.914 | 17.612 | 0.382 |
| Ridge Regression | 14.169 | 19.325 | 0.256 |
| Random Forest | 16.451 | 20.556 | 0.158 |
| Mean baseline | 18.984 | 22.406 | ~0.000 |

HistGradientBoosting performs best. It reduces RMSE by about 21% compared with
the mean baseline.

## Ablation Study

| Feature Set | MAE | RMSE | R2 |
|---|---:|---:|---:|
| Audio + metadata + simple engineered features | 13.041 | 17.735 | 0.374 |
| Audio + metadata | 13.150 | 17.886 | 0.363 |
| Audio features only | 16.150 | 19.896 | 0.211 |

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

- Dataset shape, column types, and missing-value check
- Duplicate `track_id` and data granularity check
- Popularity distribution
- Audio feature correlations with popularity
- Top and bottom genre-level popularity analysis
- Audio feature distributions
- Correlation heatmap
- Explicit vs non-explicit popularity comparison
- Low / medium / high popularity bucket profiles

## How to Run

Open `spotify_popularity_prediction.ipynb` in Cursor or Jupyter and run the
cells from top to bottom.

The notebook will recreate any figures or CSV outputs if needed.
