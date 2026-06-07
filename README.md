# Predicting Spotify Track Popularity

DSC148 final project: **Predicting Spotify Track Popularity Using Audio
Features and Genre Metadata**.

## Links

- [GitHub Pages project site](https://tiantianzheng.github.io/spotify-popularity-prediction/)
- [Interactive predictor demo](https://tiantianzheng.github.io/spotify-popularity-prediction/demo.html)
- [Final report PDF](DSC148_Final_Project.pdf)
- [Project notebook](spotify_popularity_prediction.ipynb)

## Research Question

Can Spotify track popularity be predicted from audio characteristics and
lightweight metadata, and which features contribute most to popularity?

## Dataset

- Source: `saichaitanyareddyai/spotify-tracks-dataset-audio-features`
- Rows: 114,000 tracks
- Original columns: 20
- Columns after feature engineering: 23
- Unique track IDs: 89,741
- Genres: 114
- Artists: 31,437 unique artists
- Target: `popularity`, a 0-100 Spotify popularity score

Raw identifiers and high-cardinality text fields are excluded from the model
input to reduce memorization: `track_id`, `artists`, `album_name`, and
`track_name`.

## Method

The project uses supervised regression with a grouped train/test split by
`track_id`, so the same track cannot appear in both training and testing sets.

Models compared:

- Global mean baseline
- Genre mean baseline
- Ridge Regression
- Random Forest Regressor
- HistGradientBoosting Regressor

The GitHub Pages demo uses an exported Ridge Regression model because it can run
fully in the browser without a backend.

## Results

| Model | MAE | RMSE | R2 |
|---|---:|---:|---:|
| HistGradientBoosting | 13.030 | 17.782 | 0.369 |
| Ridge Regression | 14.167 | 19.270 | 0.259 |
| Genre mean baseline | 14.234 | 19.356 | 0.253 |
| Random Forest | 16.495 | 20.589 | 0.154 |
| Global mean baseline | 18.965 | 22.389 | ~0.000 |

HistGradientBoosting performs best, reducing RMSE by about 21% compared with
the global mean baseline.

## Key Findings

- `track_genre` is the strongest predictor under permutation importance.
- Audio-only features are useful but limited.
- Genre metadata captures important market and audience context.
- High-popularity tracks are often underpredicted, suggesting that external
  factors such as artist fame, playlists, marketing, and social trends are
  important but missing from the dataset.

## Repository Contents

```text
.
├── DSC148_Final_Project.pdf
├── README.md
├── demo.html
├── index.html
├── requirements.txt
└── spotify_popularity_prediction.ipynb
```

## Run

Open `spotify_popularity_prediction.ipynb` in Jupyter, Cursor, or VS Code and
run the cells from top to bottom.
