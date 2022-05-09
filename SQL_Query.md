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

ALTER TABLE trip_data ADD PRIMARY KEY(ride_id);

/*
CREATE TABLE IF NOT EXISTS all_ride_info_per_day
	SELECT SEC_TO_TIME(SUM(TIME_TO_SEC(ride_length))) as total_ride_length
	FROM trip_data
	WHERE day_of_week = 'Monday';
# Time in hours is capped at 838:59:59
*/
    
# member_casual column was not cleaned of carriage returns
UPDATE trip_data SET member_casual = REPLACE(member_casual, '\r', '')
WHERE member_casual LIKE '%\r';

CREATE TABLE all_ride_info_per_day (
	days_of_week varchar(30) NOT NULL,
    member_count decimal(31,0) NULL,
    casual_count decimal(31,0)  NULL,
    total_ride_length decimal(31,0) NULL,
    total_rides decimal(31,0) NULL,
    PRIMARY KEY (days_of_week)
    );
    
INSERT INTO all_ride_info_per_day
VALUES(
	'Monday',
    (SELECT COUNT(member_casual) FROM trip_data WHERE day_of_week = 'Monday' AND member_casual = 'member'), # Calculate total number of rides of members on Monday throughout year
    (SELECT COUNT(member_casual) FROM trip_data WHERE day_of_week = 'Monday' AND member_casual = 'casual'), # Calculate total number of rides of casuals on Monday throughout year
    (SELECT SUM(TIME_TO_SEC(ride_length)) FROM trip_data WHERE day_of_week = 'Monday'), # Calculate total time of rides on Monday throughout year in seconds
    (SELECT COUNT(ride_id) FROM trip_data WHERE day_of_week = 'Monday') # Calculate total rides on Monday throughout year
    );
    
INSERT INTO all_ride_info_per_day
VALUES(
	'Tuesday',
    (SELECT COUNT(member_casual) FROM trip_data WHERE day_of_week = 'Tuesday' AND member_casual = 'member'),
    (SELECT COUNT(member_casual) FROM trip_data WHERE day_of_week = 'Tuesday' AND member_casual = 'casual'),
    (SELECT SUM(TIME_TO_SEC(ride_length)) FROM trip_data WHERE day_of_week = 'Tuesday'),
    (SELECT COUNT(ride_id) FROM trip_data WHERE day_of_week = 'Tuesday')
    );

INSERT INTO all_ride_info_per_day
VALUES(
	'Wednesday',
    (SELECT COUNT(member_casual) FROM trip_data WHERE day_of_week = 'Wednesday' AND member_casual = 'member'),
    (SELECT COUNT(member_casual) FROM trip_data WHERE day_of_week = 'Wednesday' AND member_casual = 'casual'),
    (SELECT SUM(TIME_TO_SEC(ride_length)) FROM trip_data WHERE day_of_week = 'Wednesday'),
    (SELECT COUNT(ride_id) FROM trip_data WHERE day_of_week = 'Wednesday')
    );
    
INSERT INTO all_ride_info_per_day
VALUES(
	'Thursday',
    (SELECT COUNT(member_casual) FROM trip_data WHERE day_of_week = 'Thursday' AND member_casual = 'member'),
    (SELECT COUNT(member_casual) FROM trip_data WHERE day_of_week = 'Thursday' AND member_casual = 'casual'),
    (SELECT SUM(TIME_TO_SEC(ride_length)) FROM trip_data WHERE day_of_week = 'Thursday'),
    (SELECT COUNT(ride_id) FROM trip_data WHERE day_of_week = 'Thursday')
    );
    
INSERT INTO all_ride_info_per_day
VALUES(
	'Friday',
    (SELECT COUNT(member_casual) FROM trip_data WHERE day_of_week = 'Friday' AND member_casual = 'member'),
    (SELECT COUNT(member_casual) FROM trip_data WHERE day_of_week = 'Friday' AND member_casual = 'casual'),
    (SELECT SUM(TIME_TO_SEC(ride_length)) FROM trip_data WHERE day_of_week = 'Friday'),
    (SELECT COUNT(ride_id) FROM trip_data WHERE day_of_week = 'Friday')
    );
    
INSERT INTO all_ride_info_per_day
VALUES(
	'Saturday',
    (SELECT COUNT(member_casual) FROM trip_data WHERE day_of_week = 'Saturday' AND member_casual = 'member'),
    (SELECT COUNT(member_casual) FROM trip_data WHERE day_of_week = 'Saturday' AND member_casual = 'casual'),
    (SELECT SUM(TIME_TO_SEC(ride_length)) FROM trip_data WHERE day_of_week = 'Saturday'),
    (SELECT COUNT(ride_id) FROM trip_data WHERE day_of_week = 'Saturday')
    );
    
INSERT INTO all_ride_info_per_day
VALUES(
	'Sunday',
    (SELECT COUNT(member_casual) FROM trip_data WHERE day_of_week = 'Sunday' AND member_casual = 'member'),
    (SELECT COUNT(member_casual) FROM trip_data WHERE day_of_week = 'Sunday' AND member_casual = 'casual'),
    (SELECT SUM(TIME_TO_SEC(ride_length)) FROM trip_data WHERE day_of_week = 'Sunday'),
    (SELECT COUNT(ride_id) FROM trip_data WHERE day_of_week = 'Sunday')
    );
    
CREATE TABLE all_ride_info_per_month (
	month_of_year varchar(30) NOT NULL,
    member_count decimal(31,0) NULL,
    casual_count decimal(31,0)  NULL,
    total_ride_length decimal(31,0) NULL,
    total_rides decimal(31,0) NULL,
    PRIMARY KEY (month_of_year)
    );
    
INSERT INTO all_ride_info_per_month
VALUES(
	'January',
    (SELECT COUNT(member_casual) FROM trip_data WHERE started_at LIKE '%-01-%' AND member_casual = 'member'), # Calculate total members for each month
    (SELECT COUNT(member_casual) FROM trip_data WHERE started_at LIKE '%-01-%' AND member_casual = 'casual'), # Calculate total casuals for each month
    (SELECT SUM(TIME_TO_SEC(ride_length)) FROM trip_data WHERE started_at LIKE '%-01-%'), # Calculate total ride length for each month in seconds
    (SELECT COUNT(ride_id) FROM trip_data WHERE started_at LIKE '%-01-%') # Calculate total rides for each month
    );
    
INSERT INTO all_ride_info_per_month
VALUES(
	'February',
    (SELECT COUNT(member_casual) FROM trip_data WHERE started_at LIKE '%-02-%' AND member_casual = 'member'),
    (SELECT COUNT(member_casual) FROM trip_data WHERE started_at LIKE '%-02-%' AND member_casual = 'casual'),
    (SELECT SUM(TIME_TO_SEC(ride_length)) FROM trip_data WHERE started_at LIKE '%-02-%'),
    (SELECT COUNT(ride_id) FROM trip_data WHERE started_at LIKE '%-02-%')
    );
    
INSERT INTO all_ride_info_per_month
VALUES(
	'March',
    (SELECT COUNT(member_casual) FROM trip_data WHERE started_at LIKE '%-03-%' AND member_casual = 'member'),
    (SELECT COUNT(member_casual) FROM trip_data WHERE started_at LIKE '%-03-%' AND member_casual = 'casual'),
    (SELECT SUM(TIME_TO_SEC(ride_length)) FROM trip_data WHERE started_at LIKE '%-03-%'),
    (SELECT COUNT(ride_id) FROM trip_data WHERE started_at LIKE '%-03-%')
    );
    
INSERT INTO all_ride_info_per_month
VALUES(
	'April',
    (SELECT COUNT(member_casual) FROM trip_data WHERE started_at LIKE '%-04-%' AND member_casual = 'member'),
    (SELECT COUNT(member_casual) FROM trip_data WHERE started_at LIKE '%-04-%' AND member_casual = 'casual'),
    (SELECT SUM(TIME_TO_SEC(ride_length)) FROM trip_data WHERE started_at LIKE '%-04-%'),
    (SELECT COUNT(ride_id) FROM trip_data WHERE started_at LIKE '%-04-%')
    );
    
INSERT INTO all_ride_info_per_month
VALUES(
	'May',
    (SELECT COUNT(member_casual) FROM trip_data WHERE started_at LIKE '%-05-%' AND member_casual = 'member'),
    (SELECT COUNT(member_casual) FROM trip_data WHERE started_at LIKE '%-05-%' AND member_casual = 'casual'),
    (SELECT SUM(TIME_TO_SEC(ride_length)) FROM trip_data WHERE started_at LIKE '%-05-%'),
    (SELECT COUNT(ride_id) FROM trip_data WHERE started_at LIKE '%-05-%')
    );
    
INSERT INTO all_ride_info_per_month
VALUES(
	'June',
    (SELECT COUNT(member_casual) FROM trip_data WHERE started_at LIKE '%-06-%' AND member_casual = 'member'),
    (SELECT COUNT(member_casual) FROM trip_data WHERE started_at LIKE '%-06-%' AND member_casual = 'casual'),
    (SELECT SUM(TIME_TO_SEC(ride_length)) FROM trip_data WHERE started_at LIKE '%-06-%'),
    (SELECT COUNT(ride_id) FROM trip_data WHERE started_at LIKE '%-06-%')
    );

INSERT INTO all_ride_info_per_month
VALUES(
	'July',
    (SELECT COUNT(member_casual) FROM trip_data WHERE started_at LIKE '%-07-%' AND member_casual = 'member'),
    (SELECT COUNT(member_casual) FROM trip_data WHERE started_at LIKE '%-07-%' AND member_casual = 'casual'),
    (SELECT SUM(TIME_TO_SEC(ride_length)) FROM trip_data WHERE started_at LIKE '%-07-%'),
    (SELECT COUNT(ride_id) FROM trip_data WHERE started_at LIKE '%-07-%')
    );
    
INSERT INTO all_ride_info_per_month
VALUES(
	'August',
    (SELECT COUNT(member_casual) FROM trip_data WHERE started_at LIKE '%-08-%' AND member_casual = 'member'),
    (SELECT COUNT(member_casual) FROM trip_data WHERE started_at LIKE '%-08-%' AND member_casual = 'casual'),
    (SELECT SUM(TIME_TO_SEC(ride_length)) FROM trip_data WHERE started_at LIKE '%-08-%'),
    (SELECT COUNT(ride_id) FROM trip_data WHERE started_at LIKE '%-08-%')
    );
    
INSERT INTO all_ride_info_per_month
VALUES(
	'September',
    (SELECT COUNT(member_casual) FROM trip_data WHERE started_at LIKE '%-09-%' AND member_casual = 'member'),
    (SELECT COUNT(member_casual) FROM trip_data WHERE started_at LIKE '%-09-%' AND member_casual = 'casual'),
    (SELECT SUM(TIME_TO_SEC(ride_length)) FROM trip_data WHERE started_at LIKE '%-09-%'),
    (SELECT COUNT(ride_id) FROM trip_data WHERE started_at LIKE '%-09-%')
    );
    
INSERT INTO all_ride_info_per_month
VALUES(
	'October',
    (SELECT COUNT(member_casual) FROM trip_data WHERE started_at LIKE '%-10-%' AND member_casual = 'member'),
    (SELECT COUNT(member_casual) FROM trip_data WHERE started_at LIKE '%-10-%' AND member_casual = 'casual'),
    (SELECT SUM(TIME_TO_SEC(ride_length)) FROM trip_data WHERE started_at LIKE '%-10-%'),
    (SELECT COUNT(ride_id) FROM trip_data WHERE started_at LIKE '%-10-%')
    );
    
INSERT INTO all_ride_info_per_month
VALUES(
	'November',
    (SELECT COUNT(member_casual) FROM trip_data WHERE started_at LIKE '%-11-%' AND member_casual = 'member'),
    (SELECT COUNT(member_casual) FROM trip_data WHERE started_at LIKE '%-11-%' AND member_casual = 'casual'),
    (SELECT SUM(TIME_TO_SEC(ride_length)) FROM trip_data WHERE started_at LIKE '%-11-%'),
    (SELECT COUNT(ride_id) FROM trip_data WHERE started_at LIKE '%-11-%')
    );
    
INSERT INTO all_ride_info_per_month
VALUES(
	'December',
    (SELECT COUNT(member_casual) FROM trip_data WHERE started_at LIKE '%-12-%' AND member_casual = 'member'),
    (SELECT COUNT(member_casual) FROM trip_data WHERE started_at LIKE '%-12-%' AND member_casual = 'casual'),
    (SELECT SUM(TIME_TO_SEC(ride_length)) FROM trip_data WHERE started_at LIKE '%-12-%'),
    (SELECT COUNT(ride_id) FROM trip_data WHERE started_at LIKE '%-12-%')
    );

```
