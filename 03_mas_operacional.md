# Tema 3. Más sobre semántica operacional
## Más allá del determinismo
### Abortar cómputos
- Para definir la semántica de la instrucción `abort` *no la definimos*. De esta
forma cuando llegamos a ella tenemos `undefined` y el programa alcanza un estado
`stuck`.

- Con esta nueva instrucción encontramos una diferencia entre la semántica operacional de
paso corto y paso largo. En el paso corto distinguimos entre bucles infinitos y
terminaciones forzosas mientras que en la de paso largo solo importa la
terminación correcta. En otras palabras, en paso corto `while true do skip` *no* es
equivalente a `abort` mientras que en paso largo *sí*.

### Indeterminación
- Añadimos una nueva instrucción `S1 or S2` que ejecuta cualquiera (pero solo
    una) de las dos sentencias de forma no determinista.

- La definición de paso largo es
    $$
    \left[ \mathrm{or}_{\mathrm{ns}}^1 \right] := \frac{\langle S_1, s \rangle
    \rightarrow s'}{\langle S_1\ \mathtt{or}\ S_2, s \rangle \rightarrow
    s'}\qquad
    \left[ \mathrm{or}_{\mathrm{ns}}^2 \right] := \frac{\langle S_2, s \rangle
    \rightarrow s'}{\langle S_1\ \mathtt{or}\ S_2, s \rangle \rightarrow s'}
    $$

- Y la de paso corto
    $$
    \left[ \mathrm{or}_{\mathrm{sos}}^1 \right] := \langle S_1\ \mathtt{or}\
    S_2, s \rangle \Rightarrow \langle S_1, s \rangle\qquad
    \left[ \mathrm{or}_{\mathrm{sos}}^2 \right] := \langle S_1\ \mathtt{or}\
    S_2, s \rangle \Rightarrow \langle S_2, s \rangle
    $$

- Esta instrucción nos aporta una nueva diferencia entre las dos semánticas
    operacionales: el paso corto *mantiene* los ciclos en presencia de
    indeterminación mientras que el largo los *elimina*.

### Paralelismo
- Extendemos ahora con la instrucción `S1 par S2` que ejecuta paralelamente las
    dos sentencias, es decir, se intercalan las instrucciones de las dos.

- El paso corto será:
    $$
    \begin{align*}
    \left[ \mathrm{par}_{\mathrm{sos}}^1 \right] &:= \frac{\langle S_1, s \rangle
    \Rightarrow \langle S_1', s' \rangle}{\langle S_1\ \mathtt{par}\ S_2, s
    \rangle \Rightarrow \langle S_1'\ \mathtt{par}\ S_2, s' \rangle}\\

    \left[ \mathrm{par}_{\mathrm{sos}}^2 \right] &:= \frac{\langle S_1, s \rangle
    \Rightarrow s'}{\langle S_1\ \mathtt{par}\ S_2, s
    \rangle \Rightarrow \langle S_2, s' \rangle}\\

    \left[ \mathrm{par}_{\mathrm{sos}}^3 \right] &:= \frac{\langle S_2, s \rangle
    \Rightarrow \langle S_2', s' \rangle}{\langle S_1\ \mathtt{par}\ S_2, s
    \rangle \Rightarrow \langle S_1\ \mathtt{par}\ S_2', s' \rangle}\\

    \left[ \mathrm{par}_{\mathrm{sos}}^2 \right] &:= \frac{\langle S_1, s \rangle
    \Rightarrow s'}{\langle S_1\ \mathtt{par}\ S_2, s
    \rangle \Rightarrow \langle S_1, s' \rangle}\\
    \end{align*}
    $$

- Sin embargo, el paso largo no será capaz de definir esta construcción puesto
    que solo capta el estado final y no los pasos intermedios, es decir, en el
    mejor de los casos definirá una construcción en la que una sentencia se
    ejecuta antes que la otra, pero no más.
## Bloques y procedimientos
### Bloques
- Extendemos **WHILE** con bloques `begin Dv S end` donde tenemos declaraciones
    de variables locales: `Dv ::= var x := a; Dv |  `. Debemos añadir que $D_V
    \in \mathbf{Dec}_V$ que es la categoría sintáctica de *declaraciones de
    variables*.  Con esto definimos el conjunto de variables declaradas en `Dv`:
    $$
    \begin{cases}
    \mathrm{DV}\left( \mathtt{var}\ x := a; D_V  \right) &= \left\{ x \right\}
    \cup \mathrm{DV}_V\\
    \mathrm{DV}\left( \varepsilon \right) &= \emptyset
    \end{cases}
    $$

- A la hora de definir la semántica de las declaraciones tenemos que tener en
    cuenta que *no* son instrucciones. Por esto se tienen que definir de una
    nueva forma. Además, para recuperar las variables globales que se han
    redeclarado necesitamos la siguiente función:
    $$
    \left( s'\left[ X \mapsto s \right] x \right) := \begin{cases}
        s\ x, &\text{si } x \in X\\
        s'\ x, &\text{si } x \not\in X
    \end{cases}
    $$
    Intuitivamente, $s'$ es el estado tras abandonar el bloque, $s$ es el de
    antes de entrar y $X$ es el conjunto de variables globales redeclaradas.

- Definimos ahora la semántica para las declaraciones de variables:
    - Caso base:
        $$
        \left[ \mathrm{none}_{\mathrm{ns}} \right] := \langle \varepsilon, s
        \rangle \rightarrow_D s
        $$
    - Caso recursivo:
        $$
        \left[ \mathrm{var}_{\mathrm{ns}} \right] := \frac{\langle D_V, s\left[
        x \mapsto \mathcal{A}\llbracket a \rrbracket s \right] \rangle
        \rightarrow_D s'}{\langle \mathtt{var}\ x := a; D_V, s \rangle
        \rightarrow_D s'}
        $$

- Y para los bloques:
    $$
    \left[ \mathrm{block}_{\mathrm{ns}} \right] := \frac{\langle D_V, s \rangle
    \rightarrow_{D} s', \langle S, s' \rangle \rightarrow s''}{\langle
    \mathtt{begin}\ D_V\ S\ \mathtt{end}, s \rangle \rightarrow s''\left[
    \mathrm{DV}\left( D_V \right) \mapsto s \right]}.
    $$
    Es decir, realizamos las declaraciones de las variables locales desde $s$ lo
    que nos da $s'$, ejecutamos $S$ con $s'$ y obtenemos $s''$, pero al final
    tenemos que devolver las variables globales a su estado original con la
    función auxiliar que hemos visto antes.

### Procedimientos
- Presentamos ahora los *procedimientos*, otra nueva construcción que acompaña a
    los bloques (que no los sustituye, es decir, son los bloques los que generan
    ámbitos, no los procedimientos).

- Tenemos que cambiar la sintaxis añadiendo una nueva instrucción `call p` y
  modificando los bloques `begin Dv Dp S end`. Además, tenemos que añadir las
  declaraciones de los procedimientos: `Dp ::= proc p is S; Dp | `. Con esto
  tenemos que $D_P \in \mathbf{Dec}_P$ que es la categoría sintáctica de
  *declaraciones de procedimientos* y $p \in \mathbf{Pname}$ que es el conjunto
  de nombres de procedimientos.

- Para definir la semántica extendida con los procedimientos tenemos tres
    posibilidades distintas

#### Variables y procedimientos dinámicos
- Entorno de procedimientos:
    : Función que asocia los nombres de los procedimientos a sus cuerpos:
    $$
    \mathbf{Env}_P = \mathbf{Pname} \hookrightarrow \mathbf{Stm}
    $$
    La función no es total para poder hacer llamadas a procedimientos
    todavía no declarados (por ejemplo, para permitir la recursividad
    mutua).

- Si ahora tenemos un entorno $env_P \in \mathbf{Env}_P$ tenemos que las
    transiciones tendrán en cuenta el entorno de la siguiente manera:
    $$
    env_P \vdash \langle S, s \rangle \rightarrow s' 
    $$

- Antes de definir la nueva semántica de largo paso, necesitamos una nueva
    función auxiliar que actualice $\mathbf{Env}_P$ cuando entramos en un bloque
    para añadir los nuevos procedimientos que se definan:
    $$
    \mathrm{upd}_P : \mathbf{Dec}_P \times \mathbf{Env}_P \rightarrow
    \mathbf{Env}_P
    $$
    Lo hacemos recursivamente:
    - Caso base:
        $$
        \mathrm{upd}_P\left( \varepsilon, env_P \right) = env_P
        $$
    - Caso recursivo:
        $$
        \mathrm{udp}_P\left( \mathtt{proc}\ p\ \mathtt{is}\ S; D_P, env_P
        \right) = \mathrm{udp}_P\left( D_P, \mathrm{udp}_P\left[ p \mapsto S \right] \right)
        $$

- Con todo esto, ya podemos actualizar la semántica de **WHILE**:
    - Asignación:
    $$
    \left[ \mathrm{ass}_{\mathrm{ns}} \right] := env_P \vdash \langle x := a, s \rangle
    \rightarrow s \left[ x \mapsto \mathcal{A}\llbracket a \rrbracket s \right]
    $$
    - *Skip*:
    $$
    \left[ \mathrm{skip}_{\mathrm{ns}} \right] := env_P \vdash \langle \mathtt{skip}, s \rangle \rightarrow s 
    $$
    - Composición:
    $$
    \left[ \mathrm{comp}_{\mathrm{ns}} \right] := \frac{env_P \vdash \langle S_1, s \rangle
    \rightarrow s',\ env_P \vdash \langle S_2, s' \rangle \rightarrow s''}{env_P \vdash \langle S_1 \mathtt{;}
    S_2, s \rangle \rightarrow s''}
    $$
    - Condicional:
        - Si se cumple:
        $$
        \left[ \mathrm{if}_{\mathrm{ns}}^{\mathrm{tt}} \right] := \frac{env_P
        \vdash \langle S_1, s \rangle \rightarrow s'}{env_P \vdash \langle
        \mathtt{if}\ b\ \mathtt{then}\ S_1\ \mathtt{else}\ S_2, s\rangle
        \rightarrow s'},\ \text{ si } \mathcal{B}\llbracket b \rrbracket s =
        \mathbf{tt}
        $$
        - Si no se cumple:
        $$
        \left[ \mathrm{if}_{\mathrm{ns}}^{\mathrm{ff}} \right] := \frac{env_P
        \vdash \langle S_2, s \rangle \rightarrow s'}{env_P \vdash \langle
        \mathtt{if}\ b\ \mathtt{then}\ S_1\ \mathtt{else}\ S_2, s\rangle
        \rightarrow s'},\ \text{ si } \mathcal{B}\llbracket b \rrbracket s =
        \mathbf{ff}
        $$
    - Bucle:
        - Si se cumple:
        $$
        \left[ \mathrm{while}_{\mathrm{ns}}^{\mathrm{tt}} \right] := \frac{env_P \vdash \langle
        S, s \rangle \rightarrow s', env_P \vdash \langle \mathtt{while}\ b\ \mathtt{do}\ S, s'
        \rangle \rightarrow s''}{env_P \vdash \langle \mathtt{while}\ b\ \mathtt{do}\ S, s\rangle \rightarrow
        s''},\\
        \text{ si } \mathcal{B}\llbracket b \rrbracket s = \mathbf{tt}
        $$
        - Si no se cumple:
        $$
        \left[ \mathrm{while}_{\mathrm{ns}}^{\mathrm{ff}} \right] := env_P \vdash \langle \mathtt{while}\ b\ \mathtt{do}\ S, s \rangle \rightarrow s,\
        \text{ si } \mathcal{B}\llbracket b \rrbracket s = \mathbf{ff}
        $$

    - Bloque:
        $$
        \left[ \mathrm{block}_{\mathrm{ns}} \right] := \frac{\langle D_V, s \rangle
        \rightarrow_{D} s',\ \mathrm{udp}_P\left( D_P, env_P \right) \vdash \langle S, s' \rangle \rightarrow s''}{env_P \vdash \langle
        \mathtt{begin}\ D_V\ S\ \mathtt{end}, s \rangle \rightarrow s''\left[
        \mathrm{DV}\left( D_V \right) \mapsto s \right]}
        $$

    - Llamada:
        $$
        \left[ \mathrm{call}_{\mathrm{ns}}^{\mathrm{rec}} \right] := \frac{env_P \vdash \langle S, s \rangle \rightarrow s'}{env_P \vdash \langle \mathtt{call}\ p, s \rangle \rightarrow s'},\ \text{ si } env_P\ p = S
        $$

#### Variables dinámicas y procedimientos estáticos
- Entorno de procedimientos:
    : Función que asocia los nombres de los procedimientos a sus cuerpos:
    $$
    \mathbf{Env}_P = \mathbf{Pname} \hookrightarrow \mathbf{Stm} \times
    \mathbf{Env}_P
    $$
    Necesitamos recordar también los procedimientos *ya declarados* en el
    momento que se declara uno nuevo (puesto que las llamadas son estáticas).
    Ahora necesitamos una nueva definición para $\mathrm{udp}_P$ que será:
    $$
    \begin{cases}
        \mathrm{udp}_P \left( \varepsilon, env_P \right) &= env_P\\
        \mathrm{udp}_P \left( \mathtt{proc}\ p\ \mathtt{is}\ S; D_P, env_P \right) &= \mathrm{udp}_P \left( D_P, env_P \left[ p \mapsto \left( S, env_P \right) \right] \right)\\
    \end{cases}
    $$
    Lo que quiere decir que, cuando actualizamos el entorno para añadir $p$,
    guardamos no solo su cuerpo sino también el entorno *en ese momento*.

- Con esto ya podemos actualizar la definición semántica. En general será igual
    a la vista en la anterior sección a excepción de las llamadas:
    $$
    \left[ \mathrm{call}_{\mathrm{ns}} \right] := \frac{env_P' \vdash \langle S, s \rangle \rightarrow s'}{env_P \vdash \langle \mathtt{call}\ p, s \rangle \rightarrow s'},\ \text{ si } env_P\ p = \left( S, env'_P \right)
    $$
    Sin embargo, en este caso las llamadas recursivas no las tenemos
    directamente y tenemos que añadir una nueva derivación para el caso de que
    una función se llame a sí misma (observar que cuando actualizamos el
    entorno, no $p$ no tiene asociado el entorno en el que ha sido declarada
    sino *el anterior*).
    $$
    \left[ \mathrm{call}_{\mathrm{ns}}^{\mathrm{rec}} \right] := \frac{env_P'\left[ p \mapsto \left( S, env'_P \right) \right] \vdash \langle S, s \rangle \rightarrow s'}{env_P \vdash \langle \mathtt{call}\ p, s \rangle \rightarrow s'},\ \text{ si } env_P\ p = \left( S, env'_P \right)
    $$
    En esta nueva definición estamos añadiendo el valor de $p$ al entorno
    anterior al que se declaró con lo que podemos realizar la llamada recursiva.

#### Variables y procedimientos estáticos
- Para tener variables estáticas necesitamos modificar el estado y «separarlo»
    en dos nuevas funciones distintas. Una guardará la *localización* de la
    variable en un memoria abstracta mientras que la otra *asignará* un valor a
    esa dirección. Con esto el estado será la composición de estas dos
    funciones.

- Funciones $\mathbf{Env}_V$:
    : Esta funciones mapean variables de $\mathbf{Var}$ a posiciones de una memoria
    abstracta, $\mathbf{Loc}$:
    $$
    \mathbf{Env}_V := \mathbf{Var} \rightarrow \mathbf{Loc}
    $$
- Ahora teniendo un "memoria" $\mathbf{Loc}$, necesitamos una forma de obtener
    la siguiente dirección libre. Para ello usamos $new$ que es una función que
    dada una dirección, nos devuelve la siguiente libre:
    $$
    \mathrm{new} : \mathbf{Loc} \rightarrow \mathbf{Loc}
    $$

- Funciones $\mathbf{Store}$:
    : Estas funciones asignan a las direcciones de $\textbf{Loc}$ valores que
    pueden tener las variables (en el caso de **WHILE**, enteros):
    $$
    \mathbf{Store} = \mathbf{Loc} \cup \left\{ \mathtt{next} \right\} \rightarrow \mathbb{Z}
    $$
    El elemento $\mathtt{next}$ representa la siguiente
    dirección de memoria libre.

- Con esto la semántica de las declaraciones de variables tendrá que ser
    actualizada de la siguiente forma:
    $$
    \langle D_V, env_V, sto \rangle \rightarrow_D \left( env'_V, sto' \right)
    $$
- Con todo esto ya sí podemos dar las definiciones de la semántica de las
    declaraciones de variables. De nuevo lo haremos de forma recursiva:
    - Caso base:
        $$
        \left[ \mathrm{none}_{\mathrm{ns}} \right] := \langle \varepsilon,
        env_V, sto \rangle \rightarrow_D \left( env_V, sto \right)
        $$
    - Caso recursivo: $\left[ \mathrm{var}_{\mathrm{ns}} \right] :=$
        $$
        \begin{gather*}
        \frac{\langle D_V, env_V
        \left[ x \mapsto l \right], sto\left[ l \mapsto v \right]\left[
        \mathtt{next} \mapsto \mathrm{new}\ l \right] \rangle \rightarrow_D
        \left( env'_V, sto' \right)}{\langle \mathtt{var}\ x := a;\ D_V, env_V, sto \rangle \rightarrow_D \left( env_V', sto' \right)},\\
        \text{si } v = \mathcal{A}\llbracket a \rrbracket\left( sto \circ env_V \right) \text{ y } l = sto\ \mathtt{next}
        \end{gather*}
        $$

- De forma intuitiva, la anterior definición nos indica varias cosas:
    - El elemento $v$ nos da el valor de la nueva variable $x$ en el estado
        anterior a su declaración mientras que $l$ nos indica la posición
        que ocupará.
    - En la premisa estamos actualizando recursivamente el entorno de las
        variables añadiendo la posición que ocupará $x$ y el *store* con el
        valor de $x$ (que se asociará a su posición $l$) así como con el nuevo
        $\mathtt{next}$ que lo aportará la función $\mathrm{new}$, que hemos
        definido antes.

- Con todas estas definiciones ya estamos en disposición de definir el *entorno
    de procedimientos* para este caso. Claramente será necesario el entorno de
    variables. Por tanto:
    $$
    \mathbf{Env}_P = \mathbf{Pname} \hookrightarrow \mathbf{Stm} \times
    \mathbf{Env}_V \times \mathbf{Env}_P
    $$
    Y, claro, la actualización tendrá que "recordar" también las variables
    declaradas en ese momento: $\mathrm{udp}_P: \mathbf{Dec}_p \times
    \mathbf{Env}_V \times \mathbf{Env}_P \rightarrow \mathbf{Env}_P$.
    $$
    \begin{cases}
        \mathrm{udp}_P \left( \varepsilon, env_V, env_P \right) &= env_P\\
        \mathrm{udp}_P \left( \mathtt{proc}\ p\ \mathtt{is}\ S; D_P, env_V, env_P \right) &= \mathrm{udp}_P \left( D_P, env_V, env_P \left[ p \mapsto \left( S, env_V, env_P \right) \right] \right)\\
    \end{cases}
    $$
