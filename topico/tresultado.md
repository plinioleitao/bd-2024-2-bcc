## Resultado

Clique [AQUI](../media/bd-2024-1-bcc-resumo.pdf) para ver as notas.

#### Avaliação em 10/04/2024
São identificadas duas chaves candidatas em ARTISTA. Um mesmo artista pode atuar em diversos eventos. Um mesmo evento pode ter diversos artistas que atuam no evento. O atributo id em ARTISTA e id em EVENTO possuem a mesma restrição de domínio.

#### Avaliação em 17/04/2024
Quantas cópias do livro intitulado ... LIVRO_COPIAS (Cod_livro) REFERENCES LIVRO (Cod_livro) ...
LIVRO_COPIAS (Cod_unidade) REFERENCES UNIDADE_BIBLIOTECA (Cod_unidade)<br>
Recupere o nome de todos os usuários ... LIVRO_EMPRESTIMOS (Nr_cartao) REFERENCES USUARIO (Nr_cartao)<br>
Para cada livro que é emprestado da unidade ... LIVRO_EMPRESTIMOS (Cod_unidade) REFERENCES UNIDADE_BIBLIOTECA (Cod_unidade) ... LIVRO_EMPRESTIMOS (Cod_livro) REFERENCES LIVRO (Cod_livro) ... LIVRO_EMPRESTIMOS (Nr_cartao) REFERENCES USUARIO (Nr_cartao)<br>
Para cada unidade da biblioteca, recupere ... LIVRO_EMPRESTIMOS (Cod_unidade) REFERENCES UNIDADE_BIBLIOTECA (Cod_unidade)<br>
Recupere o nome, endereço e número ... LIVRO_EMPRESTIMOS (Nr_cartao) REFERENCES USUARIO (Nr_cartao)<br>
Para cada livro cujo autor (ou coautor) ... LIVRO_AUTOR (Cod_livro) REFERENCES LIVRO (Cod_livro) ... LIVRO_COPIAS (Cod_unidade) REFERENCES UNIDADE_BIBLIOTECA (Cod_unidade) ... LIVRO_COPIAS (Cod_livro) REFERENCES LIVRO (Cod_livro)

#### Avaliação em 12/06/2024
1. RESULT ← π <sub>FUN.Pnome, SUP.Pnome</sub> ( ρ FUN (FUNCIONARIO) ⋈ <sub>FUN.Cpf_supervisor = SUP.CPF</sub> ρ SUP (FUNCIONARIO) )
1. JOAO_CPF ← π <sub>Cpf</sub> ( σ <sub>Pnome="Joao" &#8743; Unome="Silva"</sub> (FUNCIONARIO) )<br>
JOAO_DEPENDENTES ← π <sub>Nome_dependente</sub> ( JOAO_CPF ⋈ <sub>JOAO_CPF.Cpf = DEPENDENTE.Fcpf</sub> DEPENDENTE )<br>
TODOS_DEPENDENTES ← π <sub>Pnome, Nome_dependente</sub> ( FUNCIONARIO ⋈ <sub>FUNCIONARIO.Cpf = DEPENDENTE.Fcpf</sub> DEPENDENTE )<br>
RESULT ← TODOS_DEPENDENTES &#247; JOAO_DEPENDENTES<br>
<sup>Não considera funcionários homônimos e dependentes homônimos.</sup>
1. PROJETOS_FUNC (Cpf, Qtde_projetos) ← Fcpf ℑ CONTA Fcpf ( TRABALHA_EM )<br>
PROJETOS_FUNC_DOIS ← σ <sub>Qtde_projetos=2</sub> ( PROJETOS_FUNC )<br>
RESULT ← π <sub>Pnome</sub> ( PROJETOS_FUNC_DOIS ⋈ <sub>PROJETOS_FUNC_DOIS.Cpf = FUNCIONARIO.Cpf</sub> FUNCIONARIO )

#### Avaliação em 19/06/2024
1. SELECT DISTINCT Pessoa<br>
FROM VENDE NATURAL JOIN GOSTA<br>
WHERE Bar = 'Pipoca'<br>
&nbsp;&nbsp;&nbsp;&nbsp;_Outra alternativa ..._<br>
SELECT DISTINCT Pessoa<br>
FROM GOSTA<br>
WHERE Cerveja IN (<br>
&nbsp;&nbsp;&nbsp;&nbsp;SELECT Cerveja FROM VENDE<br>
&nbsp;&nbsp;&nbsp;&nbsp;WHERE Bar = 'Pipoca' )
2. SELECT Pessoa, COUNT(\*)<br>
FROM VENDE NATURAL JOIN GOSTA<br>
WHERE Bar = 'Pipoca'<br>
GROUP BY PESSOA<br>
HAVING COUNT(*) > 1
3. SELECT Pessoa FROM GOSTA<br>
EXCEPT<br>
SELECT Pessoa<br>
FROM VENDE NATURAL JOIN GOSTA<br>
WHERE Bar = 'Pipoca'<br>
&nbsp;&nbsp;&nbsp;&nbsp;_Outra alternativa ..._<br>
SELECT DISTINCT Pessoa<br>
FROM GOSTA<br>
WHERE NOT EXISTS (<br>
&nbsp;&nbsp;&nbsp;&nbsp;SELECT Cerveja FROM GOSTA AS INT WHERE INT.Pessoa = EXT.Pessoa<br>
&nbsp;&nbsp;&nbsp;&nbsp;INTERSECT<br>
&nbsp;&nbsp;&nbsp;&nbsp;SELECT Cerveja FROM VENDE WHERE Bar = 'Pipoca' )

#### Avaliação em 26/06/2024
1. SELECT Dnome, Count(*), AVG(Salario)<br>
FROM FUNCIONARIO JOIN DEPARTAMENTO<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ON Dnr = Dnumero<br>
GROUP BY Dnr, Dnome
2. SELECT Pnome, Unome<br>
FROM FUNCIONARIO<br>
WHERE Cpf NOT IN (<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;SELECT Fcpf FROM DEPENDENTE )
3. SELECT Pnome, Unome<br>
FROM FUNCIONARIO<br>
WHERE	Cpf IN (<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;SELECT Fcpf FROM DEPENDENTE<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;GROUP BY Fcpf<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;HAVING COUNT(\*) > 1 )<br>
AND Cpf IN (<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;SELECT Fcpf FROM TRABALHA_EM<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;GROUP BY Fcpf<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;HAVING COUNT(\*) > 1 )

#### Avaliação em 03/07/2024

(01) O tipo de relacionamento TRABALHA_PARA ... 01<br>
(01) A restrição de participação do tipo de entidade DEPENDENTE ... 01<br>
(01) Cada funcionário possui pelo menos ... 02<br>
(01) Pelo menos um empregado ... 01<br>
(01) Um empregado pode trabalhar ... 03<br>
(01) Que projetos possuem gerentes ... 03<br>
(01) Quais os departamentos ... 01<br>
(01) Qual a meta de faturamento ... 02<br>
(01) Qual a data de ingresso ... 00<br>
(01) Que supervisores são ...02<br>

#### Avaliação em 10/07/2024

1. Não existem atores ... (V) Existem alguns atores ... (F) Alguns atores podem ... (V)
2. Um filme só pode ... (V) Cada diretor foi ... (F) Nenhum produtor já ... (F)
3. Um produtor não pode ... (F) Existem filmes com ... (F)
4. Alguns produtores ... (F) Todos filmes têm ... (V)
5. Alguns filmes têm ... (F) Podem existir atores ... (V) Nenhum filme ... (F)

6. 
<img src="../media/fig-av-20240710.jpg" width="250">

#### Avaliação em 17/07/2024

1. O código e ementa ...
2. A quantidade de reprovações por média ...
3. A ementa das disciplinas com ...; Para cada aluno, os professores ...; O nome e matrícula dos alunos das ...
4. A matrícula e nome de um professor, e ...
5. A quantidade de turmas oferecidas em ...
6. Toda turma possui no máximo ...; Um aluno pode não estar  ...
7. Uma turma pode existir sem ...; Duas turmas podem ser oferecidas ...

#### Avaliação em 24/07/2024

|Mapeamento|
|-|
|ALUNO (Matr, Nome, DataEgre)<br>ALUNO (Matr) IS PRIMARY KEY|
|PROFESSOR (Matr, Nome)<br>PROFESSOR (Matr) IS PRIMARY KEY|
|TITULO (Cod, Nivel)<br>TITULO (Cod) IS PRIMARY KEY|
|DISCIPLINA (Cod, Descr, Ementa, Creditos)<br>DISCIPLINA (Cod) IS PRIMARY KEY|
|TURMA (CodDisc, Semestre, Horario, CodTurma, Se_Lab, MatrProf)<br>TURMA (CodDisc, Semestre, Horario) IS PRIMARY KEY<br>TURMA (CodDisc) REFERENCES DISCIPLINA (Cod)<br>TURMA (MatrProf) REFERENCES PROFESSOR (Matr)|
|ALUNO_TURMA (MatrAluno, CodDisc, Semestre, Horario, Freq)<br>ALUNO_TURMA (MatrAluno, CodDisc, Semestre, Horario) IS PRIMARY KEY<br>ALUNO_TURMA (MatrAluno) REFERENCES ALUNO (Matr)<br>ALUNO_TURMA (CodDisc, Semestre, Horario) REFERENCES TURMA (CodDisc, Semestre, Horario)|
|PROFESSOR_TITULO (MatrProf, CodTitulo, Area)<br>PROFESSOR_TITULO (MatrProf, CodTitulo) IS PRIMARY KEY<br>PROFESSOR_TITULO (MatrProf) REFERENCES PROFESSOR (Matr)<br>PROFESSOR_TITULO (CodTitulo) REFERENCES TITULO (Cod)|

#### Avaliação em 24/07/2024

<img src="../media/fig-av-20240731.jpg" width="350">
