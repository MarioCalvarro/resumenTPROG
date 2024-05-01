# Tema 2. Semántica operacional del lenguaje WHILE
## Semántica de paso largo
### Transiciones
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
    s\rangle \rightarrow s'},\ \text{ si } \mathcal{B}\llbracket b \rrbracket =
    \mathbf{tt}
    $$
    - Si no se cumple:
    $$
    \left[ \mathrm{if}_{\mathrm{ns}}^{\mathrm{ff}} \right] := \frac{\langle S_2, s \rangle
    \rightarrow s'}{\langle \mathtt{if}\ b\ \mathtt{then}\ S_1\ \mathtt{else}\ S_2,
    s\rangle \rightarrow s'},\ \text{ si } \mathcal{B}\llbracket b \rrbracket =
    \mathbf{ff}
    $$
- Bucle:
    - Si se cumple:
    $$
    \left[ \mathrm{while}_{\mathrm{ns}}^{\mathrm{tt}} \right] := \frac{\langle
    S, s \rangle \rightarrow s', \langle \mathtt{while}\ b\ \mathtt{do}\ S, s''
    \rangle}{\langle \mathtt{while}\ b\ \mathtt{do}\ S, s\rangle \rightarrow
    s''},\ \text{ si } \mathcal{B}\llbracket b \rrbracket = \mathbf{tt}
    $$
    - Si no se cumple:
    $$
    \langle \mathtt{while}\ b \mathtt{do}\ S, s \rangle \rightarrow s,\ \text{
    si } \mathcal{B}\llbracket b \rrbracket = \mathbf{ff}
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
## Equivalencia
