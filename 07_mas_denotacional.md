# Tema 7. Más sobre semántica denotacional
En este capítulo extendemos el lenguaje **WHILE** con *bloques* y
*procedimientos* y daremos la semántica denotacional para esta extensión.

## Entornos y memorias
En primer lugar, expandimos el lenguaje con bloques que tienen declaraciones de
variables y procedimientos (igual que como vimos en el capítulo 3). A diferencia
del capítulo 3 solo daremos el caso de *ámbito estáticos*.

### Memoria y direcciones
Definición: (Direcciones)
: Decimos que el dominio $\mathbf{Loc}$ representa las posibles localizaciones
de las variables.

Definición: (Entorno de variables)
: Definimos la siguiente familia de funciones como *entorno de variables*
$\mathbf{Env}_V = \mathbf{Var} \rightarrow \mathbf{Loc}$ que asignan a cada
variable, su dirección.

Definición: (Memoria)
: Definimos la siguiente familia de funciones como *memoria* $\mathbf{Store} =
\mathbf{Loc} \cup \left\{ \mathrm{next} \right\} \rightarrow \mathbb{Z}$ que
asignan a cada dirección, un valor.

De esta forma el estado como lo definíamos anteriormente será la composición de
funciones del entorno de variables y la memoria. Esto motiva la siguiente
definición.

Definición: (*Lookup*)
: Definimos la función *lookup* con tipo $\mathbf{Env}_V \rightarrow
\mathbf{Store} \rightarrow \mathbf{Store}$ a
$$
\mathrm{lookup}\ env_V\ sto = sto \circ env_V.
$$
Es decir, que dado un entorno de variables y una memoria, obtenemos el estado
equivalente.

Con estas nuevas definiciones el tipo de la función semántica denotacional
pasará a ser
$$
\mathcal{S}_{\mathrm{ds}}' : \mathbf{Stm} \rightarrow \mathbf{Env}_V \rightarrow
\left( \mathbf{Store} \hookrightarrow \mathbf{Store} \right)
$$
y su definición será
$$
\begin{align*}
    \mathcal{S}_{\mathrm{ds}} \llbracket x := a \rrbracket env_V\ sto &=
    sto\left[ l \mapsto \mathcal{A}\llbracket a \rrbracket\left(
    \mathrm{lookup}\ env_V\ sto \right) \right]\\

    & \text{donde } l = env_V\ x\\

    \mathcal{S}_{\mathrm{ds}} \llbracket \mathtt{skip} \rrbracket env_V &=
    \mathrm{id}\\

    \mathcal{S}_{\mathrm{ds}} \llbracket S_1\ \mathtt{;}\ S_2\ \rrbracket env_V &=
    \left( \mathcal{S}_{\mathrm{ds}}\llbracket S_2 \rrbracket env_V \right) \circ
    \left( \mathcal{S}_{\mathrm{ds}}\llbracket S_1 \rrbracket env_V \right)\\

    \mathcal{S}_{\mathrm{ds}} \llbracket \mathtt{if}\ b\ \mathtt{then}\ S_1\
    \mathtt{else}\ S_2 \rrbracket env_V &= \mathrm{cond}\left( \mathcal{B}\llbracket b \rrbracket \circ \left(
    \mathrm{lookup\ env_V} \right), \mathcal{S}_{\mathrm{ds}}\llbracket S_1
    \rrbracket env_V, \mathcal{S}_{\mathrm{ds}}\llbracket S_2 \rrbracket env_V \right)\\

    \mathcal{S}_{\mathrm{ds}} \llbracket \mathtt{while}\ b\ \mathtt{do}\ S \rrbracket env_V &= \mathrm{FIX}\ F\\

    \text{donde } F\ g &= \mathrm{cond}\left( \mathcal{B}\llbracket b
    \rrbracket \circ \left( \mathrm{lookup}\ env_V \right), g \circ \left( \mathcal{S}_{\mathrm{ds}}\llbracket S \rrbracket env_V \right),
    \mathrm{id} \right)\\
\end{align*}
$$

### Declaraciones
Una vez actualizada la definición de las instrucciones básicas de **WHILE**,
debemos dar una semántica para las nuevas construcciones, es decir, la
declaración de variables y procedimientos así como los bloques y las llamadas.
Empecemos por la declaración de variables. En primer lugar el tipo de esta
función será
$$
\mathcal{D}_{\mathrm{d}}^V: \mathrm{Dec}_V \rightarrow \left( \mathbf{Env}_V
\times \mathbf{Store} \right) \rightarrow \left( \mathbf{Env}_V
\times \mathbf{Store} \right).
$$
Es decir, dada una declaración de una variable, actualiza el estado. Su
definición es, por tanto,
$$
\begin{gather*}
\mathcal{D}_{\mathrm{d}}^V\llbracket \varepsilon \rrbracket = \mathrm{id}\\
\mathcal{D}_{\mathrm{d}}^V\llbracket \mathtt{var}\ x := a;\ D_V \rrbracket\left(
env_V, sto \right) =\\
    \mathcal{D}_d
\end{gather*}
$$
