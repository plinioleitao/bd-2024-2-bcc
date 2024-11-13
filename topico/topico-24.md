## [Tópico 24] - SQL - DML (_Data Manipulation Language_): Subconsulta (parte 2)
###### *by Prof. Plinio Sa Leitao-Junior (INF/UFG)*

Os exemplos apresentados usam o esquema lógico do **BD Empresa**, conforme abaixo.

<img src="../media/fig-esquema-logico-bdempresa.jpg" width="450">

### Subconsulta

Uma subconsulta consiste em um comando SELECT posicionado dentro de um outro comando SQL. Com maior frequência, subconsultas estão inseridas em outras consultas, ou seja, consultas aninhadas:
- A subconsulta é denominada _consulta interna_.
- A consulta na qual a subconsulta está embutida é chamada _consulta externa_.
- A subconsulta usualmente está localizada na cláusula WHERE, na cláusula FROM e/ou na cláusula SELECT, conforme necessário.

Um classificação comum para subconsultas é:
- **Subconsulta Independente**: a execução da subconsulta é independente da consulta mais externa:
  - a subconsulta é executada uma única vez;
  - o resultado da subconsulta é utilizado para o processamento da consulta mais externa.
- **Subconsulta Corretata**: a execução da subconsulta é dependente da consulta mais externa:
  - a subconsulta é potencialmente executada várias vezes:
    - a subconsulta é avaliada uma vez para cada _tupla_ (ou combinação de _tuplas_) na consulta externa.
  - a subconsulta requer dados oriundos da consulta externa para ser processada.

### Cláusulas ANY e ALL

As cláusulas ANY e ALL são usadas conjuntamente com subconsultas com os seguintes significados:<br>
■ O predicado que inclui a cláusula ANY será verdadeiro caso seja satisfeito **para pelo menos um (qualquer)** dos elementos resultantes da subconsulta.<br>
■ O predicado que inclui a cláusula ALL será verdadeiro caso seja satisfeito **para todos** os elementos resultantes da subconsulta.

### Exemplo 01: Cláusula ANY
#### Qual o nome dos funcionários que são gerentes de departamento?

|Classificação|SQL|
|-|-|
|Subconsulta independente|SELECT Pnome, Unome<br>FROM FUNCIONARIO<br>WHERE Cpf = ANY (<br>&nbsp;&nbsp;&nbsp;&nbsp;**SELECT Cpf_gerente**<br>&nbsp;&nbsp;&nbsp;&nbsp;**FROM DEPARTAMENTO** )|

### Exemplo 02: Cláusula ALL
#### Qual o nome e salário dos funcionários que "têm o menor salário" da empresa?

|Classificação|SQL|
|-|-|
|Subconsulta independente|SELECT Pnome, Unome, Salario<br>FROM FUNCIONARIO<br>WHERE Salario <= ALL  (<br>&nbsp;&nbsp;&nbsp;&nbsp;**SELECT Salario**<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**FROM FUNCIONARIO** )|

### Exemplo 03: Cláusulas ANY, ALL e EXISTS
#### << _As três expressões em SQL abaixo são equivalentes. Qual a consulta relacionada?_ >>

|Classificação|SQL|
|-|-|
|Subconsulta independente|SELECT Pnome, Unome, Salario<br>FROM FUNCIONARIO<br>WHERE Salario >= ALL  (<br>&nbsp;&nbsp;&nbsp;&nbsp;**SELECT Salario**<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**FROM FUNCIONARIO** )|
|Subconsulta independente<br>Subconsulta independente|SELECT Pnome, Unome, Salario<br>FROM FUNCIONARIO<br>WHERE Salario NOT IN (<br>&nbsp;&nbsp;&nbsp;&nbsp;**SELECT Salario**<br>&nbsp;&nbsp;&nbsp;&nbsp;**FROM FUNCIONARIO**<br>&nbsp;&nbsp;&nbsp;&nbsp;**WHERE Salario < ANY (**<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**SELECT Salario**<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**FROM FUNCIONARIO )** )|
|Subconsulta independente<br>Subconsulta correlata|SELECT Pnome, Unome, Salario<br>FROM FUNCIONARIO AS EXTERNA<br>WHERE Salario NOT IN (<br>&nbsp;&nbsp;&nbsp;&nbsp;**SELECT Salario**<br>&nbsp;&nbsp;&nbsp;&nbsp;**FROM FUNCIONARIO AS CENTRAL**<br>&nbsp;&nbsp;&nbsp;&nbsp;**WHERE EXISTS (**<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**SELECT Salario**<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**FROM FUNCIONARIO AS INTERNA<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;WHERE CENTRAL.Salario < INTERNA.Salario )** )|

### Exemplo 04: Funções agregadas
#### Qual o nome e salário dos funcionários que trabalham em 01 (um) ou em 04 (quatro) projetos?

|Classificação|SQL|
|-|-|
|Subconsulta correlata|SELECT Pnome, Unome<br>FROM FUNCIONARIO<br>WHERE (<br>&nbsp;&nbsp;&nbsp;&nbsp;**SELECT COUNT(\*)**<br>&nbsp;&nbsp;&nbsp;&nbsp;**FROM TRABALHA_EM**<br>&nbsp;&nbsp;&nbsp;&nbsp;**WHERE FUNCIONARIO.Cpf = TRABALHA_EM.Fcpf** )<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;IN (1, 4)|

### Exemplo 05: Operação UNIÃO
#### Qual o Cpf, nome e salário dos funcionários que são supervisores "ou" possuem dependentes?

|Classificação|SQL|
|-|-|
|Subconsulta independente|SELECT Cpf, Pnome, Unome, Salario<br>FROM FUNCIONARIO<br>WHERE Cpf IN<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;( **SELECT Cpf_supervisor FROM FUNCIONARIO** )<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**OR** Cpf IN<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;( **SELECT Fcpf FROM DEPENDENTE** )|

### Exemplo 06: Operação INTERSEÇÃO
#### Qual o Cpf, nome e salário dos funcionários que são supervisores "e" possuem dependentes?

|Classificação|SQL|
|-|-|
|Subconsulta independente|SELECT Cpf, Pnome, Unome, Salario<br>FROM FUNCIONARIO<br>WHERE Cpf IN<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;( **SELECT Cpf_supervisor FROM FUNCIONARIO** )<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**AND** Cpf IN<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;( **SELECT Fcpf FROM DEPENDENTE** )|

### Exemplo 07: Operação DIFERENÇA
#### Qual o Cpf, nome e salário dos funcionários que são supervisores "mas não" possuem dependentes?

|Classificação|SQL|
|-|-|
|Subconsulta independente|SELECT Cpf, Pnome, Unome, Salario<br>FROM FUNCIONARIO<br>WHERE Cpf IN<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;( **SELECT Cpf_supervisor FROM FUNCIONARIO** )<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**AND** Cpf **NOT** IN<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;( **SELECT Fcpf FROM DEPENDENTE** )|

### Exercício

1. Você consegue entender e usar os exemplos de subconsulta apresentados no tópico?<br>
Favor selecionar uma das respostas a seguir:<br>
"Sim, eu consigo entender e usar os exemplos de subconsulta apresentados no tópico", ou<br>
"Não, pois eu tenho as seguintes dificuldades: blá, blá, ...". Nesta resposta, você deve descrever as suas dificuldades, em vez de apenas listá-las.

