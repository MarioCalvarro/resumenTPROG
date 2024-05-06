# Tema 4. Implementación correcta
## La máquina correcta
La máquina abstracta está compuesta de configuraciones que siguen el siguiente
patrón:
$$
\langle c, e, s \rangle \in \mathbf{Code} \times \mathbf{Stack} \times
\mathbf{State}
$$
donde $c$ es una serie de instrucciones ($push, add \ldots$), $e$ es la pila de
ejecución y $s$ es el estado del programa. La configuración final es $\langle
\varepsilon, e, s \rangle$

Las distintas instrucciones alteran la configuración de la máquina de la
siguiente manera:
- "Empujar" un número:
    $$
    \begin{align*}
    \langle \mathtt{PUSH-}n : c, e, s \rangle &\triangleright \langle c,
    \mathcal{N}\llbracket n \rrbracket : e, s \rangle\\
    \end{align*}
    $$

- "Empujar" un booleano:
    $$
    \begin{align*}
    \langle \mathtt{TRUE} : c, e, s \rangle &\triangleright \langle c,
    \mathbf{tt}: e, s \rangle\\
    \langle \mathtt{FALSE} : c, e, s \rangle &\triangleright \langle c,
    \mathbf{ff}: e, s \rangle\\
    \end{align*}
    $$

- Operaciones con enteros, $z_1, z_2 \in \mathbb{Z}$:
    $$
    \begin{align*}
    \langle \mathtt{ADD} : c, z_1 : z_2 : e, s \rangle &\triangleright \langle
    c, \left( z_1 + z_2 \right) : e, s \rangle\\
    \langle \mathtt{SUB} : c, z_1 : z_2 : e, s \rangle &\triangleright \langle
    c, \left( z_1 - z_2 \right) : e, s \rangle\\
    \langle \mathtt{MULT} : c, z_1 : z_2 : e, s \rangle &\triangleright \langle
    c, \left( z_1 * z_2 \right) : e, s \rangle\\
    \langle \mathtt{EQ} : c, z_1 : z_2 : e, s \rangle &\triangleright \langle
    c, \left( z_1 = z_2 \right) : e, s \rangle\\
    \langle \mathtt{LE} : c, z_1 : z_2 : e, s \rangle &\triangleright \langle
    c, \left( z_1 \le z_2 \right) : e, s \rangle\\
    \end{align*}
    $$

- Operaciones con booleanos, $t, t_1, t_2 \in \mathbf{T}$:
    $$
    \begin{align*}
    \langle \mathtt{AND} : c, t_1 : t_2 : e, s \rangle &\triangleright \begin{cases}
        \langle c, \mathbf{tt} : e, s \rangle, &\text{si } t_1 = \mathbf{tt} =
        t_2\\
        \langle c, \mathbf{ff} : e, s \rangle &\text{c.c}
    \end{cases}\\
    \langle \mathtt{NEG} : c, t : e, s \rangle &\triangleright \begin{cases}
        \langle c, \mathbf{ff} : e, s \rangle &\text{si } t = \mathbf{tt}\\
        \langle c, \mathbf{tt} : e, s \rangle &\text{c.c}
    \end{cases}\\
    \end{align*}
    $$

- Acceso a memoria:
    $$
    \begin{align*}
    \langle \mathtt{FETCH-}x : c, e, s \rangle &\triangleright \langle
    c, \left( s\ x \right) : e, s \rangle\\
    \langle \mathtt{STORE-}x : c, z : e, s \rangle &\triangleright \langle
    c, e, s\left[ x \mapsto z \right] \rangle\\
    \end{align*}
    $$

- Control de "flujo":
    $$
    \begin{align*}
    \langle \mathtt{NOOP} : c, e, s \rangle &\triangleright \langle
    c, e, s \rangle\\
    \langle \mathtt{BRACH}\left( c_1, c_2 \right) : c, t : e, s \rangle
    &\triangleright \begin{cases}
    \langle c_1 : c, e, s \rangle &\text{si } t = \mathbf{tt}\\
    \langle c_2 : c, e, s \rangle &\text{c.c}
    \end{cases}\\
    \langle \mathtt{LOOP}\left( c_1, c_2 \right) : c, e, s \rangle &\triangleright \langle
    c_1 : \mathtt{BRANCH}\left( c_2: \mathtt{LOOP}\left( c_1, c_2 \right),
    \mathtt{NOOP} \right) : c, e, s \rangle\\
    \end{align*}
    $$
    Para la última instrucción tenemos que $c_1$ sería algo así como la
    "condición" y $c_2$ el cuerpo del bucle.

Un programa en esta máquina abstracta puede alcanzar dos tipos de
configuraciones: con la cola de instrucciones vacía (con lo que la ejecución ha
sido exitosa) o con la cola no vacía (con lo que se ha alcanzado una
configuración en la que la máquina no ha podido ejecutar la siguiente
instrucción).

### Propiedades
Esta máquina abstracta se rige, como acabamos de ver, por unas reglas muy
similares a las de la semántica operacional de paso corto. Por esta razón, la
mayoría de propiedades de esta última tienen equivalencia aquí (Ver ejercicios).

### La función *ejecución*
El significado de una secuencia de instrucciones será una función *parcial* de
estado a estado definida de la siguiente manera:
$$
\begin{gather*}
\mathcal{M}: \mathbf{Code} \rightarrow \left( \mathbf{State} \hookrightarrow
\mathbf{State} \right)\\
\mathcal{M}\llbracket c \rrbracket s = \begin{cases}
    s', &\text{si } \langle c, \varepsilon, s \rangle \triangleright^* \langle
    \varepsilon, e, s' \rangle\\
    \mathtt{undefinied},\ \text{c.c}
\end{cases}.
\end{gather*}
$$
Es decir, que un programa tendrá significado si se pueden ejecutar todas sus
instrucciones.

## Especificación de la traducción
### Expresiones
### Instrucciones
### Función semántica

## Corrección
### Expresiones
### Instrucciones
