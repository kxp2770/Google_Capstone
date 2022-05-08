```sql
CREATE DATABASE case_study;

USE case_study;

CREATE TABLE trip_data (
  ride_id varchar(255) NOT NULL,
  rideable_type varchar(20),
  start_at datetime,
  end_at datetime,
  ride_length varchar(100),
  day_of_week varchar(20),
  ride_month varchar(20),
  start_station_name varchar(100),
  end_station_name varchar(100),
  member_casual varchar(20)
  );


LOAD DATA INFILE '/ProgramData/MySQL/MySQL Server 8.0/Uploads/cleaned_2021.05.csv'
INTO TABLE trip_data
FIELDS TERMINATED BY ','
LINES TERMINATED BY '\n'
IGNORE 1 ROWS; # Ignore 1 row for headers

LOAD DATA INFILE '/ProgramData/MySQL/MySQL Server 8.0/Uploads/cleaned_202105.csv'
INTO TABLE trip_data
FIELDS TERMINATED BY ','
LINES TERMINATED BY '\n'
IGNORE 1 ROWS;

LOAD DATA INFILE '/ProgramData/MySQL/MySQL Server 8.0/Uploads/cleaned_202106.csv'
INTO TABLE trip_data
FIELDS TERMINATED BY ','
LINES TERMINATED BY '\n'
IGNORE 1 ROWS;

LOAD DATA INFILE '/ProgramData/MySQL/MySQL Server 8.0/Uploads/cleaned_202107.csv'
INTO TABLE trip_data
FIELDS TERMINATED BY ','
LINES TERMINATED BY '\n'
IGNORE 1 ROWS;

LOAD DATA INFILE '/ProgramData/MySQL/MySQL Server 8.0/Uploads/cleaned_202108.csv'
INTO TABLE trip_data
FIELDS TERMINATED BY ','
LINES TERMINATED BY '\n'
IGNORE 1 ROWS;

LOAD DATA INFILE '/ProgramData/MySQL/MySQL Server 8.0/Uploads/cleaned_202109.csv'
INTO TABLE trip_data
FIELDS TERMINATED BY ','
LINES TERMINATED BY '\n'
IGNORE 1 ROWS;

LOAD DATA INFILE '/ProgramData/MySQL/MySQL Server 8.0/Uploads/cleaned_202110.csv'
INTO TABLE trip_data
FIELDS TERMINATED BY ','
LINES TERMINATED BY '\n'
IGNORE 1 ROWS;

LOAD DATA INFILE '/ProgramData/MySQL/MySQL Server 8.0/Uploads/cleaned_202111.csv'
INTO TABLE trip_data
FIELDS TERMINATED BY ','
LINES TERMINATED BY '\n'
IGNORE 1 ROWS;

LOAD DATA INFILE '/ProgramData/MySQL/MySQL Server 8.0/Uploads/cleaned_202112.csv'
INTO TABLE trip_data
FIELDS TERMINATED BY ','
LINES TERMINATED BY '\n'
IGNORE 1 ROWS;




```
