# THIS DOCUMENT IS FOR FUNCTIONS ONLY
## FC1 - INPUT: EMPLOYEEID / CUSTOMERID / ANO
## OUTPUT: AMOUNT INVOICED IN THAT YEAR
```
drop function if exists FC1;
DELIMITER $$
create function FC1(_EmployeeID int, _CustomerID varchar(32), _Ano int) returns int
begin
declare _quantos int;
set _quantos = (
	select sum(UnitPrice*Quantity) as Amount from Orders A
	join OrderDetails using (OrderID)
    where EmployeeID = _EmployeeID and CustomerID = _CustomerID and year(OrderDate) = _Ano
	group by _Ano
);
return _quantos;
end $$
DELIMITER ;

select FC1('1','EASTC', 1997);
```
## Function that takes the OrderID and returns the order total
```
drop function if exists FC1;
DELIMITER $$
create function FC1(_OrderID int) returns decimal(12,2)
begin
declare _quantos varchar(32);
set _quantos = (
	select sum(UnitPrice*Quantity) as Total from orderdetails
    where OrderID = _OrderID
);
return _quantos;
end $$
DELIMITER ;

select FC1(10249);
```
## Function that takes the CustomerID and returns the amount that that customer purchased
```
drop function if exists FC1;
DELIMITER $$
create function FC1(_CustomerID varchar(32)) returns decimal(12,2)
begin
declare _quantos varchar(32);
set _quantos = (
	select sum(UnitPrice*Quantity) as Total from orders A
	join orderdetails B on B.OrderID = A.OrderID
    where A.CustomerID = _CustomerID
);
return _quantos;
end $$
DELIMITER ;

select FC1('BERGS');
```
## How many orders does client x have
```
DROP FUNCTION IF EXISTS quantas;
DELIMITER $$
CREATE FUNCTION `quantas`(_CustomerID varchar(32)) RETURNS INT
BEGIN
DECLARE _quantasencomendas INT;
SET _quantasencomendas = (
SELECT COUNT(*) FROM Orders WHERE CustomerID = _CustomerID  GROUP BY CustomerID
);
RETURN _quantasencomendas;
END $$
DELIMITER ;

SELECT quantas("QUICK");
```
## Function takes the name of the country and a boolean value[1,0]. If the value is 1 it returns the sales made in that country
```
drop function if exists TotalPorPais;
DELIMITER $$
CREATE FUNCTION TotalPorPais (_country VARCHAR(32), _boolean boolean) RETURNS int
BEGIN
	DECLARE _totaltrue int;
	DECLARE _totalfalse int;
	set _totaltrue = (
		select sum(A.Quantity * A.UnitPrice) from OrderDetails A
		join Orders B using(OrderID)
		where B.ShipCountry = _country);
	set _totalfalse = (
		select sum(A.Quantity * A.UnitPrice) from OrderDetails A
		join Orders B using(OrderID)
		where B.ShipCountry not in (_country));

	if _boolean = 1 then
		return _totaltrue;
	elseif _boolean = 0 then
		return _totalfalse;
	END if;
END $$
DELIMITER ;

select TotalPorPais('London', 1);
```
