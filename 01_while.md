---
title: "Resumen Teoría de la Programación"
author: "Mario Calvarro"
header-includes:
   - \usepackage{stmaryrd}
output:
    pdf_document
---

# Tema 1. El lenguaje WHILE
## Índice
[[toc]]
## Sintaxis
Tenemos los siguientes conjuntos:
- Numerales: $\mathrm{n} \in \mathrm{Num}$.
- Variables: $\mathrm{x} \in \mathrm{Var}$.
- Expresiones aritméticas: $\mathrm{a} \in \mathrm{Aexp}$.
- Expresiones booleanas: $\mathrm{b} \in \mathrm{Bexp}$.
- Sentencias: $\mathrm{S} \in \mathrm{Stm}$.

## Semántica
### Numerales
Siendo la sintaxis:

    n ::= 0 | 1 | n 0 | n 1

La función semántica es:

$$ \mathcal{N}: \mathrm{Num} \rightarrow \mathbb{Z} $$

$$ \mathcal{N}\llbracket 0 \rrbracket = 0\\ \mathcal{N}\llbracket 1 \rrbracket = 1\\ \mathcal{N}\llbracket n\ 0 \rrbracket = 2 \cdot \mathcal{N}\llbracket n \rrbracket\\ \mathcal{N}\llbracket n\ 1 \rrbracket = 2 \cdot \mathcal{N}\llbracket n \rrbracket + 1 $$

### Composición
Definiciones composicionales
: 1. Una categoría sintáctica se define dando una *sintaxis abstracta* de unos
   *elementos básicos* y otros *compuestos*, que se pueden descomponer en sus
   constituyentes inmediatos.
2. La semántica de esta categoría se define *composicionalmente*. Se da la
   definición directa de los elementos base y de los compuestos se da en base a
   la semántica de sus constituyentes.

Inducción estructural
: 1. Probar que la propiedad se da para los elementos base de la categoría.
2. Asumiendo, *hipótesis de inducción*, que la propiedad se cumple para los constituyentes de un elemento
   compuesto, probar que también se cumple para el elemento en sí.

Con esto buscamos *homomorfismos de álgebras* usando los constructores como
operadores.

### Expresiones
Para las expresiones tenemos que definir la función *estado*:
$$
\mathrm{State} = \mathrm{Var} \rightarrow \mathbb{Z}.
$$
Con esto podemos definir la semántica de las expresiones:
$$
\mathcal{A}: \mathrm{Aexp} \rightarrow \left( \mathrm{State} \rightarrow \mathbb{Z} \right)\\
\mathcal{B}: \mathrm{Bexp} \rightarrow \left( \mathrm{State} \rightarrow \mathbb{T} \right)
$$
que se definen composicionalmente de manera natural.

## Propiedades de la semántica
### Variables libres
Llamamos *variables libres* de una expresión a aquellas variables que se
encuentran en la misma. Se definen composicionalmente sobre los operadores de la
expresión de manera natural.

Lema
: Sean $s$ y $s'$ dos estados tales que $s\ x = s'\ x,\ \forall x \in
\mathrm{FV}\left( a \right)$. Entonces, $\mathcal{A} \llbracket a \rrbracket s =
\mathcal{A} \llbracket a \rrbracket s'$.

### Sustituciones
Una sustitución en una expresión consiste en el reemplazamiento de cada
aparición de una variable libre por una expresión. De nuevo, se define
composicionalmente de manera natural, pero para el caso base será así:
$$
x \left[ y \rightarrow a_0 \right] = \begin{cases}
    a_0 &\text{si } x = y\\
    x &\text{si } x \neq y
\end{cases}
$$
Dicho de otra forma, los estados se actualizarán tal que:
$$
(s \left[ y \rightarrow v \right]) x = \begin{cases}
    v &\text{si } x = y\\
    s\ x &\text{si } x \neq y
\end{cases}
$$
