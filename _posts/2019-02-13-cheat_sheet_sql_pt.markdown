---
layout: post
title: Consulta Rápida - SQL
date: 2019-02-13 09:20:00
comments: true
author: Talita Shiguemoto
lang: pt
ref: csSQL
---

* [SQL Básico](#basics)
	* [SELECT & FROM](#selectfrom)
	* [DISTINC](#distinc)
	* [LIMIT](#limit)
	* [ORDER BY](#orderby)
	* [WHERE](#where)
	* [Operadores Aritméticos:](#arithmetic)
	* [LIKE](#like)
	* [IN](#in)
	* [NOT](#not)
	* [AND & BETWEEN](#andbetween)
	* [OR](#or)
* [SQL Joins](#joins)
	* [JOIN duas tabelas](#twotables)
	* [JOIN mais que duas tabelas](moretwotables)
	* [Alias](#alias)
	* [INNER](#inner)
	* [LEFT](#left)
	* [RIGHT](#right)
	* [UNION](#union)
	* [Filtrando](#filtering)
* [SQL Agregação](#aggregations)
	* [NULL](#null)
	* [COUNT](#count)
	* [SUM](#sum)
	* [AVG](#avg)
	* [MIN](#min)
	* [MAX](#max)
	* [GROUP BY](#groupby)
	* [HAVING](#having)
	* [DATE_TRUNC](#datetrunc)
	* [DATE_PART](#datepart)
	* [CASE](#case)
* [SQL Subqueries](#subqueries)
	* [Básico](#basic)
	* [Subqueries com JOIN](#subjoin)
	* [WITH](#with)
* [SQL Limpeza de dados](#dataclean)
	* [LOWER](#lower)
	* [UPPER](#upper)
	* [LEFT](#left)
	* [RIGHT](#right)
	* [LENGTH](#length)
	* [POSITION & STRPOS](#positionstrpos)
	* [CONCAT](#concat)
* [SQL Função Window](#windowfunction)
	* [OVER](#over)
	* [PARTITION BY](#partitionby)
	* [ROW_NUMBER() & RANK()](#rowandrank)
	* [WINDOWN](#window)
	* [LAG & LEAD](#lagandlead)
	* [NTILE](#ntile)

## **SQL Básico** {#basics}
<br/>
Aqui estão alguns códigos para interagir com tabelas, a sintaxe básica do SQL.

### **SELECT & FROM** {#selectfrom}
<br/>

**SELECT** indica qual(is) coluna(s) você quer selecionar os dados.


**FROM** especifica de qual(is) tabela(s) você deseja selecionar as colunas.

`SELECT column1, column2, column3
FROM table_example;`

Se você quiser selecionar todas as colunas na tabela, use \*

`SELECT * FROM table_example;`

### **DISTINC** {#distinc}
<br/>

DISTINCT fornece as linhas exclusivas para todas as colunas escritas na instrução SELECT.

DISTINCT vem depois da instrução SELECT.

Exemplo: Para retornar as linhas exclusivas em todas as três colunas.

`SELECT DISTINCT column1, column2, column3
FROM table_example;`

### **LIMIT** {#limit}
<br/>

A instrução LIMIT é útil quando você deseja ver apenas as primeiras linhas de uma tabela.

O comando LIMIT é sempre a última parte de uma consulta.

Exemplo: Mostre as primeiras 15 linhas de *table_example* das colunas: *column1, column2, column3*

`SELECT column1, column2, column3
FROM table_example
LIMIT 15;`

### **ORDER BY** {#orderby}
<br/>

A instrução ORDER BY classifica nossa **saída** usando os dados em qualquer coluna.

A instrução ORDER BY vem depois das instruções SELECT e FROM, mas antes da instrução LIMIT.

`SELECT column1, column2, column3
FROM table_example
ORDER BY column1  
LIMIT 15;`

Por padrão, ORDER BY classifica em ordem crescente, mas você pode usar DESC após a coluna em sua instrução ORDER BY para classificar em ordem decrescente

Exemplo: primeiro em ASC e segundo em DESC

`SELECT column1, column2, column3
FROM table_example
ORDER BY column1, column2 DESC;`

Exemplo: primeiro no DESC e segundo no ASC

`SELECT column1, column2, column3
FROM table_example
ORDER BY column2 DESC, column3;`


### **WHERE** {#where}
<br/>

A instrução WHERE exibe subconjuntos de tabelas com base nas condições, é como filtrar os dados.

Símbolos comuns usados nas declarações WHERE:
* > (maior)
* < (meno)
* >= (maior ou igual)
* <= (menor ou igual)
* = (igual)
* != (diferente)

`SELECT column1, column2, column3
FROM table_example
WHERE column1 >= 1000;`

A instrução WHERE BY vem após as instruções SELECT e FROM, mas antes da instrução ORDER BY.

`SELECT column1, column2, column3
FROM table_example
WHERE column1 <= 1000
ORDER BY column2 DESC
LIMIT 10;`

A instrução WHERE também pode ser usada com dados categóricos, usando operadores = ou! =. É necessário usar aspas simples com os dados de texto.

`SELECT column1, name,
FROM table_example
WHERE name = 'Darth Bane';`

### **Operadores Aritméticos:** {#arithmetic}
<br/>

É possível criar uma nova coluna temporária resultante de uma operação aritmética, como adicionar duas colunas. Para dar um nome a esta coluna, é necessário usar a palavra-chave `AS`.

Operadores aritméticos:
* * (Multiplicação)
* + (Adição)
* - (Subtração)
* / (Divisão)

Exemplo: A query abaixo mostra *column1* e uma nova coluna *sum_columns*, este segundo é um resultado de `column2 + column3`

`SELECT column1, column2 + column3 AS sum_columns
FROM table_example;`

Ordem de operações

A ordem das operações é a mesma da matemática, às vezes você tem que usar parênteses () para escolher a ordem


### **LIKE** {#like}
<br/>

Quando você está usando WHERE e = e você não tem exatamente uma palavra ou frase para procurar, você pode usar `LIKE` para ajudá-lo.

O operador LIKE é sensível a maiúsculas e minúsculas e você tem que usar alguns operadores na pesquisa.

Exemplo: Encontrar todas as linhas que contêm valores que começam com `x` na coluna *name*

`SELECT name
FROM table_example
WHERE name LIKE 'x%';`

Operadores LIKE:	
* 'x%' : Encontra todos os valores que começam com "x"
* '%x' : Encontra todos os valores que terminam com "x"
* '%xyz%' : Encontra quaisquer valores que tenham "xyz" em qualquer posição
* '\_x%' : Encontra quaisquer valores que tenham "x" na segunda posição
* 'x\_%\_%' : Encontra todos os valores que começam com "x" e têm pelo menos 3 caracteres de comprimento
* 'x%z' : Encontra todos os valores que começam com "x" e terminam com "z"
* '\_2%4' : Encontra quaisquer valores que tenham um 2 na segunda posição e terminem com um 4
* '2\_\_\_4' : Encontra todos os valores com um número de cinco dígitos que começam com 2 e terminam com 4

### **IN** {#in}
<br/>

É semelhante a WHERE e =, mas por mais de uma condição.

Exemplo: Encontra todas as linhas com *Darth Bane* ou *Darth Maul* no *name*

`SELECT name
FROM table_example
WHERE name IN ('Darth Bane', 'Darth Maul');`


### **NOT** {#not}
<br/>

É usado antes de `IN` ou` LIKE` para selecionar todas as linhas inversas.

Exemplo: Encontra todas as linhas sem *Darth Bane* ou *Darth Maul* no *name* 

`SELECT name
FROM table_example
WHERE name NOT IN ('Darth Bane', 'Darth Maul');`

Exemplo: Encontra todas as linhas que não contêm valores que começam com `x` no *name*

`SELECT name
FROM table_example
WHERE name NOT LIKE 'x%';`

### **AND & BETWEEN** {#andbetween}
<br/>

Ambos os operadores são usados em uma instrução WHERE para combinar operações em que todas as condições juntas devem ser verdadeiras.

O operador AND para considerar duas ou mais cláusulas lógicas por vez. Você pode vincular tantas declarações quanto você quiser ao mesmo tempo.

`SELECT column1, column2, column3
FROM table_example
WHERE column1 > 200 AND column2 = 20 AND column3 = 10;`

Este operador trabalha com todas as operações que vimos até agora, incluindo operadores aritméticos (`+`, `*`, `-`,` / `). As lógicas LIKE, IN e NOT também podem ser vinculadas usando o operador AND.

`SELECT name
FROM table_example
WHERE name NOT LIKE 'X%' AND column4 LIKE '%z';`

O operador BETWEEN trabalha com o operador AND, usando a mesma coluna para diferentes partes da nossa declaração AND.

Em vez de escrever:
`WHERE column1 >= 6 AND column1 <= 10`

Em vez disso, podemos escrever:
`WHERE column1 BETWEEN 6 AND 10`

Exemplos:

`SELECT column1, column2, column3
FROM table_example
WHERE column1 BETWEEN 50 AND 90;`

`SELECT name, date_time
FROM table_example
WHERE name NOT IN ('Darth Bane', 'Darth Maul') AND date_time BETWEEN '2018-01-01' AND '2019-01-01'
ORDER BY date_time DESC;`


### **OR** {#or}
<br/>

O operador OR pode combinar várias instruções em uma instrução WHERE, muito semelhante ao operador AND, mas a diferença é que pelo menos uma das condições combinadas deve ser verdadeira. Este operador trabalha com todas as operações que vimos até agora.

Exemplos:

`SELECT column1, column2
FROM table_example
WHERE column1 > 100 OR column2 < 200;`

`SELECT name, date_time, column6
FROM table_example
WHERE (name LIKE 'Darth%' OR name LIKE 'Master%') 
AND (column6 LIKE %xxx% OR column6 LIKE %Xxx%)
AND (date_time BETWEEN '2018-01-01' AND '2019-01-01');`

## **SQL Joins** {#joins}
<br/>


### **JOIN duas tabelas** {#twotables}
<br/>

A instrução JOIN nos permite extrair dados de mais de uma tabela por vez.
É usado com a instrução ON que contém as duas colunas que são ligadas entre as duas tabelas.

Para os próximos exemplos, usaremos o ERD abaixo:

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/notebook/post005_erd.png" alt="" title="Exemplo ERD"/>
</div>
<div class="col three caption">
	Exemplo ERD
</div>
<br/>

Exemplo: Para JOIN o *movie* com outra tabela chamada *director* , obtendo todas as colunas, você terá que usar a seguinte consulta. Em ON são as colunas com valores únicos em ambas as tabelas, a chave.

`SELECT *
FROM movie
JOIN director
ON movie.director_id = director.id;`

Se você deseja obter apenas algumas colunas, você deve incluir na sua instrução SELECT todas as colunas necessárias, escrevendo o nome da tabela antes do ponto final e a coluna do nome (`movie.director id`).

Exemplo:

`SELECT movie.title, movie.date_time,
director.id, director.age
FROM movie
JOIN director
ON movie.director_id = director.id;`

Exemplo: obtendo todas as informações somente da tabela *movie* :

`SELECT movie.*
FROM movie
JOIN director
ON movie.director_id = director.id;`


**Chave primária (PK)**

Uma chave primária é uma coluna única em uma tabela, é comum que você seja a primeira coluna em nossas tabelas.

**Chave estrangeira (FK)**

Uma chave estrangeira é uma coluna em uma tabela que é uma chave primária em uma tabela diferente.

**Link entre chave primária e estrangeira**

É um link que conecta duas tabelas. O *director_id* é a chave estrangeira e está vinculado ao *id*, esse é o principal link de chave estrangeira que conecta essas duas tabelas.
Então, depois que a instrução FROM é uma tabela e após o JOIN é outra tabela, então depois do ON é sempre PK = FK (ou FK = PK).

### **JOIN mais de duas tabelas** {#moretwotables}
<br/>

Essa mesma lógica pode unir mais de duas tabelas juntas. Veja as três tabelas abaixo.

Se quiséssemos juntar todas essas três tabelas, poderíamos usar a mesma lógica. O código abaixo puxa todos os dados de todas as tabelas unidas.

`SELECT movie.title, movie.date_time,
character.name, actor.name
FROM movie
JOIN character
ON movie.id = character.movie_id
JOIN actor
ON character.actor_id = actor.id;`

Novamente, nosso JOIN possui uma tabela, e ON é um link para que nosso PK seja igual ao FK.

### **Alias** {#alias}
<br/>

É comum dar um alias para cada tabela, normalmente com a primeira letra do nome da tabela.

Exemplo:

`FROM movie AS m
JOIN character AS c`

É possível dar o alias sem usar o `AS`, apenas colocando o nome após um espaço em branco, exemplo:

`FROM movie m
JOIN character c`


**Aliases para Colunas na Tabela Resultante** 

Também é possível fornecer alias para as colunas na tabela resultante

Exemplo:

`Select c.name character_name, a.name actor_name
FROM character c
JOIN actor a`

O resultado vai ser assim:

| character_name | actor_name | 
|--:|--:|
| Leia Organa | Carrie Fisher | 
| Luke Skywalker | Mark Hamill | 

### **INNER Joins** {#inner}
<br/>

O INNER JOIN é o mesmo quando estamos usando JOIN, o resultado é os valores correspondentes em ambas as tabelas.

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/notebook/post005_inner.png" alt="" title="INNER JOIN"/>
</div>
<div class="col three caption">
	INNER JOIN
</div>
<br/>

A sintaxe para o INNER é:

`SELECT movie.title, director.name
FROM movie
INNER JOIN director
ON movie.director_id = director.id;`

## **OUTER Joins**
<br/>

OUTER JOIN podem retornar a junção interna mais quaisquer linhas não correspondidas da primeira tabela ou da segunda tabela, ou ambas, depende de como você usará a sintaxe.

### **LEFT Joins** {#left}
<br/>

LEFT JOIN obtém todos os dados existentes nas duas tabelas, bem como todas as linhas da tabela no FROM.

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/notebook/post005_left.png" alt="" title="INNER JOIN"/>
</div>
<div class="col three caption">
	LEFT JOIN
</div>
<br/>

Exemplo:

`SELECT movie.title, director.name
FROM movie
LEFT JOIN director
ON movie.director_id = director.id;`

### **RIGHT Joins** {#right}
<br/>

RIGHT JOIN obtém todos os dados que existem em ambas as tabelas, assim como todas as linhas da tabela no JOIN.

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/notebook/post005_right.png" alt="" title="INNER JOIN"/>
</div>
<div class="col three caption">
	RIGHT JOIN
</div>
<br/>

Exemplo:

`SELECT movie.title, director.name
FROM movie
RIGHT JOIN director
ON movie.director_id = director.id;`

### **UNION** {#union}
<br/>

UNION anexa duas tabelas, anexando apenas valores distintos. Ambas as tabelas devem ter o mesmo número de colunas e ter os mesmos tipos de dados na mesma ordem da primeira tabela. Os nomes das colunas não precisam ser os mesmos.

Example:
```SQL
SELECT * 
FROM movies1

UNION

SELECT * 
FROM movies2
```

UNION ALL é semelhante a UNION, mas inclui valores repetidos.

Example:
```SQL
SELECT * 
FROM movies1

UNION ALL

SELECT * 
FROM movies2
```

É possível usar condicional com UNION.

Example:
```SQL
SELECT * 
FROM movies1
WHERE budget < 250000000

UNION

SELECT * 
FROM movies2
```

### **Filtrando** {#filtering}
<br/>

É possível filtrar os dados usando condicionais como `WHERE` quando as duas tabelas já tiverem sido unidas. No entanto, também é possível filtrar uma ou ambas as tabelas antes de uni-las usando `AND` na instrução` ON`.

Exemplo: Crie correspondências entre as tabelas apenas para o diretor com mais de 50 anos. O filtro ocorre antes do JOIN.

`SELECT movie.title, director.name
FROM movie
LEFT JOIN director
ON movie.director_id = director.id
AND director.age > 50
ORDER BY director.age;`

Exemplo: Crie correspondências entre as tabelas apenas para o diretor com mais de 50 anos. O filtro ocorre depois do JOIN.

`SELECT movie.title, director.name
FROM movie
LEFT JOIN director
ON movie.director_id = director.id
WHERE director.age > 50
ORDER BY director.age;`


## **SQL Agregação** {#aggregations}
<br/>

### **NULL** {#null}
<br/>

NULLs são dados faltantes (*misssing data*), um tipo de dados que especifica onde não existem dados, é diferente de 0 ou espaço em branco.

Para verificar quantos NULLs estão em sua coluna, você escreve para usar `IS NULL` com` WHERE`

Exemplo:

`SELECT movie.title, movie.budget
FROM movie
WHERE movie.date_time IS NULL`

Para verificar quantos não NULLs estão em sua coluna, você escreve para usar `IS NOT NULL` com` WHERE`

Exemplo:

`SELECT movie.title, movie.budget
FROM movie
WHERE movie.date_time IS NOT NULL`

### **COUNT()** {#count}
<br/>

A função COUNT() conta todos os valores não NULL, em outras palavras, em uma coluna com todas as linhas com dados, o COUNT será o número de linhas.

Exemplo:

`SELECT COUNT(*)
FROM movie;`

`SELECT COUNT(date_time)
FROM movie;`

### **SUM()** {#sum}
<br/>

A função SUM() retorna a soma total de uma coluna numérica. Esta função só funciona em colunas numéricas.

`SELECT SUM(budget) AS total_budget
FROM movie; `

Exemplo: é possível usar um agregado e um operador matemático

`SELECT SUM(column1)/SUM(column2) 
FROM table_example; `

### Dica

Agregações podem fazer com que sua query demore a rodar, caso use LIMIT na mesma query, a agregação será executada antes e então ele irá limitar o número de linhas. Caso queira usar o LIMIT antes da agregação é necessário usar uma subquery {#subqueries} com LIMIT e depois fazer uma OUTER query com agregação.

Exemplo:

`SELECT title, 
	SUM(budget) AS total_budget
FROM ( SELECT * FROM movie LIMIT 100) sub
GROUP BY 1; `

### **AVG()** {#avg}
<br/>

A função AVG() retorna o valor médio de uma coluna numérica. Esta função só funciona em colunas numéricas.

Exemplo:

`SELECT AVG(budget) AS average_budget
FROM movie; `

### **MIN()** {#min}
<br/>

A função MIN() retorna o mínimo da coluna. Esta função trabalha com diferentes tipos de colunas, com o tempo retornará o tempo mais antigo, com o numérico retornará o valor mais baixo e com a coluna não numérica retornará o primeiro na ordem alfabética.

Exemplo:

`SELECT MIN(budget) AS min_budget
FROM movie; `


### **MAX()** {#max}
<br/>

A função MAX() retorna o máximo da coluna. Esta função trabalha com diferentes tipos de colunas como a mesma função MIN().

Exemplo:

`SELECT MAX(budget) AS min_budget
FROM movie; `

### **GROUP BY** {#groupby}
<br/>

O GROUP BY agrega subconjuntos dos dados. Se você quiser somar o *budget* por *director_id*, será necessário agrupá-los.

Exemplo:

`SELECT SUM(budget) AS sum_budget
FROM movie
GROUP BY director_id; `

Todas as colunas na instrução SELECT sem um agregador (como exemplo: `SUM`,` AVG`, `COUNT`) devem estar na cláusula GROUP BY.

Exemplo:

`SELECT director_id, SUM(budget) AS sum_budget
FROM movie
GROUP BY director_id; `

O GROUP BY sempre vai entre WHERE e ORDER BY.

É possível agrupar várias colunas de uma só vez, a ordem dos nomes das colunas aqui não importa.

Se você quiser classificar os resultados, é possível usar ORDER BY e as ordens de nomes de colunas aqui fazem diferença, a ordem é da esquerda para a direita

Exemplo:

`SELECT SUM(budget) AS sum_budget
FROM movie
GROUP BY director_id, title
ORDER BY title, director_id ; `

### **HAVING** {#having}
<br/>

HAVING é usado da mesma forma que WHERE, mas quando a consulta foi agregada. Sempre que você quiser executar um WHERE em um elemento de sua consulta criado por um agregado, será necessário usar HAVING.

Ter sempre vai entre GROUP BY e ORDER BY.

Exemplo:

`SELECT director_id, SUM(budget) AS sum_budget
FROM movie
GROUP BY director_id
HAVING budget > 3000; `

### **DATE_TRUNC** {#datetrunc}
<br/>

DATE_TRUNC() permite que você trunque sua data para uma parte específica de sua coluna de data e hora. É usado com dois parâmetros: `DATE_TRUNC ('time unit', nome da coluna)`

Algumas unidades de tempo, veja mais [aqui](https://mode.com/blog/date-trunc-sql-timestamp-function-count-on):
* hour (hora)
* day (dia)
* week (semana)
* month (mês)
* quarter (trimestre)
* year (ano)

É necessário colocar a função depois do SELECT e depois do GROUP BY também.

Exemplo:

`SELECT DATE_TRUNC('year', date_time) year,  SUM(budget) total_budget
FROM movie
GROUP BY DATE_ TRUNC ('year', date_time)
ORDER BY total_budget DESC;`

Você pode referenciar as colunas em sua instrução select nas cláusulas GROUP BY e ORDER BY com números que seguem a ordem em que aparecem na instrução select.

Exemplo:

`SELECT DATE_TRUNC('year', date_time) year,  SUM(budget) total_budget
FROM movie
GROUP BY 1
ORDER BY 2 DESC;`

GROUP BY 1: este 1 refere-se ao ano a partir da data, uma vez que é a primeira das colunas incluídas no comando SELECT

ORDER BY 2: este 2 refere-se a total_budget, uma vez que é a segunda das colunas incluídas no comando SELECT

### **DATE_PART** {#datepart}
<br/>

DATE_PART é usado para obter uma parte específica de uma data, mas puxar mês ou dia da semana(dow) significa que você não está mais mantendo os anos em ordem.

Exemplo:

`SELECT DATE_PART('year', date_time) year, SUM(budget) total_budget
FROM movie
GROUP BY 1;`

Algumas unidades de tempo (todas as unidades de DATE_TRUNC também podem ser usadas):
* dow (Dia da semana como domingo (0) a sábado (6))
* isdow (Dia da semana como segunda-feira (1) a domingo (7))
* doy (Dia do ano)

### **CASE** {#case}
<br/>

A instrução CASE é uma lista de condicionais que retorna os possíveis resultados das condicionais.

Por exemplo, se você tiver as colunas *house*, *apples* e *dogs* e quiser dividir o número de *appleS* por *dogs* GROUP BY *house*, escreva a seguinte query:

`SELECT house, SUM(apples)/SUM(dogs) AS apples_per_dogs
FROM table_dogs
GROUP BY house;` 

Se qualquer *house* não tiver um cachorro (dog = 0), você não poderá dividir e aparecerá um erro.

Em vez disso, você pode usar a instrução CASE para resolver esse problema.

`SELECT house, CASE WHEN SUM(dogs) = 0 OR SUM(dogs) IS NULL THEN 0
ELSE SUM(apples)/SUM(dogs) END AS apples_per_dogs
FROM table_dogs
GROUP BY house;` 

Agora, a primeira parte da instrução (`CASE WHEN SUM (dogs) = 0 OU SUM (cachorros) IS NULL THEN 0`) irá escrever 0 para qualquer um desses valores de divisão por zero, que causaram o erro, e a outra parte (`ELSE SUM (apples) / SUM (cachorros) END AS apples_per_dogs`) calculará a divisão normalmente.

A instrução CASE sempre entra na cláusula SELECT.

CASE deve incluir: WHEN, THEN e END. O ELSE é um componente opcional para detectar casos que não atenderam a nenhuma das outras condições anteriores do CASE.

Você pode fazer qualquer instrução condicional usando qualquer operador condicional (WHERE, LIKE, AND, OR) entre WHEN e THEN.

Você pode incluir várias instruções WHEN, bem como uma instrução ELSE novamente, para lidar com qualquer condição não endereçada.

## **SQL Subqueries** {#subqueries}
<br/>

### **Básico** {#basic}
<br/>

A **Subquery** é consulta dentro de outra consulta, isto é, quando você precisar usar tabelas resultantes para consultar novamente.

Exemplo: para obter todos os filmes com orçamento abaixo do orçamento médio de todos os filmes, você precisa criar uma consulta para calcular a média e criar outra consulta para obter todos os resultados abaixo dessa média:

`SELECT * 
FROM movie
WHERE  budget <
(SELECT AVG(budget)
FROM movie);`

É possível fazer mais de uma subconsulta juntos.

Exemplo: Para calcular uma mediana de uma tabela com um número par de linhas, você deve obter as duas linhas no meio, soma e divide-as por duas.

Vamos supor que a table *table_dogs* tenha 10 linhas e você queira saber qual é a mediana das maçãs, você precisa ordenar a coluna *apples* e obter as 6 primeiras linhas

`FROM (SELECT apples
      FROM table_dogs
      ORDER BY apples
      LIMIT 6) `

Então você tem que pegar as últimas duas linhas, ordenando novamente, mas em DESC e LIMIT e 2. Note que você já tem uma subconsulta.

`(SELECT *
FROM (SELECT apples
      FROM table_dogs
      ORDER BY apples
      LIMIT 6) AS sub
ORDER BY apples DESC
LIMIT 2)`

Finalmente, você tem que calcular a média dessas duas linhas, resultando nesta consulta:

`SELECT AVG(apples) FROM 
(SELECT *
FROM (SELECT apples
      FROM table_dogs
      ORDER BY apples
      LIMIT 6) AS sub
ORDER BY apples DESC
LIMIT 2) AS Table2;`

### **Subqueries com JOIN** {#subjoin}
<br/>

É possível usar o JOIN com subconsultas diferentes, por exemplo

Vamos imaginar banco de dados sobre vendas com 5 tabelas, este exemplo é da Udacity, e você pode ver o ERD abaixo:

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/notebook/post005_udacity.png" alt="" title="Udacity - SQL for Data Analysis"/>
</div>
<div class="col three caption">
	Font: Udacity - SQL for Data Analysis
</div>
<br/>

A consulta a seguir fornece o nome do sales_rep em cada região com a maior quantidade de vendas de total_amt_usd.

```SQL
SELECT  t3.region_name, t3.rep_name, t3.total_amt
FROM(SELECT t1.region_name, MAX(total_amt) total_amt
	FROM (SELECT s.name rep_name, r.name region_name, SUM(o.total_amt_usd) total_amt
		FROM sales_reps s
		JOIN accounts a
		ON a.sales_rep_id = s.id
		JOIN orders o
		ON o.account_id = a.id
		JOIN region r
		ON r.id = s.region_id
		GROUP BY 1,2) t1
	GROUP BY 1) t2
JOIN
	(SELECT s.name rep_name, r.name region_name, SUM(o.total_amt_usd) total_amt
	FROM sales_reps s
	JOIN accounts a
	ON a.sales_rep_id = s.id
	JOIN orders o
	ON o.account_id = a.id
	JOIN region r
	ON r.id = s.region_id
	GROUP BY 1,2
	ORDER BY 3 DESC) t3
ON t3.region_name = t2.region_name AND t3.total_amt = t2.total_amt;
```

### **WITH** {#with}
<br/>

A instrução WITH também é chamada de Common Table Expression (CTE). É uma boa prática usar o WITH para o mesmo propósito das subconsultas, tornando-o mais limpo para futuros leitores.

A instrução WITH vem antes da consulta, SELECT

Exemplo: veja a consulta abaixo

`SELECT * 
FROM movie
WHERE  budget <
(SELECT AVG(budget)
FROM movie);`

Para facilitar a leitura, vamos usar WITH, colocando a subconsulta como:

`WITH avg_budget AS (
SELECT AVG(budget)
FROM movie) `

Então você pode escrever a consulta final:

```SQL
WITH avg_budget AS (
SELECT AVG(budget)
FROM movies

SELECT * 
FROM movie
WHERE budget < avg_budget
```

A consulta do exemplo earlist pode ser escrita assim:

```SQL
WITH t1 AS (SELECT s.name rep_name, r.name region_name, SUM(o.total_amt_usd) total_amt
	FROM sales_reps s
	JOIN accounts a
	ON a.sales_rep_id = s.id
	JOIN orders o
	ON o.account_id = a.id
	JOIN region r
	ON r.id = s.region_id
	GROUP BY 1,2),

    t3 AS(SELECT s.name rep_name, r.name region_name, SUM(o.total_amt_usd) total_amt
	FROM sales_reps s
	JOIN accounts a
	ON a.sales_rep_id = s.id
	JOIN orders o
	ON o.account_id = a.id
	JOIN region r
	ON r.id = s.region_id
	GROUP BY 1,2
	ORDER BY 3 DESC),

	t2 AS (SELECT t1.region_name, MAX(total_amt) total_amt
	FROM  t1
	GROUP BY 1)

SELECT  t3.region_name, t3.rep_name, t3.total_amt
FROM t2
JOIN t3
ON t3.region_name = t2.region_name AND t3.total_amt = t2.total_amt;
```

## **SQL Limpeza de dados** {#dataclean}
<br/>


### **LOWER** {#lower}
<br/>

LOWER transforma todas as letras em minúsculas

```SQL
SELECT LOWER(title)
FROM movie;
```

### **UPPER** {#upper}
<br/>

UPPER transforma todas as letras em maiúsculas

```SQL
SELECT UPPER(title)
FROM movie;
```

### **LEFT** {#left}
<br/>

LEFT obtém um número especificado de caracteres para cada linha em uma coluna, começando da esquerda

Exemplo: Obtendo a primeira letra de todos os títulos
```SQL
SELECT LEFT(title, 1) as first_letter
FROM movie;
```

Para contar quantos filmes tem a mesma primeira letra:
```SQL
SELECT LEFT(UPPER(title), 1) as first_letter, COUNT(*) count_movies
FROM movie
GROUP BY 1;
```

### **RIGHT** {#right}
<br/>

RIGHT obtém um número especificado de caracteres para cada linha em uma coluna a partir da direita

Exemplo: Obtendo a última letra para todos os ids
```SQL
SELECT RIGHT(id, 1) as last_letter
FROM movie;
```

### **LENGTH** {#lenght}
<br/>

LENGTH oObtém o número de caracteres para cada linha de uma coluna. Lembre-se: espaço em branco também é um personagem

Exemplo: contagem do número de caracteres para todos os títulos.
```SQL
SELECT title, LENGTH(title) as num_characters_title
FROM movie;
```

### **POSITION & STRPOS** {#positionstrpos}
<br/>

POSITION fornece o índice onde esse caractere é para cada linha, sendo 1 para o índice da primeira posição. O synstax é POSITION ('*caracter*' IN *nome_da_tabela*).

Exemplo: procurando o índice para o primeiro espaço em branco
```SQL
SELECT POSITION(' ' IN title)
FROM movie;
```

STRPOS fornece o mesmo resultado que POSITION, mas a sintaxe é diferente: STRPOS (*nome_da_tabela*, '*caracter*').
```SQL
SELECT STRPOS(title, ' ')
FROM movie;
```

POSITION e STRPOS diferenciam maiúsculas de minúsculas.

```SQL
SELECT LEFT(name, STRPOS(name, ' ') -1 ) first_name, 
RIGHT(name, LENGTH(name) - STRPOS(name, ' ')) last_name
FROM director;
```

### **CONCAT** {#concat}
<br/>

CONCAT combina colunas juntas. A sintaxe é CONCAT (*first_column*, *second_column*) ou você pode usar o piping `||`: *first_column* || *second_column*.

É possível concatenar caracteres com as colunas, como exemplo: CONCAT (*first_column*, '', *second_column*)

Exemplo: concatenar o título com date_time
```SQL
SELECT CONCAT(title, '---', date_time)
FROM movie;
```
## **Função Window** {#windowfunction}
<br/>

A função WINDOW executa um cálculo cumulativo nas linhas.

### **OVER** {#over}
<br/>

OVER cria uma função de janela. É necessário colocar ORDER BY para obter o cálculo acumulado.

Exemplo: Adiciona todos os *budget* acumulando-os até *date_time*
```SQL
SELECT budget, 
	SUM(budget) OVER(ORDER BY date_time) AS cumulative_sum
FROM movie;
```

### **PARTITION BY** {#partitionby}
<br/>

PARTITION BY divide o cálculo cumulativo pela coluna indicada.

Exemplo: Adiciona todos os *budget* acumulados até *date_time* para cada *director_id*
```SQL
SELECT budget, director_id,
	SUM(budget) OVER(PARTITION BY director_id ORDER BY date_time) AS cumulative_sum
FROM movie;
```

É possível escrever várias funções WINDOW na mesma consulta.
```SQL
SELECT budget, director_id,
	DATE_TRUNC('month', date_time) AS month,
	SUM(budget) OVER(PARTITION BY director_id ORDER BY DATE_TRUNC('month', date_time)) AS sum_budget,
	AVG(budget) OVER(PARTITION BY director_id ORDER BY DATE_TRUNC('month', date_time)) AS avg_budget,
	MIN(budget) OVER(PARTITION BY director_id ORDER BY DATE_TRUNC('month', date_time)) AS min_budget,
	MAX(budget) OVER(PARTITION BY director_id ORDER BY DATE_TRUNC('month', date_time)) AS max_budget,
FROM movie;
```

### **ROW_NUMBER() & RANK()** {#rowandrank}
<br/>

ROW_NUMBER () enumera linhas em sequência. Você pode usar para contar o número de linhas e usando PARTITION BY a consulta retornará uma nova sequência para cada variável em PARTITION BY.

Exemplo: Enumere o *budget* do maior para o menor para cada *diretor id*
```SQL
SELECT director_id, title, budget, 
	ROW_NUMBER() OVER(PARTITION BY director_id ORDER BY budget DESC) AS sequence_budget
FROM movie;
```

RANK () enumera as linhas por sequência também, mas se a tabela tiver os mesmos valores na coluna em ORDER BY, a consulta repetirá o número na sequência, pulando valores.

Exemplo:
```SQL
SELECT director_id, budget, 
	RANK() OVER(PARTITION BY director_id ORDER BY budget DESC) AS rank_budget
FROM movie;
```
Output:

| director_id | budget | rank_budget |
|----:|----:|----:|
| 1208 | 275,000,000 | 1 |
| 1208 | 260,000,000 | 2 |
| 1208 | 260,000,000 | 2 |
| 1208 | 190,000,000 | 4 |
| 1208 | 180,000,000 | 5 |
| 1208 | 180,000,000 | 5 |
| 1208 | 175,000,000 | 7 |

DENSE_RANK () semelhante a RANK (), mas sem pulando valores.

Exemplo:
```SQL
SELECT director_id, budget, 
	DENSE_RANK() OVER(PARTITION BY director_id ORDER BY budget DESC) AS rank_budget
FROM movie;
```
Output:

| director_id | budget | rank_budget |
|----:|----:|----:|
| 1208 | 275,000,000 | 1 |
| 1208 | 260,000,000 | 2 |
| 1208 | 260,000,000 | 2 |
| 1208 | 190,000,000 | 3 |
| 1208 | 180,000,000 | 4 |
| 1208 | 180,000,000 | 4 |
| 1208 | 175,000,000 | 5 |

### **WINDOW** {#}
<br/>

WINDOW nos permite agrupar todas as funções da janela com a mesma consulta, usando o mesmo alias. WINDOW vem depois da instrução FROM.

```SQL
SELECT budget, director_id,
	DATE_TRUNC('month', date_time) AS month,
	SUM(budget) OVER main_window AS sum_budget,
	AVG(budget) OVER main_window AS avg_budget,
	MIN(budget) OVER main_window AS min_budget,
	MAX(budget) OVER main_window AS max_budget,
FROM movie;

WINDOW main_window AS (PARTITION BY director_id ORDER BY DATE_TRUNC('month', date_time))
```

### **LAG & LEAD** {#lagandlead}
<br/>

### LAG

A função LAG retorna o valor de uma linha anterior para a linha atual na tabela.

Exemplo: para comparar o orçamento total de um mês para o outro, primeiro crie uma consulta interna para obter a soma para o * orçamento * em cada mês
```SQL
SELECT DATE_TRUNC('month', date_time) AS month,	
	SUM(budget) OVER(PARTITION BY director_id ORDER BY DATE_TRUNC('month', date_time)) AS sum_budget
FROM movie
GROUP BY   1
```
Para ver o *sum_budget* do mês anterior, use a função LAG como:

`LAG(sum_budget) OVER (ORDER BY sum_budget) AS lag`

Então, para ver a diferença do orçamento atual para o anterior, escreva uma linha com a função *sum_budge* - LAG, como:

`sum_budget - LAG(sum_budget) OVER (ORDER BY sum_budget) AS lag_difference`

A query inteira:
```SQL
SELECT month, sum_budget,
	LAG(sum_budget) OVER (ORDER BY sum_budget) AS lag,
	sum_budget - LAG(sum_budget) OVER (ORDER BY sum_budget) AS lag_difference
FROM(
		SELECT DATE_TRUNC('month', date_time) AS month,	
			SUM(budget) OVER(PARTITION BY director_id ORDER BY DATE_TRUNC('month', date_time)) AS sum_budget
		FROM movie
		GROUP BY   1
    ) sub
ORDER BY 1;
```
O ORDER BY vem no final porque a diferença tem que ser por mês.

### LEAD

LEAD retorna o valor da linha após a linha atual na tabela.

Exemplo: semelhante ao exemplo anterior, para calcular a diferença entre o orçamento a seguir e o atual, basta usar a função LEAD - *sum_budget*, como:

`LEAD(sum_budget) OVER (ORDER BY sum_budget) AS lead_difference - sum_budget`

Sendo a função LEAD apenas: `LEAD(sum_budget) OVER (ORDER BY sum_budget) AS lead_difference`

```SQL
SELECT month, sum_budget,
	LEAD(sum_budget) OVER (ORDER BY sum_budget) AS lead_difference,
	LEAD(sum_budget) OVER (ORDER BY sum_budget) AS lead_difference - sum_budget
FROM(
		SELECT DATE_TRUNC('month', date_time) AS month,	
			SUM(budget) OVER(PARTITION BY director_id ORDER BY DATE_TRUNC('month', date_time)) AS sum_budget
		FROM movie
		GROUP BY   1
    ) sub
ORDER BY 1;
```

### **NTILE** {#ntile}
<br/>

A função NTILE identifica em que quartil (ou percentil ou qualquer outra subdivisão) uma determinada linha se enquadra. A subdivisão vem dentro dos parênteses: NTILE (*número*), enquanto ORDER BY determina qual coluna usar para calcular os quartis

```SQL
SELECT director_id, title, budget, 
	NTILE(100) OVER(PARTITION BY director_id ORDER BY budget) AS budget_percentile
FROM movie;
```
