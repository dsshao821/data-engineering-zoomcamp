# Module 1 Homework

## Question 1. Understanding docker first run

In bash shell, with the Dockerfile in this repo, run:
docker build -t test .
pip --version

## Question 2. Understanding Docker networking and docker-compose

Answer: internal port is 5432, and the service is named db.

## Question 3. Trip Segmentation Count

### Up to one mile
SELECT
    COUNT(*) AS trip_count
FROM
    green_taxi_data
WHERE
    lpep_dropoff_datetime >= '2019-10-01' AND
    lpep_dropoff_datetime < '2019-11-01' AND
	trip_distance <= 1
;

### In between 1 (exclusive) and 3 miles (inclusive),
SELECT
    COUNT(*) AS trip_count
FROM
    green_taxi_data
WHERE
    lpep_dropoff_datetime >= '2019-10-01' AND
    lpep_dropoff_datetime < '2019-11-01' AND
	trip_distance > 1 AND
    trip_distance <= 3
;

### In between 3 (exclusive) and 7 miles (inclusive),
SELECT
    COUNT(*) AS trip_count
FROM
    green_taxi_data
WHERE
    lpep_dropoff_datetime >= '2019-10-01' AND
    lpep_dropoff_datetime < '2019-11-01' AND
	trip_distance > 3 AND
    trip_distance <= 7
;

### In between 7 (exclusive) and 10 miles (inclusive),
SELECT
    COUNT(*) AS trip_count
FROM
    green_taxi_data
WHERE
    lpep_dropoff_datetime >= '2019-10-01' AND
    lpep_dropoff_datetime < '2019-11-01' AND
	trip_distance > 7 AND
    trip_distance <= 10
;

### Over 10 miles
SELECT
    COUNT(*) AS trip_count
FROM
    green_taxi_data
WHERE
    lpep_dropoff_datetime >= '2019-10-01' AND
    lpep_dropoff_datetime < '2019-11-01' AND
	trip_distance > 10
;

## Question 4. Longest trip for each day

The code below shows the single date with the longest trip, and what that distance was

SELECT
    DATE(lpep_pickup_datetime) AS pickup_date,
    MAX(trip_distance) as longest_trip
FROM
    green_taxi_data
GROUP BY
    DATE(lpep_pickup_datetime)
ORDER BY
    longest_trip desc
LIMIT 1
;

## Question 5. Three biggest pickup zones

The code below shows the top 3 pickup locations on 10/18 with a total amount of over 13000

SELECT
    z."Zone",
    COUNT(1) as num_trips
FROM
    green_taxi_data g
LEFT JOIN zones z
	ON z."LocationID" = g."PULocationID"
WHERE DATE(g.lpep_pickup_datetime) = '2019-10-18'
GROUP BY
    z."Zone"
HAVING
	SUM(total_amount) > 13000
ORDER BY
    num_trips DESC
LIMIT 3
;

## Question 6. Largest tip

The code below shows the single drop off zone with the largest tip in October 2019

SELECT
    zd."Zone" as drop_off_zone,
    tip_amount
FROM
    green_taxi_data g
LEFT JOIN zones z
	ON z."LocationID" = g."PULocationID"
LEFT JOIN zones zd
	ON zd."LocationID" = g."DOLocationID"
WHERE lpep_dropoff_datetime >= '2019-10-01'
  AND lpep_dropoff_datetime < '2019-11-01'
  AND z."Zone" = 'East Harlem North'
ORDER BY
    tip_amount DESC
LIMIT 1
;