# Spotify Popularity Prediction Results

These results come from the notebook `spotify_popularity_prediction.ipynb`.

## Dataset

This project uses the Kaggle Spotify Tracks Dataset | Audio Features. The
dataset contains 114,000 tracks across 114 genres.
The target variable is `popularity`, a Spotify popularity score from 0 to 100.

Raw identifier and text fields are excluded from the model input:
`track_id, artists, album_name, track_name`. The project focuses on audio features and
lightweight metadata instead of full text analysis.

Target summary:

```json
{
  "count": 114000.0,
  "mean": 33.24,
  "std": 22.31,
  "min": 0.0,
  "25%": 17.0,
  "50%": 35.0,
  "75%": 50.0,
  "max": 100.0
}
```

## Predictive Task

Given a track's audio features and metadata, predict its numeric Spotify
popularity score. This is treated as a regression task.

## Model Results

| model                |     MAE |    RMSE |      R2 |
|:---------------------|--------:|--------:|--------:|
| HistGradientBoosting | 12.914  | 17.6121 |  0.3821 |
| Ridge Regression     | 14.169  | 19.3249 |  0.2561 |
| Random Forest        | 16.451  | 20.5556 |  0.1584 |
| Mean baseline        | 18.9841 | 22.4062 | -0      |

Best model: **HistGradientBoosting** with RMSE = **17.612** and R2 =
**0.382**.

Compared with the mean baseline RMSE of **22.406**, the best
model reduces error by approximately
**21.4%**.

## Ablation Study

| feature_set                                   |     MAE |    RMSE |     R2 |
|:----------------------------------------------|--------:|--------:|-------:|
| Audio + metadata + simple engineered features | 13.0408 | 17.7348 | 0.3735 |
| Audio + metadata                              | 13.1496 | 17.8862 | 0.3628 |
| Audio features only                           | 16.1496 | 19.8963 | 0.2115 |

## Feature Importance

The most influential features under permutation importance are:

- track_genre
- track_name_length
- acousticness
- danceability
- loudness
- valence
- energy
- duration_ms

## Interpretation

The model comparison and ablation study measure whether audio features alone
are enough to predict track popularity and whether adding genre/metadata
improves performance. Any strong role for `track_genre` should be interpreted
carefully: genre is useful for prediction, but it may also encode market and
playlist context beyond acoustic properties.
