# goit-rdb-hw-05

# Task 1

SELECT 
    od.id, 
    od.order_id, 
    od.product_id, 
    od.quantity, 
    (SELECT o.customer_id 
     FROM orders_2 o 
     WHERE o.id = od.order_id) AS customer_id
FROM 
    order_details od;

# Task 2

SELECT 
    od.id, 
    od.order_id, 
    od.product_id, 
    od.quantity
FROM 
    order_details od
WHERE 
    od.order_id IN (SELECT o.id 
                    FROM orders_2 o 
                    WHERE o.shipper_id = 3);

# Task 3

SELECT 
    subquery.order_id, 
    AVG(subquery.quantity) AS avg_quantity
FROM 
    (SELECT 
        od.order_id, 
        od.quantity 
     FROM 
        order_details od
     WHERE 
        od.quantity > 10) AS subquery
GROUP BY 
    subquery.order_id;


# Task 4

WITH temp AS (
    SELECT 
        od.order_id, 
        od.quantity 
    FROM 
        order_details od
    WHERE 
        od.quantity > 10
)
SELECT 
    temp.order_id, 
    AVG(temp.quantity) AS avg_quantity
FROM 
    temp
GROUP BY 
    temp.order_id;


# Task 5

DELIMITER //

DROP FUNCTION IF EXISTS divide_numbers //

CREATE FUNCTION divide_numbers(
    param1 FLOAT,
    param2 FLOAT
) RETURNS FLOAT
DETERMINISTIC
BEGIN
    IF param2 = 0 THEN
        RETURN NULL;
    ELSE
        RETURN param1 / param2;
    END IF;
END //

DELIMITER ;

SELECT 
    id, 
    order_id, 
    product_id, 
    quantity, 
    divide_numbers(quantity, 5) AS divided_quantity
FROM 
    order_details;
