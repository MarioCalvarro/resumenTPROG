# Tema 5. Semántica denotacional
La diferencia fundamental de esta nueva semántica con las operacionales es que
se consigue definir de manera completamente *composicional*.

## Función semántica, valores semánticos y definición composicional
La semántica denotacional asocia significado a las instrucciones de **WHILE** a
través de la siguiente función:
$$
\mathcal{S}_{\mathrm{ds}} : \mathbf{Stm} \rightarrow \left( \mathbf{State}
\hookrightarrow \mathbf{State} \right)
$$
que se define de la siguiente manera para cada instrucción:
$$
\begin{align*}
    \mathcal{S}_{\mathrm{ds}} \llbracket x := a \rrbracket s &= s\left[ x \mapsto
    \mathcal{A}\llbracket a \rrbracket s \right]\\

    \mathcal{S}_{\mathrm{ds}} \llbracket \mathtt{skip} \rrbracket &=
    \mathrm{id}\\

    \mathcal{S}_{\mathrm{ds}} \llbracket S_1\ \mathtt{;}\ S_2\ \rrbracket &=
    \mathcal{S}_{\mathrm{ds}}\llbracket S_2 \rrbracket \circ
    \mathcal{S}_{\mathrm{ds}}\llbracket S_1 \rrbracket\\

    \mathcal{S}_{\mathrm{ds}} \llbracket \mathtt{if}\ b\ \mathtt{then}\ S_1\
    \mathtt{else}\ S_2 \rrbracket &= \mathrm{cond}\left( \mathcal{B}\llbracket b
    \rrbracket, \mathcal{S}_{\mathrm{ds}}\llbracket S_1 \rrbracket,
    \mathcal{S}_{\mathrm{ds}}\llbracket S_2 \rrbracket \right)\\

    \mathcal{S}_{\mathrm{ds}} \llbracket \mathtt{while}\ b\ \mathtt{do}\ S \rrbracket &= \mathrm{FIX}\ F\\

    \text{donde } F\ g &= \mathrm{cond}\left( \mathcal{B}\llbracket b
    \rrbracket, g \circ \mathcal{S}_{\mathrm{ds}}\llbracket S \rrbracket,
    \mathrm{id} \right)\\
\end{align*}
$$
Como se puede observar, tenemos dos funciones auxiliares $\mathrm{cond}$ y
$\mathrm{FIX}$ que debemos definir a su vez. Empezamos con $\mathrm{cond}$ que
tiene como tipo
$$
\mathrm{cond}: \left( \mathbf{State} \rightarrow \mathbf{T} \right) \times \left( \mathbf{State} \hookrightarrow \mathbf{State} \right) \times \left( \mathbf{State} \hookrightarrow \mathbf{State} \right) \rightarrow \left( \mathbf{State} \hookrightarrow \mathbf{State} \right)
$$
y se define como
$$
\mathrm{cond}\left( p, g_1, g_2 \right) s = \begin{cases}
    g_1\ s, &\text{si } p\ s = \mathbf{tt}\\
    g_2\ s, &\text{si } p\ s = \mathbf{ff}\\
\end{cases}.
$$
Por otro lado, el tipo de $\mathrm{FIX}$ será
$$
\mathrm{FIX}: \left( \left( \mathbf{State} \hookrightarrow \mathbf{State}
\right) \rightarrow \left( \mathbf{State} \hookrightarrow \mathbf{State} \right) \right) \rightarrow \left( \mathbf{State} \hookrightarrow \mathbf{State} \right)
$$
y su definición será
$$
\mathrm{FIX}\ F = g \text{ tal que } F\ g = g.
$$
Sin embargo, no todas las funciones tienen punto fijo o no es único por lo que
tendremos que hacer un estudio más detallado para refinar la definición.

## Teoría de puntos fijos. Dominios semánticos. Funciones continuas
### Requerimientos para el punto fijo
Al ejecutar un bucle del lenguaje **WHILE** se nos presentan tres posibles
casos:
- El bucle termina correctamente para cualquier posible estado
- El bucle cicla *localmente*, es decir, existen una cantidad *finita* de estados
    en los que el bucle no termina.
- El bucle cicla *globalmente*, es decir, existen una cantidad *infinita* de estados
    en los que el bucle no termina.

Los dos primeros casos no dan problemas ya que el punto fijo es único. Sin
embargo, en el tercer caso podemos tener múltiples funciones que son punto fijo.
Por esta razón, debemos encontrar la función $g_0$ punto fijo que cumpla la
siguiente propiedad:
$$
\forall g \text{ punto fijo de } F \Leftrightarrow g_0\ s = s' \Rightarrow
g\ s = s'\text{, pero no al revés.}
$$
O en otras palabras, que $g_0$ contenga la *mínima cantidad de información*
posible (lo que la hace la más general posible).

### Conjuntos parcialmente ordenados
Definición: (Orden parcial)
: Sean $g_1, g_2 \in \mathbf{State} \hookrightarrow \mathbf{State}$. Decimos que
$g_1 \sqsubseteq g_2$ si, y sólo si,
$$
\forall s, s' \in \mathbf{State},\ g_1\ s = s' \Rightarrow g_2\ s = s'
$$
Es decir, toda la información contenida en $g_1$, también lo esta en $g_2$.

*Observación*:
: El conjunto $\mathbf{State} \hookrightarrow \mathbf{State}$ junto con
$\sqsubseteq$ es un ejemplo de *conjunto parcialmente ordenado* ya que se
cumplen las propiedades *reflexiva*, *transitiva* y *antisimétrica*.

Definición: (Elemento mínimo)
: Llamaremos *elemento mínimo*, denotado por $\bot$, de un conjunto parcialmente ordenado $\left( D,
\sqsubseteq \right)$ a aquel que cumpla que
$$
\forall d \in D,\ \bot \sqsubseteq d.
$$
Decimos que $\bot$ *no contiene información*.

## Equivalencia entre la semántica operacional y la denotacional
