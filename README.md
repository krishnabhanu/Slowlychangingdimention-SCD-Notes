# Slowlychangingdimention-SCD-Notes
These Notes contain  the concept of Slowly Changing Dimension (SCD) in SnowflakeDB and its types - SCD 1, SCD 2, SCD 3, and SCD 6 with an example. I used a customer table with basic columns and dummy data to apply each type of SCD one by one for better understanding. 


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
