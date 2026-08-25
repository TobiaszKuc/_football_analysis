# Florian Wirtz - Bayer Leverkusen 2023/24 Analysis

A Python-based football data analysis project investigating Florian Wirtz's influence on Bayer Leverkusen during the 2023/24 Bundesliga season.

## Overview

I built this project primarily to develop my Python and data-analysis skills by working with a real-world dataset in a subject I am interested in: football.

The main question behind the project was:

**How influential was Florian Wirtz for Bayer Leverkusen during the 2023/24 Bundesliga season?**

To investigate this, I used event-level data from StatsBomb and Python to extract, transform, analyze, and visualize Bayer Leverkusen's match data.

The analysis focuses mainly on Wirtz's passing and chance-creation activity, while also comparing his results with other Bayer Leverkusen players.

---

## What I Practiced

This project gave me practical experience with several aspects of Python-based data analysis:

- Retrieving data using a Python package
- Working with large event-level datasets
- Data cleaning and filtering with **Pandas**
- Grouping and aggregating data
- Merging multiple DataFrames
- Handling missing values
- Creating new metrics from raw data
- Normalizing statistics by playing time
- Data visualization with **Matplotlib**
- Investigating unexpected results
- Comparing calculated results with external data
- Documenting assumptions and limitations

---

## Technologies

- **Python**
- **Pandas**
- **Matplotlib**
- **statsbombpy**
- **Jupyter Notebook**

---

## Data

The project uses **StatsBomb Open Data**, accessed through the `statsbombpy` Python package.

The dataset selected in the notebook is:

- **Competition:** 1. Bundesliga
- **Season:** 2023/2024
- **Gender:** Male

The available open-data sample contains **34 matches** and approximately **137,765 event records**.

Because of this limitation, the main player comparisons in this project are made **within Bayer Leverkusen's squad**, rather than across the entire Bundesliga.

---

## Analysis

### 1. Playing Time

StatsBomb's event data does not provide a direct minutes-played column for each player.

I therefore derived playing time from:

- Matches played
- Starting appearances
- Substitutions
- Minute of substitution
- Full matches played

For Florian Wirtz, this resulted in an estimated:

**2,377 minutes played**

The result was also compared with external football-statistics sources as a basic validation step. The notebook notes a small difference between the calculated value and one external source.

I then extended the same approach to the rest of the Bayer Leverkusen squad, allowing event-based statistics to be normalized by playing time.

---

### 2. Passing Analysis

I aggregated passing events for Bayer Leverkusen players and calculated several passing-related statistics, including:

- Total passes
- Completed passes
- Pass completion percentage
- Total pass distance
- Average pass length
- Through balls
- Shot assists
- Assists

For Florian Wirtz, the analysis produced:

| Metric | Wirtz |
|---|---:|
| Passes | 1,841 |
| Pass completion | 83.81% |
| Through balls | 22 |
| Shot assists | 61 |
| Assists | 10 |

These values were calculated directly from the StatsBomb event data.

The purpose of this analysis was not simply to collect statistics, but to practice transforming raw event data into meaningful player-level metrics.

---

### 3. Through Balls per 90

Because players had different amounts of playing time, I also normalized through-ball counts by minutes played.

The analysis shows that **Florian Wirtz recorded the highest number of through balls per 90 minutes among the Bayer Leverkusen players analyzed**.

This provided a better comparison between players than using total through-ball counts alone.

---

### 4. Expected Assists (xA)

I also implemented my own event-based expected assists calculation.

The approach was:

1. Identify passes that directly assisted a shot.
2. Match each pass to the corresponding shot using the shot ID.
3. Retrieve the StatsBomb xG value of the resulting shot.
4. Attribute that xG value to the player who made the pass.
5. Sum the resulting values for each player.

In other words:

**xA = Sum of the xG values of shots directly assisted by a player's passes**

Using this method, Florian Wirtz's calculated xA was:

**7.73**

He ranked third among Bayer Leverkusen players in the resulting xA table, behind Alejandro Grimaldo and Jonas Hofmann.
---

## Investigating the xA Discrepancy

One of the most interesting parts of the project came when the calculated xA did not match figures reported by other football-statistics sources.

Rather than assuming the calculation was incorrect, I investigated possible reasons for the discrepancy.

### Assist classification

The notebook records **10 assists** for Wirtz, while external sources reported 11.

I found that one of the assists involved a shot that deflected off the post and then went in via the goalkeeper. StatsBomb classifies this event as an **"Own Goal For"**, meaning it is separated from the normal shot-event chain and does not contain an xG value linked back to the assisting pass.

As a result, this particular assist cannot be incorporated into the xA calculation using the same event-based method.

### Number of shot-assisting passes

I also checked whether the discrepancy could be explained by the number of passes leading to shots.

The calculation identified **71 shot-assisting passes**, which falls within the range reported by external sources. Therefore, the number of qualifying passes did not appear to explain the discrepancy.

### Different xG models

The remaining difference most likely comes from the underlying xG models.

Different providers can assign different xG values to the same shot. Since xA is calculated from the xG values of shots created by a player's passes, different xG models can consequently produce different xA values.

This explanation was not independently verified further because the required shot-by-shot comparison between providers was not available in the data used for this project.

This was an important lesson from the project: **a calculated statistic is only as meaningful as the methodology and data behind it.**

---

## Key Findings

The analysis suggests that Wirtz was an important creative player for Bayer Leverkusen during the 2023/24 Bundesliga season.

Some of the main findings were:

- **2,377 estimated minutes played**
- **1,841 passes**
- **83.81% pass completion**
- **22 through balls**
- **71 shot assists**
- **10 assists recorded in the StatsBomb event data**
- **7.73 calculated xA**
- Highest through-ball rate per 90 among the Bayer Leverkusen players analyzed
- Third-highest calculated xA within the Bayer Leverkusen squad

These results should be interpreted in the context of the dataset and methodology used.

The project was primarily built as an opportunity to develop Python and data-analysis skills, rather than as a definitive evaluation of Wirtz's overall football performance.

---

## Limitations

There are several important limitations to this analysis:

- The available StatsBomb Open Data does not contain the complete 2023/24 Bundesliga dataset.
- Player comparisons are therefore primarily made within Bayer Leverkusen.
- Minutes played are derived from event data rather than taken from a dedicated minutes-played field.
- The minutes calculation simplifies matches to 90 minutes.
- The xA calculation depends on StatsBomb's event classification.
- xA is model-dependent because it uses underlying xG values.
- Results from different football-data providers should therefore not necessarily be expected to match.

These limitations are part of the reason I included validation and investigation of discrepancies rather than treating the calculated values as unquestionable.