+++ 
title = "Os comandos SQL essenciais"
date = 2021-05-23T19:58:03-03:00
draft = false
author = "Matheus Almeida Costa"
description = "Entenda os comandos SQL essenciais na prática"
categoria = [
    "Programação",
]
+++


## Definição

SQL é a sigla para "Standard Query Language", que significa, traduzindo para o português,"Linguagem de Consulta Estruturada". Trata-se de uma linguagem de consulta a banco de dados relacionais.

Com o SQL, você pode executar vários comandos para gerenciar, criar, alterar, consultar informações no banco de dados.

Costumamos dizer que bancos SQL seguem uma modelagem relacional, pois estes se baseiam no fato de que todos seus dados sejam armazenados em tabelas que podem se relacionar entre si.

A linguagem SQL é utilizada de maneira relativamente parecida entre os principais bancos de dados relacionais do mercado: MySQL, MariaDB, PostgreSQL, Microsoft SQL Server, Oracle, SQLite, entre muitos outros.

## Comandos no terminal

Mostrar os bancos de dados:

`show databases;`

Entrar no banco de dados:

`use name_database;`

Mostrar tabelas do banco de dado:

`show tables;`

Visualizar estrutura da tabela:

`describe name_table;`

Criar uma cópia do banco de dados (dump):

`mysqldump -u root -p database_name > database_name.sql;`

Criar uma cópia somente da estrutura da base de dados:

`mysqldump -u root -p database_name --no-data > database_name.sql;`

Criar uma cópia de todos os bancos de de dados:

`mysqldump -u user -p --all-databases > full_path_to\file.sql`

Importar banco de dados:

`mysql -u root -p database_name < database_name.sql;`

# Atributos e tipos de dados

- `CHAR` Caracteres string de tamanho fixo

- `VARCHAR` Caracteres string de tamanha variável

- `TEXT`  Armazena texto (não deve ser do tipo NOT NULL).

- `INT` Números inteiros.

- `FLOAT`, `DOUBLE` Números fracionários (ponto flutuante).

- `DATE` Data, por padrão  é ano-mês-dia.

- `AUTO_INCREMENT` Indica que valor do campo será incrementado automaticamente a cada nova linha ou a partir de um número `CREATE TABLE contatos AUTO_INCREMENT=100;` .

- `ENUM` Deve escolher um dos valores, se escolher um valor que não existe no ENUM gera um erro, usa muito em select.

### Constraint (restrições)

- `PRIMARY KEY` (PK)

A primary key identifica de forma única cada registro em uma tabela do banco de dados.

1. deve conter valor único, ou seja, não pode repetir.
2. na coluna de chave primaria não pode haver valor nulo.
3. só 1 primary key é permitido, não pode ter outra primary key.

Existem 2 tipos de primary key:

1. Surrogate Key ⇒ Valor sintético criado apenas para identificar uma linha → não tem relação com o valor em si, é só um identificador mesmo.
2. Natural key ⇒ Identificador único, como por exemplo um CPF, porém é complexo para mudar depois.

- `FOREIGN KEY` (FK) Chave que referência outra coluna em outra tabela (geralmente a primary key da outra tabela)

- `UNIQUE KEY`  Chave única, não pode repetir os valores.

- `NOT NULL`  Indica que o atributo não pode ser nulo.

# **DDL - Linguagem de Definição de Dados**

> Data Definition Language

Permite a criação e alteração de dados. Exemplos de comandos:

Objetos são tabelas, index, funções, views, store procedure e triggers.

- `CREATE`  É utilizado para criar banco de dados ou objetos.
- `ALTER`  É utilizado para deletar objetos de um banco de dados.
- `DROP`  É utilizado para alterar a estrutura de uma banco de dados.
- `TRUNCATE` É utilizado para remover todos os registros de uma tabela, incluindo todos os espaços alocados para os registros serem removidos, nenhuma TRIGGER é disparada.

## **CREATE**

O comando `CREATE` é utilizado para criar tabelas e bases de dados.

Banco de dados:

`CREATE DATABASE nome_database`

Tabela:

`CREATE TABLE nome_tabela`

Podemos complementar o nosso código com a sintaxe opcional `IF NOT EXISTS`, que permite ao MySQL verificar se o nome escolhido esteja sendo utilizando no servidor.
Banco de dados:

`CREATE DATABASE IF NOT EXISTS nome_tabela;`

Tabela:

`CREATE [TEMPORARY] TABLE [IF NOT EXISTS] tbl_name`

Criaremos então uma tabela chamada `users` com algumas colunas.

```
--@block
CREATE TABLE users (
	id INT NOT NULL AUTO_INCREMENT,
	email VARCHAR(255) NOT NULL UNIQUE,
	country CHAR(2),
  PRIMARY KEY (id)
);
```

Podemos utilizar o CREATE TABLE IF NOT EXISTS, no primeiro bloco não irá criar pois já existe uma tabela com esse nome, já no segundo bloco irá criar uma nova tabela.

```
--@block
CREATE TABLE IF NOT EXISTS users (
	email VARCHAR(255) NOT NULL UNIQUE
);
--@block
CREATE TABLE IF NOT EXISTS users_test (
	email VARCHAR(255) NOT NULL UNIQUE
);
```

## DROP

Use `DROP` para excluir tabelas e bases de dados.

Excluir banco de dados:

`DROP DATABASE nome_database;`

Excluir tabela:

`DROP TABLE nome_tabela;`

```
--@block
DROP TABLE users_test;
```

## ALTER

Comando `ALTER` é utilizado para modificar a estrutura da tabela.

### ADD

Neste exemplo iremos adicionar duas colunas, uma que cria a data e hora atual automaticamente quando adiciona novos dados e outra que faz a mesma coisa, porém atualiza-se quando se altera um dado.

```
--@block
ALTER TABLE users ADD created_at DATETIME DEFAULT CURRENT_TIMESTAMP;
ALTER TABLE users ADD updated_at DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP;
```

Observação: Caso não gere a data atual, na consulta troque o `CURRENT_TIMESTAMP` por `NOW()`

- CHANGE, MODIFY, RENAME

    ### **CHANGE**

    Ele tem mais capacidade do que o `MODIFY`, pois permite a alteração do nome da coluna. Ele é mais utilizado, quando há algum erro no nome da coluna e na suas definições.

    Pode ser utilizado para renomear uma coluna e alterar suas definições, como o tipo de dados de uma coluna, por exemplo:

    ```
    CREATE TABLE clientes(
        nome int,
        id int,
        endereco int
    );

    ALTER TABLE clientes CHANGE COLUMN nome nome_cliente VARCHAR(50);
    ```


    Permite utilizar o `FIRST` e o `AFTER` para reordenar as colunas, por exemplo:

    ```
    ALTER TABLE clientes CHANGE COLUMN id id_cliente INT FIRST;

    ALTER TABLE clientes CHANGE COLUMN id id_cli INT AFTER endereco;
    ```

    ### **MODIFY**

    É mais utilizado quando quer alterar somente as definições da coluna.

    Pode ser utilizado para alterar as definições de uma coluna, mas não o seu nome, por exemplo:

    ```
    CREATE TABLE clientes(
        nome int,
        id int,
        endereco int
    );

    ALTER TABLE clientes MODIFY COLUMN nome VARCHAR(50);
    ```

    Permite utilizar o `FIRST` e o `AFTER` para reordenar as colunas, por exemplo:

    ```
    ALTER TABLE clientes CHANGE COLUMN id id_cliente INT FIRST;

    ALTER TABLE clientes CHANGE COLUMN id id_cli INT AFTER endereco;
    ```

    ### RENAME

    SQL RENAME TABLE é uma sintaxe utilizada para renomear o nome de uma tabela.

    ```
    ALTER TABLE table_name   
    RENAME TO new_table_name;
    ```

    Ou apenas:

    ```
    RENAME old_table_name TO new_table_name
    ```

    Alguns bancos de dados renomear o nome da coluna também

    ```
    ALTER TABLE table_name
    RENAME COLUMN old_column_name TO new_column_name;
    ```

# **DML - Linguagem de Manipulação de Dados**

> Data Manipulation Language

São comandos que permitem a manipulação de registros numa tabela do banco de dados, como exclusão, inclusão e alterações. 

- `INSERT` Permite adicionar dados em uma tabela.
- `UPDATE` Permite atualizar dados existentes em uma tabela.
- `DELETE` Permite apagar dados de uma tabela.

## **INSERT**

Na linguagem SQL, podemos utilizar o comando INSERT para inserir dados em uma tabela.

`INSERT INTO nome_tabela (campos) VALUES (valores)`

Observação:  Números não precisam de aspas.

### Única linha

```
--@block
INSERT INTO users (email, country)
VALUES ('hello1@email.com', 'US');
```

### Múltiplas linhas

```
--@block
INSERT INTO users (email, country)
VALUES 
	('hello2@email.com', 'BR'),
	('hello3@email.com', 'AR'),
	('hello4@email.com', 'FR'),
  ('hello5@email.com', 'BR'),
  ('hello6@email.com', 'US');
```

## UPDATE

O comando UPDATE atualiza dados de uma tabela.

`UPDATE nome_tabela SET nome_coluna = "valor" WHERE nome_coluna = "valor"`

```
--@block
UPDATE users SET country = 'BR' WHERE id = 3;
```

## **DELETE**

Comando para deletar dados em tabelas.

`DELETE FROM nome_tabela WHERE nome_coluna = "valor"`

```
--@block
DELETE FROM users WHERE id = 4;
```

# **DQL - Linguagem de Consulta de Dados**

> Data Query Language

O Objetivo do DQL é retornar dados da tabela do banco de dados com base nas consultas

O comando `SELECT` é utilizado para ler dados de tabelas

As buscas podem ser melhoradas com cláusulas:

`SELECT * FROM nome_tabela`

Neste caso realiza uma busca por todos os dados  `*`  da `FROM` tabela chamada u`sers`.

Você poderia especificar colunas que deseja fazer a seleção, alterando o `*` pelo nome da coluna

`SELECT nome_coluna FROM nome_tabela`

Ainda há outras cláusulas:

- `WHERE` Indica as condições
- `GROUP BY` Realiza agrupamentos
- `ORDER BY` Ordena os dados de maneira decrescente `DESC` ou crescente `ASC`

Ainda podemos combinar buscas com operadores lógicos.

- `AND` Avalia se duas condições são verdadeiras
- `OR` Avalia se uma condição é verdadeira
- `NOT` Negação

Operadores relacionais permitem fazer comparações nas consultas:

- `<` Menor
- `>` Maior
- `<=`  Menor ou igual
- `>=` Maior ou igual
- `=` Igual
- `<>` Diferente

No final da consulta podemos limitar a quantidade de resultados

- `LIMIT` Limitar resultados

## **SELECT**

Na linguagem SQL, podemos utilizar o comando SELECT para ler dados de tabelas.

```
SELECT * FROM users;
```

Obtemos o seguinte resultado:

![sql1](https://almeidamatheus.netlify.app/uploads/21/05/1sql.png)

Nota-se que os registros foram criados com os seus devidos valores, o registro de `id` 1 foi alterado,  o valor da coluna `country` e também o valor da coluna `updated_at` de maneira automática. O registro de `id` igual a 4 foi deletado.

Podemos realizar buscas mais especificas combinando as várias cláusulas existentes:

```
--@block
SELECT id, email, country FROM users
WHERE country = 'BR'
OR id > 2
ORDER BY id ASC
LIMIT 2;

--@block
SELECT id, email, country FROM users
WHERE country = 'US'
AND email LIKE 'hello%'
ORDER BY id DESC
LIMIT 2;--@block
CREATE TABLE cars(
	id INT AUTO_INCREMENT,
	model VARCHAR(255),
    age INT NOT NULL,
	owner_id INT NOT NULL,
	PRIMARY KEY (id),
	FOREIGN KEY (owner_id) REFERENCES users(id)
);
```

Resultado do primeiro bloco:

![sql2](https://almeidamatheus.netlify.app/uploads/21/05/2sql.png)


Resultado do segundo bloco:

![sql3](https://almeidamatheus.netlify.app/uploads/21/05/3sql.png)


# JOINs

Para utilizarmos o join devemos primeiramente criar uma nova tabela com uma `FOREIGN KEY`  que faça referência a `PRIMARY KEY` da outra tabela.

```
--@block
CREATE TABLE cars(
	id INT AUTO_INCREMENT,
	model VARCHAR(255),
  price DOUBLE NOT NULL,
	owner_id INT NOT NULL,
	PRIMARY KEY (id),
	FOREIGN KEY (owner_id) REFERENCES users(id)
);
```

E então iremos adicionar dados nesta tabela.

```
--@block
INSERT INTO cars (owner_id, model, price)
VALUES
	(1, 'fiat uno', 12000.78),
	(1, 'ford escort', 15000),
	(2, 'honda civic', 17092.54),
	(3, 'audi a3', 25000);
```

Vale ressaltar que neste exemplo nem todos os usuários estão ligados a um carro mas um carro está sempre ligada a um usuário, ou seja, é uma relação de 1 para muitos.

![sql4](https://almeidamatheus.netlify.app/uploads/21/05/4sql.png)


## INNER JOIN

![sql5](https://almeidamatheus.netlify.app/uploads/21/05/5sql.png)

Basicamente irá mostrar os registros que tem valores em comuns, no caso do exemplo é a `PRIMARY KEY` do `users` com a `FOREIGN KEY` do `cars`

```
--@block
SELECT * FROM users
INNER JOIN cars
ON cars.owner_id = users.id;
```

Obtendo assim o seguinte resultado:

![sql6](https://almeidamatheus.netlify.app/uploads/21/05/6sql.png)

Vale a pena ressaltar que caso não queremos obter valores repetidos por algum motivo, por exemplo o `email`, deveríamos então no final da consulta adicionar `GROUP BY email;`

Também é possível utilizar junto com as outras clausulas da DQL, aliado a isso eu utilizei o `AS`, que cria uma espécie de apelido temporário para uma tabela, neste exemplo renomeei `users` para `u` e `cars` para `c`

```
--@block
SELECT
  u.id,
  c.owner_id,
	u.email,
	c.model
FROM users AS u
INNER JOIN cars as c ON c.owner_id = u.id
WHERE u.created_at > '2021-05-21';
```

Obtendo assim o seguinte resultado:

![sql7](https://almeidamatheus.netlify.app/uploads/21/05/7sql.png)

## LEFT JOIN

![sql8](https://almeidamatheus.netlify.app/uploads/21/05/8sql.png)

O `LEFT JOIN` seleciona todos os registros da tabela a esquerda `users` e os registros em comuns das duas tabelas.

```
--@block
SELECT * FROM users
LEFT JOIN cars
ON cars.owner_id = users.id;
```

Portanto irá retornar todos os registros da esquerda e os que tem relação com a tabela da direita.

![sql9](https://almeidamatheus.netlify.app/uploads/21/05/9sql.png)

## RIGHT JOIN

![sql10](https://almeidamatheus.netlify.app/uploads/21/05/10sql.png)

O `RIGHT JOIN` seleciona todos os registros da tabela a direita `cars` e os registros em comuns das duas tabelas.

```
--@block
SELECT * FROM users
RIGHT JOIN cars
ON cars.owner_id = users.id;
```

Neste caso irá retornar apenas as linhas que tem valores comuns, já que a tabela `cars` não tem dados que não seja vinculado a tabela `users`, porque um dos valores inseridos nessa tabela é o `owner_id`, que referência o id de `users` e tem o atributo `UNIQUE KEY`. A final toda casa tem um dono.

Portanto o resultado é semelhante ao `LEFT JOIN`, a única diferença é que nesta consulta não irá retornar o registro de `id` igual a 5 e 6.

![sql11](https://almeidamatheus.netlify.app/uploads/21/05/11sql.png)

## FULL OUTER JOIN

![sql12](https://almeidamatheus.netlify.app/uploads/21/05/12sql.png)

```
--@block
SELECT * FROM users
FULL OUTER JOIN cars
ON cars.owner_id = users.id;
```

Vale ressaltar que alguns bancos de dados como o MySQL não existe o comando `FULL OUTER JOIN`, nesse caso devemos utilizar a união  `UNION` do `LEFT JOIN` e `RIGHT JOIN` 

```
--@block
SELECT * FROM users
LEFT JOIN cars
ON cars.owner_id = users.id
UNION
SELECT * FROM users
RIGHT JOIN cars
ON cars.owner_id = users.id;
```

O resultado desta consulta será o mesmo da seção `LEFT JOIN`.




