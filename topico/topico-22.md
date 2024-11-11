## [Tópico 22] - SQL - DML (_Data Manipulation Language_): União, Interseção, Diferença, Subconsulta (primeiros passos)
###### *by Prof. Plinio Sa Leitao-Junior (INF/UFG)*

Os exemplos apresentados usam o esquema lógico do **BD Empresa**, conforme abaixo.

<img src="../media/fig-esquema-logico-bdempresa.jpg" width="450">

### União, Interseção e Diferença

As operações União, Interseção e Diferença estão disponíveis na SQL com o uso das cláusulas UNION, INTERSECT e EXCEPT, respectivamente:
- Contudo há outras construções sintáticas que implementam essas operações.
- A restrição de união-compatibilidade é requerida na SQL, similarmente à Álgebra Relacional.

### Exemplo 01: União
#### Quais os funcionários que são supervisores ou gerentes de departamento?

|Álgebra Relacional|SQL|
|-|-|
|π <sub>Cpf_supervisor</sub> (FUNCIONARIO)<br>∪<br>π <sub>Cpf_gerente</sub> (DEPARTAMENTO)|SELECT Cpf_supervisor <br>FROM FUNCIONARIO<br>UNION<br>SELECT Cpf_gerente <br>FROM DEPARTAMENTO|
|π <sub>S.Cpf, S.Pnome, S.Unome</sub><br>&nbsp;&nbsp;( ρ <sub>S</sub> (FUNCIONARIO)<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;⨝ <sub>F.Cpf_supervisor = S.Cpf</sub><br>&nbsp;&nbsp;&nbsp;&nbsp;ρ <sub>F</sub> (FUNCIONARIO) )<br>∪<br>π <sub>F.Cpf, F.Pnome, F.Unome</sub><br>&nbsp;&nbsp;( ρ <sub>D</sub> (DEPARTAMENTO)<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;⨝ <sub>D.Cpf_gerente = F.Cpf</sub><br>&nbsp;&nbsp;&nbsp;&nbsp;ρ <sub>F</sub> (FUNCIONARIO) )|SELECT S.Cpf, S.Pnome, S.Unome<br>FROM FUNCIONARIO AS F JOIN FUNCIONARIO AS S<br>&nbsp;&nbsp;&nbsp;&nbsp;ON F.Cpf_supervisor = S.Cpf<br>UNION<br>SELECT F.Cpf, F.Pnome, F.Unome <br>FROM DEPARTAMENTO AS D JOIN FUNCIONARIO AS F<br>&nbsp;&nbsp;&nbsp;&nbsp;ON D.Cpf_gerente = F.Cpf<br><br>**_# O comando abaixo está incorreto?_**<br>SELECT S.Cpf, S.Pnome <br>FROM FUNCIONARIO AS F JOIN FUNCIONARIO AS S<br>&nbsp;&nbsp;&nbsp;&nbsp;ON F.Cpf_supervisor = S.Cpf<br>UNION<br>SELECT F.Cpf, F.Pnome, F.Unome <br>FROM DEPARTAMENTO AS D JOIN FUNCIONARIO AS F<br>&nbsp;&nbsp;&nbsp;&nbsp;ON D.Cpf_gerente = F.Cpf|

### Exemplo 02: Interseção
#### Quais os funcionários que são supervisores e possuem dependentes?

|Álgebra Relacional|SQL|
|-|-|
|π <sub>Cpf_supervisor</sub> (FUNCIONARIO)<br>∩<br>π <sub>Fcpf</sub> (DEPENDENTE)|SELECT Cpf_supervisor<br>FROM FUNCIONARIO<br>INTERSECT<br>SELECT FCpf<br>FROM DEPENDENTE|
|π <sub>S.Cpf, S.Pnome, S.Unome</sub><br>&nbsp;&nbsp;( ρ <sub>S</sub> (FUNCIONARIO)<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;⨝ <sub>F.Cpf_supervisor = S.Cpf</sub><br>&nbsp;&nbsp;&nbsp;&nbsp;ρ <sub>F</sub> (FUNCIONARIO) )<br>∩<br>π <sub>F.Cpf, F.Pnome, F.Unome</sub><br>&nbsp;&nbsp;( ρ <sub>D</sub> (DEPENDENTE)<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;⨝ <sub>D.Fcpf = F.Cpf</sub><br>&nbsp;&nbsp;&nbsp;&nbsp;ρ <sub>F</sub> (FUNCIONARIO) )|SELECT S.Cpf, S.Pnome, S.Unome<br>FROM FUNCIONARIO AS F JOIN FUNCIONARIO AS S<br>&nbsp;&nbsp;&nbsp;&nbsp;ON F.Cpf_supervisor = S.Cpf<br>INTERSECT<br>SELECT F.Cpf, F.Pnome, F.Unome <br>FROM DEPENDENTE AS D JOIN FUNCIONARIO AS F<br>&nbsp;&nbsp;&nbsp;&nbsp;ON D.Fcpf = F.Cpf|

### Exemplo 03: Diferença
#### Quais os funcionários que são supervisores e não possuem dependentes?

|Álgebra Relacional|SQL|
|-|-|
|π <sub>Cpf_supervisor</sub> (FUNCIONARIO)<br>–<br>π <sub>Fcpf</sub> (DEPENDENTE)|SELECT Cpf_supervisor<br>FROM FUNCIONARIO<br>EXCEPT<br>SELECT FCpf<br>FROM DEPENDENTE|
|π <sub>S.Cpf, S.Pnome, S.Unome</sub><br>&nbsp;&nbsp;( ρ <sub>S</sub> (FUNCIONARIO)<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;⨝ <sub>F.Cpf_supervisor = S.Cpf</sub><br>&nbsp;&nbsp;&nbsp;&nbsp;ρ <sub>F</sub> (FUNCIONARIO) )<br>–<br>π <sub>F.Cpf, F.Pnome, F.Unome</sub><br>&nbsp;&nbsp;( ρ <sub>D</sub> (DEPENDENTE)<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;⨝ <sub>D.Fcpf = F.Cpf</sub><br>&nbsp;&nbsp;&nbsp;&nbsp;ρ <sub>F</sub> (FUNCIONARIO) )|SELECT S.Cpf, S.Pnome, S.Unome<br>FROM FUNCIONARIO AS F JOIN FUNCIONARIO AS S<br>&nbsp;&nbsp;&nbsp;&nbsp;ON F.Cpf_supervisor = S.Cpf<br>EXCEPT<br>SELECT F.Cpf, F.Pnome, F.Unome <br>FROM DEPENDENTE AS D JOIN FUNCIONARIO AS F<br>&nbsp;&nbsp;&nbsp;&nbsp;ON D.Fcpf = F.Cpf|

### Exemplo 04: Diferença
#### Quais os funcionários que possuem dependentes e não são supervisores?

|Álgebra Relacional|SQL|
|-|-|
|π <sub>Fcpf</sub> (DEPENDENTE)<br>–<br>π <sub>Cpf_supervisor</sub> (FUNCIONARIO)|SELECT FCpf<br>FROM DEPENDENTE<br>EXCEPT<br>SELECT Cpf_supervisor<br>FROM FUNCIONARIO|
|π <sub>F.Cpf, F.Pnome, F.Unome</sub><br>&nbsp;&nbsp;( ρ <sub>D</sub> (DEPENDENTE)<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;⨝ <sub>D.Fcpf = F.Cpf</sub><br>&nbsp;&nbsp;&nbsp;&nbsp;ρ <sub>F</sub> (FUNCIONARIO) )<br>–<br>π <sub>S.Cpf, S.Pnome, S.Unome</sub><br>&nbsp;&nbsp;( ρ <sub>S</sub> (FUNCIONARIO)<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;⨝ <sub>F.Cpf_supervisor = S.Cpf</sub><br>&nbsp;&nbsp;&nbsp;&nbsp;ρ <sub>F</sub> (FUNCIONARIO) )|SELECT F.Cpf, F.Pnome, F.Unome <br>FROM DEPENDENTE AS D JOIN FUNCIONARIO AS F<br>&nbsp;&nbsp;&nbsp;&nbsp;ON D.Fcpf = F.Cpf<br>EXCEPT<br>SELECT S.Cpf, S.Pnome, S.Unome<br>FROM FUNCIONARIO AS F JOIN FUNCIONARIO AS S<br>&nbsp;&nbsp;&nbsp;&nbsp;ON F.Cpf_supervisor = S.Cpf|

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

### Exemplo 05: Subconsulta
#### Qual o nome dos funcionários que são gerentes de departamento?

|Classificação|SQL|
|-|-|
|Subconsulta independente|SELECT Pnome, Unome<br>FROM FUNCIONARIO<br>WHERE Cpf IN (<br>&nbsp;&nbsp;&nbsp;&nbsp;**SELECT Cpf_gerente**<br>&nbsp;&nbsp;&nbsp;&nbsp;**FROM DEPARTAMENTO** )|
|Subconsulta correlata|SELECT Pnome, Unome<br>FROM FUNCIONARIO<br>WHERE EXISTS (<br>&nbsp;&nbsp;&nbsp;&nbsp;**SELECT \***<br>&nbsp;&nbsp;&nbsp;&nbsp;**FROM DEPARTAMENTO**<br>&nbsp;&nbsp;&nbsp;&nbsp;**WHERE FUNCIONARIO.Cpf = DEPARTAMENTO.Cpf_gerente** )|

### Exercício

1. Em relação ao BD Empresa, escreva em SQL a consulta "Qual o primeiro e último nomes dos funcionários que possuem dois ou mais dependentes e que trabalham em dois ou mais projetos?"<br>O comando SQL deve:<br>■ usar pelo menos uma das operações UNIÃO, INTERSEÇÃO e DIFERENÇA, necessariamente conforme a sintaxe apresentada no tópico;<br>■ usar a cláusula HAVING.
