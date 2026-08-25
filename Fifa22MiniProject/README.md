# FIFA 22 - Mohamed Salah Replacement Analysis

## Overview

This project uses **FIFA 22 player data** to explore a practical football recruitment question:

> **If Liverpool needed to replace Mohamed Salah, which young and affordable winger could be a potential long-term replacement?**

I built this project primarily as a way to improve my **Python and data-analysis skills** while working with a football dataset that could support a realistic decision-making problem.

The analysis starts with the FIFA 22 player database available on *kaggle.com*, creates several custom performance metrics, and then uses those metrics together with **age, position, and wage** to produce a shortlist of potential Salah replacements.

The project is intentionally a relatively small analysis rather than a complete scouting model. Its main purpose was to practice turning a raw dataset into a structured analytical process.

---

## What I Practiced

This project gave me practical experience with:

* Loading and exploring CSV data with **Pandas**
* Inspecting DataFrame structure and descriptive statistics
* Filtering rows using multiple conditions
* Selecting and transforming columns
* Creating new calculated metrics
* Applying weighted averages
* Working with missing values
* Using string-based filtering for player positions
* Sorting and narrowing down a dataset
* Creating visualizations with **Matplotlib**
* Translating a real-world question into data-based selection criteria

---

## Technologies

* **Python**
* **Pandas**
* **Matplotlib**
* **Jupyter Notebook**

---

## Dataset

The project uses a FIFA 22 player dataset containing detailed player attributes.

The dataset contains:

* **19,239 players**
* **110 columns**

The available attributes cover areas such as:

* Overall rating
* Potential
* Age
* Height and weight
* Position
* Club
* Wage
* Market value
* Pace
* Shooting
* Passing
* Dribbling
* Defending
* Physical attributes
* Technical attributes
* Mental attributes
* Position-specific ratings

The dataset was loaded into a Pandas DataFrame and explored before building the analysis.  

---

## Analysis

### 1. Defining a Player Profile

The first step was to create a simple measure based on three attributes that were considered particularly relevant to Salah's style:

* Pace
* Shooting
* Dribbling

The initial weighted score was defined as:

```text
40% Pace
35% Shooting
25% Dribbling
```

This produced a first custom metric for comparing players rather than relying solely on FIFA's overall rating.  

---

### 2. Creating Custom Performance Metrics

The analysis was then expanded by combining multiple underlying attributes into more specific metrics.

#### Dribbling

The custom dribbling metric combines:

* FIFA Dribbling
* Ball Control

with the following weights:

```text
35% Dribbling
65% Ball Control
```



#### Shooting

The shooting metric combines:

* Shooting
* Composure
* Finishing

using:

```text
25% Shooting
35% Composure
40% Finishing
```



#### Pace

The pace metric combines:

* Agility
* Sprint Speed
* Acceleration

using:

```text
25% Agility
40% Sprint Speed
35% Acceleration
```



These custom metrics were intended to provide a more targeted description of a player's characteristics than using individual FIFA attributes in isolation.

---

## 3. Creating a Salah-Specific Overall Score

The three custom metrics were combined into a final score designed specifically around the profile being sought.

The weighting was:

```text
45% Shooting
35% Pace
20% Dribbling
```

This produced an `overall_average_salah` score.

For comparison, Mohamed Salah received a score of approximately:

**89.82**

using this methodology. 

The weighting was based on my own reasoning about which attributes were most important to Salah's style of play rather than being derived from a statistical model. 

---

## 4. Building the Replacement Shortlist

The next step was to turn the performance score into a more realistic recruitment filter.

The analysis identified players who met all of the following criteria:

* **Overall Salah score ≥ 80**
* Listed as **LW or RW**
* **Age ≤ 25**
* **Wage ≤ €150,000**

The intention was to identify players who:

1. Have characteristics reasonably similar to the profile being analyzed.
2. Are young enough to represent a potential long-term option.
3. Are not already earning extremely high wages.
4. Play in a position relevant to replacing Salah.

This filtering process reduced the dataset to a shortlist of potential candidates. 

---

## Results

The resulting shortlist included players such as:

| Player          | Age | Wage (€) | Salah Score |
| --------------- | --: | -------: | ----------: |
| Kingsley Coman  |  25 |  120,000 |       84.43 |
| Diogo Jota      |  24 |  120,000 |       84.04 |
| Serge Gnabry    |  25 |  110,000 |       83.76 |
| Federico Chiesa |  23 |   74,000 |       83.73 |
| Phil Foden      |  21 |  125,000 |       83.33 |
| Leon Bailey     |  23 |   75,000 |       83.29 |
| Mikel Oyarzabal |  24 |   57,000 |       83.12 |
| Antony          |  21 |   17,000 |       82.73 |
| Rodrygo         |  20 |  115,000 |       82.51 |
| Vinícius Júnior |  20 |  120,000 |       82.41 |

These values come directly from the filtered FIFA 22 dataset. 

The final shortlist also retained additional information such as position, age, wage, individual custom metrics, and current club position, allowing the candidates to be compared beyond a single score. 

---

## Data Validation & Debugging

An interesting part of the project was identifying an issue in the position filtering.

The initial filtering logic searched for `"RW"` and `"LW"` within the `player_positions` column.

This created an unintended match because **"RB, RWB" contains the characters "RW"**, causing Achraf Hakimi to be incorrectly considered a winger.

I identified the problem and changed the filtering logic to search for the actual position code using a word-boundary regular expression:

```python
r"\bRW\b"
```

and:

```python
r"\bLW\b"
```

This prevents positions such as `RWB` from being incorrectly classified as `RW`.  

This was a useful lesson in how seemingly simple string filtering can introduce errors into a data-analysis pipeline.

---

## Key Takeaways

The project demonstrates how a large player dataset can be transformed into a simple recruitment-oriented analysis.

The main outcomes were:

* A raw dataset of **19,239 players** was explored and filtered.
* Several custom performance metrics were created from underlying FIFA attributes.
* A Salah-specific weighted score was developed.
* Age, position, and wage were incorporated to make the shortlist more realistic.
* The analysis produced a shortlist of **young, relatively affordable wingers**.
* A flaw in the initial position-filtering logic was identified and corrected.

The resulting shortlist should **not** be interpreted as a definitive list of the best Salah replacements. The score is a manually designed metric based on FIFA ratings, and the weights reflect subjective assumptions about Salah's playing profile.

The value of the project is primarily in demonstrating the process of converting a football recruitment question into a reproducible data-analysis workflow.

---

## Limitations

There are several important limitations to this analysis.

### Subjective weighting

The custom metrics and final Salah score use manually chosen weights.

For example:

```text
Shooting → 45%
Pace → 35%
Dribbling → 20%
```

These weights are based on my interpretation of the attributes that matter for Salah's style rather than being statistically optimized. 

### FIFA ratings are not real-world performance data

The analysis uses FIFA 22 player attributes rather than match-event data or real-world performance statistics.

Therefore, the resulting score measures similarity according to the FIFA ratings included in the dataset, not necessarily similarity in actual football performance.

### No transfer-fee analysis

The project uses player wages as an affordability filter, but it does not model:

* Transfer fees
* Contract length
* Release clauses
* Transfer likelihood
* Player willingness to join
* Club willingness to sell

Therefore, a player appearing on the shortlist does not mean that they would realistically have been available to Liverpool.

### Position classification

Players can have multiple listed positions, and FIFA's position labels do not necessarily capture a player's tactical role within a team.

### Historical dataset

The analysis uses FIFA 22 data and therefore represents the player ratings and information available in that dataset rather than current player profiles.

---

## Future Improvements

There are several ways this project could be developed further:

* Use real-world match statistics instead of relying exclusively on FIFA ratings.
* Optimize the metric weights using historical player-performance data.
* Compare the custom score against FIFA's overall rating.
* Include **potential** in the recruitment model.
* Incorporate market value and transfer fees.
* Add contract information.
* Introduce a more detailed positional model.
* Separate left-wingers and right-wingers.
* Use statistical similarity methods rather than manually selected weights.
* Build a ranking system that balances performance, age, wage, and potential.
* Create a more advanced visualization of the final shortlist.

---

## Project Structure

```text
FIFA-22-Mini-Project/
│
├── Fifa22MiniProject.ipynb
├── README.md
```

---

## Why I Built This Project

I built this project as a practical way to improve my Python and data-analysis skills.

Rather than working only with isolated programming exercises, I wanted to take a real-world question — **"Who could potentially replace Mohamed Salah?"** — and turn it into a structured data problem.

The project allowed me to practice the full process of:

**Question → Data → Exploration → Feature Engineering → Filtering → Analysis → Visualization → Result**

The football recruitment scenario provided the context, while the main goal was to develop my ability to work with data using Python and Pandas.
