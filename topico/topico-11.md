## [Tópico T11] - Álgebra Relacional - Fundamentos e Primeiras operações
###### *by Prof. Plinio Sa Leitao-Junior (INF/UFG)*

A ***Álgebra Relacional*** é uma _linguagem formal_ para o Modelo Relacional (MR):
- É considerada parte integrante do próprio Modelo de Dados Relacional.
- Possui um ***conjunto de operações***, que tipicamente são aplicadas a **esquemas lógicos** do MR.
- As operações podem ser empregadas para a manipulação de dados: consulta, inclusão, alteração e exclusão de dados.

Sobre as **Operações da Álgebra Relacional**:
- A **entrada** de uma operação pode ser:
  - uma única relação (tabela) - operação unária; ou
  - duas relações - operação binária.
- A **saída** de uma operação é uma nova relação:
  - se for uma operação de consulta (solicitação de recuperação), a operação retorna em uma nova relação (mas não modifica o [conteúdo do] banco de dados);
  - se for uma operação de atualização (inserção, remoção ou modificação de _tuplas_), a operação potenciamente altera o [conteúdo do] banco de dados (mas não retorna qualquer relação).
- Operações podem ser (usualmente são) aninhadas, tal que o resultado de uma operação é a entrada de outra operação.
- Em termos formais, há dois grupos de operações:
  - baseada na teoria matemática de conjuntos, tais como UNIÃO, INTERSEÇÃO, DIFERENÇA e PRODUTO CARTESIANO;
  - criada especificamente para bancos de dados relacionais, tais como  SELEÇÃO, PROJEÇÃO e JUNÇÃO, entre outras.

### Ilustração do BD Simples

Para exemplificar algumas das operações da Álgebra Relacional, considere a ilustração a seguir, referente a um banco de dados simples (**BD Simples**).

CATEGORIA
| <ins>CodCateg</ins> | Nome |
|-|-|
| **papel** | Papelaria e escritório |
| **esporte** | Material esportivo |

PRODUTO
| <ins>CodProduto</ins> | Descrição | Preço | _Categ_ |
|-|-|-|-|
| **1234** | Lápis | 0.70 | _papel_ |
| **1111** | Bola | 20.00 | _esporte_ |
| **2222** | Caneta | 1.20  | _papel_ |
| **1212** | Meião | 12.40 | _esporte_ |
| **1112** | Viseira| 12.40 | _esporte_ |

### Operação SELEÇÃO (SELECT)

A **Operação SELEÇÃO** é usada para obter um subconjunto das _tuplas_ de uma relação, as quais devem satisfazer a um **predicado de seleção** (condição de seleção):
- O símbolo **σ** identifica a operação.
- O número de _tuplas_ resultante da operação é igual ou menor ao número de _tuplas_ da entrada.

**[Pergunta]** A Operação SELEÇÃO é comutativa ?<br>
&#8718; noutras palavras, σ<cond<sub>1</sub>>(σ<cond<sub>2</sub>>(R)) = σ<cond<sub>2</sub>>(σ<cond<sub>1</sub>>(R)) ?<br>
**[Pergunta]** Uma sequência (cascata) de operações pode ser combinada em um única operação ?<br>
&#8718; noutras palavras, σ<cond<sub>1</sub>>(σ<cond<sub>2</sub>>(... (σ<cond<sub>n></sub>(R)) ...)) = σ<cond<sub>1</sub>> AND<cond<sub>2</sub>> AND...AND <cond<sub>n</sub>>(R) ?

#### SELEÇÃO Exemplo 1:
A expressão **σ <sub>Categ="papel"</sub> (PRODUTO)** resulta em
| CodProduto | Descrição | Preço | _Categ_   |
|-|-|-|-|
| **1234** | Lápis | 0.70 | _papel_ |
| **2222** | Caneta | 1.20  | _papel_ |

#### SELEÇÃO Exemplo 2:
A expressão **σ <sub>Categ="esporte" &#x0245; Preço>15.00</sub> (PRODUTO)** resulta em
| CodProduto | Descrição | Preço | _Categ_   |
|-|-|-|-|
| **1111** | Bola | 20.00 | _esporte_ |

### Operação PROJEÇÃO (PROJECT)

A **Operação PROJEÇÃO** é empregada para selecionar atributos de uma relação (colunas de uma tabela):
- O símbolo **π** identifica a operação.
- O número de _tuplas_ resultante da operação é igual ou menor ao número de _tuplas_ da entrada.

**[Pergunta]** A Operação PROJEÇÃO é comutativa ?<br>
&#8718; noutras palavras, π<list<sub>1</sub>>(π<list<sub>2</sub>>(R)) = π<list<sub>2</sub>>(π<list<sub>1</sub>>(R)) ?<br>

#### PROJEÇÃO Exemplo 1:
A expressão **π <sub>CodProduto, Preço</sub> (PRODUTO)** resulta em
| CodProduto | Preço |
|-|-|
| 1234 | 0.70 |
| 1111 | 20.00 |
| 2222 | 1.20 |
| 1212 | 12.40 |
| 1112 | 12.40 |

#### PROJEÇÃO Exemplo 2:
A expressão **π <sub>Preço, Categ</sub> (PRODUTO)** resulta em
| Preço | Categ |
|-|-|
| 0.70 | papel |
| 20.00 | esporte |
| 1.20  | papel |
| 12.40 | esporte |
| ~~12.40~~ | ~~esporte~~ |

IMPORTANTE:<br>
No Exemplo 2, a última _tupla_ é excluída do resultado da consulta.<br>
Por quê?

**Combinando operações:**<br>
Neste exemplo, as operações SELEÇÃO e PROJEÇÃO são combinadas em um única expressão.<br>
A expressão **π <sub>CodProduto, Preço</sub> (σ <sub>Categ="papel"</sub> (PRODUTO))** resulta em
| CodProduto | Preço |
|-|-|
| 1234 | 0.70 |
| 2222 | 1.20 |

### Operação RENOMEAÇÃO#1 (RENAME#1)

A **Operação RENOMEAÇÃO#1** aplica a renomeação da relação e/ou dos atributos do resultado de uma operação:
- A renomeação é restrita aos resultados das operações, não afeta o banco de dados.
- O símbolo **&#8592;** identifica a operação.
- As seguintes construções estão disponíveis:
  - **S &#8592; <expressão>**
  - **S(B<sub>1</sub>, B<sub>2</sub>, ... , B<sub>n</sub>) &#8592; <expressão>**

#### RENOMEAÇÃO#1 Exemplo 1:
A expressão **π <sub>CodProduto, Preço</sub> (σ <sub>Categ="papel"</sub> (PRODUTO))** pode ser reescrita como um sequência de duas expressões:<br>
&#8718; TEMP &#8592; σ <sub>Categ="papel"</sub> (PRODUTO)<br>
&#8718; RESULT &#8592; π <sub>CodProduto, Preço</sub> (TEMP)<br>
Esta sequência de expressões resulta em<br>

RESULT
| CodProduto | Preço |
|-|-|
| 1234 | 0.70 |
| 2222 | 1.20 |

#### RENOMEAÇÃO#1 Exemplo 2:
Outra alternativa para renomeação:<br>
&#8718; TEMP &#8592; σ <sub>Categ="papel"</sub> (PRODUTO)<br>
&#8718; RESULT(Código, Valor) &#8592; π <sub>CodProduto, Preço</sub> (TEMP)<br>
Esta sequência de expressões resulta em<br>

RESULT
| Código | Valor |
|-|-|
| 1234 | 0.70 |
| 2222 | 1.20 |

> No primeiro exemplo, houve a _renomeação da relação resultante_ em cada expressão:
>> TEMP e RESULT são novos nomes para o resultado de cada expressão.

> No segundo exemplo, houve **também** a _renomeação de atributos_ na segunda expressão:
>> _Código_ e _Valor_ são novos nomes dos atributos para o resultado da segunda expressão.

### Operação RENOMEAÇÃO#2 (RENAME#2)

A **Operação RENOMEAÇÃO#2** ***também*** aplica a renomeação da relação e/ou dos atributos do resultado de uma operação:
- A renomeação é restrita aos resultados das operações, não afeta o banco de dados.
- O símbolo **ρ** identifica a operação.
- As seguintes construções estão disponíveis:
  - **ρS(B<sub>1</sub>, B<sub>2</sub>, ... , B<sub>n</sub>)(<expressão>)**
  - **ρS(<expressão>)**
  - **ρ(B<sub>1</sub>, B<sub>2</sub>, ... , B<sub>n</sub>)(<expressão>)**

#### RENOMEAÇÃO#2 Exemplo 1:
Seja a sequência de expressões:<br>
&#8718; **ρ <sub>TEMP</sub> (σ <sub>Categ="papel"</sub> (PRODUTO))**<br>
&#8718; **ρ <sub>RESULT(Código, Valor)</sub> (π <sub>CodProduto, Preço</sub> (TEMP))**<br>
Esta sequência de expressões resulta em<br>

RESULT
| Código | Valor |
|-|-|
| 1234 | 0.70 |
| 2222 | 1.20 |

### Em síntese ...

O presente tópico introduziu três operações básicas da álgebra relacional, a saber: SELEÇÃO, PROJEÇÃO e RENOMEAÇÃO (#1 e #2).<br>
**O entendimento dessas operações é crucial para a apropriação de novos conteúdos da álgebra relacional.**

### Para refletir 01 ...

Em relação ao **BD Simples**, cuja ilustração está no início do presente tópico, qual o resultado da sequência de expressões da álgebra relacional?<br>
&#8718; **ρ <sub>S1</sub> (π <sub>CodProduto, Descrição, Preço, Categ</sub> (PRODUTO))**<br>
&#8718; **ρ <sub>S2</sub> (π <sub>Descrição, Preço, Categ</sub> (S1))**<br>
&#8718; **ρ <sub>S3</sub> (π <sub>Preço, Categ</sub> (S2))**<br>
&#8718; **ρ <sub>RESULT</sub> (π <sub>Categ</sub> (S3))**


### Para refletir 02 ...

Sobre o [Tópico 09](./topico-09.md), é possível uma chave estrangeira composta por vários atributos?

MUNICIPIO (<ins>Nome, Estado</ins>, Região)<br>
| <ins>Nome</ins> | <ins>Estado</ins> | Região |
|-|-|-|
| **Santa Rosa** | **GO** | Centro-oeste |
| **Goiânia** | **GO** | Centro-oeste |
| **Santa Rosa** | **MG** | Sudeste |

BAIRRO (<ins>Cidade, UF, Nome</ins>, Area)<br>
BAIRRO (Cidade, UF) REFERENCIA MUNICIPIO (Nome, Estado)<br>
| <ins>Cidade</ins> | <ins>UF</ins> | <ins>Nome</ins> | Área |
|-|-|-|-|
| **Santa Rosa** | **GO** | **Centro** | 100 |
| **Santa Rosa** | **MG** | **Oeste** | 120 |
| **Santa Rosa** | **MG** | **Centro** | 111 |

## Exercício

Um banco de dados simples (**BD Simples**) possui o seguinte esquema conceitual:<br>
_<<apenas para ilustrar, pois ainda não estudamos o modelo entidade-relacionamento (elaborar o esquema conceitual)>>_

<img src="../media/fig-der-simples-1.jpg" width="350">

O projetista de banco de dados analisou o diagrama acima e criou o esquema lógico, conforme abaixo:<br>
&#8718; PRODUTO(<ins>CodProduto</ins>, Descrição, Preço, _Categ_)<br>
&#8718; CATEGORIA(<ins>CodCateg</ins>, Nome)<br>
&#8718; PRODUTO(Categ) REFERENCIA CATEGORIA(CodCateg)<br>
Noutras palavras:<br>
&#8718; os esquemas de relação PRODUTO(CodProduto, Descrição, Preço, Categ) e CATEGORIA(CodCateg, Nome);<br>
&#8718; as chaves primárias (PKs) _CodProduto_ e _CodCateg_ para PRODUTO e CATEGORIA, respectivamente;<br>
&#8718; o atributo _Categ_ é uma chave estrangeira (FK) em PRODUTO que referencia CATEGORIA.

#### Em relação ao BD Simples, considere o "esquema lógico" (acima) e a "ilustração" (no início do tópico) para executar esta atividade.

1. Quais as restrições de integridade - _domínio_, _chave_, _integridade de entidade_ e _integridade referencial_ - são violadas em cada uma das seguintes operações?<br>
**(a)** Incluir a _tupla_ **<NULL,"Boné",12.00,"vestuário">** em PRODUTO.<br>
**(b)** Incluir a _tupla_ **<1212,"Borracha",2.10,"papel">** em PRODUTO.<br>
**(c)** Incluir a _tupla_ **<1212,2.10,"Borracha","papel">** em PRODUTO.<br>
**(d)** Alterar a _tupla_ **<"papel","Papelaria e escritório">** para **<"papeis","Papelaria e Outros">** em CATEGORIA.<br>
**(e)** Excluir a _tupla_ **<"papel","Papelaria e escritório">** em CATEGORIA.<br>
**(g)** Alterar a _tupla_ **<"papel","Papelaria e escritório">** para **<"papelaria","Papelaria e Escritório">** em CATEGORIA.<br>
**(h)** Excluir a _tupla_ **<1111,"Bola",20.00,"esporte">** em PRODUTO.

IMPORTANTE:<br>
&#8718; Para analisar cada operação, considere o banco de original conforme a ilustração. Por exemplo, para analisar a operação em (d), desconsidere possíveis modificações no banco de dados pelas operações em (a), (b) e (c).<br>
&#8718; Ao responder, calcule o somatório dos números que identificam cada restrição violada pela operação, e acrescente o número de letras do seu primeiro nome (o primeiro nome do discente):<br>
(01) Restrição de domínio<br>
(02) Restrição de chave<br>
(04) Restrição de integridade de entidade<br>
(08) Restrição de integridade referencial<br>

&#8718; Para ilustrar, se o seu primeiro nome é 'Pedro' (05 letras) e as restrições violadas são 'restrição de domínio' e 'restrição de chave', responda da seguinte forma (apenas para exemplificar):<br>
(a) **08**: Restrição de domínio , Restrição de chave<br>
(b) ...<br>
&#8718; Responda, **necessariamente**, neste formato.

