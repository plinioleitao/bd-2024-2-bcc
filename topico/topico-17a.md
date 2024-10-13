## [Tópico 17a] - Álgebra Relacional - Ferramenta RelaX (parte 1)
###### *by Prof. Plinio Sa Leitao-Junior (INF/UFG)*

**RelaX** (_relational algebra calculator_) é uma calculadora de álgebra relacional, criada por Johannes Kessler (_Databases and Information Systems Group_), no Instituto de Ciência da Computação da Universidade de Innsbruck (Áustria), sob supervisão de Michael Tschuggnall e Prof. Dr. Günther Specht.

### Primeiros Passos:
1. Inicie a ferramenta via o link https://dbis-uibk.github.io/relax/landing
1. Clique em ‘Get Started’.
1. Se a _interface_ estiver em “Inglês”, você poderá trocar para “Português”.

### Seleção de um **Banco de Dados de Trabalho**:
1. Clique em ‘Datasets’ no menu do canto superior esquerdo.
1. Clique no banco de dados ‘PhotoDB’ (último banco de dados da lista).<br>O banco de dados possui o seguinte esquema (chave primária em **negrito**):
   - https://dbis-uibk.github.io/relax/calc/gist/c306ecf21c6e6d175508d3ac6b4355e7/photodb/0

|Significado|Esquema de Relação|
|-|-|
|PESSOA|persons ( **id** , lastname , firstname , birthday )|
|EMPREGADO|employees ( **personId** , salary , experience )<br>employees ( personID ) references persons ( id )|
|FOTÓGRAFO|photographers ( **employeeId** )<br>photographers ( employeeId ) references employees ( personID )|
|VENDEDOR|salespersons ( **employeeId** , areaOfExpertise )<br>salespersons ( employeeId ) references employees ( personID )|
|GERENTE|seniors ( **employeeId** , numGreyHairs , bonus )<br>seniors ( employeeId ) references employees ( personID )|
|CAMERA|cameras ( **id** , brand , model )|
|FOTO|photos ( **id** , location , unixTime , photographerId , cameraId )<br>photos ( photographerId ) references photographers ( employeeId )<br>photos ( cameraId ) references cameras ( id )|

### Execução de Expressões da Álgebra relacional

1. Selecione a aba **AlgRel**.
1. Para cada relação (_persons_, _employees_, _photographers_, _salespersons_, _seniors_, _cameras_, _photos_):
   - digite o nome da relação;
   - clique em ‘Executar consulta’;
   - observe os dados (conteúdo) de cada relação.

#### Operação SELEÇÃO

- σ id >= 90 (photos)
- σ id >= 90 ∧ photographerId = 3 (photos)
- σ id >= 90 ∨ photographerId = 3 (photos)

#### Operação PROJEÇÃO

- π firstname, birthday (persons)
- π persons.firstname, persons.birthday (persons)
- π firstname, birthday (σ birthday > date('1975-01-01') (persons))

#### Operação RENOMEAÇÃO#1

Nos exemplos abaixo, a renomeação é uma atribuição ('=', _assignment_):
- temp = ρ data_nasc←birthday (persons)<br>σ data_nasc > date('1975-01-01') (temp)
- temp = σ photographerId = 3 (photos)<br>σ id >= 90 (temp)

#### Operação RENOMEAÇÃO#2

A renomeação de atributos (colunas) ocorre pelo operador **←** :
- π pessoas.firstname, pessoas.birthday (ρ pessoas (persons))
- ρ data_nasc←birthday (persons)
- σ data_nasc > date('1975-01-01') (ρ data_nasc←birthday (persons))

#### Operações UNIÃO, INTERSEÇÃO e DIFERENÇA

1. Quais os empregados que são fotógrafos OU vendedores?
   - **π employeeId (photographers) ∪ π employeeId (salespersons)**
1. Quais os empregados que são fotógrafos E vendedores?
   - **π employeeId (photographers) ∩ π employeeId (salespersons)**
1. Quais os empregados que NÃO SÃO vendedores?
   - **π personId (employees) - π employeeId (salespersons)**
1. Quais as pessoas que NÃO SÃO empregados?
   - **π id (persons) - π personId (employees)**
1. Há erro na execução da expressão abaixo?
   - **employees ∪ salespersons**
1. Há erro na execução da expressão abaixo?
   - **π birthday (persons) ∩ π employeeId (salespersons)**

#### Operação PRODUTO CARTESIANO

O símbolo ⨯ é usado na operação:
- **employees ⨯ photographers**
- **employees ⨯ photos**

#### Operação JUNÇÃO

1. Qual o salário de cada fotógrafo?
   - **(employees) ⨝ employees.personId=photographers.employeeId (photographers)**
1. Qual o salário de cada fotógrafo?
   - **employees ⨝ personId=employeeId photographers**
1. Qual o salário de cada fotógrafo?
   - **π employeeId, salary (employees ⨝ personId=employeeId photographers)**
1. Qual o nome e o salário de cada fotógrafo?
   - **f_salario = π employeeId, salary (employees ⨝ personId=employeeId photographers)**<br>**π employeeId, firstname, salary (persons ⨝ id=employeeId f_salario)**
1. Qual o nome das pessoas que NÃO SÃO empregados?
   - **nao_empreg = π personId←id (persons) - π personId (employees)**<br>**π id, firstname, lastname (persons ⨝ id=personId nao_empreg)**

#### Operação JUNÇÃO NATURAL

O símbolo da JUNÇÃO NATURAL é **⨝**, mas sem o predicado de junção:
|JUNÇÃO|JUNÇÃO NATURAL|
|-|-|
|pessoa = π personId, firstname (ρ personId←id (persons))<br>pessoa ⨝ persons.personId=employees.personId employees|pessoa = π personId, firstname (ρ personId←id (persons))<br>**pessoa ⨝ employees**|

#### Operações FUNÇÕES AGREGADAS e AGRUPAMENTO

Algumas distinções com respeito à notação:
- O símbolo usado para AGRUPAMENTO é **γ** ;
- As FUNÇÕES AGREGADAS possui notação distinta da (mas similar à) notação adotada na disciplina.

1. Qual a quantidade de fotos por fotógrafo?
   - **γ photographerId; count(id) → quantidade_fotos (photos)**
1. Qual a quantidade de fotos por câmera?
   - **γ cameraId; count(id) → qtd_fotos (photos)**
1. Qual a quantidade de fotos por fabricante de câmera?
   - **γ brand; count(cameraId) → qtd_fotos (photos ⨝ cameraId = cameras.id cameras)**
1. Qual a quantidade de pessoas que nasceram em cada dia, independente do mês e ano?
   - Observe o uso da função **_day_**;
   - **temp = π day(birthday) → dia, id (persons)**<br>**γ dia; count(id) → qtd_pessoas (temp)**
1. Qual o total (soma) de salário de cada cargo?
   - **fotografo = π 'fotografo'→cargo, salary (employees ⨝ personId=employeeId photographers)**<br>
**vendedor  = π 'vendedor'→cargo,   salary (employees ⨝ personId=employeeId salespersons)**<br>
**gerente   = π 'gerente'→cargo,    salary (employees ⨝ personId=employeeId seniors)**<br>
**γ cargo; sum(salary)→total_salario (fotografo ∪ vendedor ∪ gerente)**

