# Predicting Spotify Track Popularity Using Audio Features and Metadata

## 1. Introduction

Music streaming platforms such as Spotify expose large collections of songs
along with machine-generated audio features. These features describe musical
properties such as danceability, energy, acousticness, loudness, valence, tempo,
and instrumentalness. A natural data mining question is whether these musical
attributes can predict how popular a track becomes.

This project studies the question: **Can Spotify track popularity be predicted
from audio features and metadata?** We formulate the task as a regression
problem where the target variable is `popularity`, a Spotify score ranging from
0 to 100. The full experiment is implemented in the notebook
`spotify_popularity_prediction.ipynb`.

## 2. Dataset and Exploratory Analysis

The dataset is the Kaggle **Spotify Tracks Dataset | Audio Features** dataset.
It contains 114,000 tracks across 114 genres and 31,437 unique artists. Each row
represents one Spotify track and includes track metadata, a popularity score,
and Spotify audio features.

The original columns include `track_id`, `artists`, `album_name`, `track_name`,
`popularity`, `duration_ms`, `explicit`, `danceability`, `energy`, `key`,
`loudness`, `mode`, `speechiness`, `acousticness`, `instrumentalness`,
`liveness`, `valence`, `tempo`, `time_signature`, and `track_genre`.

The target variable is `popularity`. Its mean is 33.24, median is 35, and the
range is 0 to 100. The distribution is broad, which makes regression meaningful
but also difficult.

We exclude raw identifier and text fields: `track_id`, `artists`,
`album_name`, and `track_name`. These fields may cause memorization of specific
songs or artists. The project focuses on audio features and lightweight
metadata rather than full text analysis.

The notebook's exploratory analysis focuses on:

- Distribution of track popularity.
- Correlations between audio features and popularity.
- Average popularity by genre.
- Relationships between danceability, acousticness, loudness, and popularity.

## 3. Predictive Task

Given a track's audio features and metadata, predict the track's numeric
Spotify popularity score. This is a supervised regression task.

The evaluation metrics are:

- **MAE**, which measures average absolute prediction error.
- **RMSE**, which penalizes larger mistakes more heavily.
- **R2**, which measures the proportion of variance explained by the model.

The baseline model predicts the mean popularity from the training set. This
baseline is appropriate because it shows whether machine learning models learn
structure beyond the marginal popularity distribution.

## 4. Models

The notebook compares four models:

- **Mean baseline**: predicts the average training-set popularity.
- **Ridge Regression**: a regularized linear model.
- **Random Forest Regressor**: a nonlinear bagged tree ensemble.
- **HistGradientBoosting Regressor**: a scalable gradient boosting model for
  tabular data.

Numerical features are median-imputed. Categorical features are imputed with the
most frequent value and one-hot encoded. Ridge Regression uses standardized
numerical features.

The main proposed model is HistGradientBoosting because gradient boosting is
well suited for tabular prediction tasks and can capture nonlinear feature
interactions between genre, audio features, and metadata.

## 5. Literature Review Plan

The literature review should connect this project to:

- Music information retrieval.
- Spotify audio feature analysis.
- Hit song or music popularity prediction.
- Content-based music recommendation.

Relevant search terms:

- "Spotify popularity prediction audio features"
- "music popularity prediction machine learning"
- "hit song science audio features"
- "content based music recommendation Spotify audio features"

The novelty of this project is not a new algorithm. The contribution is a
clear notebook-based comparison between audio-only and audio-plus-metadata
prediction settings, with feature importance analysis explaining which signals
matter most.

## 6. Results

| Model | MAE | RMSE | R2 |
|---|---:|---:|---:|
| HistGradientBoosting | 12.914 | 17.612 | 0.382 |
| Ridge Regression | 14.169 | 19.325 | 0.256 |
| Random Forest | 16.451 | 20.556 | 0.158 |
| Mean baseline | 18.984 | 22.406 | ~0.000 |

HistGradientBoosting performs best, with an RMSE of 17.612 and an R2 of 0.382.
Compared with the mean baseline RMSE of 22.406, the best model reduces error by
about 21.5%.

The R2 score shows that popularity is only partially predictable from the
available features. This is expected because song popularity also depends on
factors not present in the dataset, such as marketing, playlists, release date,
artist fame, social media trends, and listener network effects.

## 7. Ablation Study

| Feature Set | MAE | RMSE | R2 |
|---|---:|---:|---:|
| Audio + metadata + simple engineered features | 13.041 | 17.735 | 0.374 |
| Audio + metadata | 13.150 | 17.886 | 0.363 |
| Audio features only | 16.150 | 19.896 | 0.211 |

The ablation study shows that audio features alone contain predictive signal,
but metadata improves performance substantially. In particular, adding
`track_genre`, `duration_ms`, and `explicit` increases R2 from about 0.211 to
about 0.363. Simple engineered features such as track name length, artist
count, and duration in minutes add a small additional improvement.

## 8. Feature Importance and Interpretation

Permutation importance identifies the most influential features as:

- `track_genre`
- `track_name_length`
- `acousticness`
- `danceability`
- `loudness`
- `valence`
- `energy`
- `duration_ms`

The strongest feature is `track_genre`, which suggests that popularity is
partly shaped by genre-level listening patterns and market context. Among audio
features, acousticness, danceability, loudness, valence, and energy are
important. These features describe the sonic character of the song and help the
model distinguish tracks that may be more likely to appeal to large audiences.

## 9. Limitations

First, Spotify popularity is time-dependent, but the dataset provides only a
single snapshot. Second, the dataset does not include release date, playlist
placement, streaming count, artist follower count, or marketing information.
Third, genre is useful for prediction but may encode external popularity
patterns rather than purely musical properties. Finally, excluding raw artist
and track names improves generalization, but it also removes information about
artist fame.

## 10. Conclusion

This project shows that Spotify track popularity can be predicted moderately
well from audio features and metadata. The best model, HistGradientBoosting,
outperforms the mean baseline and explains about 38% of the variance in
popularity. The ablation study shows that audio features alone are useful, but
genre and metadata provide major additional signal. The main takeaway is that
track popularity is not purely an acoustic property; it also reflects genre and
market context.
