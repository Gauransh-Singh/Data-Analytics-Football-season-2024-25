# ⚽ Football Players Stats 2024/25

> End-to-end data analytics project analyzing **2,694 professional footballers** across the top 5 European leagues using Python and Power BI.

## 🔗 Quick Links
[![Live Dashboard](https://img.shields.io/badge/Live%20Dashboard-View%20Here-00e5a0?style=for-the-badge)](https://gauransh-singh.github.io/Data-analytics-football-season-2024-25/Web/index.html)
[![GitHub Repo](https://img.shields.io/badge/GitHub-Repository-181717?style=for-the-badge&logo=github)](https://github.com/Gauransh-Singh/data-analytics-football-season-2024-25)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Demo%20Video-0077B5?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/posts/gauransh-singh-211586294_dataanalytics-python-powerbi-ugcPost-7449704503110692864-Yim0?utm_source=share&utm_medium=member_desktop&rcm=ACoAAEdGra0BCqfscl-7qFPqTYIKnrC1zsbXh5M)


---

## 📌 Project Overview

This project covers the complete data analytics workflow — from raw data ingestion and cleaning to exploratory analysis and interactive dashboard development. The dataset contains player statistics from the **2024/25 season** across the Premier League, La Liga, Serie A, Bundesliga, and Ligue 1.

| Detail | Value |
|---|---|
| **Data Source** | FBref via Kaggle (hubertsidorowicz) |
| **Raw Dataset** | 2,854 rows · 165 columns |
| **Players Analyzed** | 2,694 (after cleaning) |
| **Leagues Covered** | 5 major European leagues |
| **Tools Used** | Python · Pandas · Plotly · Power BI |

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|---|---|
| **Python** | Data cleaning, feature engineering |
| **Pandas** | Data manipulation and aggregation |
| **Plotly** | Interactive EDA visualizations |
| **Power BI** | Interactive dashboard |
| **Jupyter Notebook** | Analysis documentation |

---

## 📂 Project Structure

```
football-players-stats-2024-25/
│
├── Data/
│   ├── players_data_light-2024_2025.csv   ← raw dataset (light version)
│   ├── player_clean.csv                   ← one row per player (aggregated)
│   └── league_clean.csv                   ← one row per player per club stint
│
├── Notebooks/
│   ├── 01_data_cleaning.ipynb
│   ├── 02_phase1_overview.ipynb
│   ├── 03_phase2_player_rankings.ipynb
│   ├── 04_phase3_league_analysis.ipynb
│   ├── 05_phase4_xg_analysis.ipynb
│   ├── 06_phase5_position_analysis.ipynb
│   └── 07_phase6_correlation.ipynb
│
├── Dashboard/
│   └── Football_2024-25.pbix
│
├── Web/
│   └── index.html            ← standalone web dashboard
│
└── README.md
```

---

## 🧹 Data Cleaning

The raw dataset required significant cleaning before analysis:

**Null Handling**
- GK-specific columns (`GA`, `Saves`, `Save%` etc.) — filled with `0` for outfield players since these stats don't apply to them
- Rate/percentage columns (`G/Sh`, `Pass_Completion_Pct`, `Tkl%` etc.) — kept as `NaN` since a zero denominator means the stat is undefined, not missing. Pandas skips `NaN` automatically in mean calculations which gives accurate averages
- 8 players with missing `Age` data — dropped (all had under 700 minutes, fringe squad players)
- 7 players with missing `Nation` — filled with `'Unknown'`

**Column Renaming**
- All 165 columns renamed from cryptic FBref abbreviations to readable snake_case names (e.g. `Gls` → `goals`, `#OPA` → `sweeper_actions`)
- Full rename dictionary available in `notebooks/01_data_cleaning.ipynb`

**Multi-Club Players**
- 152 players transferred mid-season and appear as multiple rows (one per club)
- Handled by creating two separate dataframes:
  - `player_df` — one row per player with stats aggregated across clubs (counting stats summed, rate stats weighted by minutes)
  - `league_df` — one row per player per club stint for league-level analysis

**Feature Engineering**
- `primary_position` — extracted first position from combo values like `MF,FW`
- `sufficient_minutes` — boolean flag, `True` if minutes ≥ 900 (10 full games)
- Per-90 columns — `goals_per90`, `assists_per90`, `xg_per90` etc.

---

## ⚠️ Important: Filters Applied in Analysis

All player rankings and league comparisons use a **900-minute minimum filter** (`sufficient_minutes = True`). This means:

- **1,576 players** meet the threshold and are included in rankings
- **1,118 players** are excluded — mostly squad rotation players, injury returnees, and late-season signings
- This filter prevents inflated per-90 stats from players who played very few minutes

**Why this matters for the numbers:**
> Average tackles per player in the Premier League appears as **36.85** rather than ~23 because the 900-minute filter removes low-minute players (substitutes, fringe squad players) who have very few tackles. The filtered average reflects only regular starters — a more meaningful comparison.

**League analysis** uses `league_df` (not `player_df`) so players who transferred between leagues contribute stats to each league separately.

---

## 📊 EDA — 6 Phases, 36 Charts

| Phase | Focus | Charts |
|---|---|---|
| Phase 1 | Data Overview | Position/league distribution, age, minutes |
| Phase 2 | Player Rankings | Top 10 by position — goals, assists, xG, defensive stats |
| Phase 3 | League Comparisons | Goals, passing, defensive intensity, age by league |
| Phase 4 | xG Analysis | Overperformers, underperformers, penalty dependency |
| Phase 5 | Position Analysis | Radar charts, progressive actions, aerial duels |
| Phase 6 | Correlation | Heatmaps, goals vs shots, key passes vs assists |

All charts built with **Plotly** for interactivity — hover to see player name, club, league, and stat values.

---

## 📈 Power BI Dashboard — 7 Pages

| Page | Content |
|---|---|
| Page 1 | Overview — KPI cards, player/league/position distribution |
| Page 2 | Forward Rankings — Goals, Assists, xG, Goals vs xG |
| Page 3 | Midfielder Rankings — Key Passes, Progressive Passes, SCA, Pass Completion % |
| Page 4 | Defender Rankings — Tackles Won, Interceptions, Clearances, Aerial Duel Win % |
| Page 5 | Goalkeeper Rankings — Save %, Clean Sheets, PSxG Difference |
| Page 6 | League Analysis — Goals, passing, defensive intensity, age, xG by league |
| Page 7 | xG Deep Dive — Scatter plot, over/underperformers, penalty vs non-penalty xG |

**Dashboard features:**
- League slicer on every page — filter to any of the 5 leagues
- Minutes played filter — set to 900 minimum by default
- Top N filter on all ranking charts — shows exactly top 10 per metric
- Conditional formatting on all tables — green gradient highlights top performers
- Color coded by league throughout — consistent colors across all pages

---

## 🔍 Key Findings

- **Mohamed Salah** led all players with **29 goals and 18 assists** — the only player to top both categories across all 5 leagues
- **Premier League** had the highest defensive intensity — **36.85 average tackles** per player among regular starters
- **Ligue 1** had the highest average pass completion (**80.30%**) and the youngest average player age (**25.38 years**)
- **Bundesliga** had the lowest pass completion (**77.47%**) — a statistical signature of their high-press, direct play style
- **Patrik Schick** was the biggest xG overperformer — scored **8.3 goals more** than his expected goals model predicted
- **Premier League** scored the most total goals (**976**) among players with sufficient minutes — 87 more than second-placed La Liga

---

## 🖥️ Dashboard Preview

### Page 1 — Overview
<img width="1438" height="805" alt="Overview" src="https://github.com/user-attachments/assets/de5fc424-dc9a-4297-ade8-0297fad71075" />

### Page 2 — Forward Rankings
<img width="1437" height="806" alt="Forwards" src="https://github.com/user-attachments/assets/2ab2243a-7737-4427-b589-ed628fe7e8f1" />

### Page 3 — Midfielder Rankings
<img width="1437" height="808" alt="Midfielders" src="https://github.com/user-attachments/assets/0307b31d-0201-4441-88d8-a00cdb323f11" />

### Page 4 — Defender Rankings
<img width="1437" height="807" alt="Defenders" src="https://github.com/user-attachments/assets/e9087a03-1ce8-4c58-bbed-a7e921af7cfe" />

### Page 5 — Goalkeeper Rankings
<img width="1437" height="806" alt="Goalkeepers" src="https://github.com/user-attachments/assets/521c5b57-cfca-4341-afb4-66cfbdc5250f" />

### Page 6 — League Analysis
<img width="1437" height="807" alt="League Analysis" src="https://github.com/user-attachments/assets/72299743-8d4f-463c-b73c-e79d2a5e4c65" />

### Page 7 — xG Deep Dive
<img width="1437" height="806" alt="xG Analysis" src="https://github.com/user-attachments/assets/a584135a-08ad-46a8-bb0f-9e185a696a35" />

---

## 🚀 How to Run

```bash
# 1. Clone the repository
git clone https://github.com/Gauransh-Singh/football-players-stats-2024-25

# 2. Install dependencies
pip install pandas plotly jupyter

# 3. Run notebooks in order
jupyter notebook notebooks/01_data_cleaning.ipynb

# 4. Open dashboard
# Download football_dashboard.pbix and open in Power BI Desktop (free)
```

---

## 📦 Dataset

- **Source:** [Kaggle — Football Players Stats 2024/25](https://www.kaggle.com/datasets/hubertsidorowicz/football-players-stats-2024-2025/data)
- **Original rows:** 2,854 · **After cleaning:** 2,694 players
- **Columns:** 165 (light version) covering standard stats, shooting, passing, defense, possession, GCA/SCA, goalkeeping

---

## 👤 Author

**Gauransh Singh**
BCA Student · AI/ML Specialization · Greater Noida, India

[![Portfolio](https://img.shields.io/badge/Portfolio-gauransh--singh.github.io-00e5a0?style=flat)](https://gauransh-singh.github.io)
[![GitHub](https://img.shields.io/badge/GitHub-Gauransh--Singh-181717?style=flat&logo=github)](https://github.com/Gauransh-Singh)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0077B5?style=flat&logo=linkedin)](www.linkedin.com/in/gauransh-singh-211586294)

---

*Data sourced from FBref via Kaggle. All analysis for educational and portfolio purposes.*
