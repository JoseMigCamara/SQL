# Exercises with Procedure
## Give the employees of london with procedure 
```
Drop procedure if exists SP1;
Delimiter $$
create procedure `SP1` (in _City varchar(50))
begin
	select * from Employees
	where city = _City;
end$$
delimiter ;

call SP1('London');
```
## Show those who are Mr. or Mrs. between 2 ids 
```
Drop procedure if exists SP2;
Delimiter $$
create procedure `SP2` (in _idin int, _idfim int, _Titulo varchar(50))
begin
	select * from Employees
	where EmployeeID 
	between _idin and _idfim
	having TitleOfCourtesy = _Titulo;
end$$
delimiter ;

call SP2(1, 5, 'Mr.');
```
# Make functions to show customer with the total and show the average of the employee who sold it
## CUSTOMER AMOUNT FUNCTION
```
drop function if exists montCust;
DELIMITER $$
create function montCust(_CustomerID varchar(50)) returns int
begin
declare _montCust int;
set _montCust = (
	select sum(B.UnitPrice*B.Quantity) from Orders A
	join OrderDetails B using (OrderID)
    where A.CustomerID = _CustomerID
	group by A.CustomerID
);
return _montCust;
end $$
DELIMITER ;
```
## FUNCTION THAT SHOWS EMPLOYEE AMOUNT
```
drop function if exists FunCli;
DELIMITER $$
create function FunCli(_EmployeeID varchar(50)) returns int
begin
declare _FunCli int;
set _FunCli = (
	select sum(C.UnitPrice*C.Quantity) from Orders B
	join OrderDetails C using (OrderID)
    where B.EmployeeID = _EmployeeID
	group by B.EmployeeID
);
return _FunCli;
end $$
DELIMITER ;
```
## PROCEDURE WITH THE FUNCTIONS
```
Drop procedure if exists SP3;
Delimiter $$
create procedure SP3(in _CustomerID varchar(50))
begin
	select A.EmployeeID, D.Lastname, FunCli(A.EmployeeID) as 'TotFuncionário', A.CustomerID, C.CompanyName, 
    montCust(_CustomerID) as TotCliente, (montCust(_CustomerID)/FunCli(A.EmployeeID)*100) as '%'
    from Orders A
    join OrderDetails B using (OrderID)
    join Customers C using (CustomerID)
    join Employees D using (EmployeeID)
    where A.CustomerID = _CustomerID
    group by A.EmployeeID, D.Lastname;
end$$
delimiter ;

call SP3('QUEEN');
```
