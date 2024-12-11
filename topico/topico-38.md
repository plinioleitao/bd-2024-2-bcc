## [Tópico T38] - Mapeamento MER para MR (parte 4)
###### *by Prof. Plinio Sa Leitao-Junior (INF/UFG)*

> Motivação: Qual o esquema lógico pertinente ao DER abaixo:

<img src="../media/fig-der-especializacao-3.jpg" width="400">

### Regra 08 - Mapeamento de Especialização/Generalização

Para ilustrar, considere o esquema conceitual (DER) do **BD simples**, mostrado a seguir.

<img src="../media/fig-der-especializacao-4.jpg" width="400">

Como mapear cada hierarquia de generalização/especialização?<br>
Abstratamente, considere que:
- **SUPER** é a superclasse que possui **n+1** atributos - **{k, a<sub>1</sub>, ..., a<sub>n</sub>}** - tal que **k** é o atributo chave;
- Há **m** subclasses **{SUB<sub>1</sub>, SUB<sub>2</sub>, ..., SUB<sub>m</sub>}**.

#### OPÇÃO 1: Relações múltiplas - superclasse e subclasses

- Alternativa adequada para **qualquer generalização** (total ou parcial) e **qualquer especialização** (disjunta ou sobreposta).
- Criar uma relação **R_SUPER** para a superclasse **SUPER**:
  - com os atributos **Attrs(R_SUPER) = {k, a<sub>1</sub>,…, a<sub>n</sub>}**; e
  - **PK(R_SUPER) = k**.
- Criar uma relação **R_SUB<sub>i</sub>** para cada subclasse **SUB<sub>i</sub>**, **1 ≤ i ≤ m**:
  - os atributos **Attrs(R_SUB<sub>i</sub>) = {k} ∪ {atributos de SUB<sub>i</sub>}**;
  - **PK(R_SUB<sub>i</sub>) = k**.

Sobre o **BD Simples**, a aplicação desta opção resulta em:

|Esquema de relação|
|-|
|CLIENTE (CodCli, Fone)<br>CLIENTE (CodCli) IS PRIMARY KEY|
|PESSOAJURIDICA (CodCli, CGC, RazaoSocial)<br>PESSOAJURIDICA (CodCli) IS PRIMARY KEY<br>PESSOAJURIDICA (CodCli) REFERENCES CLIENTE (CodCli)|
|PESSOAFISICA (CodCli, CPF, Nome, DataNasc, Sexo)<br>PESSOAFISICA (CodCli) IS PRIMARY KEY<br>PESSOAFISICA (CodCli) REFERENCES CLIENTE (CodCli)|

#### OPÇÃO 2: Relações múltiplas - restrito a subclasses

- Alternativa adequada para **generalização total** e **especialização exclusiva**:
  - se a especialização se sobrepõe, a mesma entidade pode ser duplicada em várias relações.
- Criar uma relação **R_SUB<sub>i</sub>** para cada subclasse **SUB<sub>i</sub>**, **1 ≤ i ≤ m**:
  - os atributos **Attrs(R_SUB<sub>i</sub>) = {k, a1,…, an} ∪ {atributos de SUB<sub>i</sub>}**;
  - **PK(R_SUB<sub>i</sub>) = k**.

Sobre o **BD Simples**, a aplicação desta opção resulta em:

|Esquema de relação|
|-|
|PESSOAJURIDICA (CodCli, Fone, CGC, RazaoSocial)<br>PESSOAJURIDICA (CodCli) IS PRIMARY KEY|
|PESSOAFISICA (CodCli, Fone, CPF, Nome, DataNasc, Sexo)<br>PESSOAFISICA (CodCli) IS PRIMARY KEY|

#### OPÇÃO 3: Relação única, com um atributo de tipo de especialização

- Alternativa adequada para **especialização exclusiva**:
  - pode haver elevado número de valores nulos, se há muitos atributos específicos nas subclasses.
- Criar uma única relação **R_SUPER** para ambas, a superclasse e todas as subclasses **SUB<sub>i</sub>**, **1 ≤ i ≤ m**:
  - os atributos **Attrs(R_SUPER) = {k, a1,…, an} ∪ {atributos de SUB<sub>1</sub>} ∪ {atributos de SUB<sub>2</sub>} ∪ ... ∪ {atributos de SUB<sub>m</sub>} ∪ {t}**;
  - o atributo **t** é chamado **atributo de tipo** (ou _atributo discriminante_), cujo valor indica a subclasse à qual cada _tupla_ pertence, se houver;
  - **PK(R_SUPER) = k**.

Sobre o **BD Simples**, a aplicação desta opção resulta em:

|Esquema de relação|
|-|
|PESSOA (CodCli, **Tipo**, Fone, CGC, RazaoSocial, CPF, Nome, DataNasc, Sexo)<br>PESSOA (CodCli) IS PRIMARY KEY|

#### OPÇÃO 4: Relação única, com multiplos atributos de tipo de especialização

- Alternativa adequada para especialização cujas subclasses estão sobrepostas.
- Criar uma única relação **R_SUPER** para ambas, a superclasse e todas as subclasses **SUB<sub>i</sub>**, **1 ≤ i ≤ m**:
  - os atributos **Attrs(R_SUPER) = {k, a1,…, an} ∪ {atributos de SUB<sub>1</sub>} ∪ {atributos de SUB<sub>2</sub>} ∪ ... ∪ {atributos de SUB<sub>m</sub>} ∪ {t<sub>1</sub>, t<sub>2</sub>,…, t<sub>m</sub>}**;
  - Cada **t<sub>i</sub>**, **1 ≤ i ≤ m**, é um atributo do tipo booleano que indica se uma _tupla_ pertence ou não à subclasse **SUB<sub>i</sub>**.
  - **PK(R_SUPER) = k**.

Sobre o **BD Simples**, a aplicação desta opção resulta em:

|Esquema de relação|
|-|
|PESSOA (CodCli, **Tipo_pessoa_fisica**, **Tipo_pessoa_juridica**, Fone, CGC, RazaoSocial, CPF, Nome, DataNasc, Sexo)<br>PESSOA (CodCli) IS PRIMARY KEY|
