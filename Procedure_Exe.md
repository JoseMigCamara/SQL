# SOME EXERCISES ABOUT PROCEDURES 2
## SP SHOW THE BEST X CUSTOMERS --> CALL SP(3)
```
Drop procedure if exists SP3;
Delimiter $$
create procedure SP3(in _topn int)
begin
	select CustomerID, sum(UnitPrice*Quantity) as Montante from Orders
	join OrderDetails using (OrderID)
	group by CustomerID
	Order by Montante desc
    limit _topn; -- limite de linhas que queremos ver
end$$
delimiter ;

call SP3('3');
```
## CUSTOMERID, COMPANYNAME, MONTANTE AVG, MIN, MAX --> CALL SP('ANTON')
```
Drop procedure if exists SP3;
Delimiter $$
create procedure SP3(in _CustomerID varchar(50))
begin
	select CustomerID, CompanyName, sum(UnitPrice*Quantity) as Montante, avg(UnitPrice*Quantity) as 'Média', 
	min(UnitPrice*Quantity) as 'Minimo', Max(UnitPrice*Quantity) as 'Máximo'
    from Customers
	join Orders using (CustomerID)
	join OrderDetails using (OrderID)
	where CustomerID = _CustomerID
	group by CustomerID, CompanyName;
end$$
delimiter ;

call SP3('ANTON');
```
## COSTUMERID, COMPANYNAME, ORDERID --> CALL SP('SEAFOOD')
```
Drop procedure if exists SP3;
Delimiter $$
create procedure SP3(in _CategoryName varchar(50))
begin
	select A.CustomerID, A.CompanyName, B.OrderID from Customers A
	join Orders B using (CustomerID)
	join OrderDetails C using (OrderID)
	join Products D using (ProductID)
	join Categories E using (CategoryID)
	where E.CategoryName = _CategoryName
    order by A.CustomerID;
end$$
delimiter ;

call SP3('Seafood');
```
