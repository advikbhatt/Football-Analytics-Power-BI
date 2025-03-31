# ⚽ Premier League Goals Analysis — Power BI Project

This project is a data analysis and visualization solution built using **Power BI Desktop**, focused on **Premier League match statistics**, with an emphasis on calculating **total goals per team** across both home and away matches using custom **DAX measures** and relational modeling techniques.

---

## 📂 Dataset Overview

- Premier League match data containing:
  - `Date`
  - `HomeTeam`, `AwayTeam`
  - `Full Time Home Team Goals`, `Full Time Away Team Goals`
  - `Match Result`, and other match-level metrics

---

## 🧠 Key Features

### 🔁 Relationship Modeling
- Utilized **bi-directional relationships** and `USERELATIONSHIP()` function to analyze goals scored by a team regardless of whether they played home or away.
- Created two active relationships between:
  - `HomeTeam` and `Teams` table
  - `AwayTeam` and `Teams` table
  (Activated contextually via DAX for accurate measure evaluation.)

### 🧮 Custom Measures Using DAX
#### `M TotalGoals`
Calculates total goals scored by a team dynamically using `USERELATIONSHIP`:
```DAX
M TotalGoals = 
VAR TotalHomeGoals = 
    CALCULATE(
        SUM('dataset'[Full Time Home Team Goals]),
        USERELATIONSHIP('dataset'[HomeTeam], Teams[Teams])
    )
VAR TotalAwayGoals =
    CALCULATE(
        SUM('dataset'[Full Time Away Team Goals]),
        USERELATIONSHIP('dataset'[AwayTeam], Teams[Teams])
    )
RETURN
    IF(
        ISFILTERED(Teams[Teams]),
        TotalHomeGoals + TotalAwayGoals,
        SUM('dataset'[Full Time Home Team Goals]) + SUM('dataset'[Full Time Away Team Goals])
    )
