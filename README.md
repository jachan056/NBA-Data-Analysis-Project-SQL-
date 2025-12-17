## Description
This project is about writing some SQL queries on some data from the 2024-25 NBA season. It utilizes data from an nba_api combined with pandas code on pycharm to filter and only include the 24-25 season's data before exporting that data as a csv file. The csv file is then connected to Beekeeper Studio as an SQLite database so that SQL queries can be done on the dataset. 

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
3. Make sure the downloaded folder is in your Users/your_username folder before proceeding. Type in ".import C:Users\your_username\NBA-Data-Analysis-Project-SQL-\NBAA.csv NBAA" and press enter. This imports the all the data into a table named NBAA for you to perform queries on later.
4. Then type in ".mode column" before pressing enter and type ".header on" before pressing enter. This will display the data in a more table-like structure instead of just comma seperated values.
5. To verify if the data was imported correctly, you can do a simple query statement like "SELECT * FROM NBA_Table LIMIT 15;" which displays the first 15 rows of the table.
6. If the table shows up, then you are ready to do the other queries.

### Performing the Queries
1. We are going to have to create a new table, and import Create table better_nba (full_name, id, team_id, team_abbreviation, player_age, gp, gs, min, fgm, fga, fg_pct, fg3m, fg3a, fg3_pct, ftm, fta, ft_pct, orep, dreb, reb, ast, stl, blk, tov, pf, pts, position, 'height (in)', 'wing span (in)');


## Data Source and Acknowledgements
This project uses the nba_api (https://github.com/swar/nba_api/tree/master) library created by Swar Patel to retrieve NBA data utilized in this project. The nba_api is licensed under the MIT License. 
