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

Só existe no MySQL. É um ponteiramento pra um comando SELECT, na verdade ele tá fazendo um comando SELECT.

```
+-------------------+
| Tables_in_exemplo |
+-------------------+
| Cliente           |
+-------------------+
```

### Estrutura de uma tabela

Como são os campos de uma tabela, como foi criada:

```
DESC Cliente;
```

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
```

### Diferença entre CHAR e VARCHAR

Cada caracter vale 1 byte.

Para a palavra "João"

CHAR(10) = 10 bytes
VarCHAR(10) = 4 bytes

CHAR, mais performático, não varia de tamanho, usar quando a coluna nunca for variante.

## Enum

Enum está presente apenas no MySQL, nos outros, é implantada como uma CONSTRAINT.

## Tipos numéricos

### INT

Números inteiros.

CPF, CNPJ, melhor colocar como VARCHAR, pois não vamos calcular nada em cima deles.

### FLOAT

Ponto flutuante => FLOAT(10,2)
10 casas no total, incluindo as duas depois da vírgula.

## Inserindo dados em uma tabela

Um registro na tabela também é chamado de tupla.

```
INSERT INTO Cliente VALUES('João', 'M', null, 1234567890, '(14)3456-1234', 'rua numero zero');
INSERT INTO Cliente VALUES('Célia', 'F', null, 123327890, '(14)3456-0987', 'rua aoaoaoa');
INSERT INTO Cliente VALUES('João', 'M', null, 117890, '(14)3456-4321', 'rua black mesa');
```

Podemos também especificar a coluna, para evitar erros do tipo, uma inserção onde trocou dois campos varchar, e eles vão ser inseridos em colunas erradas.

```
INSERT INTO Cliente(nome, sexo, endereco, telefone, cpf) VALUES('Célia', 'F', 'Rua Marlosa', '(11)1234-1234', 1234523);
```

Forma compacta de inserir:

```
INSERT INTO Cliente VALUES('Leonardo', 'M', null, 1234567890, '(14)3456-1234', 'rua numero zero'),
    ('Felipe', 'M', null, 1234567890, '(14)3456-1234', 'rua numero zero');
```

Essa inserção compacta só funciona no MySQL, não em outro banco.

***Observação importante: Veja que o CPF tem 11 dígitos, neste caso, ele vai dar um OuOfRange, um erro, apesar de ser criado com 11 dígitos, mas foi com INT, então o INT tem um número máximo, vai de -2147483648 a 2147483647. Por isso melhor usar o VARCHAR mesmo com esse tipo de campo.***

## Consultando dados em uma tabela

Comando SELECT. Ele não mostra, não busca, ele projeta.

```
SELECT NOW();
```

```
SELECT 'Leonardo Ruoso Vendramini';
```

### Alias de colunas

```
SELECT NOW() as 'Data e hora';
+---------------------+
| Data e hora         |
+---------------------+
| 2022-12-08 11:03:36 |
+---------------------+
```

### Projetando colunas

```
SELECT nome, sexo, email FROM Cliente;
+----------+------+-------+
| nome     | sexo | email |
+----------+------+-------+
| João     | M    | NULL  |
| Célia    | F    | NULL  |
| João     | M    | NULL  |
| Célia    | F    | NULL  |
| Leonardo | M    | NULL  |
| Felipe   | M    | NULL  |
+----------+------+-------+
```

Com alias:

```
SELECT nome as Cliente, sexo, endereco, NOW() as 'Data e hora' FROM Cliente;
+----------+------+-----------------+---------------------+
| Cliente  | sexo | endereco        | Data e hora         |
+----------+------+-----------------+---------------------+
| João     | M    | rua numero zero | 2022-12-08 11:29:33 |
| Célia    | F    | rua aoaoaoa     | 2022-12-08 11:29:33 |
| João     | M    | rua black mesa  | 2022-12-08 11:29:33 |
| Célia    | F    | Rua Marlosa     | 2022-12-08 11:29:33 |
| Leonardo | M    | rua numero zero | 2022-12-08 11:29:33 |
| Felipe   | M    | rua numero zero | 2022-12-08 11:29:33 |
+----------+------+-----------------+---------------------+
```

**Apenas para fins acadêmicos**

```
SELECT * FROM Cliente;
```

## Filtros

### WHERE

O FROM é parte da projeção, ele filtra apenas colunas, mas traz todos os dados.

O filtro não filtra em nível de coluna, mas em linhas.

```
SELECT nome, sexo FROM Cliente
WHERE sexo = 'M';
+----------+------+
| nome     | sexo |
+----------+------+
| João     | M    |
| João     | M    |
| Leonardo | M    |
| Felipe   | M    |
+----------+------+
```

Seleção é diferente do filtro:

```
SELECT nome, endereco FROM Cliente
WHERE sexo = 'F';
+--------+-------------+
| nome   | endereco    |
+--------+-------------+
| Célia  | rua aoaoaoa |
| Célia  | Rua Marlosa |
+--------+-------------+
```

```
SELECT nome, endereco FROM Cliente
Where endereco LIKE 'Mesa';
Empty set (0.000 sec)
```

Usando o coringa %.

```
SELECT nome, endereco FROM Cliente
Where endereco LIKE '%Mesa';
+-------+----------------+
| nome  | endereco       |
+-------+----------------+
| João  | rua black mesa |
+-------+----------------+
```

Posso utilizar o coringa de outras maneiras, por exemplo pesquisar no meio:

```
SELECT nome, endereco FROM Cliente
Where endereco LIKE '%black%';
+-------+----------------+
| nome  | endereco       |
+-------+----------------+
| João  | rua black mesa |
+-------+----------------+
```

Ou determinando quantos caracteres tem que ter antes ou depois com o underline _. Mas isso não é muito utilizado.

```
SELECT nome, endereco FROM Cliente
Where endereco LIKE '____black%';
+-------+----------------+
| nome  | endereco       |
+-------+----------------+
| João  | rua black mesa |
+-------+----------------+
```

O like não é tão performático mas temos que usar as vezes.

