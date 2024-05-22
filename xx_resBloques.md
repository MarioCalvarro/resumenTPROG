# Reglas bloques
## Operacional
### Dinámico
- Variables: Cambiar estado de la variable declarada
- Bloques: 
    - Significado de la declaración de variables
    - Actualizar el entorno de procedimientos (cada nombre, una sentencia)
    - Ejecutar el código del bloque
    - Restaurar variables y entorno (no hay que hacer nada para procedimientos)

- Llamadas: Sacar el código de $p$ y ejecutarlo. Son directamente recursivas.

### *Mixed*
- Todo menos procedimientos.
- Procedimientos: Mapa de nombres a pareja sentencia y entorno en el momento que
    se declaran.
- Llamadas:
    - No recursivas: Ejecutamos $p$ en el entorno de procedimientos que se
        declaró.
    - Recursivas: Igual que no recursivas, pero añadimos al entorno la
        declaración de $p$. Tenemos que hacerlo en cada llamada.

### Estático
- Variables: 
    - El estado pasa a ser una combinación de variables a memoria
        ($\mathbf{Loc}$) y valores en estas direcciones $\mathbf{Store}$.
    - Necesitamos $\mathrm{new}$, que da la siguiente dirección libre dada otra
        dirección, y $\mathtt{next}$ que es la dirección libre.
    - La semántica de las declaraciones devolverá una pareja de entornos y
        stores. Tendrá que dar a la variable su dirección, poner en esta
        dirección su valor y generar el nuevo $\mathtt{next}$.

- Procedimientos:
    - Tendrán que almacenar también el entorno de variables.
    - Al actualizarlo tenemos que asignar a un nombre su cuerpo y los entornos
        de variables y procedimientos en ese momento.

- Bloques:
    - Significado de la declaración de variables.
    - Actualizamos entorno de procedimientos.
    - Ejecutamos el bloque.
    - No hace falta restaurar la memoria porque las direcciones nuevas que se
        "se marcan" como libres porque el entorno original (que es el que se
        tiene al final) no las tenía.

- Llamadas: Sacamos los entornos de variables y procedimientos de $p$ y
    ejecutamos ahí. Si es recursiva, añadimos $p$ al antiguo entorno.

## Denotacional
- Será todo estático
- Usamos el artilugio de tener una memoria abstracta, pero añadimos la función
    $\mathrm{lookup}$ que devuelve el estado habitual.
- Actualizamos las definiciones habituales para tener usar el entorno en vez del
    estado.
- Variables: al declarar damos localización a la nueva variable, asignamos valor
    a esa posición y generamos el nuevo $\mathtt{next}$.

- Procedimientos: el entorno asociará un nombre con un *significado*
    directamente. Este significado será el valor semántico de su cuerpo en los
    entornos de ese momento.
    - Recursivos: Tenemos que hacer un punto fijo al añadir el propio $p$ al
        entorno de procedimientos. El funcional es:
        $$
        F\ g = \mathcal{S}_{\mathrm{ds}} \llbracket S \rrbracket\ env_V\ env_P
        \left[ p \mapsto g \right].
        $$
        Es decir, el significado de $p$ será el punto fijo de eso.

- Bloques: Su significado será ejecutar el cuerpo tras actualizar los entornos y
    la memoria con las nuevas declaraciones.

- Llamadas: Su significado es directamente el del procedimiento.
