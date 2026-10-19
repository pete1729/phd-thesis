# Characterising the Social Media Temporal Response to External Events

Supplementary data for my PhD thesis (School of Mathematical Sciences, University of Adelaide, 2019). The thesis links to this repository so its analysis can be replicated.

**Thesis:** [University of Adelaide digital repository](https://hdl.handle.net/2440/120053)

## About the thesis

Social media reacts to offline events faster than traditional news. This thesis studies the timing of that reaction on Twitter, using case studies from politics and sport:

- **Tweets and news (Chapter 3):** Granger causality shows the Twitter reaction to events comes before the news reaction. The chapter also looks at how well tweet and news volumes can predict each other.
- **Retweet timing (Chapters 4–5):** How quickly a tweet stops being retweeted is better described by a power law with exponential cutoff than by the widely used plain power law. This finding underpins a method for estimating the final size of a retweet cascade from its early activity.
- **Clustering and event times (Chapter 6):** The chance that two tweets are about the same event decays exponentially with the time between them. SMERC (Social Media Event Response Clustering) uses this to cluster tweets by text and time together. The chapter also estimates when an event happened from the timing of the Twitter response.

## Contents

| File | Chapter | Description |
|---|---|---|
| `Chapter4/tweet_ids.txt` | 4 | 1,676 comma-separated seed tweet IDs, posted 5 April – 4 May 2016, whose retweets were collected for the retweet-timing analysis. |
| `Chapter4/ClausetTestOutput.pdf` | 4 | Full per-dataset results of Clauset's statistical test for a power law (number of data points, fitted α, p-value), summarised in Section 4.3.3. |
| `Chapter6/SecondPrelimData.csv` | 6 | About 7,300 tweets from the 2017 AFL preliminary final, Richmond vs GWS Giants (`#AFLTigersGiants`), 23 September 2017. |
| `Chapter6/WBBLFirstWeekend.csv` | 6 | About 3,150 tweets tagged `#WBBL03` from the opening of the 2017/18 Women's Big Bash League, 1–11 December 2017. |
| `Chapter6/HeatStars.csv` | 6 | About 5,000 tweets tagged `#BBL07` around the Brisbane Heat vs Melbourne Stars BBL match, 20 December 2017. |

### Chapter 6 file format

Each line has three fields separated by `^` (tweet text can contain commas, so a comma separator wasn't used):

```
tweet text^timestamp^label
```

- **timestamp:** `YYYY-MM-DD HH:MM:SS`, in UTC.
- **label:** the manual event label. `U` means the tweet wasn't linked to a labelled event. Any other letter (`A`, `G`, `H`, …) identifies the on-field event the tweet responds to, and tweets sharing a letter respond to the same event.

Loading a file in pandas:

```python
import pandas as pd

df = pd.read_csv(
    "Chapter6/HeatStars.csv",
    sep="^",
    names=["text", "timestamp", "label"],
    parse_dates=["timestamp"],
    quoting=3,  # csv.QUOTE_NONE: tweet text contains stray quote marks
    engine="python",
)
```

## Rebuilding the retweet datasets

Twitter's terms allowed sharing tweet IDs but not the full collected data, so Chapter 4 provides IDs only. To rebuild the data, look up ("rehydrate") each ID and collect its retweets. Access to the Twitter/X API has changed a lot since 2016, and deleted tweets can no longer be recovered, so a rebuilt dataset will be smaller than the original.

## Related publications

- P. Mathews, L. Mitchell, G. Nguyen and N. Bean. *The nature and origin of heavy tails in retweet activity.* WWW '17 Companion, pp. 1493–1498, 2017.
- P. Mathews, C. Gray, L. Mitchell, G. Nguyen and N. Bean. *SMERC: Social media event response clustering using textual and temporal information.* IEEE International Conference on Big Data, pp. 3695–3700, 2018.

## Acknowledgements

Supervised by Prof. Nigel Bean, Dr Lewis Mitchell and Dr Giang Nguyen. Funded by the Data to Decisions CRC, the ARC Centre of Excellence for Mathematical and Statistical Frontiers (ACEMS), and an Australian Government Research Training Program Scholarship.
