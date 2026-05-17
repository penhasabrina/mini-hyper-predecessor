## 1. Objetivo

Este experimento utiliza o programa MINI-HYPER em Prolog para aprender automaticamente a relação `predecessor/2`, a partir de exemplos positivos, exemplos negativos e conhecimento de fundo sobre relações familiares.

A relação `predecessor(X,Y)` representa que `X` é predecessor, ou ancestral, de `Y`.

A hipótese esperada é composta por duas regras:

predecessor(X,Y) :- parent(X,Y).

predecessor(X,Y) :- parent(X,Z), predecessor(Z,Y).


A primeira regra representa o caso direto, em que `X` é pai ou mãe de `Y`.

A segunda regra representa o caso recursivo, em que `X` é pai ou mãe de alguém `Z`, e `Z` é predecessor de `Y`.

---

## 2. Arquivo utilizado

O arquivo principal utilizado foi:

mini_hyper.pl


Esse arquivo contém:

* o algoritmo MINI-HYPER;
* a base de conhecimento familiar;
* os exemplos positivos;
* os exemplos negativos;
* a hipótese inicial;
* os literais de fundo usados para refinamento.

---

## 3. Base de conhecimento

A base de conhecimento usada foi:

parent(pam, bob).
parent(tom, bob).
parent(tom, liz).
parent(bob, ann).
parent(bob, pat).
parent(pat, jim).


Essa base define relações diretas de parentesco.

---

## 4. Exemplos positivos

Os exemplos positivos indicam casos verdadeiros de `predecessor/2`:

ex(predecessor(pam, bob)).
ex(predecessor(pam, jim)).
ex(predecessor(tom, ann)).
ex(predecessor(tom, jim)).
ex(predecessor(tom, liz)).


Esses exemplos mostram tanto relações diretas quanto relações indiretas.

Por exemplo:

ex(predecessor(pam, jim)).


é verdadeiro porque:

parent(pam, bob).
parent(bob, pat).
parent(pat, jim).

Logo, `pam` é predecessor de `jim`.

---

## 5. Exemplos negativos

Os exemplos negativos indicam casos que não devem ser provados:

nex(predecessor(liz, bob)).
nex(predecessor(pat, bob)).
nex(predecessor(pam, liz)).
nex(predecessor(liz, jim)).
nex(predecessor(liz, liz)).

Eles servem para impedir que o algoritmo aprenda uma hipótese muito genérica.

---

## 6. Hipótese inicial

A hipótese inicial foi definida com duas cláusulas vazias, pois a solução esperada possui duas regras: uma regra direta e uma regra recursiva.

start_hyp([
    [predecessor(X1, Y1)] / [X1, Y1],
    [predecessor(X2, Y2)] / [X2, Y2]
]).


---

## 7. Execução

Primeiro, abra o terminal na pasta onde está o arquivo:

cd ~/Downloads


Depois, execute o SWI-Prolog com o arquivo:

swipl mini_hyper.pl

Dentro do interpretador Prolog, execute:

?- induce(H).

---

## 8. Resultado obtido

A execução apresentou a busca por profundidade iterativa:

MaxD= 0
MaxD= 1
MaxD= 2
MaxD= 3
MaxD= 4
MaxD= 5
MaxD= 6
MaxD= 7
MaxD= 8


Em seguida, o programa retornou a hipótese aprendida:

H = [
  [predecessor(_A, _B), [atom(_A), parent(_A, _C)], [atom(_C), predecessor(_C, _B)]]/[_A, _C, _B],
  [predecessor(_D, _E), [atom(_D), parent(_D, _E)]]/[_D, _E]
]


---

## 9. Interpretação do resultado

A hipótese retornada equivale às seguintes regras lógicas:

predecessor(A,B) :-
    atom(A),
    parent(A,C),
    atom(C),
    predecessor(C,B).

predecessor(D,E) :-
    atom(D),
    parent(D,E).


Ignorando os predicados auxiliares `atom/1`, usados apenas para reduzir o espaço de busca, a hipótese aprendida corresponde a:

predecessor(X,Y) :- parent(X,Y).

predecessor(X,Y) :- parent(X,Z), predecessor(Z,Y).


Portanto, o sistema aprendeu corretamente que uma pessoa pode ser predecessor de outra de duas formas:

1. diretamente, quando é pai ou mãe dela;
2. indiretamente, quando é pai ou mãe de alguém que também é predecessor dela.
