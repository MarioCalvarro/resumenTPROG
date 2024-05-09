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
