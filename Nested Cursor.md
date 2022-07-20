# EXERCISE NESTED CURSOR!
### FOR THE PURPOSE OF INSERTING A CUSTOMER AND TWO VALUES MEANING THE NUMBER OF LINES
#### SHOULD SHOW THIS CUSTOMER HOW MANY ORDERS EXIST WITH LESS ROW NUMBERS THAN N1, BETWEEN N1 AND N2, AND MORE THAN N2.
```
DROP PROCEDURE IF EXISTS cli_orders_between;
DELIMITER $$
CREATE PROCEDURE `cli_orders_between`(_CustomerID varchar(32), _n1 int, _n2 int)
BEGIN
    DECLARE done1 INT DEFAULT FALSE;

    DECLARE _customerOrders int;
	DECLARE _menorN1 int;
	DECLARE _entre int;
	DECLARE _maiorn2 int;
	   
    DECLARE cursor1 CURSOR FOR
	select OrderID from Orders
	where CustomerID = _CustomerID;
    DECLARE CONTINUE HANDLER FOR NOT FOUND SET done1 = TRUE;    
    set _menorN1 = 0;
	set _entre = 0;
	set _maiorn2 = 0;
           
    OPEN cursor1;
    READ_LOOP1: LOOP
        FETCH cursor1 INTO _customerOrders;
        IF done1 THEN
            LEAVE READ_LOOP1;
        END IF;
        BLOCK2: BEGIN  
            DECLARE done2 INT DEFAULT FALSE;
			DECLARE _quantiOrders int;
           
            DECLARE cursor2 CURSOR FOR
				select count(OrderID) from OrderDetails
                where OrderID in (select _customerOrders)
                group by OrderID;
            DECLARE CONTINUE HANDLER FOR NOT FOUND SET done2 = TRUE;  
           
            OPEN cursor2;
            READ_LOOP2: LOOP
                FETCH cursor2 INTO _quantiOrders;
                IF done2 THEN
                    LEAVE READ_LOOP2;
                END IF;
               
                IF _quantiOrders < _n1 THEN
					set _menorN1 = _menorN1 + 1;
				ELSEIF _quantiOrders between _n1 and _n2 THEN
					set _entre = _entre + 1;
				ELSEIF _quantiOrders > _n2 THEN
					set _maiorn2 = _maiorn2 + 1;
				END IF;
               
			END LOOP;
            CLOSE cursor2;
        END BLOCK2;        
    END LOOP;
    select _menorN1 as MenorN1, _entre as EntreN1eN2, _maiorn2 as MaiorN2;
    CLOSE cursor1;
END$$
DELIMITER ;

CALL cli_orders_between('QUEEN', 2, 3);
```
