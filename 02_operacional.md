# Tema 2. Semántica operacional del lenguaje WHILE
## Semántica de paso largo
### Transiciones
Configuraciones
: - La *sentencia* $S$ se ejecuta desde el estado $s$: $\langle S, s \rangle$.
- Estado $s'$ que se alcanza al terminar su ejecución.

Transiciones ($\langle S, s \rangle \rightarrow s'$)
:
- Asignación:
$$
\left[ \mathrm{ass}_{\mathrm{ns}} \right] := \langle x := a, s \rangle
\rightarrow s \left[ x \mapsto \mathcal{A}\llbracket a \rrbracket s \right]
$$
- *Skip*:
$$
\left[ \mathrm{skip}_{\mathrm{ns}} \right] := \langle \mathtt{skip}, s \rangle \rightarrow s 
$$
- Composición:
$$
\left[ \mathrm{comp}_{\mathrm{ns}} \right] := \frac{\langle S_1, s \rangle
\rightarrow s',\ \langle S_2, s' \rangle \rightarrow s''}{\langle S_1 \mathtt{;}
S_2, s \rangle \rightarrow s''}
$$
- Condicional:
    - Si se cumple:
    $$
    \left[ \mathrm{if}_{\mathrm{ns}}^{\mathrm{tt}} \right] := \frac{\langle S_1, s \rangle
    \rightarrow s'}{\langle \mathtt{if}\ b\ \mathtt{then}\ S_1\ \mathtt{else}\ S_2,
    s\rangle \rightarrow s'},\ \text{ si } \mathcal{B}\llbracket b \rrbracket s =
    \mathbf{tt}
    $$
    - Si no se cumple:
    $$
    \left[ \mathrm{if}_{\mathrm{ns}}^{\mathrm{ff}} \right] := \frac{\langle S_2, s \rangle
    \rightarrow s'}{\langle \mathtt{if}\ b\ \mathtt{then}\ S_1\ \mathtt{else}\ S_2,
    s\rangle \rightarrow s'},\ \text{ si } \mathcal{B}\llbracket b \rrbracket s =
    \mathbf{ff}
    $$
- Bucle:
    - Si se cumple:
    $$
    \left[ \mathrm{while}_{\mathrm{ns}}^{\mathrm{tt}} \right] := \frac{\langle
    S, s \rangle \rightarrow s', \langle \mathtt{while}\ b\ \mathtt{do}\ S, s'
    \rangle \rightarrow s''}{\langle \mathtt{while}\ b\ \mathtt{do}\ S, s\rangle \rightarrow
    s''},\ \text{ si } \mathcal{B}\llbracket b \rrbracket s = \mathbf{tt}
    $$
    - Si no se cumple:
    $$
    \left[ \mathrm{while}_{\mathrm{ns}}^{\mathrm{ff}} \right] := \langle \mathtt{while}\ b\ \mathtt{do}\ S, s \rangle \rightarrow s,\ \text{
    si } \mathcal{B}\llbracket b \rrbracket s = \mathbf{ff}
    $$

Una sentencia $S$ es un estado $s$ tiene dos posibilidades:
- **Terminar** si, y sólo si, $\exists s' \in \mathbf{State}. \langle S, s \rangle \rightarrow s'$.
- **Ciclar** si, y sólo si, $\nexists s' \in \mathbf{State}. \langle S, s \rangle \rightarrow s'$.

### Árboles de derivación
Para todo programa escrito en el lenguaje **WHILE** podemos escribir un árbol de
derivación. Este árbol tendrá como raíz el programa entero y sus hijos serán
aplicaciones de las reglas de la anterior sección. Las hojas serán *axiomas*
(aquellas reglas que no tienen «conclusión»).

Si el programa «cicla» tendrá un árbol infinito, mientras que si termina será
finito. En esta versión del **WHILE** para toda transición habrá una sola regla
que se pueda aplicar.

Inducción sobre el árbol de derivación
: 1. Probar que la propiedad se cumple para los *axiomas*.
2. Probar para cada regla compuesta: asumiendo que se cumple para las hipótesis,
   ver que también es cierta para las conclusiones.

### Propiedades
Definición (Equivalencia entre sentencias)
: Decimos que dos sentencias $S_1$ y $S_2$ son semánticamente equivalentes si, y
sólo si, $\forall s \in \mathbf{State}$ se cumple que:
$$
\langle S_1, s \rangle \rightarrow s' \Leftrightarrow \langle S_2, s \rangle
\rightarrow s'
$$

Definición y proposición (Determinismo)
: Decimos que una semántica es *determinista* si dada una sentencia $S$ y
cualquier estado $s$ se cumple que:
$$
\langle S, s \rangle \rightarrow s' \land \langle S, s \rangle \rightarrow s''
\Rightarrow s' = s''
$$

### Función semántica
Definición (Significado de una sentencia)
: Definimos la semántica operacional de paso largo de las sentencias a través de
la siguiente función:
$$
\mathcal{S}_{ns} : \mathbf{Stm} \rightarrow \left( \mathbf{State}
\hookrightarrow \mathbf{State} \right)\\

\mathcal{S}_{ns}\llbracket S \rrbracket s = \begin{cases}
    s', &\text{si } \langle S, s \rangle \rightarrow s'\\
    \mathtt{indefinido}, &\text{c.c } 
\end{cases}
$$

## Semántica de paso corto
Transición
: Primer paso de ejecución de $S$: $\langle S, s \rangle \Rightarrow \gamma$.
- Si $\gamma = \langle S', s' \rangle$, ejecución *no terminada*.
- Si $\gamma = s'$, ejecución *terminada*.

Si para un $\langle S, s \rangle$ *no existe* ningún $\gamma. \langle S, s
\rangle \Rightarrow \gamma$, la configuración estará *bloqueada*.

- Asignación:
$$
\left[ \mathrm{ass}_{\mathrm{sos}} \right] := \langle x := a, s \rangle
\Rightarrow s \left[ x \mapsto \mathcal{A}\llbracket a \rrbracket s \right]
$$
- *Skip*:
$$
\left[ \mathrm{skip}_{\mathrm{sos}} \right] := \langle \mathtt{skip}, s \rangle \Rightarrow s 
$$
- Composición:
$$
\begin{align*}
\left[ \mathrm{comp}_{\mathrm{sos}}^{1} \right] &:= \frac{\langle S_1, s \rangle
\Rightarrow \langle S_1', s' \rangle}{\langle S_1 \mathtt{;}
S_2, s \rangle \Rightarrow \langle S_1' \mathtt{;} S_2, s' \rangle}
\\
\left[ \mathrm{comp}_{\mathrm{sos}}^{2} \right] &:= \frac{\langle S_1, s \rangle
\Rightarrow s'}{\langle S_1 \mathtt{;}
S_2, s \rangle \Rightarrow \langle S_2, s' \rangle}
\end{align*}
$$
- Condicional:
    - Si se cumple:
    $$
    \left[ \mathrm{if}_{\mathrm{sos}}^{\mathrm{tt}} \right] := \langle
    \mathtt{if}\ b\ \mathtt{then}\ S_1\ \mathtt{else}\ S_2, s\rangle \Rightarrow
    \langle S_1, s \rangle,\ \text{ si } \mathcal{B}\llbracket b \rrbracket s =
    \mathbf{tt}
    $$
    - Si no se cumple:
    $$
    \left[ \mathrm{if}_{\mathrm{sos}}^{\mathrm{tt}} \right] := \langle
    \mathtt{if}\ b\ \mathtt{then}\ S_1\ \mathtt{else}\ S_2, s\rangle \Rightarrow
    \langle S_2, s \rangle,\ \text{ si } \mathcal{B}\llbracket b \rrbracket s =
    \mathbf{ff}
    $$
- Bucle:
    $$
    \left[ \mathrm{while}_{\mathrm{sos}} \right] := \langle \mathtt{while}\ b\ \mathtt{do}\ S, s \rangle \Rightarrow\\ \langle
    \mathtt{if}\ b\ \mathtt{then}\ \left( S,\ \mathtt{while}\ b\ \mathtt{do}\ S \right)\ \mathtt{else}\ \mathtt{skip}, s\rangle
    $$

Las secuencias de derivación pueden *terminar* (con éxito, alcanza un $s'$, o no) o *ciclar*.

Inducción sobre la longitud de secuencia
: 1. Demostrar la propiedad para las secuencias de longitud $0$.
2. Asumiendo que se cumple para las secuencias de longitud $k$, probarla para
   secuencias de longitud $k + 1$.

### Propiedades
Lema
: Si tenemos $\langle S_1 \mathtt{;} S_2, s \rangle \Rightarrow^k s''$, entonces
existen $s' \in \mathbf{State}$ y $k_1, k_2 \in \mathbb{N}$ tales que:
$$
\langle S_1, s \rangle \Rightarrow^{k_1} s' \land \langle S_2, s \rangle \Rightarrow^{k_2} s'', \text{ con } k = k_1 + k_2.
$$

Definición (Equivalencia semántica)
: Decimos que dos sentencias $S_1$ y $S_2$ son semánticamente equivalentes si
para todo estado $s$ se cumple que:
- $\langle S_1, s \rangle \Rightarrow^* \gamma \Leftrightarrow \langle S_2, s
    \rangle \Rightarrow^* \gamma$, para cada $\gamma$ terminal o bloqueada.
- La secuencia que inicia con $\langle S_1, s \rangle$ es *infinita* si, y sólo
    si, lo es la que inicia con $\langle S_2, s \rangle$.

### Función semántica
Definición (Significado de una sentencia)
: Definimos la semántica operacional de paso corto de las sentencias a través de
la siguiente función:
$$
\mathcal{S}_{sos} : \mathbf{Stm} \rightarrow \left( \mathbf{State}
\hookrightarrow \mathbf{State} \right)\\

\mathcal{S}_{sos}\llbracket S \rrbracket s = \begin{cases}
    s', &\text{si } \langle S, s \rangle \Rightarrow^* s'\\
    \mathtt{indefinido}, &\text{c.c } 
\end{cases}
$$

## Equivalencia
Teorema
: Para toda sentencia $S \in \mathbf{Stm}$ se cumple la siguiente relación:
$$
\mathcal{S}_{ns}\llbracket S \rrbracket = \mathcal{S}_{sos}\llbracket S \rrbracket
$$
Es decir, la semántica operacional de paso corto y paso largo con
*equivalentes*.
