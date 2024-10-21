## [Tópico T19] - SQL - DML (_Data Manipulation Language_): Primeiros comandos
###### *by Prof. Plinio Sa Leitao-Junior (INF/UFG)*

A **Linguagem de Manipulação de Dados** (DML - _Data Manipulation Language_) se refere às operações do dia-a-dia de um banco de dados, a saber: consultar dados, inserir dados, alterar dados e excluir dados.<br>
A SQL possui comandos para a DML.<br>
Vamos iniciar com **Consultar Dados**.

Os exemplos apresentados usam o esquema lógico do **BD Empresa**, conforme abaixo.
<img src="../media/fig-esquema-logico-bdempresa.jpg" width="450">

A instrução **SELECT** é usada para consultas em SQL. A estrutura básica da instrução é:<br>
&nbsp;&nbsp;&nbsp;&nbsp;SELECT **_lista-de-atributos_**<br>
&nbsp;&nbsp;&nbsp;&nbsp;FROM **_lista-de-relações_**<br>
&nbsp;&nbsp;&nbsp;&nbsp;WHERE **_predicado-de-seleção_**<br>
Onde:<br>
■ **_lista-de-atributos_** é uma lista dos atributos, cujos valores são retornados pela consulta;<br>
■ **_lista-de-relações_** é uma lista dos nomes das relações, que são fontes de dados para a consulta;<br>
■ **_predicado-de-seleção_** é uma expressão condicional, que seleciona as _tuplas_ para o processamento consulta.

### Exemplo 01: Iniciando ...
#### Quais os funcionários da empresa?

|Álgebra Relacional|SQL|
|-|-|
|FUNCIONARIO<br><br>σ <sub>1=1</sub> (FUNCIONARIO)<br><br>σ <sub>Cpf=Cpf</sub> (FUNCIONARIO)|SELECT * <br>FROM FUNCIONARIO|

### Exemplo 02: PROJEÇÃO
#### Qual o CPF, primeiro nome e último nome dos funcionários da empresa?

|Álgebra Relacional|SQL|
|-|-|
|π <sub>Cpf, Pnome, Unome</sub> (FUNCIONARIO)|SELECT Cpf, Pnome, Unome <br>FROM FUNCIONARIO|

### Exemplo 03: SELEÇÃO e PROJEÇÃO
#### Qual o CPF, primeiro nome e último nome dos funcionários de sexo feminino?

|Álgebra Relacional|SQL|
|-|-|
|FUNC_F ← σ <sub>Sexo='F'</sub> (FUNCIONARIO)<br>π <sub>Cpf, Pnome, Unome</sub> (FUNC_F)|SELECT Cpf, Pnome, Unome <br>FROM FUNCIONARIO <br> WHERE Sexo='F' <br><br>SELECT Cpf, Pnome, Unome <br>FROM (SELECT * FROM FUNCIONARIO WHERE Sexo='F') AS FUNC_F|

### Exemplo 04: Valores constantes (literais) na PROJEÇÃO

|Álgebra Relacional|SQL|
|-|-|
|FUNC_F ← σ <sub>Sexo='F'</sub> (FUNCIONARIO)<br>π <sub>'Feminino', Pnome, Unome, 10, NULL</sub> (FUNC_F)|SELECT 'Feminino', Pnome, Unome, 10, NULL <br>FROM FUNCIONARIO <br> WHERE Sexo='F'|

||Pnome|Unome|||
|-|-|-|-|-|
|Feminino|Alice|Zelaya|10|NULL|
|Feminino|Jennifer|Souza|10|NULL|
|Feminino|Joice|Leite|10|NULL|

### Exemplo 05: Computação de valores na SELEÇÃO e na PROJEÇÃO

|SQL|
|-|
|SELECT Cpf, PNome, UNome, Salario, **Salario * 1.1** <br>FROM  FUNCIONARIO <br>WHERE Sexo = 'M' <br>AND Salario > 25000 <br>AND **(Salario + 5000) > (Salario * 1.1)**|

### Exemplo 06: Renomeação de atributos e relações

|SQL|
|-|
|SELECT  FUNC.Cpf, FUNC.PNome **AS 'Primeiro Nome'**, 'Valor' **AS Constante**, Salario * 1.1 **AS NovoSalario** <br>FROM FUNCIONARIO **AS FUNC**<br>WHERE Salario > 30000|
|SELECT  FUNC.Cpf, FUNC.PNome **'Primeiro Nome'**, UNome, Salario * 1.1 **NovoSalario** <br>FROM FUNCIONARIO **FUNC**<br>WHERE FUNC.Salario > 30000|

### Exemplo 07: SELEÇÃO por faixa de valores e por lista de valores

|SQL|
|-|
|SELECT Cpf, Pnome, Unome, Salario<br>FROM FUNCIONARIO<br>WHERE Salario **BETWEEN 30000 AND 50000**|
|SELECT Cpf, Pnome, Unome, Salario<br>FROM FUNCIONARIO<br>WHERE Salario **NOT BETWEEN 30000 AND 50000**|
|SELECT Cpf, Pnome, Unome, Salario<br>FROM FUNCIONARIO<br>WHERE Salario **IN (40000, 50000, 43000)**|
|SELECT Cpf, Pnome, Unome, Salario<br>FROM FUNCIONARIO<br>WHERE Salario **NOT IN (40000, 50000, 43000)**|

### Exemplo 08: Padrão de _strings_

Quais os funcionários cujo primeiro nome termina (ou não termina) com a letra **'a'**?
|SQL|
|-|
|SELECT Cpf, Pnome, Unome<br>FROM FUNCIONARIO<br>WHERE **Pnome LIKE '%a'**|
|SELECT Cpf, Pnome, Unome<br>FROM FUNCIONARIO<br>WHERE **Pnome NOT LIKE '%a'**|

Quais os funcionários cuja segunda letra primeiro nome é a letra **'a'**?<br>
∎ WHERE Pnome LIKE '_a%'

Quais os clientes cujo nome tem a palavra 'SILVA'?<br>
∎ WHERE Nome LIKE '%SILVA%'

### Exemplo 09: Exclusão de _tuplas_ duplicadas
#### Quais os salários da empresa?

|Álgebra Relacional|SQL|
|-|-|
|π <sub>Salario</sub> (FUNCIONARIO)|SELECT DISTINCT Salario<br>FROM FUNCIONARIO|
||SELECT Salario<br>FROM FUNCIONARIO<br><br>SELECT ALL Salario<br>FROM FUNCIONARIO|

### Exemplo 10: Funções para _strings_ e datas

Em geral, os Sistemas Gerenciadores de Bancos de Dados (SGBDs) incluem funções para a formação de _strings_ e datas:
- Entretanto, o nome e a sintaxe das funções são específicas de cada SGBD.

O exemplo abaixo se refere a algumas das funções **específicas** do Sistema **MariaDB**.

|SQL|
|-|
|SELECT <br>&nbsp;&nbsp;&nbsp;&nbsp;CONCAT(Pnome, ' ', Minicial, ' ', Unome) AS 'Nome Completo', <br>&nbsp;&nbsp;&nbsp;&nbsp;CHAR_LENGTH(Pnome) AS 'Tamanho do Primeiro Nome', <br>&nbsp;&nbsp;&nbsp;&nbsp;Datanasc AS 'Data de Nascimento', <br>&nbsp;&nbsp;&nbsp;&nbsp;YEAR(NOW()) - <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;YEAR(Datanasc) - <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;CASE <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;WHEN (YEAR(NOW()) > YEAR(Datanasc)) AND <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;( (MONTH(NOW())*100 + DAY(NOW())) < ((MONTH(Datanasc)*100 + DAY(Datanasc))) ) <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;THEN 1 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ELSE 0 <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;END AS 'Idade'<br>FROM FUNCIONARIO|

### Exemplo 11: PRODUTO CARTESIANO

|Álgebra Relacional|SQL|
|-|-|
|FUNCIONARIO ⨯ DEPARTAMENTO|SELECT * FROM FUNCIONARIO, DEPARTAMENTO<br><br>SELECT * FROM FUNCIONARIO CROSS JOIN DEPARTAMENTO|

### Exemplo 12: JUNÇÃO ... primeiro exemplo ... 
#### Qual o primeiro nome, último nome e endereço dos funcionários que trabalham no departamento 'Pesquisa'?

|Álgebra Relacional|SQL|
|-|-|
|DEPTO_P ← σ <sub>Dnome='Pesquisa'</sub> (DEPARTAMENTO)<br>AUX ← FUNCIONARIO ⨝ <sub>Dnr=Dnumero</sub> DEPTO_P<br>π <sub>Pnome, Unome, Salario</sub> (AUX)|SELECT Pnome, Unome, Salario <br>FROM FUNCIONARIO, DEPARTAMENTO <br>WHERE Dnr=Dnumero AND Dnome='Pesquisa'<br><br>SELECT Pnome, Unome, Salario <br>FROM FUNCIONARIO JOIN DEPARTAMENTO <br>&nbsp;&nbsp;&nbsp;&nbsp;ON Dnr=Dnumero <br>WHERE Dnome='Pesquisa'|

### Pratique ...

Veja o conteúdo do arquivo [empresa.sql](../data/empresa.sql):
- O conteúdo do arquivo possui comandos para a **definição** (criação das estruturas de dados) e **construção** (carga inicial) do **BD Empresa**.
- Há comandos DDL da SQL: CREATE TABLE e ALTER TABLE.
- Há comandos DML da SQL: INSERT.

Vamos executar o conteúdo do arquivo [empresa.sql](../data/empresa.sql), tal que possamos ter um banco de dados em nosso estudo sobre a SQL. Os passos abaixo se referem à ferramenta _SQLiteOnline_, entretanto você pode usar outra ferramenta de sua preferência.

1. Iniciar a _interface_ em https://sqliteonline.com/.
1. Selecionar um SGBD: **MariaDB** ou **PostgreSQL**.
1. Conectar-se ao SGBD selecionado (clique em **_click to connect_**).
1. Copiar o conteúdo do arquivo [empresa.sql](../data/empresa.sql) e colar na área de comandos SQL (parte central da _interface_).
1. Clicar em **&#x27A4;Run** (no _menu_ superior), para executar o conteúdo do arquivo.
1. Checar no menu à esquerda (no SGBD selecionado) se as relações (tabelas) do **BD Empresa** foram criadas.
1. Limpar a área de comandos SQL (parte central da _interface_).

Pronto, o **BD Empresa** foi criado e está pronto para ser manipulado (usado).<br>
**Execute cada consulta apresentada nos exemplos acima**

### Exercício

1. Baseado na [ilustração do BD Empresa](../media/fig-mr-2.jpg), escreva em SQL a consulta:<br>
Para cada dependente, apresente o nome do dependente e o primeiro e último nomes do funcionário responsável pelo dependente, mas restrito aos dependentes em que:
- as duas primeiras letras do primeiro nome do dependente ou do empregado responsável são as mesmas do seu primeiro nome (se refere a você, que é discente da disciplina), e
- as duas últimas letras do primeiro nome do dependente ou do empregado responsável são as mesmas do seu primeiro nome (se refere a você, que é discente da disciplina), e
- o tamanho do primeiro nome do dependente ou do empregado responsável é o mesmo que o tamanho do seu primeiro nome (se refere a você, que é discente da disciplina) 
- obs.: use a função **CHAR_LENGTH(att)** para calcular o tamanho da cadeia (valor) para o atributo _att_.
