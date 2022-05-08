```sql

CREATE DATABASE case_study;

USE case_study;

# Table created without PK to increase load speeds and will be implemented afterwards
CREATE TABLE trip_data (
  ride_id varchar(255) NOT NULL,
  rideable_type varchar(20),
  started_at datetime,
  ended_at datetime,
  ride_length time,
  day_of_week varchar(20),
  start_station_name varchar(100),
  end_station_name varchar(100),
  member_casual varchar(20)
    );

LOAD DATA INFILE '/ProgramData/MySQL/MySQL Server 8.0/Uploads/202204_tripdata.csv'
REPLACE INTO TABLE trip_data
FIELDS TERMINATED BY ','
LINES TERMINATED BY '\n'
IGNORE 1 ROWS; # Ignore first row for headers

LOAD DATA INFILE '/ProgramData/MySQL/MySQL Server 8.0/Uploads/202203_tripdata.csv'
INTO TABLE trip_data
FIELDS TERMINATED BY ','
LINES TERMINATED BY '\n'
IGNORE 1 ROWS;

LOAD DATA INFILE '/ProgramData/MySQL/MySQL Server 8.0/Uploads/202202_tripdata.csv'
INTO TABLE trip_data
FIELDS TERMINATED BY ','
LINES TERMINATED BY '\n'
IGNORE 1 ROWS;

LOAD DATA INFILE '/ProgramData/MySQL/MySQL Server 8.0/Uploads/202201_tripdata.csv'
INTO TABLE trip_data
FIELDS TERMINATED BY ','
LINES TERMINATED BY '\n'
IGNORE 1 ROWS;

LOAD DATA INFILE '/ProgramData/MySQL/MySQL Server 8.0/Uploads/202112_tripdata.csv'
INTO TABLE trip_data
FIELDS TERMINATED BY ','
LINES TERMINATED BY '\n'
IGNORE 1 ROWS;

LOAD DATA INFILE '/ProgramData/MySQL/MySQL Server 8.0/Uploads/202111_tripdata.csv'
INTO TABLE trip_data
FIELDS TERMINATED BY ','
LINES TERMINATED BY '\n'
IGNORE 1 ROWS;

LOAD DATA INFILE '/ProgramData/MySQL/MySQL Server 8.0/Uploads/202110_tripdata.csv'
INTO TABLE trip_data
FIELDS TERMINATED BY ','
LINES TERMINATED BY '\n'
IGNORE 1 ROWS;

LOAD DATA INFILE '/ProgramData/MySQL/MySQL Server 8.0/Uploads/202109_tripdata.csv'
INTO TABLE trip_data
FIELDS TERMINATED BY ','
LINES TERMINATED BY '\n'
IGNORE 1 ROWS;

LOAD DATA INFILE '/ProgramData/MySQL/MySQL Server 8.0/Uploads/202108_tripdata.csv'
INTO TABLE trip_data
FIELDS TERMINATED BY ','
LINES TERMINATED BY '\n'
IGNORE 1 ROWS;

LOAD DATA INFILE '/ProgramData/MySQL/MySQL Server 8.0/Uploads/202107_tripdata.csv'
INTO TABLE trip_data
FIELDS TERMINATED BY ','
LINES TERMINATED BY '\n'
IGNORE 1 ROWS;

LOAD DATA INFILE '/ProgramData/MySQL/MySQL Server 8.0/Uploads/202106_tripdata.csv'
INTO TABLE trip_data
FIELDS TERMINATED BY ','
LINES TERMINATED BY '\n'
IGNORE 1 ROWS;

LOAD DATA INFILE '/ProgramData/MySQL/MySQL Server 8.0/Uploads/202105_tripdata.csv'
INTO TABLE trip_data
FIELDS TERMINATED BY ','
LINES TERMINATED BY '\n'
IGNORE 1 ROWS;




```
