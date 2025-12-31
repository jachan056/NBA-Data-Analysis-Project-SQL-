## Description
This SQL-based NBA data analysis project investigates how veteran players (aged 30 and older with at least 60 games played) performed during the 2024-25 season relative to the league's overall averages. The project creates two core analytical tables: the first calculates league-wide per-minute statistics across key performance categories: including points, rebounds, assists, steals, blocks, and turnovers to establish a baseline of average performance. The second table isolates the per-minute efficiency metrics for qualifying veterans, enabling a direct comparison between this experienced cohort and the league standard. By structuring the data this way, the analysis seeks to reveal whether veteran players maintain, exceed, or fall below league-average efficiency, providing data-driven insights into the impact of age and experience on on-court productivity in professional basketball.

## Step-By-Step Guide
### Prereqs
1. Install git and git bash on https://git-scm.com/install/ based on your operating system.
2. Keep all installation options on the default settings git set-up provides.

### Clone Repository
1. Open up the git-bash terminal that was installed earlier.
2. Type in "git clone https://github.com/jachan056/NBA-Data-Analysis-Project-SQL-.git" on your terminal.
   
### Set-Up Sqlite
1.  Go to the official sqlite website at https://sqlite.org/download.html to download sqlite for windows. Make sure to pick the zip file with "sqlite-tools-aaa-bbb-ccccccc.zip" that matches your operating system in the source code section.
2.  Set-up your sqlite by unzipping the folder and going through the installation process by running the sqlite3 file.
3.  Whenever you run any code on sqlite3, make sure to run the application named "sqlite3".

### Running Sqlite on Terminal
1. Open up the sqlite3 app to run your code on the terminal.
2. Type in .mode csv
3. Make sure the downloaded folder is in your Users/your_username folder before proceeding. Type in ".import C:\Users\your_username\NBA-Data-Analysis-Project-SQL-\NBAA.csv better_nba" and press enter. This imports the all the data into a table named better_nba for you to perform queries on later.
4. Then type in ".mode column" before pressing enter and type ".header on" before pressing enter. This will display the data in a more table-like structure instead of just comma seperated values.
5. To verify if the data was imported correctly, you can do a simple query statement like "SELECT * FROM better_nba LIMIT 15;" which displays the first 15 rows of the table.
6. If the table shows up, then you are ready to do the other queries.

### Performing the Queries
1. We are going to create another table, league_wide_minute_stats, which contains the league wide average stats per minute to give us a metric to compare with some of the older players in the league. 
Use "Create table league_wide_minute_stats ("dummy_name", "league-pts-pm", "league-reb-pm", "league-ast-pm", "league-stl-pm", "league-blk-pm", "league-tov-pm");"

2. Next, the data will be inserted into the table. The calculations are done only on specific player-stats: points, rebounds, assists, steals, blocks, and turn-overs. 
Use "INSERT into league_wide_minute_stats ("dummy_name", "league-pts-pm", "league-reb-pm", "league-ast-pm", "league-stl-pm", "league-blk-pm", "league-tov-pm")
SELECT better_nba.full_name,
  ROUND(CAST(SUM(better_nba.pts) AS DOUBLE) / SUM(better_nba.min), 4) AS 'league-pts-pm', 
  ROUND(CAST(SUM(better_nba.reb) AS DOUBLE) / SUM(better_nba.min), 4) AS 'league-reb-pm', 
  ROUND(CAST(SUM(better_nba.ast) AS DOUBLE) / SUM(better_nba.min), 4) AS 'league-ast-pm',
  ROUND(CAST(SUM(better_nba.stl) AS DOUBLE) / SUM(better_nba.min), 4) AS 'league-stl-pm', 
  ROUND(CAST(SUM(better_nba.blk) AS DOUBLE) / SUM(better_nba.min), 4) AS 'league-blk-pm', 
  ROUND(CAST(SUM(better_nba.tov) AS DOUBLE) / SUM(better_nba.min), 4) AS 'league-tov-pm'
FROM better_nba;"

3. Use "SELECT * FROM league_wide_minute_stats;" to check if the data was properly inserted into the table. The dummy name can be ignored, the league-wide statistics are all we need from this table. The table should look something like this. [League Average Table](Images/league_avg_unc_stats.PNG) If correct, move on to the next step.

4. The next table will have the statistics of all the players we are comparing to the league-average. That is all of the players from the 24-25 season who are above the age 30, with 60 or more games played will be considered a "league veteran". 
Use "Create table league_uncs ("full_name", "pts-pm", "reb-pm", "ast-pm", "stl-pm", "blk-pm", "tov-pm");"

5. Next, the data will be inserted into the table. The calculations, similar to the previous table, will be done on their points, rebounds, assists, steals, blocks, and turnovers per minute. 
Use "INSERT INTO league_uncs (
  "full_name", "pts-pm", "reb-pm", "ast-pm", "stl-pm", "blk-pm", "tov-pm"
)
SELECT 
  DISTINCT better_nba.full_name, ROUND(Cast(better_nba.pts AS DOUBLE) / better_nba.min, 4) as "pts-pm", ROUND(Cast(better_nba.reb AS DOUBLE) / better_nba.min, 4) as "reb-pm", 
      ROUND(Cast(better_nba.ast AS DOUBLE) / better_nba.min, 4) as "ast-pm", ROUND(Cast(better_nba.stl AS DOUBLE) / better_nba.min, 4) as "stl-pm", 
      ROUND(Cast(better_nba.blk AS DOUBLE) / better_nba.min, 4) as "blk-pm", ROUND(Cast(better_nba.tov AS DOUBLE) / better_nba.min, 4) as "tov-pm"
  FROM better_nba WHERE better_nba.player_age >= 30 and better_nba.gp >= 60 and TEAM_ABBREVIATION != 'TOT';"
  
6. Use "SELECT * FROM league_uncs;" to verify data was inserted properly. The dataset should look something like this: [Average Stats Table](Images/league_unc_stats.PNG) If correct, move onto the next step. 

7. Next we use a relative performance score to evaluate NBA players by comparing their per-minute production to league averages across five key statistical categories. Formula: Σ [(Player_Stat_Per Minute / League_Avg_Per Minute) - 1] for points, rebounds, assists, steals, blocks. This formula is applied accross all 5 positions of basketball: point-guard, shooting-guard, small-forward, power-forward, and center. Performance scores determine the quality of each person's play: Above 0 means the player performs above the league average, while being below 0 means the player performs below the league average and 0 being the player performs at league average.

### Point Guard's Evaluation
Code: WITH guard_table AS (SELECT DISTINCT better_nba.full_name, player_age, gp, position, "height (in)", "wing span (in)", "pts-pm", "reb-pm", "ast-pm", "stl-pm", "blk-pm", "tov-pm" FROM better_nba INNER JOIN league_uncs ON better_nba.full_name = league_uncs.full_name WHERE gp >= 60 and (position = 'PG')), evaluate_performance AS (SELECT full_name, player_age, gp, position, ROUND(((a."pts-pm" / b."league-pts-pm") - 1) + ((a."reb-pm" / b."league-reb-pm") - 1) + ((a."ast-pm" / b."league-ast-pm") - 1) + ((a."stl-pm" / b."league-stl-pm") - 1) + ((a."blk-pm" / b."league-blk-pm") - 1), 4) AS performance_score FROM guard_table as a, league_wide_minute_stats as b)
SELECT DISTINCT full_name, player_age, gp, performance_score, RANK() OVER (ORDER BY performance_score DESC) AS veteran_rank FROM evaluate_performance ORDER BY veteran_rank;

Table: [League Average Table](Images/league_avg_unc_stats.PNG)

Analysis: ...
#

### Shooting Guard's Evaluation
Code: WITH guard_table AS (SELECT DISTINCT better_nba.full_name, player_age, gp, position, "height (in)", "wing span (in)", "pts-pm", "reb-pm", "ast-pm", "stl-pm", "blk-pm", "tov-pm" FROM better_nba INNER JOIN league_uncs ON better_nba.full_name = league_uncs.full_name WHERE gp >= 60 and (position = 'SG')), evaluate_performance AS (SELECT full_name, player_age, gp, position, ROUND(((a."pts-pm" / b."league-pts-pm") - 1) + ((a."reb-pm" / b."league-reb-pm") - 1) + ((a."ast-pm" / b."league-ast-pm") - 1) + ((a."stl-pm" / b."league-stl-pm") - 1) + ((a."blk-pm" / b."league-blk-pm") - 1), 4) AS performance_score FROM guard_table as a, league_wide_minute_stats as b)
SELECT DISTINCT full_name, player_age, gp, performance_score, RANK() OVER (ORDER BY performance_score DESC) AS veteran_rank FROM evaluate_performance ORDER BY veteran_rank;

Table: [League Average Table](Images/league_avg_unc_stats.PNG)

Analysis: ...
#

### Small Forward's Evaluation
Code: WITH big_table AS (SELECT DISTINCT better_nba.full_name, player_age, gp, position, "height (in)", "wing span (in)", "pts-pm", "reb-pm", "ast-pm", "stl-pm", "blk-pm", "tov-pm" FROM better_nba INNER JOIN league_uncs ON better_nba.full_name = league_uncs.full_name WHERE gp >= 60 and (position = 'SF')), evaluate_performance AS (SELECT full_name, player_age, gp, position, ROUND(((a."pts-pm" / b."league-pts-pm") - 1) + ((a."reb-pm" / b."league-reb-pm") - 1) + ((a."ast-pm" / b."league-ast-pm") - 1) + ((a."stl-pm" / b."league-stl-pm") - 1) + ((a."blk-pm" / b."league-blk-pm") - 1), 4) AS performance_score FROM big_table as a, league_wide_minute_stats as b)
SELECT DISTINCT full_name, player_age, gp, performance_score, RANK() OVER (ORDER BY performance_score DESC) AS veteran_rank FROM evaluate_performance ORDER BY veteran_rank;

Table: [League Average Table](Images/league_avg_unc_stats.PNG)

Analysis: ...
#

### Power Forward's Evaluation
Code: WITH big_table AS (SELECT DISTINCT better_nba.full_name, player_age, gp, position, "height (in)", "wing span (in)", "pts-pm", "reb-pm", "ast-pm", "stl-pm", "blk-pm", "tov-pm" FROM better_nba INNER JOIN league_uncs ON better_nba.full_name = league_uncs.full_name WHERE gp >= 60 and (position = 'PF')), evaluate_performance AS (SELECT full_name, player_age, gp, position, ROUND(((a."pts-pm" / b."league-pts-pm") - 1) + ((a."reb-pm" / b."league-reb-pm") - 1) + ((a."ast-pm" / b."league-ast-pm") - 1) + ((a."stl-pm" / b."league-stl-pm") - 1) + ((a."blk-pm" / b."league-blk-pm") - 1), 4) AS performance_score FROM big_table as a, league_wide_minute_stats as b)
SELECT DISTINCT full_name, player_age, gp, performance_score, RANK() OVER (ORDER BY performance_score DESC) AS veteran_rank FROM evaluate_performance ORDER BY veteran_rank;

Table: [League Average Table](Images/league_avg_unc_stats.PNG)

Analysis: ...
#

### Center's Evaluation
Code: WITH big_table AS (SELECT DISTINCT better_nba.full_name, player_age, gp, position, "height (in)", "wing span (in)", "pts-pm", "reb-pm", "ast-pm", "stl-pm", "blk-pm", "tov-pm" FROM better_nba INNER JOIN league_uncs ON better_nba.full_name = league_uncs.full_name WHERE gp >= 60 and (position = 'C')), evaluate_performance AS (SELECT full_name, player_age, gp, position, ROUND(((a."pts-pm" / b."league-pts-pm") - 1) + ((a."reb-pm" / b."league-reb-pm") - 1) + ((a."ast-pm" / b."league-ast-pm") - 1) + ((a."stl-pm" / b."league-stl-pm") - 1) + ((a."blk-pm" / b."league-blk-pm") - 1), 4) AS performance_score FROM big_table as a, league_wide_minute_stats as b)
SELECT DISTINCT full_name, player_age, gp, performance_score, RANK() OVER (ORDER BY performance_score DESC) AS veteran_rank FROM evaluate_performance ORDER BY veteran_rank;

Table: [League Average Table](Images/league_avg_unc_stats.PNG)

Analysis: ...

Last Commentary:
s

## Data Source and Acknowledgements
This project uses the nba_api (https://github.com/swar/nba_api/tree/master) library created by Swar Patel to retrieve NBA data utilized in this project. The nba_api is licensed under the MIT License. 
