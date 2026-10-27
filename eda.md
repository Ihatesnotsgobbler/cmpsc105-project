# Deliverable 1: Data Exploration (eda.md)

## Dataset overview
The dataset is a Spotify tracks file with 114,000 rows and 21 columns. In the raw
file, one row is one track listed under one genre. It covers 89,741 unique tracks,
31,437 artists, and 114 genres, with exactly 1,000 rows per genre. Each row has
track information (ID, artist, album, name), a popularity score (0 to 100), and
audio features such as danceability, energy, loudness, tempo, and valence.

## Data quality and cleaning
- Missing values: one row has no artist, album name, or track name. We removed
  it because it is 1 of 114,000 rows and will not change any results.
- Repeated tracks: 24,259 rows repeat a track ID that already appears. No row is
  an exact copy. The same song is listed once per genre it belongs to, so only the
  genre differs.
- Unnecessary column: `Unnamed: 0` is an old row index, so we dropped it.
- Deduplication: we grouped rows by track ID, artist, album, and track name to
  get one row per track (89,740 after the missing row was removed). Audio features and
  popularity were averaged across repeats, `duration_ms` and `explicit` use the first
  value, and genres were joined into one sorted, comma-separated string, so the genre
  information is kept.
- Genre groups: we mapped the 114 genre labels into 20 broader groups (for
  example `latino` into Latin, `indie-pop` into Pop) and kept the original label.

## Summary statistics
Average popularity is about 33 (median 33) with a standard deviation of about 21.
Many tracks have popularity 0 (about 16,000 rows in the raw data). Tracks are moderately
danceable (mean 0.56) and energetic (mean 0.63), with an average tempo of about 122 BPM.

## Visualizations and observations
- Popularity vs danceability (regression plot, 8,000-track sample): the red
  trendline is almost flat, so there is no strong linear relationship.
- Hexbin density plot: shows where tracks concentrate. Most are in the middle of
  the danceability range, with a large cluster at popularity 0.
- Correlation heatmap: popularity has very weak correlations with every audio
  feature (all between -0.13 and 0.07). The strongest relationships are among the audio
  features themselves: energy and loudness (0.76), and energy and acousticness (-0.73).

## Dataset quirks and assumptions
- Balanced by design: every genre has about 1,000 tracks, so the data was sampled
  to balance genres. Row counts do not show how common a genre is on Spotify.
- Genres overlap: one song can have several genres, and some labels are moods or
  countries (`sleep`, `french`), not styles. Our genre groups are a judgment call.
- Popularity zeros are uneven: about 68% of jazz tracks have popularity 0, versus
  far fewer in most other groups. We keep them, but they weaken correlations.
- Popularity is a snapshot from one point in time.
- Tempo 0: 157 rows have tempo 0, mostly `sleep` tracks (probably non-musical audio).
- Assumption:averaging popularity across repeated rows is fine, since only 720
  track IDs have differing popularity across genres.