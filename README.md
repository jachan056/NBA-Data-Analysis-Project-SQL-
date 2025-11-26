## Description
This project is about writing some SQL queries on some data from the 2024-25 NBA season. It utilizes data from an nba_api combined with pandas code on pycharm to filter and only include the 24-25 season's data before exporting that data as a csv file. The csv file is then connected to Beekeeper Studio as an SQLite database so that SQL queries can be done on the dataset. 

## Prerequisites:


## Step-By-Step Guide
### Clone Repository
1. git clone https://github.com/jachan056/NBA-Data-Analysis-Project-SQL-.git on your terminal.
   
### Set-Up Sqlite
1.  Go to the official sqlite website at https://sqlite.org/download.html to download sqlite for windows. Make sure to pick the zip file with "sqlite-tools-aaa-bbb-ccccccc.zip" that matches your operating system in the source code section.
2.  Set-up your sqlite by unzipping the folder and going through the installation process by running the sqlite3 file.

### Running Sqlite on Terminal
1. Open up terminal and type sqlite3 to activate your sqlite environment on your terminal.
2. Type in .mode csv
3. Type in .import NBA-Data-Analysis-Project-SQL-/NBA_Data.csv NBA_Table and press enter. This imports the all the data and and names the table NBA_Table so that you can do your queries later.
4. Then type in .mode column before pressing enter and type .header on before pressing enter. This will display the data in a more table-like structure instead of just comma seperated values.
5. To verify if the data was imported correctly, you can do a simple query statement like "SELECT * FROM NBA_Table LIMIT 15;" which displays the first 15 rows of the table.
6. If the table shows up, then you are ready to do the other queries.

### Performing the Queries
1. 


## Data Source and Acknowledgements
This project uses the nba_api (https://github.com/swar/nba_api/tree/master) library created by Swar Patel to retrieve NBA data utilized in this project. The nba_api is licensed under the MIT License. 
