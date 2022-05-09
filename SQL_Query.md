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

/*
CREATE TABLE IF NOT EXISTS ride_length_per_day
	SELECT SEC_TO_TIME(SUM(TIME_TO_SEC(ride_length))) as mon_total_ride_length 
	FROM trip_data
	WHERE day_of_week = 'Monday';
# Time in hours is capped at 838:59:59
*/

CREATE TABLE IF NOT EXISTS ride_length_per_day
	SELECT SUM(TIME_TO_SEC(ride_length)) as mon_total_ride_length
	FROM trip_data
	WHERE day_of_week = 'Monday';
# Calculated total time as seconds and will convert based on data viz needs

ALTER TABLE ride_length_per_day 
	ADD tues_total_ride_length decimal(31,0) NULL
		AFTER mon_total_ride_length,
	ADD wed_total_ride_length decimal(31,0) NULL
		AFTER tues_total_ride_length,
	ADD thu_total_ride_length decimal(31,0) NULL
		AFTER wed_total_ride_length,
	ADD fri_total_ride_length decimal(31,0) NULL
		AFTER thu_total_ride_length,
	ADD sat_total_ride_length decimal(31,0) NULL
		AFTER fri_total_ride_length,
	ADD sun_total_ride_length decimal(31,0) NULL
		AFTER sat_total_ride_length;
# Added new columns for each day of week
 
INSERT INTO ride_length_per_day 
VALUES(
	(SELECT SUM(TIME_TO_SEC(ride_length)) FROM trip_data WHERE day_of_week = 'Monday'),
	(SELECT SUM(TIME_TO_SEC(ride_length)) FROM trip_data WHERE day_of_week = 'Tuesday'),
	(SELECT SUM(TIME_TO_SEC(ride_length)) FROM trip_data WHERE day_of_week = 'Wednesday'),
	(SELECT SUM(TIME_TO_SEC(ride_length)) FROM trip_data WHERE day_of_week = 'Thursday'),
	(SELECT SUM(TIME_TO_SEC(ride_length)) FROM trip_data WHERE day_of_week = 'Friday'),
	(SELECT SUM(TIME_TO_SEC(ride_length)) FROM trip_data WHERE day_of_week = 'Saturday'),
	(SELECT SUM(TIME_TO_SEC(ride_length)) FROM trip_data WHERE day_of_week = 'Sunday')
    );

# Inital creation of table had first row with 0 for other columns, removed any NULLs or 0 values
DELETE FROM ride_length_per_day WHERE thu_total_ride_length IS NULL;
DELETE FROM ride_length_per_day WHERE thu_total_ride_length = 0;

# Creating table with total rides per month with columns before adding values
CREATE TABLE rides_per_month (
	january decimal(31,0) NULL,
	february decimal(31,0) NULL,
	march decimal(31,0) NULL,
	april decimal(31,0) NULL, 
	may decimal(31,0) NULL,
	june decimal(31,0) NULL,
	july decimal(31,0) NULL, 
	august decimal(31,0) NULL,
	september decimal(31,0) NULL,
	october decimal(31,0) NULL,
	novemeber decimal(31,0) NULL,
	december decimal(31,0) NULL
    );

# Calculating total rides per month in the past year
INSERT INTO rides_per_month
VALUES(
	(SELECT COUNT(ride_id) FROM trip_data WHERE started_at LIKE '%-01-%'),
	(SELECT COUNT(ride_id) FROM trip_data WHERE started_at LIKE '%-02-%'),
	(SELECT COUNT(ride_id) FROM trip_data WHERE started_at LIKE '%-03-%'),
	(SELECT COUNT(ride_id) FROM trip_data WHERE started_at LIKE '%-04-%'),
	(SELECT COUNT(ride_id) FROM trip_data WHERE started_at LIKE '%-05-%'),
	(SELECT COUNT(ride_id) FROM trip_data WHERE started_at LIKE '%-06-%'),
	(SELECT COUNT(ride_id) FROM trip_data WHERE started_at LIKE '%-07-%'),
	(SELECT COUNT(ride_id) FROM trip_data WHERE started_at LIKE '%-08-%'),
	(SELECT COUNT(ride_id) FROM trip_data WHERE started_at LIKE '%-09-%'),
	(SELECT COUNT(ride_id) FROM trip_data WHERE started_at LIKE '%-10-%'),
	(SELECT COUNT(ride_id) FROM trip_data WHERE started_at LIKE '%-11-%'),
	(SELECT COUNT(ride_id) FROM trip_data WHERE started_at LIKE '%-12-%')
    );
    
# member_casual column was not cleaned of carriage returns
UPDATE trip_data SET member_casual = REPLACE(member_casual, '\r', '')
WHERE member_casual LIKE '%\r';

CREATE TABLE ride_length_per_month (
	january decimal(31,0) NULL,
	february decimal(31,0) NULL,
	march decimal(31,0) NULL,
	april decimal(31,0) NULL, 
	may decimal(31,0) NULL,
	june decimal(31,0) NULL,
	july decimal(31,0) NULL, 
	august decimal(31,0) NULL,
	september decimal(31,0) NULL,
	october decimal(31,0) NULL,
	novemeber decimal(31,0) NULL,
	december decimal(31,0) NULL
    );

# Calculated total ride duration per month in seconds    
INSERT INTO ride_length_per_month 
VALUES(
	(SELECT SUM(TIME_TO_SEC(ride_length)) FROM trip_data WHERE started_at LIKE '%-01-%'),
	(SELECT SUM(TIME_TO_SEC(ride_length)) FROM trip_data WHERE started_at LIKE '%-02-%'),
	(SELECT SUM(TIME_TO_SEC(ride_length)) FROM trip_data WHERE started_at LIKE '%-03-%'),
	(SELECT SUM(TIME_TO_SEC(ride_length)) FROM trip_data WHERE started_at LIKE '%-04-%'),
	(SELECT SUM(TIME_TO_SEC(ride_length)) FROM trip_data WHERE started_at LIKE '%-05-%'),
	(SELECT SUM(TIME_TO_SEC(ride_length)) FROM trip_data WHERE started_at LIKE '%-06-%'),
	(SELECT SUM(TIME_TO_SEC(ride_length)) FROM trip_data WHERE started_at LIKE '%-07-%'),
	(SELECT SUM(TIME_TO_SEC(ride_length)) FROM trip_data WHERE started_at LIKE '%-08-%'),
	(SELECT SUM(TIME_TO_SEC(ride_length)) FROM trip_data WHERE started_at LIKE '%-09-%'),
	(SELECT SUM(TIME_TO_SEC(ride_length)) FROM trip_data WHERE started_at LIKE '%-10-%'),
	(SELECT SUM(TIME_TO_SEC(ride_length)) FROM trip_data WHERE started_at LIKE '%-11-%'),
	(SELECT SUM(TIME_TO_SEC(ride_length)) FROM trip_data WHERE started_at LIKE '%-12-%')
    );
    
CREATE TABLE rides_per_day (
	monday decimal(31,0) NULL,
	tuesday decimal(31,0) NULL,
	wednesday decimal(31,0) NULL,
	thursday decimal(31,0) NULL, 
	friday decimal(31,0) NULL,
	saturday decimal(31,0) NULL,
	sunday decimal(31,0) NULL
    );
		
INSERT INTO rides_per_day
VALUES(
	(SELECT COUNT(ride_id) FROM trip_data WHERE day_of_week = 'Monday'),
	(SELECT COUNT(ride_id) FROM trip_data WHERE day_of_week = 'Tuesday'),
	(SELECT COUNT(ride_id) FROM trip_data WHERE day_of_week = 'Wednesday'),
	(SELECT COUNT(ride_id) FROM trip_data WHERE day_of_week = 'Thursday'),
	(SELECT COUNT(ride_id) FROM trip_data WHERE day_of_week = 'Friday'),
	(SELECT COUNT(ride_id) FROM trip_data WHERE day_of_week = 'Saturday'),
	(SELECT COUNT(ride_id) FROM trip_data WHERE day_of_week = 'Sunday')
    );
        
        
CREATE TABLE ride_info_per_day (
	mon_member decimal(31,0) NULL,
	tues_member decimal(31,0) NULL,
	wed_member decimal(31,0) NULL,
	thu_member decimal(31,0) NULL,
	fri_member decimal(31,0) NULL,
	sat_member decimal(31,0) NULL,
	sun_member decimal(31,0) NULL,
    
	mon_casual decimal(31,0) NULL,
	tues_casual decimal(31,0) NULL,
	wed_casual decimal(31,0) NULL,
	thu_casual decimal(31,0) NULL,
	fri_casual decimal(31,0) NULL,
	sat_casual decimal(31,0) NULL,
	sun_casual decimal(31,0) NULL
    );
    
# Calculate total number of rides depending on if ride is from members or casuals based on days of the week
 INSERT INTO ride_info_per_day
 VALUES(
	(SELECT COUNT(member_casual) FROM trip_data WHERE day_of_week = 'Monday' AND member_casual = 'member'),
	(SELECT COUNT(member_casual) FROM trip_data WHERE day_of_week = 'Tuesday' AND member_casual = 'member'),
	(SELECT COUNT(member_casual) FROM trip_data WHERE day_of_week = 'Wednesday' AND member_casual = 'member'),
	(SELECT COUNT(member_casual) FROM trip_data WHERE day_of_week = 'Thursday' AND member_casual = 'member'),
	(SELECT COUNT(member_casual) FROM trip_data WHERE day_of_week = 'Friday' AND member_casual = 'member'),
	(SELECT COUNT(member_casual) FROM trip_data WHERE day_of_week = 'Saturday' AND member_casual = 'member'),
	(SELECT COUNT(member_casual) FROM trip_data WHERE day_of_week = 'Sunday' AND member_casual = 'member'),
	(SELECT COUNT(member_casual) FROM trip_data WHERE day_of_week = 'Monday' AND member_casual = 'casual'),
	(SELECT COUNT(member_casual) FROM trip_data WHERE day_of_week = 'Tuesday' AND member_casual = 'casual'),
	(SELECT COUNT(member_casual) FROM trip_data WHERE day_of_week = 'Wednesday' AND member_casual = 'casual'),
	(SELECT COUNT(member_casual) FROM trip_data WHERE day_of_week = 'Thursday' AND member_casual = 'casual'),
	(SELECT COUNT(member_casual) FROM trip_data WHERE day_of_week = 'Friday' AND member_casual = 'casual'),
	(SELECT COUNT(member_casual) FROM trip_data WHERE day_of_week = 'Saturday' AND member_casual = 'casual'),
	(SELECT COUNT(member_casual) FROM trip_data WHERE day_of_week = 'Sunday' AND member_casual = 'casual')
    );
    
CREATE TABLE ride_info_per_month (
	jan_member decimal(31,0) NULL,
	feb_member decimal(31,0) NULL,
	mar_member decimal(31,0) NULL,
	april_member decimal(31,0) NULL,
	may_member decimal(31,0) NULL,
	june_member decimal(31,0) NULL,
	jul_member decimal(31,0) NULL,
	aug_member decimal(31,0) NULL,
	sept_member decimal(31,0) NULL,
	oct_member decimal(31,0) NULL,
	nov_member decimal(31,0) NULL,
	dec_member decimal(31,0) NULL,
    
	jan_casual decimal(31,0) NULL,
	feb_casual decimal(31,0) NULL,
	mar_casual decimal(31,0) NULL,
	april_casual decimal(31,0) NULL,
	may_casual decimal(31,0) NULL,
	june_casual decimal(31,0) NULL,
	jul_casual decimal(31,0) NULL,
	aug_casual decimal(31,0) NULL,
	sept_casual decimal(31,0) NULL,
	oct_casual decimal(31,0) NULL,
	nov_casual decimal(31,0) NULL,
	dec_casual decimal(31,0) NULL
    );

# Calculate total number of rides depending on if ride is from members or casuals based on months throughout past year
INSERT INTO ride_info_per_month
VALUES(
	(SELECT COUNT(member_casual) FROM trip_data WHERE started_at LIKE '%-01-%' AND member_casual = 'member'),
	(SELECT COUNT(member_casual) FROM trip_data WHERE started_at LIKE '%-02-%' AND member_casual = 'member'),
	(SELECT COUNT(member_casual) FROM trip_data WHERE started_at LIKE '%-03-%' AND member_casual = 'member'),
	(SELECT COUNT(member_casual) FROM trip_data WHERE started_at LIKE '%-04-%' AND member_casual = 'member'),
	(SELECT COUNT(member_casual) FROM trip_data WHERE started_at LIKE '%-05-%' AND member_casual = 'member'),
	(SELECT COUNT(member_casual) FROM trip_data WHERE started_at LIKE '%-06-%' AND member_casual = 'member'),
	(SELECT COUNT(member_casual) FROM trip_data WHERE started_at LIKE '%-07-%' AND member_casual = 'member'),
	(SELECT COUNT(member_casual) FROM trip_data WHERE started_at LIKE '%-08-%' AND member_casual = 'member'),
	(SELECT COUNT(member_casual) FROM trip_data WHERE started_at LIKE '%-09-%' AND member_casual = 'member'),
	(SELECT COUNT(member_casual) FROM trip_data WHERE started_at LIKE '%-10-%' AND member_casual = 'member'),
	(SELECT COUNT(member_casual) FROM trip_data WHERE started_at LIKE '%-11-%' AND member_casual = 'member'),
	(SELECT COUNT(member_casual) FROM trip_data WHERE started_at LIKE '%-12-%' AND member_casual = 'member'),
	(SELECT COUNT(member_casual) FROM trip_data WHERE started_at LIKE '%-01-%' AND member_casual = 'casual'),
	(SELECT COUNT(member_casual) FROM trip_data WHERE started_at LIKE '%-02-%' AND member_casual = 'casual'),
	(SELECT COUNT(member_casual) FROM trip_data WHERE started_at LIKE '%-03-%' AND member_casual = 'casual'),
	(SELECT COUNT(member_casual) FROM trip_data WHERE started_at LIKE '%-04-%' AND member_casual = 'casual'),
	(SELECT COUNT(member_casual) FROM trip_data WHERE started_at LIKE '%-05-%' AND member_casual = 'casual'),
	(SELECT COUNT(member_casual) FROM trip_data WHERE started_at LIKE '%-06-%' AND member_casual = 'casual'),
	(SELECT COUNT(member_casual) FROM trip_data WHERE started_at LIKE '%-07-%' AND member_casual = 'casual'),
	(SELECT COUNT(member_casual) FROM trip_data WHERE started_at LIKE '%-08-%' AND member_casual = 'casual'),
	(SELECT COUNT(member_casual) FROM trip_data WHERE started_at LIKE '%-09-%' AND member_casual = 'casual'),
	(SELECT COUNT(member_casual) FROM trip_data WHERE started_at LIKE '%-10-%' AND member_casual = 'casual'),
	(SELECT COUNT(member_casual) FROM trip_data WHERE started_at LIKE '%-11-%' AND member_casual = 'casual'),
	(SELECT COUNT(member_casual) FROM trip_data WHERE started_at LIKE '%-12-%' AND member_casual = 'casual')
    );



```
