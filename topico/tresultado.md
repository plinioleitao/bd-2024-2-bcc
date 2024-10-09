## Resultado

Clique [AQUI](../media/bd-2024-2-bcc-resumo.pdf) para ver as notas.

#### Avaliação em 04/09/2024

(F) A definição de dados normalmente... (V) Um sistema de banco de dados contém... (F) A construção do banco de dados envolve... (F) A definição do banco de dados envolve... (F) Os metadados em geral alteram... (V) Descrição, definição, catálogo... (V) Um SGBD é um elemento... (V) Os metadados possuem propósito... (V) Uma visão pode ser um subconjunto... (V) Um banco de dados normalmente tem muitos... (F) Uma transação é uma unidade lógica que representa... (V) O SGBD permite que várias... (V) O dado redundante eleva... (F) O programador de aplicação, necessariamente... (V) Transações de banco de dados são em geral... (V) A execução de uma transação inclui... (V) Tipo de dados é um exemplo... (V) A manipulação de banco de dados envolve... (V) Estado de banco de dados e instância... (V) O esquema do banco de dados é uma representação...

#### Avaliação em 11/09/2024

(V) Um esquema de dados descreve... (V) Modelo de dados e esquema de banco de dados... (F) Independência física de dados é a capacidade... (F) Independência lógica de dados é a capacidade... (F) O banco de dados possui a sequência... (F) O log denota os dados... (V) Um item de dados pode ser inserido... (V) Índices são estruturas... (V) SGBD é de uso geral... (V) Metadados são específicos... (V) O dado redundante eleva... (F) O programador de aplicação... (V) A definição do banco de dados envolve... (F) A manipulação do banco de dados envolve... (V) O escalonamento serial... (F) Há n! escalonamentos... (F) Abstração de dados se refere... (V) A abordagem de banco de dados... (F) Tipo de dado descreve... (V) Um modelo de dados representa...

#### Avaliação em 18/09/2024

(F) Uma tupla representa um conjunto... (V) O domínio do atributo Ai, denotado... (V) Um método comum... (F) Uma relação r(R), também denominada... (F) Cada _n-tupla_ t é um conjunto... (V) O i-ésimo valor na tupla... (F) Uma relação r(R) contém... (V) Na restrição de domínio... (F) Na restrição de chave... (V) Um esquema de banco de dados relacional é um... (F) Segundo a restrição de integridade de entidade... (V) Os atributos da chave estrangeira... (F) Chave primária e chave estrangeira... (F) Uma superchave pode ser... (V) Um esquema de relação possui pelo... (F) Uma superchave é uma... (V) Pode haver relacionamento (associação) entre duas _tuplas_ de relações distintas. (V) Pode haver relacionamento (associação) entre duas _tuplas_ da mesma relação. (V) O modelo relacional é um modelo em nível... (F) É comum a analogia entre relação e tabela...

#### Avaliação em 25/09/2024

|Resposta|Operação|
|-|-|
|12|Incluir a tupla <NULL,"Boné",12.00,"vestuário"> em PRODUTO|
|03|Incluir a tupla <1212,2.10,"Borracha","papel"> em PRODUTO|
|08|Alterar a tupla <"papel","Papelaria e escritório"> para <"papeis","Papelaria e Outros"> em CATEGORIA|
|08|Excluir a tupla <"papel","Papelaria e escritório"> em CATEGORIA|
|02|Incluir a tupla <1212,"Borracha",2.10,"papel"> em PRODUTO|
|08|Alterar a tupla <"papel","Papelaria e escritório"> para <"papelaria","Papelaria e Escritório"> em CATEGORIA|
|00|Excluir a tupla <1111,"Bola",20.00,"esporte"> em PRODUTO|

#### Avaliação em 02/10/2024

1. π Pnome ( FUNCIONARIO ⋈ Cpf=Fcpf  DEPENDENTE )

2. π Cpf, Pnome ( FUNCIONARIO ⋈ Cpf=Fcpf   TRABALHA_EM )

3. SUP_E_GER &#8592; π Cpf_gerente ( FUNCIONARIO ⋈ Cpf_supervisor=Cpf_gerente  DEPARTAMENTO )<br>π Pnome ( FUNCIONARIO ⨝ Cpf=Cpf_gerente SUP_E_GER )<br>OU<br>SUPER &#8592; π S.Cpf, S.Pnome ( ρ F (FUNCIONARIO) ⋈ F.Cpf_supervisor = S.Cpf ρ S (FUNCIONARIO) )<br>π Pnome ( SUPER ⨝ Cpf=Cpf_gerente DEPARTAMENTO )

#### Avaliação em 09/10/2024

2. TEMP1 &#8592; π TE1.Fcpf  (<br>&nbsp;&nbsp;&nbsp;&nbsp;σ TE1.Pnr ≠ TE2.Pnr (<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ρ TE1 TRABALHA_EM<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;⨝ TE1.Fcpf = TE2.Fcpf ρ TE2 TRABALHA_EM ) )<br>π Cpf, Pnome ( TEMP1 ⨝ Fcpf = Cpf FUNCIONARIO )

3. EXCETO_MAIOR_DATA &#8592; π D1.Data_inicio_gerente<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;( ρ D1 DEPARTAMENTO<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;⨝ D1.Data_inicio_gerente < D2.Data_inicio_gerente<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ρ D2 DEPARTAMENTO )<br>MAIOR_DATA &#8592; π Data_inicio_gerente ( DEPARTAMENTO ) &#8213; EXCETO_MAIOR_DATA<br>CPF_MAIOR_DATA &#8592; π Cpf_gerente ( MAIOR_DATA * DEPARTAMENTO )<br>π Pnome ( CPF_MAIOR_DATA ⨝ Cpf_gerente = Cpf FUNCIONARIO )

&nbsp;&nbsp;&nbsp;&nbsp;
