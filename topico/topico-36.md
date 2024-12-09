## [Tópico T36] - Mapeamento MER para MR (parte 2)
###### *by Prof. Plinio Sa Leitao-Junior (INF/UFG)*

O conteúdo apresentado usa o esquema conceitual do **BD Empresa**, conforme abaixo.

<img src="../media/fig-der-empresa.jpg" width="450">

### Mapeamento de Tipo de Relacionamento Binário

Seja o tipo de relacionamento binário **R** no esquema conceitual (esquema ER), conforme a figura abaixo:
- **E<sub>1</sub>** e **E<sub>2</sub>** são os tipos de entidade que participam de **R**.
- **T<sub>1</sub>** e **T<sub>2</sub>** corrspondem às relações mapeadas a partir de **E<sub>1</sub>** e **E<sub>2</sub>**, respectivamente.

<img src="../media/fig-mapeamento-relacionamento.jpg" width="400">

> Pergunta: Como mapear o tipo de relacionamento **R** para o modelo relacional?
>> Resposta: As decisões de projeto em nível conceitual impactam na forma como o mapeamento é executado.

Por exemplo, a restrição de participação (parcial ou total) e a restrição de cardinalidade (1:1, 1:N ou M:N) guiam as decisões em nível lógico (deccisões para o modelo relacional).

Em geral, **há três abordagens** de mapeamento possíveis:
- **Abordagem de chave estrangeira**:
  - O tipo de relacionamento é mapeado pela inclusão de uma chave estrangeira na relação **T<sub>1</sub>** ou na relação **T<sub>2</sub>**:
    - por exemplo, considere que a chave estrangeira foi inserida em **T<sub>1</sub>**;
    - então, a chave estrangeira em **T<sub>1</sub>** referencia a chave primária de **T<sub>2</sub>**;
    - incluir na relação **T<sub>1</sub>** todos os atributos simples (ou componentes simples de atributos compostos) do tipo de relacionamento **R**.

<img src="../media/fig-mapeamento-relacionamento-2.jpg" width="200">

- **Abordagem de fusão de relações**:
  - O tipo de relacionamento é mapeado pela fusão das relações **T<sub>1</sub>** e **T<sub>2</sub>**:
    - a _relação de fusão_ substitui **T<sub>1</sub>** e **T<sub>2</sub>**;
    - a _relação de fusão_ terá todos os atributos de **T<sub>1</sub>** e de **T<sub>2</sub>**;
    - a _relação de fusão_ terá como chave primária e chave primária de **T<sub>1</sub>** ou a chave primária de **T<sub>2</sub>**.
  - Incluir na _relação de fusão_ todos os atributos simples (ou componentes simples de atributos compostos) do tipo de relacionamento **R**.

<img src="../media/fig-mapeamento-relacionamento-3.jpg" width="200">

- **Abordagem de criação de relação**:
  - O tipo de relacionamento é mapeado pela criação de uma nova relação:
    - a _nova relação_ possui duas chaves estrangeiras:
      - uma chave estrangeira referencia a chave primária de **T<sub>1</sub>**; e
      - uma chave estrangeira referencia a chave primária de **T<sub>2</sub>**.
    - incluir na _nova relação_ todos os atributos simples (ou componentes simples de atributos compostos) do tipo de relacionamento **R**.
  - Abordagem que promove o aumento de **junções**:
    - uma junção extra é requerida quando combina _tuplas_ (junção) de **T<sub>1</sub>** e **T<sub>2</sub>**.

<img src="../media/fig-mapeamento-relacionamento-4.jpg" width="250">

### Regra 03 - Mapeamento de Tipo de Relacionamento Binário 1:1

|Abordagem|Comentário|
|-|-|
|**Chave estrangeira**|Alternativa em geral preferida.<br>Sobre a decisão para incluir a chave estrangeira em **T<sub>1</sub>** ou em **T<sub>2</sub>**:<br>&#9745; preferir a relação que corresponde ao tipo de entidade (**E<sub>1</sub>** ou **E<sub>2</sub>**) com participação total em **R**;<br>&#9728; se ambos **E<sub>1</sub>** ou **E<sub>2</sub>** têm participação parcial, preferir a relação com menor número de _tuplas_.|
|**Fusão de relações**|Alternativa possível quando ambos os tipos de entidade (**E<sub>1</sub>** ou **E<sub>2</sub>**) têm participação total em **R**:<br>&#9745; as duas relações - **T<sub>1</sub>** e **T<sub>2</sub>** - terão exatamente o mesmo número de _tuplas_,<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;em todos os momentos.|
|**Criação de relação**|A chave primária da nova relação será uma das chaves estrangeiras:<br>&#9745; a chave estrangeira referencia a chave primária de **T<sub>1</sub>**; OU<br>&#9745; a chave estrangeira referencia a chave primária de **T<sub>2</sub>**.|

Sobre o BD Empresa, a aplicação desta regra resulta em (_realce em **negrito**_):
|Esquema de relação|
|-|
|FUNCIONARIO (Pnome, Minicial, Unome, Cpf, Datanasc, Endereco, Sexo, Salario)<br>FUNCIONARIO (Cpf) IS PRIMARY KEY|
|DEPARTAMENTO (Dnome, Dnumero, **Cpf_gerente**, **Data_inicio_gerente**)<br>DEPARTAMENTO (Dnumero) IS PRIMARY KEY<br>**DEPARTAMENTO (Cpf_gerente) REFERENCES FUNCIONARIO (Cpf)**|
|PROJETO (Projnome, Projnumero, Projlocal)<br>PROJETO (Projnumero) IS PRIMARY KEY|
|DEPENDENTE (Fcpf, Nome_dependente, Sexo, Datanasc, Parentesco)<br>DEPENDENTE (Fcpf, Nome_dependente) IS PRIMARY KEY<br>DEPENDENTE (Fcpf) REFERENCES FUNCIONARIO (Cpf)|

### Regra 04 - Mapeamento de Tipo de Relacionamento Binário 1:N

|Abordagem|Comentário|
|-|-|
|**Chave estrangeira**|Alternativa em geral preferida.<br>**(1)** Identificar a relação (**T<sub>1</sub>** ou **T<sub>1</sub>**) que corresponde ao tipo de entidade (**E<sub>1</sub>** ou **E<sub>1</sub>**) que representa o tipo de entidade no lado **N** do tipo de relacionamento **R**.<br>&#9745; Se a relação identificada for **T<sub>1</sub>**, incluir em **T<sub>1</sub>** uma chave estrangeira que referencia a chave primária de **T<sub>2</sub>**.<br>&#9745; Se a relação identificada for **T<sub>2</sub>**, incluir em **T<sub>2</sub>** uma chave estrangeira que referencia a chave primária de **T<sub>1</sub>**.|
|**Fusão de relações**|_Alternativa não aplicável_.|
|**Criação de relação**|A chave primária da nova relação é a chave estrangeira que referencia a relação que corresponde:<br>&#9745; ao tipo de entidade - **E<sub>1</sub>** ou **E<sub>1</sub>** - no lado **N** do tipo de relacionamento.|

Sobre o BD Empresa, a aplicação desta regra resulta em (_realce em **negrito**_):
|Esquema de relação|
|-|
|FUNCIONARIO (Pnome, Minicial, Unome, Cpf, Datanasc, Endereco, Sexo, Salario, **Cpf_supervisor**, **Dnr**)<br>FUNCIONARIO (Cpf) IS PRIMARY KEY<br>**FUNCIONARIO (Cpf_supervisor) REFERENCES FUNCIONARIO (Cpf)**<br>**FUNCIONARIO (Dnr) REFERENCES DEPARTAMENTO (Dnumero)**|
|DEPARTAMENTO (Dnome, Dnumero, Cpf_gerente, Data_inicio_gerente)<br>DEPARTAMENTO (Dnumero) IS PRIMARY KEY<br>DEPARTAMENTO (Cpf_gerente) REFERENCES FUNCIONARIO (Cpf)|
|PROJETO (Projnome, Projnumero, Projlocal, **Dnum**)<br>PROJETO (Projnumero) IS PRIMARY KEY<br>**PROJETO (Dnum) REFERENCES DEPARTAMENTO (Dnumero)**|
|DEPENDENTE (Fcpf, Nome_dependente, Sexo, Datanasc, Parentesco)<br>DEPENDENTE (Fcpf, Nome_dependente) IS PRIMARY KEY<br>DEPENDENTE (Fcpf) REFERENCES FUNCIONARIO (Cpf)|

### Regra 05 - Mapeamento de Tipo de Relacionamento Binário M:N

|Abordagem|Comentário|
|-|-|
|**Chave estrangeira**|_Alternativa não aplicável_.|
|**Fusão de relações**|_Alternativa não aplicável_.|
|**Criação de relação**|A chave primária da nova relação é composta por ambas: <br>&#9745; a chave estrangeira que referencia a relação **T<sub>1</sub>**; e<br>&#9745; a chave estrangeira que referencia a relação **T<sub>2</sub>**.|

Sobre o BD Empresa, a aplicação desta regra resulta em (_realce em **negrito**_):
|Esquema de relação|
|-|
|FUNCIONARIO (Pnome, Minicial, Unome, Cpf, Datanasc, Endereco, Sexo, Salario, Cpf_supervisor, Dnr)<br>FUNCIONARIO (Cpf) IS PRIMARY KEY<br>FUNCIONARIO (Cpf_supervisor) REFERENCES FUNCIONARIO (Cpf)<br>FUNCIONARIO (Dnr) REFERENCES DEPARTAMENTO (Dnumero)|
|DEPARTAMENTO (Dnome, Dnumero, Cpf_gerente, Data_inicio_gerente)<br>DEPARTAMENTO (Dnumero) IS PRIMARY KEY<br>DEPARTAMENTO (Cpf_gerente) REFERENCES FUNCIONARIO (Cpf)|
|PROJETO (Projnome, Projnumero, Projlocal, Dnum)<br>PROJETO (Projnumero) IS PRIMARY KEY<br>PROJETO (Dnum) REFERENCES DEPARTAMENTO (Dnumero)|
|DEPENDENTE (Fcpf, Nome_dependente, Sexo, Datanasc, Parentesco)<br>DEPENDENTE (Fcpf, Nome_dependente) IS PRIMARY KEY<br>DEPENDENTE (Fcpf) REFERENCES FUNCIONARIO (Cpf)|
|**TRABALHA_EM (Fcpf, Pnr, Horas)<br>TRABALHA_EM (Fcpf, Pnr) IS PRIMARY KEY<br>TRABALHA_EM (Fcpf) REFERENCES FUNCIONARIO (Cpf)<br>TRABALHA_EM (Pnr) REFERENCES PROJETO (Projnumero)**|
