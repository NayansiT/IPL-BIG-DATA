# ipl-data-analysis-apache-spark-project

# 🏏 IPL Data Analysis using Apache Spark
🚀 **Project by NayansiT**

## 📘 Overview
This project performs comprehensive data analysis on Indian Premier League (IPL) datasets using **Apache Spark (PySpark)**.  
It covers data cleaning, transformation, aggregation, SQL queries, and visualization — helping derive valuable insights about players, matches, teams, and overall trends.

## ⚙️ Tech Stack
- **Language:** Python (PySpark)  
- **Big Data Framework:** Apache Spark  
- **Visualization:** Matplotlib, Seaborn  
- **Storage:** AWS S3 (CSV data)  
- **IDE:** Databricks / Jupyter / VS Code  

## 📂 Dataset Information
The project uses 5 main datasets stored on S3:

| File Name           | Description                                |
|--------------------|--------------------------------------------|
| Ball_By_Ball.csv    | Delivery-level details for each match      |
| Match.csv           | Match metadata (venue, toss, results, etc.) |
| Player.csv          | Player information and attributes          |
| Player_match.csv    | Player performance per match               |
| Team.csv            | Team information                            |

## 🧱 Data Schema
Schemas are explicitly defined using **StructType** and **StructField** in PySpark for:
- Ball by Ball Data  
- Match Data  
- Player Data  
- Player-Match Data  
- Team Data  

This ensures strict typing and consistent data ingestion.

## 🧮 Key Data Transformations
**Data Cleaning:**  
- Filled missing `batting_hand` and `bowling_skill` values  
- Normalized player names (removed special characters, lowercase conversion)  

**Feature Engineering:**  
- `running_total_runs` (cumulative score in match)  
- `high_impact` flag (balls with >6 runs or wickets)  
- `win_margin_category` (High / Medium / Low)  
- `toss_match_winner` (Did toss-winning team also win the match?)  
- `veteran_status` (based on player age)  
- `years_since_debut` (current year - debut season)  

## 📊 Analytical Queries & Insights
**1. Top Scoring Batsmen per Season**  
```sql
SELECT p.player_name, m.season_year, SUM(b.runs_scored) AS total_runs
FROM ball_by_ball b
JOIN match m ON b.match_id = m.match_id
JOIN player_match pm ON m.match_id = pm.match_id AND b.striker = pm.player_id
JOIN player p ON p.player_id = pm.player_id
GROUP BY p.player_name, m.season_year
ORDER BY m.season_year, total_runs DESC;
