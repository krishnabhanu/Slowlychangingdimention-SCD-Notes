# Slowlychangingdimention-SCD-Notes
These Notes contain  the concept of Slowly Changing Dimension (SCD) in SnowflakeDB and its types - SCD 1, SCD 2, SCD 3 with an example. I used a customer table with basic columns and dummy data to apply each type of SCD one by one for better understanding. 


## Customer Table

Let's create a customer table with the following basic columns and dummy data.

CREATE TABLE customer (
   customer_id INT,
   customer_name VARCHAR(50),
   customer_email VARCHAR(50),
   customer_phone VARCHAR(15),
   load_date DATE,
	 customer_address VARCHAR(255)
);

INSERT INTO customer VALUES
   (1, 'John Doe', 'john.doe@example.com', '123-456-7890', '2022-01-01', '123 Main St'),
   (2, 'Jane Doe', 'jane.doe@example.com', '987-654-3210', '2022-01-01', '456 Elm St'),
   (3, 'Bob Smith', 'bob.smith@example.com', '555-555-5555', '2022-01-01', '789 Oak St');


## SCD Type 1

SCD Type 1 updates the current record with the new data, overwriting the old data. The load date remains the same.

Let's alter our customer table to implement SCD Type 1.

-- Update customer address for customer_id=2
UPDATE customer SET customer_address = '789 Maple St' WHERE customer_id = 2;

SELECT * FROM customer;


## SCD Type 2

SCD Type 2 creates a new record for each change in the data, with a new surrogate key and load date. The old record remains in the table with an end date equal to the start date of the new record.

Let's alter our customer table to implement SCD Type 2.

ALTER TABLE customer ADD COLUMN customer_segment VARCHAR(20);
ALTER TABLE customer ADD COLUMN start_date DATE;
ALTER TABLE customer ADD COLUMN end_date DATE;
ALTER TABLE customer ADD COLUMN version BIGINT DEFAULT 1;

-- Update customer segment for customer_id=2
UPDATE customer SET customer_segment = 'Gold', start_date = '2022-02-01', end_date = '9999-12-31' WHERE customer_id = 2;

-- Insert new record for customer_id=2
INSERT INTO customer (customer_id, customer_name, customer_email, customer_phone, customer_address, customer_segment, start_date, end_date, version, load_date)
   SELECT customer_id, customer_name, customer_email, customer_phone, customer_address, 'Platinum', '2022-03-01', '9999-12-31', version + 1, '2022-03-01' FROM customer WHERE customer_id = 2;

SELECT * FROM customer;


## SCD Type 3

SCD Type 3 keeps the current and previous values in the same record, with separate columns for each value.

Let's alter our customer table to implement SCD Type 3.


ALTER TABLE customer ADD COLUMN customer_status VARCHAR(10);
ALTER TABLE customer ADD COLUMN prev_customer_status VARCHAR(10);

-- Update customer status for customer_id=2
UPDATE customer SET customer_status = 'Active', prev_customer_status = 'Inactive' WHERE customer_id = 2;

SELECT * FROM customer;

## SCD Type 6

This is the combined version of 1 and 2.

