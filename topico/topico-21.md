## [Tópico T21] - SQL - DML (Data Manipulation Language): Funções Agregadas, Agrupamento de dados, Ordenação de Dados
###### *by Prof. Plinio Sa Leitao-Junior (INF/UFG)*

Os exemplos apresentados usam o esquema lógico do **BD Empresa**, conforme abaixo.

<img src="../media/fig-esquema-logico-bdempresa.jpg" width="450">

Neste tópico, a instrução **SELECT** possui a estrutura genérica abaixo:<br>
&nbsp;&nbsp;&nbsp;&nbsp;SELECT **_lista-de-atributos-de-projeção_**<br>
&nbsp;&nbsp;&nbsp;&nbsp;FROM **_lista-de-relações_**<br>
&nbsp;&nbsp;&nbsp;&nbsp;WHERE **_predicado-de-seleção-de-tuplas_**<br>
&nbsp;&nbsp;&nbsp;&nbsp;GROUP BY **_lista-de-atributos-de-agrupamento_**<br>
&nbsp;&nbsp;&nbsp;&nbsp;HAVING **_predicado-de-seleção-de-grupos_**<br>
&nbsp;&nbsp;&nbsp;&nbsp;ORDER BY **_lista-de-atributos-de-ordenação_**<br>
Onde:<br>
■ **_lista-de-atributos-de-projeção_** é uma lista dos atributos, cujos valores são retornados pela consulta;<br>
■ **_lista-de-relações_** é uma lista dos nomes das relações, as quais são fontes de dados para a consulta;<br>
■ **_predicado-de-seleção-de-tuplas_** é uma expressão condicional, que seleciona as _tuplas_ para o processamento consulta;<br>
■ **_lista-de-atributos-de-agrupamento_** é uma lista de atributos que define a formação de grupos de _tuplas_;<br>
■ **_predicado-de-seleção-de-grupos_** é uma expressão condicional que usualmente usa funções agregadas para a seleção de grupos de _tuplas_;<br>
■ **_lista-de-atributos-de-ordenação_** é uma lista de atributos que define como as _tuplas_ do resultado da consulta serão ordenadas.

### Funções Agregadas e Agrupamento de dados

Funções agregadas e agrupamento de dados estão presentes na SQL de forma similar às construções disponíveis na Álgebra Relacional:
- A funções agregadas usualmente empregadas na SQL são:
  - COUNT, SUM, AVG, MIN e MAX.
- A Cláusulas GROUP BY e HAVING são empregadas para o agrupamento de dados.

### Exemplo 01: Funções agregadas
#### Qual a quantidade de funcionários bem como a soma e a média de salários da empresa?

|Álgebra Relacional|SQL|
|-|-|
|ℑ CONTA Cpf, SOMA Salario, MÉDIA Salario<br>&nbsp;&nbsp;&nbsp;&nbsp;(FUNCIONARIO)|SELECT Count(Cpf), SUM(Salario), AVG(Salario)<br>FROM FUNCIONARIO|

### Exemplo 02: Funções agregadas
#### Quantos salários distintos há na empresa?

|Álgebra Relacional|SQL|
|-|-|
|ℑ CONTA Salario (π <sub>Salario</sub>FUNCIONARIO)|SELECT Count(\*)<br>FROM ( <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;SELECT DISTINCT Salario<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;FROM FUNCIONARIO ) AS S<br><br>SELECT COUNT(DISTINCT Salario)<br>FROM FUNCIONARIO|

### Exemplo 03: Funções agregadas
#### Quantos funcionários são supervisores na empresa?

|Álgebra Relacional|SQL|
|-|-|
|ℑ CONTA Cpf_supervisor (π <sub>Cpf_supervisor</sub>FUNCIONARIO)|SELECT Count(Cpf_supervisor)<br>FROM ( <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;SELECT DISTINCT Cpf_supervisor<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;FROM FUNCIONARIO ) AS S<br><br>SELECT COUNT(DISTINCT Cpf_supervisor)<br>FROM FUNCIONARIO<br><br>**_# O comando abaixo está correto?_**<br>SELECT Count(\*)<br>FROM ( <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;SELECT DISTINCT Cpf_supervisor<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;FROM FUNCIONARIO ) AS S|

### Exemplo 04: Funções agregadas e Agrupamento de dados
#### Qual a quantidade de funcionários e média salarial por departamento da empresa?

|Álgebra Relacional|SQL|
|-|-|
|**<sub>Dnr</sub> ℑ** CONTA <sub>Cpf</sub>, MÉDIA <sub>Salario</sub><br>&nbsp;&nbsp;&nbsp;&nbsp;(FUNCIONARIO)<br>|SELECT Dnr, Count(Cpf), AVG(Salario)<br>FROM FUNCIONARIO<br>**GROUP BY Dnr**|
|<br>ρ <sub>RESULT</sub> (<br>&nbsp;&nbsp;&nbsp;&nbsp;<sub>Identificacao_do_departamento,</sub><br>&nbsp;&nbsp;&nbsp;&nbsp;<sub>Quantidade_de_funcionarios,</sub><br>&nbsp;&nbsp;&nbsp;&nbsp;<sub>Salario_medio</sub> )</sub><br>&nbsp;&nbsp;&nbsp;&nbsp;( **<sub>Dnr</sub> ℑ** CONTA <sub>Cpf</sub>, MÉDIA <sub>Salario</sub><br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(FUNCIONARIO) )|SELECT<br>&nbsp;&nbsp;&nbsp;&nbsp;**Dnr** AS Identificacao_do_departamento,<br>&nbsp;&nbsp;&nbsp;&nbsp;**Count(Cpf)** AS Quantidade_de_funcionarios,<br>&nbsp;&nbsp;&nbsp;&nbsp;**AVG(Salario)** AS Salario_medio<br>FROM FUNCIONARIO<br>**GROUP BY Dnr**|

### Exemplo 05: Funções agregadas e Agrupamento de dados
#### Apresente o código e o nome do departamento, bem como a quantidade de funcionários e a média salarial do departamento.

|Álgebra Relacional|SQL|
|-|-|
|<sub>**Dnr, Dnome</sub> ℑ** CONTA <sub>Cpf</sub>, MÉDIA <sub>Salario</sub><br>&nbsp;&nbsp;(FUNCIONARIO ⋈<sub>Dnr=Dnumero</sub> DEPARTAMENTO)<br>|SELECT Dnr, Dnome, Count(Cpf), AVG(Salario)<br>FROM FUNCIONARIO JOIN DEPARTAMENTO<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ON Dnr = Dnumero<br>**GROUP BY Dnr, Dnome**|

### Exemplo 06: Funções agregadas e Agrupamento de dados
#### Qual o nome dos funcionários que possuem dois ou mais dependentes?

|Álgebra Relacional|SQL|
|-|-|
|**_# EQUIJUNÇÃO_**<br>RESUMO(Fcpf, Qtde_dep)←<br>&nbsp;&nbsp;&nbsp;&nbsp;<sub>**Fcpf</sub> ℑ** CONTA <sub>Fcpf</sub> (DEPENDENTE)<br>RESULT ← π <sub>Pnome, Unome, Qtde_dep</sub> (<br>&nbsp;&nbsp;&nbsp;&nbsp;σ **<sub>Qtde_dep >= 2</sub>** (<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;RESUMO ⋈<sub>Fcpf=Cpf</sub> FUNCIONARIO ) )|SELECT Pnome, Unome, Qtde_dep<br>FROM FUNCIONARIO JOIN<br>&nbsp;&nbsp;&nbsp;&nbsp;( SELECT Fcpf, COUNT(Fcpf) AS Qtde_dep<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;FROM DEPENDENTE<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**GROUP BY Fcpf** ) AS RESUMO<br>&nbsp;&nbsp;&nbsp;&nbsp;ON Cpf = Fcpf<br>**WHERE Qtde_dep >=2**<br><br>**_# Cláusula HAVING_**<br>SELECT Pnome, Unome, COUNT(Fcpf)<br>FROM FUNCIONARIO JOIN DEPENDENTE<br>&nbsp;&nbsp;&nbsp;&nbsp;ON Cpf = Fcpf<br>**GROUP BY Fcpf, Pnome, Unome**<br>**HAVING COUNT(Fcpf) >= 2**<br><br>**_# Cláusula HAVING_**<br>**_# Incorreto se há vários funcionários_**<br>**_#&nbsp;&nbsp;&nbsp;&nbsp; com os mesmos primeiro e último nomes_**<br>SELECT Pnome, Unome, COUNT(Fcpf)<br>FROM FUNCIONARIO JOIN DEPENDENTE<br>&nbsp;&nbsp;&nbsp;&nbsp;ON Cpf = Fcpf<br>**GROUP BY Pnome, Unome**<br>**HAVING COUNT(Fcpf) >= 2**|
|**_# JUNÇÃO NATURAL_**<br>RESUMO(**Cpf**, Qtde_dep)←<br>&nbsp;&nbsp;&nbsp;&nbsp;**<sub>Fcpf</sub> ℑ** CONTA <sub>Fcpf</sub> (DEPENDENTE)<br>RESULT ← π <sub>Pnome, Unome, Qtde_dep</sub> (<br>&nbsp;&nbsp;&nbsp;&nbsp;σ **<sub>Qtde_dep >= 2</sub>** (<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;RESUMO * FUNCIONARIO ) )|SELECT Pnome, Unome, Qtde_dep<br>FROM FUNCIONARIO NATURAL JOIN<br>&nbsp;&nbsp;&nbsp;&nbsp;( SELECT **Fcpf AS Cpf**, COUNT(Fcpf) AS Qtde_dep<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;FROM DEPENDENTE<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**GROUP BY Fcpf** ) AS RESUMO<br>**WHERE Qtde_dep >=2**|

### Ordenação de Dados

A SQL permite a ordenação de _tuplas_ no resultado de uma consulta, considerando os valores de um ou mais dos atributos que aparecem no resultado da consulta:
- A cláusula ORDER BY é aplicada para a ordenação de _tuplas_.

### Exemplo 07: Ordenação de dados
#### Recupere uma lista de funcionários e os projetos em que estão trabalhando, ordenados por departamento e, dentro de cada departamento, ordenados alfabeticamente por sobrenome, depois o nome.

|SQL|
|-|
|SELECT D.Dnome, F.Unome, F.Pnome, P.Projnome<br>FROM DEPARTAMENTO AS D <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;JOIN FUNCIONARIO AS F ON D.Dnumero = F.Dnr <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;JOIN TRABALHA_EM AS T ON F.Cpf = T.Fcpf <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;JOIN PROJETO AS P ON T.Pnr = P.Projnumero<br>ORDER BY D.Dnome, F.Unome, F.Pnome|

Experimente as seguintes alternativas para o comando acima:<br>
■ ORDER BY D.Dnome ASC, F.Unome DESC, F.Pnome<br>
■ ORDER BY 1, 2, 3<br>
■ ORDER BY 1, 3, 2<br>
■ ORDER BY 1 DESC, 3 DESC, 2 DESC

### Exercícios

1. Seja o comando SQL:<br>&nbsp;&nbsp;SELECT Salario<br>&nbsp;&nbsp;FROM FUNCIONARIO<br>&nbsp;&nbsp;WHERE Salario >= 13000 + ( SELECT MIN(Salario) FROM FUNCIONARIO )<br>Considere que na relação FUNCIONARIO:<br>■ há 4 (quatro) _tuplas_;<br>■ os salários 10000, 20000, 30000, 25000.<br>Qual a soma dos salários que são retornados pela consulta?

1. Seja o comando SQL:<br>
SELECT Salario FROM FUNCIONARIO WHERE Salario > <br>
&nbsp;&nbsp;( SELECT MIN(Salario) FROM FUNCIONARIO WHERE Salario < <br>
&nbsp;&nbsp;&nbsp;&nbsp;( SELECT MAX(Salario) FROM FUNCIONARIO WHERE Salario < <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;( SELECT MAX(Salario) FROM FUNCIONARIO ) ) ) <br>

Se a relação FUNCIONARIO possui 6 (seis) valores distintos de salário, então '_Salário 1_' é o menor salário e '_Salário 6_' é o maior salário. Some as sentenças verdadeiras:<br>(01) **Salário 1** está no resultado da consulta.<br>(02) **Salário 2** está no resultado da consulta.<br>(04) **Salário 3** está no resultado da consulta.<br>(08) **Salário 4** está no resultado da consulta.<br>(16) **Salário 5** está no resultado da consulta.<br>(32) **Salário 6** está no resultado da consulta.
