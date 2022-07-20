# These exercises are for testing the triggers we created earlier in the triggers.md file
## Select faturas and registos
```
SELECT * FROM faturas;
SELECT * FROM registos;
```
## Insert in the faturas table
```
insert into faturas (id, nome, codigo_postal, saldo, ultima_fatura) 
values ('1', 'Maria', '9500-100', '14020', '12000');

insert into faturas (id, nome, codigo_postal, saldo, ultima_fatura) 
values ('2', 'Pedro', '9540-140', '24920', '16000');
```
## Delete from the faturas table
```
delete from faturas
where id = 2;
```
## Update the faturas table
```
update faturas
set nome = 'Maria'
where nome = 'Pedro';
```
## And if you select the table registos ```SELECT * FROM registos;``` you will see that it is updating everything you have done, from insert, update and delete

