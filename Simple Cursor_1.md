## EXERCISES ABOUT CURSOR
### 1.) Show how many orders the employee have
```
DROP PROCEDURE IF EXISTS SP_cursor1;
DELIMITER $$
CREATE PROCEDURE SP_cursor1 ()
BEGIN
	DECLARE done INT DEFAULT FALSE;
	DECLARE _CustomerID VARCHAR(5);
	DECLARE _CompanyName VARCHAR(40);
	DECLARE _City VARCHAR(15);
	DECLARE _Country VARCHAR(15);
	 
	DECLARE cursor1 CURSOR FOR
		SELECT CustomerID, CompanyName, City, Country FROM Customers where Country = 'France';
	DECLARE CONTINUE HANDLER FOR NOT FOUND SET done = TRUE;
          -- OPEN CURSOR
	OPEN cursor1;
		read_loop: LOOP
			FETCH cursor1 INTO _CustomerID, _CompanyName, _City, _Country;
			IF done THEN
				LEAVE read_loop;
			END IF;
			select _CustomerID, _CompanyName, _City, _Country, count(*) as 'Orders' 
                        from Orders where CustomerID = _CustomerID;
		END LOOP;
	CLOSE cursor1;
    
END$$

CALL SP_cursor1();
```
