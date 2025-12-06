
# ERD dan Implementasi Database Ojek Online

## ERD
(Deskripsi singkat ERD; diagram not rendered here)

## Struktur Tabel
```sql
CREATE TABLE admin (
  id SERIAL PRIMARY KEY,
  name VARCHAR(100)
);

CREATE TABLE customer (
  id SERIAL PRIMARY KEY,
  name VARCHAR(100)
);

CREATE TABLE driver (
  id SERIAL PRIMARY KEY,
  name VARCHAR(100)
);

CREATE TABLE ride (
  id SERIAL PRIMARY KEY,
  customer_id INT REFERENCES customer(id),
  driver_id INT REFERENCES driver(id),
  pickup_location VARCHAR(200),
  dropoff_location VARCHAR(200),
  start_time TIMESTAMP,
  end_time TIMESTAMP,
  rating INT
);
```

## Dummy Data
```sql
INSERT INTO admin (name) VALUES ('Admin1');
INSERT INTO customer (name) VALUES ('Andre'), ('Budi');
INSERT INTO driver (name) VALUES ('Doni'), ('Rizal');

INSERT INTO ride (customer_id, driver_id, pickup_location, dropoff_location, start_time, end_time, rating)
VALUES
(1,1,'ITPLN','Grogol','2025-12-01 08:00','2025-12-01 08:30',5),
(2,2,'Grogol','Ciledug','2025-12-02 09:00','2025-12-02 09:50',4);
```

## Query Fitur
```sql
-- 1. Total order tiap bulan
SELECT DATE_TRUNC('month', start_time) AS bulan, COUNT(*) FROM ride GROUP BY 1;

-- 2. Nama customer paling sering order tiap bulan
SELECT bulan, customer_id, COUNT(*) AS total
FROM (
  SELECT DATE_TRUNC('month', start_time) AS bulan, customer_id FROM ride
) x
GROUP BY bulan, customer_id;

-- 3. Lokasi pickup terbanyak
SELECT pickup_location, COUNT(*) FROM ride GROUP BY pickup_location ORDER BY 2 DESC;

-- 4. Waktu paling ramai
SELECT EXTRACT(HOUR FROM start_time) AS jam, COUNT(*) FROM ride GROUP BY jam ORDER BY 2 DESC;

-- 5. Customer sedang login (dummy: asumsi)
SELECT * FROM customer;

-- 6. Driver paling rajin
SELECT driver_id, COUNT(*) AS total FROM ride GROUP BY driver_id ORDER BY total DESC;
```


## Dummy Data Tambahan
```sql

-- Expanded Dummy Data

INSERT INTO customer (name) VALUES 
('Cindy'), ('Dewi'), ('Eka'), ('Fajar'), ('Galih'), ('Hana'), ('Indra'), ('Joko');

INSERT INTO driver (name) VALUES
('Surya'), ('Bagus'), ('Hafiz'), ('Yoga');

INSERT INTO ride (customer_id, driver_id, pickup_location, dropoff_location, start_time, end_time, rating) VALUES
(3,1,'Kebon Jeruk','Slipi','2025-12-03 07:30','2025-12-03 07:55',5),
(4,2,'Ciledug','BSD','2025-12-03 10:00','2025-12-03 10:40',4),
(5,3,'Grogol','Kuningan','2025-12-04 12:10','2025-12-04 12:55',5),
(6,4,'ITPLN','Pesanggrahan','2025-12-04 09:15','2025-12-04 09:45',3),
(7,5,'Kali Deres','Cengkareng','2025-12-05 14:00','2025-12-05 14:30',5),
(8,6,'Slipi','ITC Permata Hijau','2025-12-05 15:10','2025-12-05 15:50',4),
(2,3,'Tangerang Kota','Kebon Jeruk','2025-12-06 08:20','2025-12-06 08:50',5),
(1,4,'Ciledug','ITPLN','2025-12-06 18:00','2025-12-06 18:25',5),
(3,2,'Grogol','Thamrin','2025-12-07 11:00','2025-12-07 11:30',4),
(4,1,'Slipi','Ciledug','2025-12-07 20:00','2025-12-07 20:50',5);

```