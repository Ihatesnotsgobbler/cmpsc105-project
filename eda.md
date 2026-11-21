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
Balanced by design: every genre has about 1,000 tracks, so the data was sampled
  to balance genres. Row counts do not show how common a genre is on Spotify.
Genres overlap: one song can have several genres, and some labels are moods or
  countries (`sleep`, `french`), not styles. Our genre groups are a judgment call.
 Popularity zeros are uneven: about 68% of jazz tracks have popularity 0, versus
  far fewer in most other groups. We keep them, but they weaken correlations.
 Popularity is a snapshot from one point in time.
 Tempo 0: 157 rows have tempo 0, mostly `sleep` tracks (probably non-musical audio).
Assumption:averaging popularity across repeated rows is fine, since only 720
  track IDs have differing popularity across genres.
# Deliverable 2: Data Exploration (eda.md)
  1.
   - Using the 95th percentile of popularity (67.0) on deduplicated tracks (4,520 Hits vs. 85,221 Standard), Hit tracks show a higher median and mean—and a smaller variance and IQR—than Standard tracks across energy (median 0.689 vs. 0.676, variance 0.0407 vs. 0.0671), danceability (median 0.625 vs. 0.573, variance 0.0238 vs. 0.0315), and loudness (median -6.1235 vs. -7.2550 dB, variance 13.2942 vs. 27.8617). While the narrower spread indicates a somewhat tighter "sweet spot" for hits—especially in avoiding very quiet or low-energy/low-danceability tracks—the distributions still overlap widely, showing a producer that these features are tendencies rather than a guaranteed hit formula.
 2.
- Across the 89,741 unique tracks, popularity has essentially no linear relationship with valence (cov = -0.0624, corr = -0.0115) and a negligible negative correlation with acousticness (cov = -0.2705, corr = -0.0388). Higher valence (musical positivity) does not reliably track with higher popularity across the full dataset.
- Splitting into hip-hop (n = 842) and metal (n = 232) shows that correlations shift by genre: in hip-hop, valence remains near zero (cov = -0.1258, corr = -0.0176) while acousticness turns moderately positive (cov = 1.6659, corr = 0.2669), whereas metal shows a weak-to-moderate negative correlation for valence (cov = -0.7836, corr = -0.1886) and a slight negative correlation for acousticness (cov = -0.1249, corr = -0.0731). Because correlations are modest and change depending on the genre, label executives cannot apply a single universal "hit formula" across their entire catalog and must account for genre norms, artist reach, and promotion.
 3. 
 - Testing whether songs under 2 minutes and 30 seconds have higher average popularity than standard-length songs uses the null hypothesis that short songs do not have a higher mean popularity than standard songs, and the alternative hypothesis that short songs have a higher mean popularity. In the 2,000-shuffle simulation, short songs (12,805 tracks, mean popularity 30.0689) actually averaged lower popularity than standard-length songs (76,936 tracks, mean popularity 33.7193), giving an observed difference of -3.6504 and a one-sided p-value of 1.0000. We fail to reject the null hypothesis, concluding that shorter tracks do not yield higher average popularity and the executive's assumption is not supported