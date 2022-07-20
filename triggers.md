## Create Triggers for table 'faturas'

```
drop trigger  if exists faturas_before_insert;
drop trigger  if exists faturas_after_insert;
drop trigger  if exists faturas_before_update;
drop trigger  if exists faturas_after_update;

delimiter //
create trigger faturas_before_insert
before insert on faturas
for each row
begin
DECLARE mensagem VARCHAR(255);
DECLARE nome_erro VARCHAR(255);
```
### Check if had some id null
```
if new.id is null then
signal sqlstate '45000' set message_text = 'O ID não pode ser nulo.';
end if;
if new.nome is null then
signal sqlstate '45000' set message_text = 'O nome não pode ser nulo.';
end if;
    if new.codigo_postal is null then
signal sqlstate '45000' set message_text = 'O codigo-postal não pode ser nulo.';
end if;
if new.saldo is null then
signal sqlstate '45000' set message_text = 'O saldo não pode ser nulo.';
end if;
    if new.ultima_fatura is null then
signal sqlstate '45000' set message_text = 'A ultima fatura não pode ser nulo.';
end if;
```
### Check if the id already exists
```
if (select count(*) from faturas where id = new.id) > 0 then
set nome_erro = (select id from faturas where id = new.id);
set mensagem = concat('O id do registo ',new.id,' - ', nome_erro,' já existe.');
signal sqlstate '45000' set message_text = mensagem;
end if;
end;
//
```
### INSERT TRIGGER INFORMATION FOR 'REGISTOS'
```
delimiter //
create trigger faturas_after_insert
after insert on faturas
for each row
begin
INSERT INTO registos (DATETIME, Action, id, nome, codigo_postal, saldo, ultima_fatura)
    VALUES (NOW(), 'inserir', new.id, new.nome, new.codigo_postal, new.saldo, new.ultima_fatura);
end;
//
```
### BEFORE UPDATE TRIGGER INFORMATION FOR 'REGISTOS'
```
delimiter //
create trigger faturas_before_update
before update on faturas
for each row
begin
DECLARE mensagem VARCHAR(255);
    DECLARE nome_erro VARCHAR(255);
   
INSERT INTO registos (DATETIME, Action, id, nome, codigo_postal, saldo, ultima_fatura)
    VALUES (NOW(), 'antes', old.id, old.nome, old.codigo_postal, old.saldo, old.ultima_fatura);
   
    if new.id is null then
signal sqlstate '45000' set message_text = 'O ID não pode ser nulo.';
end if;
if new.nome is null then
signal sqlstate '45000' set message_text = 'O nome não pode ser nulo.';
end if;
    if new.codigo_postal is null then
signal sqlstate '45000' set message_text = 'O codigo-postal não pode ser nulo.';
end if;
if new.saldo is null then
signal sqlstate '45000' set message_text = 'O saldo não pode ser nulo.';
end if;
    if new.ultima_fatura is null then
signal sqlstate '45000' set message_text = 'A ultima fatura não pode ser nulo.';
end if;
```
### Check if the id already exists
```
    if new.id <> old.id then
if new.id in (select id from faturas) then
set nome_erro = (select id from faturas where id = new.id);
set mensagem = concat('O ID ',new.id,' - ', nome_erro,' já existe. Criaria um registo duplicado.');
signal sqlstate '45000' set message_text = mensagem;
end if;
    end if;
end;
//
```
### THEN UPDATE TRIGGER INFORMATION TO 'REGISTOS'
```
delimiter //
create trigger faturas_after_update
after update on faturas
for each row
begin
INSERT INTO registos (DATETIME, Action, id, nome, codigo_postal, saldo, ultima_fatura)
    VALUES (NOW(), 'depois', new.id, new.nome, new.codigo_postal, new.saldo, new.ultima_fatura);
end;
//
```
### DELETE TRIGGER INFORMATION FOR 'REGSTOS'
```
delimiter //
create trigger faturas_after_delete
after delete on faturas
for each row
begin
INSERT INTO registos (DATETIME, Action, id, nome, codigo_postal, saldo, ultima_fatura)
    VALUES (NOW(), 'delete', old.id, old.nome, old.codigo_postal, old.saldo, old.ultima_fatura);
end;
//
```
