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
    \mathcal{S}'_{\mathrm{ds}} \llbracket x := a \rrbracket\ env_V\ sto &=
    sto\left[ l \mapsto \mathcal{A}\llbracket a \rrbracket \left(
    \mathrm{lookup}\ env_V\ sto \right) \right]\\

    & \text{donde } l = env_V\ x\\

    \mathcal{S}'_{\mathrm{ds}} \llbracket \mathtt{skip} \rrbracket\ env_V &=
    \mathrm{id}\\

    \mathcal{S}'_{\mathrm{ds}} \llbracket S_1\ \mathtt{;}\ S_2\ \rrbracket\ env_V &=
    \left( \mathcal{S}'_{\mathrm{ds}}\llbracket S_2 \rrbracket\ env_V \right) \circ
    \left( \mathcal{S}'_{\mathrm{ds}}\llbracket S_1 \rrbracket\ env_V \right)\\

    \mathcal{S}'_{\mathrm{ds}} \llbracket \mathtt{if}\ b\ \mathtt{then}\ S_1\
    \mathtt{else}\ S_2 \rrbracket\ env_V &= \mathrm{cond}\left( \mathcal{B}\llbracket b \rrbracket \circ \left(
    \mathrm{lookup}\ env_V \right), \mathcal{S}'_{\mathrm{ds}}\llbracket S_1
    \rrbracket\ env_V, \mathcal{S}'_{\mathrm{ds}}\llbracket S_2 \rrbracket\ env_V \right)\\

    \mathcal{S}'_{\mathrm{ds}} \llbracket \mathtt{while}\ b\ \mathtt{do}\ S \rrbracket\ env_V &= \mathrm{FIX}\ F\\

    \text{donde } F\ g &= \mathrm{cond}\left( \mathcal{B}\llbracket b
    \rrbracket \circ \left( \mathrm{lookup}\ env_V \right), g \circ \left( \mathcal{S}'_{\mathrm{ds}}\llbracket S \rrbracket\ env_V \right),
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
\mathcal{D}_{\mathrm{ds}}^V: \mathrm{Dec}_V \rightarrow \left( \mathbf{Env}_V
\times \mathbf{Store} \right) \rightarrow \left( \mathbf{Env}_V
\times \mathbf{Store} \right).
$$
Es decir, dada una declaración de una variable, actualiza el estado. Su
definición es, por tanto,
$$
\begin{align*}
&\mathcal{D}_{\mathrm{ds}}^V\llbracket \varepsilon \rrbracket = \mathrm{id}\\

&\begin{aligned}
\mathcal{D}_{\mathrm{ds}}^V\llbracket \mathtt{var}\ x := a;\ D_V \rrbracket &\left(
env_V, sto \right) =\\

&\mathcal{D}_{\mathrm{ds}}^V \llbracket D_V \rrbracket \left( env_V
\left[ x \mapsto l \right], sto\left[ l \mapsto v \right]\left[ \mathrm{next}
\mapsto \mathrm{new}\ l \right] \right),\\

\text{donde } l =\ &sto\ \mathrm{next} \text{ y } v = \mathcal{A}\llbracket a
\rrbracket \left( \mathrm{lookup}\ env_V\ sto \right).
\end{aligned}
\end{align*}
$$
Con lo que al declarar cada variable le asignamos la dirección $l$ (que es la
siguiente disponible), le damos el valor correspondiente a esa dirección y
actualizamos el siguiente hueco.

Por tanto, la semántica de un bloque sin procedimientos será
$$
\begin{gather*}
\mathcal{S}_{\mathrm{ds}}'\llbracket \mathtt{begin}\ D_V\ \varepsilon\
\mathtt{end} \rrbracket\ env_V\ sto = \mathcal{S}_{\mathrm{d}}\llbracket S \rrbracket\ env_V'\ sto'\\
\text{donde } \mathcal{D}_{\mathrm{ds}}^V\llbracket D_V \rrbracket \left( env_V,
sto \right) = \left( env_V', sto' \right).
\end{gather*}
$$

Veamos, entonces, la declaración de procedimientos. Los entornos de
procedimientos serán funciones de tipo
$$
\mathbf{Env}_P = \mathbf{Pname} \rightarrow \left( \mathbf{Store}
\hookrightarrow \mathbf{Store} \right)
$$
Es decir, vamos a asignar a cada procedimiento su *significado* directamente (es
conveniente recordar que en el caso operacional, asignábamos a cada
procedimiento la sentencia que ejecutaba, pero no su significado). Con esto, la
función semántica tendrá tipo
$$
\mathcal{D}_{\mathrm{ds}}^P : \mathbf{Dec}_p \rightarrow \mathbf{Env}_V
\rightarrow \mathbf{Env}_P \rightarrow \mathbf{Env}_P
$$
y su definición recursiva (para procedimientos no recursivos),
$$
\begin{align*}
&\mathcal{D}_{\mathrm{ds}}^P\llbracket \varepsilon \rrbracket = \mathrm{id}\\

&\begin{aligned}
\mathcal{D}_{\mathrm{ds}}^P \llbracket \mathtt{proc}\ p\ \mathtt{is}\ S;\ D_P
\rrbracket\ env_V\ env_P &= \mathcal{D}_{\mathrm{ds}}^P \llbracket
D_P \rrbracket\ env_V \left( env_P \left[ p \mapsto g \right] \right),\\

\text{donde } g &= \mathcal{S}_{\mathrm{ds}} \llbracket S \rrbracket\ env_V\ env_P.
\end{aligned}
\end{align*}
$$
Es decir, asignamos al procedimiento $p$ el valor de su sentencia $S$.

## La función semántica $\mathcal{S}_{\mathrm{ds}}$
Por último, veamos como queda al final la función semántica. En primer lugar, su
tipo tendrá que tener en cuenta los entornos de variable y de variables. Por
tanto,
$$
\mathcal{S}_{\mathrm{ds}} : \mathbf{Stm} \rightarrow \mathbf{Env}_V \rightarrow
\mathbf{Env}_P \rightarrow \left( \mathbf{Store} \hookrightarrow \mathbf{Store} \right)
$$
Por otro lado, la definición de esta nueva función semántica será igual a la
dada por $\mathcal{S}_{\mathrm{ds}}'$ únicamente añadiendo como parámetro el
entorno de procedimientos $env_P$. Sin embargo, los bloques y las llamadas sí
debemos especificarlas:
$$
\begin{align*}
&\begin{aligned}
\mathcal{S}_{\mathrm{ds}}\llbracket \mathtt{begin}\ D_V\ D_P\ S\ \mathtt{end}
\rrbracket\ env_V\ env_P\ sto &= \mathcal{S}_{\mathrm{ds}}\llbracket S \rrbracket\ env_V'\ env_P'\
sto'\\

\text{donde } \mathcal{D}_{\mathrm{ds}}^V \llbracket D_V \rrbracket \left(
env_V, sto \right) &= \left( env_V', sto' \right)\\

\text{y } \mathcal{D}_{\mathrm{ds}}^P \llbracket D_P \rrbracket\ env_V'\ env_P &=
env_P'
\end{aligned}\\

&\mathcal{S}_{\mathrm{ds}}\llbracket \mathtt{call}\ p \rrbracket\ env_V\ env_P =
env_P\ p
\end{align*}
$$

### Procedimientos recursivos
Lo último que nos queda por ver es el significado de los procedimientos con
llamadas recursivas en sus instrucciones. A diferencia de la forma que
utilizábamos para tratar con este caso en la semántica operacional en la que
actualizábamos el entorno cada vez que se realizaba una llamada recursiva, en
este caso, la recursión será manejada *de una sola vez* cuando se realice la
declaración del procedimiento. Esto guarda una clara relación con la iteración
de los bucles por lo que, de la misma manera, utilizaremos el *punto fijo* de un
funcional. En definitiva,
$$
\begin{align*}
&\mathcal{D}_{\mathrm{ds}}^P\llbracket \varepsilon \rrbracket = \mathrm{id}\\

&\begin{aligned}
\mathcal{D}_{\mathrm{ds}}^P \llbracket \mathtt{proc}\ p\ \mathtt{is}\ S;\ D_P
\rrbracket env_V, env_P &= \mathcal{D}_{\mathrm{ds}}^P \llbracket
D_P \rrbracket\ env_V \left( env_P \left[ p \mapsto \mathrm{FIX}\ F \right] \right),\\

\text{donde } F\ g &= \mathcal{S}_{\mathrm{ds}} \llbracket S \rrbracket\ env_V\ env_P\left[ p \mapsto g \right].
\end{aligned}
\end{align*}
$$
