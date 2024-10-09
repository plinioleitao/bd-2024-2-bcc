## [Tópico T14] - Álgebra Relacional - Divisão, Função Agregada, Agrupamento
###### *by Prof. Plinio Sa Leitao-Junior (INF/UFG)*

Para ilustrar as operações da Álgebra Relacional, considere os esquemas (conceitual e lógico) do BD Empresa.

<img src="../media/fig-der-empresa.jpg" width="400"><img src="../media/fig-esquema-logico-bdempresa.jpg" width="450">

### Operação DIVISÃO (DIVISION)

A figura abaixo ilustra a operação DIVISÃO.

<img src="../media/fig-algebra-divisao.jpg" width="150">

A operação DIVISÃO é usualmente referida como "para todas":
- Em T ← R ÷ S na figura:
  - o conjunto de atributos de **R** é **X<sub>R</sub> = {A, B}**;
  - o conjunto de atributos de **S** é **X<sub>S</sub> = {A}**;
  - os atributos de **S** são um subconjunto dos atributos de **R**, isto é, **X<sub>S</sub> ⊆ X<sub>R</sub>**;
  - o conjunto de atributos de T é **X<sub>R</sub> - X<sub>S</sub> = {B}**;
  - cada _tupla_ em **T** se refere às _tuplas_ de **R** que existem "para todas" as _tuplas_ de **S**, conforme ilustrado na figura.

#### DIVISÃO Exemplo 1:

Qual o nome dos funcionários que trabalham em todos os projetos que o "João Silva" trabalha?

|Expressão|Operação|
|-|-|
|**SILVA ← σ<sub>Pnome=‘João’ AND Unome=‘Silva’</sub>(FUNCIONARIO)**|RENOMEAÇÃO, SELEÇÃO|
|**PROJETOS_SILVA ← π<sub>Pnr</sub>(TRABALHA_EM ⋈<sub>Fcpf=Cpf</sub>SILVA)**|RENOMEAÇÃO, EQUIJUNÇÃO, PROJEÇÃO|
|**PROJETOS_TODOS_FUNCS ← π<sub>Fcpf, Pnr</sub>(TRABALHA_EM)**|RENOMEAÇÃO, PROJEÇÃO|
|**FUNC(Cpf) ← PROJETOS_TODOS_FUNCS ÷ PROJETOS_SILVA**|RENOMEAÇÃO, ***DIVISÃO***|
|**RESULT ← π<sub>Pnome, Unome</sub>(FUNC * FUNCIONARIO)**|RENOMEAÇÃO, JUNÇÃO NATURAL, PROJEÇÃO|

#### DIVISÃO Exemplo 2:

Qual o nome dos funcionários que possui o mesmo salário do "João Silva"?<br>
■ _escreva a consulta em álgebra relacional_...

### FUNÇÃO AGREGADA (AGGREGATE FUNCTION) e AGRUPAMENTO (GROUPING)

As **FUNÇÕES AGREGADAS** usam funções matemáticas para cálculos estatísticos simples nos conjuntos do banco de dados:
- O símbolo ℑ identifica a operação (_F Script_).
- A funções agregadas são CONTA (COUNT), SOMA (SUM), MÉDIA (AVERAGE), MÁXIMO (MAXIMUM) e MÍNIMO (MINIMUM).
- Alguns exemplos de aplicação das funções agregadas são:
  - qual a média dos salários dos funcionários?
  - qual a soma dos salários dos funcionários?
  - qual o número (a quantidade) de funcionários?

#### FUNÇÃO AGREGADA e AGRUPAMENTO Exemplo 1:

Qual a quantidade de funcionários bem como a soma e a média de salários da empresa?<br>
■ ℑ CONTA <sub>Cpf</sub>, SOMA <sub>Salario</sub>, MÉDIA <sub>Salario</sub> (FUNCIONARIO)

O **AGRUPAMENTO** envolve o **agrupamento de _tuplas_** em uma relação, a partir do valor um ou mais atributos:
- Os atributos usados para estabelecer o agrupamento de _tuplas_ são denominados **atributos de agrupamento**. 
- Cada grupo é formado pelas _tuplas_ que possuem o mesmo valor para os **atributos de agrupamento**.
- A sintaxe genérica para o agrupamento é:
  - **<sub>atributos-de-agrupamento</sub> ℑ <sub>lista-de-funções-agregadas</sub> (R)**
- A relação resultante é formada pelos valores dos atributos de agrupamento e pelos valores das funções agregadas.

#### FUNÇÃO AGREGADA e AGRUPAMENTO Exemplo 2:

Qual a quantidade de funcionários e média salarial por departamento da empresa?<br>
■ <sub>Dnr</sub> ℑ CONTA <sub>Cpf</sub>, MÉDIA <sub>Salario</sub> (FUNCIONARIO)<br>
OU<br>
■ ρ <sub>RESULT</sub> <sub>(Identificacao_do_departamento, Quantidade_de_empregados, Salario_medio)</sub> (<sub>Dnr</sub> ℑ CONTA <sub>Cpf</sub>, MÉDIA <sub>Salario</sub> (FUNCIONARIO))

#### FUNÇÃO AGREGADA e AGRUPAMENTO Exemplo 3:

Qual o nome dos funcionários que possuem dois ou mais dependentes?

|Expressão|Operação|
|-|-|
|RESUMO(Cpf, Qtde_depend)← <sub>Fcpf</sub> ℑ CONTA <sub>Fcpf</sub> (DEPENDENTE)|RENOMEAÇÃO, ***FUNÇÃO AGREGADA***, ***AGRUPAMENTO***|
|RESUMO_DOIS ← σ <sub>Qtde_depend >= 2</sub> (RESUMO)|RENOMEAÇÃO, SELEÇÃO|
|RESULT ← π <sub>Pnome, Unome</sub> (RESUMO_DOIS * FUNCIONARIO)|RENOMEAÇÃO, JUNÇÃO NATURAL, PROJEÇÃO|

### Exercício  

Escreva em álgebra relacional as seguintes consultas ao BD Empresa: 
1. Qual o nome dos funcionários, que possuem mais de um dependente e trabalham em mais de um projeto?
1. Qual o nome dos dependentes, cujo funcionário responsável pelo dependente é supervisor direto de mais de um funcionário?

IMPORTANTE: Use a sintaxe da Álgebra Relacional conforme os exemplos apresentados.
