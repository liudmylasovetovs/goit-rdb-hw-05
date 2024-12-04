SELECT *, (SELECT customer_id FROM orders
WHERE orders.id = order_details.order_id) AS c_id
FROM order_details;

SELECT *
FROM order_details
WHERE order_id IN (SELECT id FROM orders WHERE shipper_id = 3);

SELECT order_id, AVG(quantity) AS avg_quant
FROM (SELECT * FROM order_details WHERE quantity>10) AS order_table
GROUP BY order_id;

WITH temp AS (
SELECT * 
FROM order_details 
WHERE quantity>10
)
SELECT order_id, AVG(quantity) AS avg_quant
FROM temp
GROUP BY order_id;

DROP FUNCTION IF EXISTS division_try;

DELIMITER $$

CREATE FUNCTION division_try(num FLOAT, denom FLOAT)
RETURNS FLOAT
DETERMINISTIC 

BEGIN
	IF denom = 0 THEN
		RETURN NULL;
    ELSE
		RETURN num / denom;
    END IF;
END $$

DELIMITER ;

SELECT id, order_id, product_id, quantity, division_try(quantity, 3.0) AS division_res
FROM order_details;
