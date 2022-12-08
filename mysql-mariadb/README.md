# MySQL / MariaDB

## Onde está o banco?

```
show variables like 'datadir';
```

## Criando um banco

```
CREATE DATABASE exemplo;
```

Conectando ao banco: `USE exemplo`

## Criando tabela

```
CREATE TABLE Cliente(
    nome VARCHAR(40),
    sexo CHAR(1),
    email VARCHAR(40),
    cpf INT(11),
    telefone VARCHAR(30),
    endereco VARCHAR(100)
);
```

Ver as tabelas:

```
SHOW TABLES;
```

+-------------------+
| Tables_in_exemplo |
+-------------------+
| Cliente           |
+-------------------+

### Estrutura de uma tabela

Como são os campos de uma tabela, como foi criada:

```
DESC Cliente;
```

+----------+--------------+------+-----+---------+-------+
| Field    | Type         | Null | Key | Default | Extra |
+----------+--------------+------+-----+---------+-------+
| nome     | varchar(40)  | YES  |     | NULL    |       |
| sexo     | char(1)      | YES  |     | NULL    |       |
| email    | varchar(40)  | YES  |     | NULL    |       |
| cpf      | int(11)      | YES  |     | NULL    |       |
| telefone | varchar(30)  | YES  |     | NULL    |       |
| endereco | varchar(100) | YES  |     | NULL    |       |
+----------+--------------+------+-----+---------+-------+

