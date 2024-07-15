# DB_CaseStudy
Part of DB training

## DB MODEL

GIVEN
![WhatsApp Image 2024-07-03 at 10 08 26 AM](https://github.com/vedantnaudiyal/DB_CaseStudy/assets/174436138/c6710e9f-966b-48cf-b2e4-dc969c469450)


## SOLUTION:
### ER-MODEL:
<img width="625" alt="image" src="https://github.com/vedantnaudiyal/DB_CaseStudy/assets/174436138/adf742e4-0c59-4ae7-ab5d-dc7b0ff5c9a3">


### table query ss
<img width="825" alt="image" src="https://github.com/vedantnaudiyal/DB_CaseStudy/assets/174436138/af736920-4194-458d-8ea1-4ba5abc121af">



## PROBLEM STATEMENT


<img width="448" alt="image" src="https://github.com/vedantnaudiyal/DB_CaseStudy/assets/174436138/7be93587-3241-477b-9612-ab38d48d9218">


### SOLUTION ON DB FIDDLE
**Schema (MySQL v8.0)**

    CREATE TABLE IF NOT EXISTS unlabeled_image_predictions (
        image_id int,
        score float
    );
    
    INSERT INTO unlabeled_image_predictions (image_id, score) VALUES
    (18281, 0.31491),
    (17051, 0.98921),
    (1146, 0.56161),
    (594, 0.76701),
    (232, 0.15981),
    (5241, 0.98761),
    (306, 0.6487),
    (11321, 0.88231),
    (19061, 0.83941),
    (12721, 0.97781),
    (1616, 0.10031),
    (1161, 0.71131),
    (1715, 0.89211),
    (11091, 0.1151),
    (1424, 0.77901),
    (1609, 0.52411),
    (1631, 0.25521),
    (12761, 0.26721),
    (17011, 0.0758),
    (554, 0.4418),
    (998, 0.0379),
    (809, 0.1058),
    (1219, 0.71431),
    (402, 0.7655),
    (3631, 0.26611),
    (16241, 0.82701),
    (1640, 0.8790),
    (913, 0.2421),
    (1439, 0.33871),
    (1464, 0.36741),
    (1405, 0.69291),
    (19861, 0.89311),
    (344, 0.37611),
    (847, 0.48891),
    (482, 0.50231),
    (1823, 0.33611),
    (16171, 0.02181),
    (1471, 0.00721),
    (18671, 0.4050),
    (1961, 0.44981),
    (126, 0.35641),
    (943, 0.0452),
    (1115, 0.53091),
    (1417, 0.7168),
    (1706, 0.96491),
    (1166, 0.25071),
    (1991, 0.41911),
    (1465, 0.0895),
    (153, 0.8169),
    (971, 0.9871);

---

**Query #1**

    with cte1 as (select image_id, score, ROW_NUMBER() over (order by score desc) as rowNum from unlabeled_image_predictions),
    cte2 as (select image_id, score, ROW_NUMBER() over (order by score) as rowNum from unlabeled_image_predictions)
    select image_id, round(score) as weak_label from cte1 where rowNum%3=1 and score>0.5 and rowNum<29999
    union
    select image_id, round(score) as weak_label from cte2 where rowNum%3=1 and score<0.5 limit 10000;

| image_id | weak_label |
| -------- | ---------- |
| 17051    | 1          |
| 12721    | 1          |
| 1715     | 1          |
| 19061    | 1          |
| 1424     | 1          |
| 1417     | 1          |
| 1405     | 1          |
| 1115     | 1          |
| 1471     | 0          |
| 943      | 0          |
| 1616     | 0          |
| 232      | 0          |
| 1631     | 0          |
| 18281    | 0          |
| 126      | 0          |
| 18671    | 0          |
| 1961     | 0          |

---

[View on DB Fiddle](https://www.db-fiddle.com/)


--------------------------------------------------------------------------------------------------------------------------------------------

# DB TASK -2

# Problem Statement

## Database Schema

```sql
CREATE TABLE city
(
	id INT PRIMARY KEY,
	name VARCHAR(50) NOT NULL
);

CREATE TABLE hotel
(
	id INT PRIMARY KEY,
	city_id INT NOT NULL REFERENCES cityl,
	name VARCHAR(50) NOT NULL,
	day_price NUMERIC(8, 2) NOT NULL,
	photos JSONB DEFAULT '[]'
);


CREATE TABLE booking
(
	id int PRIMARY KEY,
	hotel_id INT NOT NULL REFERENCES hotel,
	booking_date DATE NOT NULL,
	start_date DATE NOT NULL,
	end_date DATE NOT NULL
);
```

## DESCRIPTION

Your task is to prepare a list of cities with the date of last reservation made in the city and a main photo (photos[0]) of the most popular (by number of bookings) hotel in this city.

Sort results in ascending order by city. If many hotels have the same amount of bookings sort them by ID (ascending order).
Remember that the query will also be run of different datasets.

## Example Output

|   City    | last_booking_date | hotel_id | hotel_photo |
| :-------: | :---------------: | :------: | :---------: |
| Barcelona |    2024-06-15     |    3     |   3-1.jpg   |
|   Roma    |    2024-06-16     |    6     |   6-1.jpg   |

## Sample Data

### Table: city

| id  |   name    |
| :-: | :-------: |
|  1  | Barcelona |
|  2  |   Roma    |
|  3  |   Paris   |
|  4  | New York  |
|  5  |   Tokyo   |
|  6  |  Sydney   |

### Table: hotel

| id  | city_id |      name       | day_price |          photos          |
| :-: | :-----: | :-------------: | :-------: | :----------------------: |
|  1  |    1    |  The New View   |   67.00   |  ["1-1.jpg", "1-2.jpg"]  |
|  2  |    1    | The Upper House |   99.00   |  ["2-1.jpg", "2-2.jpg"]  |
|  3  |    2    |    Ace Hotel    |   90.00   |  ["3-1.jpg", "3-2.jpg"]  |
|  4  |    2    |   Hotel Diva    |  111.00   |  ["4-1.jpg", "4-2.jpg"]  |
|  5  |    3    |  Hotel Triton   |  105.00   |  ["5-1.jpg", "5-2.jpg"]  |
|  6  |    4    |  Grand Palace   |  150.00   |  ["6-1.jpg", "6-2.jpg"]  |
|  7  |    5    | Tokyo Tower Inn |  200.00   |  ["7-1.jpg", "7-2.jpg"]  |
|  8  |    6    |  Sydney Suites  |  180.00   |  ["8-1.jpg", "8-2.jpg"]  |
|  9  |    4    |  New York Inn   |  120.00   |  ["9-1.jpg", "9-2.jpg"]  |
| 10  |    5    |  Sakura Hotel   |  220.00   | ["10-1.jpg", "10-2.jpg"] |

### Table: booking

| id  | hotel_id | booking_date | start_date |  end_date  |
| :-: | :------: | :----------: | :--------: | :--------: |
|  1  |    1     |  2024-06-10  | 2024-07-01 | 2024-07-05 |
|  2  |    2     |  2024-06-11  | 2024-07-02 | 2024-07-06 |
|  3  |    3     |  2024-06-12  | 2024-07-03 | 2024-07-07 |
|  4  |    4     |  2024-06-13  | 2024-07-04 | 2024-07-08 |
|  5  |    5     |  2024-06-14  | 2024-07-05 | 2024-07-09 |
|  6  |    6     |  2024-06-15  | 2024-07-06 | 2024-07-10 |
|  7  |    7     |  2024-06-16  | 2024-07-07 | 2024-07-11 |
|  8  |    8     |  2024-06-17  | 2024-07-08 | 2024-07-12 |
|  9  |    9     |  2024-06-18  | 2024-07-09 | 2024-07-13 |
| 10  |    10    |  2024-06-19  | 2024-07-10 | 2024-07-14 |
| 11  |    1     |  2024-06-20  | 2024-07-11 | 2024-07-15 |
| 12  |    2     |  2024-06-21  | 2024-07-12 | 2024-07-16 |
| 13  |    3     |  2024-06-22  | 2024-07-13 | 2024-07-17 |
| 14  |    4     |  2024-06-23  | 2024-07-14 | 2024-07-18 |
| 15  |    5     |  2024-06-24  | 2024-07-15 | 2024-07-19 |
| 16  |    6     |  2024-06-25  | 2024-07-16 | 2024-07-20 |
| 17  |    7     |  2024-06-26  | 2024-07-17 | 2024-07-21 |
| 18  |    8     |  2024-06-27  | 2024-07-18 | 2024-07-22 |
| 19  |    9     |  2024-06-28  | 2024-07-19 | 2024-07-23 |
| 20  |    10    |  2024-06-29  | 2024-07-20 | 2024-07-24 |

### Expected ouput

| city_name | last_booking_date        | hotel_id | hotel_photo |
| --------- | ------------------------ | -------- | ----------- |
| Barcelona | 2024-06-21T00:00:00.000Z | 1        | 1-1.jpg     |
| Barcelona | 2024-06-21T00:00:00.000Z | 2        | 2-1.jpg     |
| New York  | 2024-06-28T00:00:00.000Z | 6        | 6-1.jpg     |
| New York  | 2024-06-28T00:00:00.000Z | 9        | 9-1.jpg     |
| Paris     | 2024-06-24T00:00:00.000Z | 5        | 5-1.jpg     |
| Roma      | 2024-06-23T00:00:00.000Z | 3        | 3-1.jpg     |
| Roma      | 2024-06-23T00:00:00.000Z | 4        | 4-1.jpg     |
| Sydney    | 2024-06-27T00:00:00.000Z | 8        | 8-1.jpg     |
| Tokyo     | 2024-06-29T00:00:00.000Z | 7        | 7-1.jpg     |
| Tokyo     | 2024-06-29T00:00:00.000Z | 10       | 10-1.jpg    |

# Solution

```sql
CREATE TABLE city
(
	id INT PRIMARY KEY,
	name VARCHAR(50) NOT NULL
);

CREATE TABLE hotel
(
	id INT PRIMARY KEY,
	city_id INT NOT NULL REFERENCES city,
	name VARCHAR(50) NOT NULL,
	day_price NUMERIC(8, 2) NOT NULL,
	photos JSONB DEFAULT '[]'
);


CREATE TABLE booking
(
	id int PRIMARY KEY,
	hotel_id INT NOT NULL REFERENCES hotel,
	booking_date DATE NOT NULL,
	start_date DATE NOT NULL,
	end_date DATE NOT NULL
);

INSERT INTO city (id, name) VALUES
(1, 'Barcelona'),
(2, 'Roma'),
(3, 'Paris'),
(4, 'New York'),
(5, 'Tokyo'),
(6, 'Sydney');

INSERT INTO hotel (id, city_id, name, day_price, photos) VALUES
(1, 1, 'The New View', 67.00, '["1-1.jpg", "1-2.jpg"]'),
(2, 1, 'The Upper House', 99.00, '["2-1.jpg", "2-2.jpg"]'),
(3, 2, 'Ace Hotel', 90.00, '["3-1.jpg", "3-2.jpg"]'),
(4, 2, 'Hotel Diva', 111.00, '["4-1.jpg", "4-2.jpg"]'),
(5, 3, 'Hotel Triton', 105.00, '["5-1.jpg", "5-2.jpg"]'),
(6, 4, 'Grand Palace', 150.00, '["6-1.jpg", "6-2.jpg"]'),
(7, 5, 'Tokyo Tower Inn', 200.00, '["7-1.jpg", "7-2.jpg"]'),
(8, 6, 'Sydney Suites', 180.00, '["8-1.jpg", "8-2.jpg"]'),
(9, 4, 'New York Inn', 120.00, '["9-1.jpg", "9-2.jpg"]'),
(10, 5, 'Sakura Hotel', 220.00, '["10-1.jpg", "10-2.jpg"]');

INSERT INTO booking (id, hotel_id, booking_date, start_date, end_date) VALUES
(1, 1, '2024-06-10', '2024-07-01', '2024-07-05'),
(2, 2, '2024-06-11', '2024-07-02', '2024-07-06'),
(3, 3, '2024-06-12', '2024-07-03', '2024-07-07'),
(4, 4, '2024-06-13', '2024-07-04', '2024-07-08'),
(5, 5, '2024-06-14', '2024-07-05', '2024-07-09'),
(6, 6, '2024-06-15', '2024-07-06', '2024-07-10'),
(7, 7, '2024-06-16', '2024-07-07', '2024-07-11'),
(8, 8, '2024-06-17', '2024-07-08', '2024-07-12'),
(9, 9, '2024-06-18', '2024-07-09', '2024-07-13'),
(10, 10, '2024-06-19', '2024-07-10', '2024-07-14'),
(11, 1, '2024-06-20', '2024-07-11', '2024-07-15'),
(12, 2, '2024-06-21', '2024-07-12', '2024-07-16'),
(13, 3, '2024-06-22', '2024-07-13', '2024-07-17'),
(14, 4, '2024-06-23', '2024-07-14', '2024-07-18'),
(15, 5, '2024-06-24', '2024-07-15', '2024-07-19'),
(16, 6, '2024-06-25', '2024-07-16', '2024-07-20'),
(17, 7, '2024-06-26', '2024-07-17', '2024-07-21'),
(18, 8, '2024-06-27', '2024-07-18', '2024-07-22'),
(19, 9, '2024-06-28', '2024-07-19', '2024-07-23'),
(20, 10, '2024-06-29', '2024-07-20', '2024-07-24');


-- Write your code here
### SOLUTION

with cte1 as (
        select city.id as cityID, hotel.id as hotelID, photos, city.name as City 
      	from city join hotel 
      	on city.id = hotel.city_id
    ),
    cte2 as (
      select cte1.*, id as bookingID, booking_date
      from cte1 join booking
      on booking.hotel_id=cte1.hotelID
    ),
    cte3 as (
          select City, hotelID, photos, count(bookinGID) as no_of_bookings
      	from cte2
      	group by City, hotelID, photos
    ),
    cte4 as (
      select City, hotelID, no_of_bookings, photos[0] as hotel_photo, RANK() over (partition by City order by no_of_bookings desc) as ranking
      	from cte3
    ),
    cte5 as (
      select City, max(booking_date) as last_booking_date
      from cte2
      group by City
    )
    
    select City, last_booking_date, hotelID, hotel_photo 
    from cte4 join cte5
    using(City)
    where ranking=1
    order by City, hotelID;


```


-------------------------------------------------------------- ### DB FIDDLE SOLUTION --------------------------------------------------------------

**Schema (PostgreSQL v15)**

    CREATE TABLE city
        (
        	id INT PRIMARY KEY,
        	name VARCHAR(50) NOT NULL
        );
        
        CREATE TABLE hotel
        (
        	id INT PRIMARY KEY,
        	city_id INT NOT NULL REFERENCES city,
        	name VARCHAR(50) NOT NULL,
        	day_price NUMERIC(8, 2) NOT NULL,
        	photos JSONB DEFAULT '[]'
        );
        
        
        CREATE TABLE booking
        (
        	id int PRIMARY KEY,
        	hotel_id INT NOT NULL REFERENCES hotel,
        	booking_date DATE NOT NULL,
        	start_date DATE NOT NULL,
        	end_date DATE NOT NULL
        );
        INSERT INTO city (id, name) VALUES
        (1, 'Barcelona'),
        (2, 'Roma'),
        (3, 'Paris'),
        (4, 'New York'),
        (5, 'Tokyo'),
        (6, 'Sydney');
        
        INSERT INTO hotel (id, city_id, name, day_price, photos) VALUES
        (1, 1, 'The New View', 67.00, '["1-1.jpg", "1-2.jpg"]'),
        (2, 1, 'The Upper House', 99.00, '["2-1.jpg", "2-2.jpg"]'),
        (3, 2, 'Ace Hotel', 90.00, '["3-1.jpg", "3-2.jpg"]'),
        (4, 2, 'Hotel Diva', 111.00, '["4-1.jpg", "4-2.jpg"]'),
        (5, 3, 'Hotel Triton', 105.00, '["5-1.jpg", "5-2.jpg"]'),
        (6, 4, 'Grand Palace', 150.00, '["6-1.jpg", "6-2.jpg"]'),
        (7, 5, 'Tokyo Tower Inn', 200.00, '["7-1.jpg", "7-2.jpg"]'),
        (8, 6, 'Sydney Suites', 180.00, '["8-1.jpg", "8-2.jpg"]'),
        (9, 4, 'New York Inn', 120.00, '["9-1.jpg", "9-2.jpg"]'),
        (10, 5, 'Sakura Hotel', 220.00, '["10-1.jpg", "10-2.jpg"]');
        
        INSERT INTO booking (id, hotel_id, booking_date, start_date, end_date) VALUES
        (1, 1, '2024-06-10', '2024-07-01', '2024-07-05'),
        (2, 2, '2024-06-11', '2024-07-02', '2024-07-06'),
        (3, 3, '2024-06-12', '2024-07-03', '2024-07-07'),
        (4, 4, '2024-06-13', '2024-07-04', '2024-07-08'),
        (5, 5, '2024-06-14', '2024-07-05', '2024-07-09'),
        (6, 6, '2024-06-15', '2024-07-06', '2024-07-10'),
        (7, 7, '2024-06-16', '2024-07-07', '2024-07-11'),
        (8, 8, '2024-06-17', '2024-07-08', '2024-07-12'),
        (9, 9, '2024-06-18', '2024-07-09', '2024-07-13'),
        (10, 10, '2024-06-19', '2024-07-10', '2024-07-14'),
        (11, 1, '2024-06-20', '2024-07-11', '2024-07-15'),
        (12, 2, '2024-06-21', '2024-07-12', '2024-07-16'),
        (13, 3, '2024-06-22', '2024-07-13', '2024-07-17'),
        (14, 4, '2024-06-23', '2024-07-14', '2024-07-18'),
        (15, 5, '2024-06-24', '2024-07-15', '2024-07-19'),
        (16, 6, '2024-06-25', '2024-07-16', '2024-07-20'),
        (17, 7, '2024-06-26', '2024-07-17', '2024-07-21'),
        (18, 8, '2024-06-27', '2024-07-18', '2024-07-22'),
        (19, 9, '2024-06-28', '2024-07-19', '2024-07-23'),
        (20, 10, '2024-06-29', '2024-07-20', '2024-07-24');
    

---

**Query #1**

    with cte1 as (
            select city.id as cityID, hotel.id as hotelID, photos, city.name as City 
          	from city join hotel 
          	on city.id = hotel.city_id
        ),
        cte2 as (
          select cte1.*, id as bookingID, booking_date
          from cte1 join booking
          on booking.hotel_id=cte1.hotelID
        ),
        cte3 as (
              select City, hotelID, photos, count(bookinGID) as no_of_bookings
          	from cte2
          	group by City, hotelID, photos
        ),
        cte4 as (
          select City, hotelID, no_of_bookings, photos[0] as hotel_photo, RANK() over (partition by City order by no_of_bookings desc) as ranking
          	from cte3
        ),
        cte5 as (
          select City, max(booking_date) as last_booking_date
          from cte2
          group by City
        )
        
        select City, last_booking_date, hotelID, hotel_photo 
        from cte4 join cte5
        using(City)
        where ranking=1
        order by City, hotelID;

| city      | last_booking_date        | hotelid | hotel_photo |
| --------- | ------------------------ | ------- | ----------- |
| Barcelona | 2024-06-21T00:00:00.000Z | 1       | 1-1.jpg     |
| Barcelona | 2024-06-21T00:00:00.000Z | 2       | 2-1.jpg     |
| New York  | 2024-06-28T00:00:00.000Z | 6       | 6-1.jpg     |
| New York  | 2024-06-28T00:00:00.000Z | 9       | 9-1.jpg     |
| Paris     | 2024-06-24T00:00:00.000Z | 5       | 5-1.jpg     |
| Roma      | 2024-06-23T00:00:00.000Z | 3       | 3-1.jpg     |
| Roma      | 2024-06-23T00:00:00.000Z | 4       | 4-1.jpg     |
| Sydney    | 2024-06-27T00:00:00.000Z | 8       | 8-1.jpg     |
| Tokyo     | 2024-06-29T00:00:00.000Z | 7       | 7-1.jpg     |
| Tokyo     | 2024-06-29T00:00:00.000Z | 10      | 10-1.jpg    |

---

[View on DB Fiddle](https://www.db-fiddle.com/)
