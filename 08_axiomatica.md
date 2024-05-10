# Tema 8. Semántica axiomática
## Aserciones de corrección parcial
La semántica axiomática se basa en el uso de *aserciciones de corrección
parcial* que podemos definir como tuplas con la siguiente estructura
$$
\left\{ P \right\} S \left\{ Q \right\}
$$
donde $P$ y $Q$ son dos predicados y $S$ una sentencia. El significado de esta
tupla es el siguiente: «Si la precondición $P$ se cumple en el *estado inicial* y
la sentencia $S$ *acaba* partiendo de ese mismo estado, el *estado final*
cumplirá la postcondición $Q$».

*Observación*:
: Si la sentencia $S$ *no* acaba, no podemos afirmar nada sobre el estado final
en relación con la postcondición. Esto es lo que llamamos *corrección parcial*
pues no sabemos nada sobre su *terminación*.

Como lenguaje para expresar los predicados podemos seguir dos "filosofías", la
*intensional* o la *extensional*. En este caso, usaremos la extensional en la
que los predicados pueden ser considerados como funciones del tipo
$\mathbf{State} \rightarrow \mathbf{T}$. Por ejemplo, cualquier expresión
booleana describe un predicado $\mathcal{B}\llbracket b \rrbracket$. Tendremos
la siguiente notación:
$$
\begin{align*}
P_1 \land P_2 &\equiv P,\ \text{donde } P\ s = \left( P_1\ s \right) \text{ y }
\left( P_2\ s \right)\\

P_1 \lor P_2 &\equiv P,\ \text{donde } P\ s = \left( P_1\ s \right) \text{ ó }
\left( P_2\ s \right)\\

¬P_1 &\equiv P,\ \text{donde } P\ s = ¬\left( P_1\ s \right)\\

P_1 \left[ x \mapsto \mathcal{A}\llbracket a \rrbracket \right] &\equiv P,\
\text{donde } P\ s = P_1 \left( s\left[ x \mapsto \mathcal{A}\llbracket a
\rrbracket s \right] \right)\\

P_1 \Rightarrow P_2 &\equiv P_1\ s \Rightarrow P_2\ s,\ \forall s \in
\mathbf{State}
\end{align*}
$$
Considero necesario dar una breve aclaración sobre el significado del predicado
asignación. Simplemente, este predicado será *verdadero* si el predicado sobre el
que se aplica es verdadero una vez que se realice la sustitución indicada en la
asignación.

### Sistema de inferencia
Para describir la semántica de **WHILE** de manera axiomática será necesario dar
lo que conocemos como *sistema de inferencia* que, al igual que la semántica
operacional, consiste en un conjunto de axiomas y reglas a través de las tuplas
que hemos descrito antes. Para cada instrucción del lenguaje tenemos una regla y
son las siguientes:
$$
\begin{align*}
\left[ \mathrm{ass}_{\mathrm{p}} \right] &:= \left\{ P\left[ x \mapsto
\mathcal{A}\llbracket a \rrbracket \right] \right\} x := a \left\{ P \right\}\\

\left[ \mathrm{skip}_{\mathrm{p}} \right] &:= \left\{ P \right\} \mathtt{skip}
\left\{ P \right\}\\

\left[ \mathrm{comp}_{\mathrm{p}} \right] &:= \frac{\left\{ P \right\} S_1
\left\{ Q \right\},\ \left\{ Q \right\} S_2 \left\{ R \right\}}{\left\{ P
\right\} S_1 ; S_2 \left\{ R \right\}}\\

\left[ \mathrm{if}_{\mathrm{p}} \right] &:= \frac{\left\{ \mathcal{B}\llbracket
b\rrbracket \land P \right\} S_1 \left\{ Q \right\},\ \left\{
¬\mathcal{B}\llbracket b \rrbracket \land P \right\} S_2 \left\{ Q
\right\}}{\left\{ P \right\} \mathtt{if}\ b\ \mathtt{then}\ S_1\ \mathtt{else}\
S_2 \left\{ Q \right\}}\\

\left[ \mathrm{while}_{\mathrm{p}} \right] &:= \frac{\left\{
\mathcal{B}\llbracket b\rrbracket \land P \right\} S \left\{ P \right\}}{\left\{
P \right\} \mathtt{while}\ b\ \mathtt{do}\ S \left\{ ¬\mathcal{B}\llbracket b
\rrbracket \land P \right\}}\\

\left[ \mathrm{cons}_{\mathrm{p}} \right] &:= \frac{\left\{ P' \right\} S
\left\{ Q' \right\}}{\left\{ P \right\} S \left\{ Q \right\}},\ \text{donde } P
\Rightarrow P' \text{ y } Q' \Rightarrow Q.\\
\end{align*}
$$
Daré un par de aclaraciones que considero convenientes:
- La asignación, a primera vista, parece estar al revés. Sin embargo, esto no es
    así ya que si recordamos la definición de la notación para $P\left[ x
    \mapsto \mathcal{A}\llbracket a \rrbracket \right]$ tenemos que será cierto
    el predicado si lo es $P$ tras realizar la sustitución. Por lo tanto, lo que
    nos dice la tupla de la asignación es que $P$ se cumplirá si sigue siendo
    cierto al realizar la sustitución.

- La última regla nos permite *fortalecer* las precondiciones y *debilitar* las
    postcondiciones. Es decir, nos permite dar un caso más concreto que se sigue
    cumpliendo. De esta forma podemos, por ejemplo, encontrar un predicado que
    se cumpla para las dos postcondiciones de un `if`.

Notación: (Demostrabilidad de una tupla)
: Cuando una tupla quede probada (al realizar todo su *árbol de inferencia*) lo
denotaremos por:
$$
\vdash_p \left\{ P \right\} S \left\{ Q \right\}
$$

### Equivalencia axiomática
Definición: (Equivalencia semántica)
: Diremos que dos sentencias $S_1$ y $S_2$ son *equivalentes demostrablemente*
si, $\forall P, Q$ precondiciones y postcondiciones tenemos que:
$$
\vdash_p \left\{ P \right\} S_1 \left\{ Q \right\} \Leftrightarrow\ \vdash_p
\left\{ P \right\} S_2 \left\{ Q \right\}
$$

## Corrección y completitud de la semántica axiomática
Definición: (Aserciones bajo semántica)
: Una aserción de corrección parcial es válida bajo una semántica (por ejemplo,
operacional de paso largo) si cumple que
$$
\vDash_p \left\{ P \right\} S \left\{ Q \right\} \xLeftrightarrow{\mathrm{def}}
\forall s \in \mathbf{State} \left[ \left( P\ s = \mathbf{tt} \land \exists s' :
\langle S, s \rangle \rightarrow s' \right) \Rightarrow Q\ s' = \mathbf{tt} \right]
$$
Es decir, que la postcondición se cumple para todos los estados en los que la
precondición se cumple y la ejecución de $S$ termina exitosamente.

Teorema:
: Para toda aserción de corrección parcial $\left\{ P \right\} S \left\{ Q
\right\}$ tenemos que
$$
\vDash_p \left\{ P \right\} S \left\{ Q \right\} \Leftrightarrow\ \vdash_p \left\{
P \right\} S \left\{ Q \right\}.
$$
O en tras palabras, la semántica axiomática es *completa* y *correcta*.
