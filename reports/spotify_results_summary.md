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

The experiment uses a group-based train/test split by `track_id`, preventing
the same track from appearing in both training and testing sets.

## Model Results

| model                |     MAE |    RMSE |      R2 |
|:---------------------|--------:|--------:|--------:|
| HistGradientBoosting | 13.0298 | 17.7821 |  0.3692 |
| Ridge Regression     | 14.1671 | 19.2701 |  0.2592 |
| Genre mean baseline  | 14.2342 | 19.3559 |  0.2526 |
| Random Forest        | 16.4954 | 20.5886 |  0.1543 |
| Global mean baseline | 18.9647 | 22.3895 | -0.0001 |

Best model: **HistGradientBoosting** with RMSE = **17.782** and R2 =
**0.369**.

Compared with the global mean baseline RMSE of **22.389**, the best
model reduces error by approximately
**20.6%**.

## Ablation Study

| feature_set                                   |     MAE |    RMSE |     R2 |
|:----------------------------------------------|--------:|--------:|-------:|
| Audio + metadata + simple engineered features | 13.1341 | 17.8674 | 0.3631 |
| Audio + metadata                              | 13.2655 | 18.0723 | 0.3484 |
| Audio features only                           | 16.6330 | 20.5757 | 0.1554 |

## Error Analysis

The best model underpredicts high-popularity tracks. In the high-popularity
bucket, the mean actual popularity is about 73.8, while the mean prediction is
about 39.8. This suggests that external factors such as artist fame, playlist
placement, and marketing are important but missing from the dataset.

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
