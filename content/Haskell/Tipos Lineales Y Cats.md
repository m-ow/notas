## Motivación 

Al inicio del curso, presentamos la correspondencia Curry-Howard:

| Lógica | Computación |
|---|---|
| Proposición | Tipo |
| Demostración | Programa |

Más adelante, profundizamos en esta conexión:

| Lógica Intuicionista | Cálculo $\lambda$ tipado |
|---|---|
| Proposición | Tipo |
| Demostración | Término $\lambda$ |

En este proceso, descubrimos que existe más de una lógica, cada una con características particulares.

Ahora, en la recta final del curso, continuaremos explorando esta conexión. Veremos que no se limita únicamente a la lógica y la computación, sino que se extiende a la teoría de categorías (lo que se conoce como la correspondencia Curry-Howard-Lambek).


| Lógica | Computación | Teoría de Categorías |
|---|---|---|
| Proposición | Tipo | Objeto |
| Demostración | Programa | Morfismo |

Hasta el momento, ya hemos estudiado la lógica y el cálculo $\lambda$ lineal. Y, así como existe más de una lógica y más de un cálculo $\lambda$, cabe preguntarse: ¿existe también más de una teoría de categorías? 

La respuesta es un rotundo sí. Las categorías cartesianas cerradas corresponden a la lógica intuicionista, mientras que las categorías monoidales simétricas cerradas corresponden a la lógica lineal.

Como ya sabemos, la peculiaridad principal de la lógica lineal es que no nos permite duplicar ni descartar variables  En programación, esto nos ayuda a evitar errores; por ejemplo, facilita el seguimiento de la asignación y liberación de bloques de memoria.

Sin embargo, para mí, esto no es lo más interesante. La mayor ventaja de abandonar
las categorías cartesianas cerradas y abrazar las monoidales simétricas, junto con la lógica lineal, es que la correspondencia Curry-Howard se extiende nuevamente: a la Física y a la Topología.

| Lógica   | Computación  | Teoría de Categorías  | Física  | Topología  |
|---|---|---|---|---|
| Proposición  | Tipo  | Objeto  |  Sistema | Variedad  | 
|  Demostración |  Programa | Morfismo  | Proceso  | Cobordismo  |

¿Saben qué es la Piedra de Rosetta? Es un antiguo bloque de roca que contiene el mismo texto escrito en tres idiomas diferentes, que fue clave para descifrar los jeroglíficos egipcios.

Con este resultado ocurre algo muy similar. Puede que los físicos y los topólogos estén convencidos de no saber nada de computación o de lógica, pero en realidad pueden estar aplicando sus principios sin saberlo. 
Y viceversa, quizá se "descubran" nuevas cosas en física y topología que los computólogos y los lógicos ya conocían décadas atrás. Todo se podría reducir a un problema de traducción:
estar usando palabras distintas para referirnos, en el fondo, a lo mismo.

En esta sesión, veremos cómo, empezando desde la teoría de categorías, podemos pasar por Haskell para finalmente llegar a la lógica lineal.

### ¿Qué es una categoría?

Una categoría está formada por objetos y morfismos que los unen. Es un concepto muy sencillo. 
Un objeto se puede dibujar como un círculo o un punto, y un morfismo… es simplemente una flecha.

#### Objeto Inicial & Objeto Terminal

Un objeto que tiene una única flecha que sale hacia cada objeto. Se denomina objeto inicial, denotado con el símbolo $0$.

$$0 \to \_ $$

Por otro lado, un objeto al que le llega una única  flecha desde cada objeto. Se denomina objeto terminal, 
denotado con el símbolo $1$. 

$$\_ \to 1$$

En lógica, el objeto terminal representa a la verdad, simbolizada por $\top$ (top). El hecho de que haya una flecha que apunta hacia él desde cualquier objeto significa que $\top$ es verdadero, independientemente de cuáles sean tus hipótesis.

$$ \_ \implies \top $$

Por el contrario, el objeto inicial representa la falsedad lógica, la contradicción y se simboliza con $\bot$ (bot). El hecho de que haya una flecha desde él hacia cualquier objeto significa que se puede demostrar cualquier cosa partiendo de premisas falsas.

$$ \bot \implies \_ $$


| Haskell | Teoría de Categorías | Lógica |
|---|---|---|
| Tipo | Objeto | Proposición |
| Función | Morfismo (flecha) | Implicación |
| `Void` | Objeto inicial, $0$ | Falso $\bot$ |
| `()` | Objeto terminal, $1$ | Verdadero $\top$ |

En Haskell, el nombre del objeto terminal es `()` un par de paréntesis vacíos, que se pronuncia
"unidad", porque lo une todo. Mientras 
que el objeto inicial se llama `Void` (vacío), 
porque no tiene flechas que le lleguen (pues en lógica y en programación no queremos eso). 

En teoría de categorías, $x$ es un elemento de $A$ si $x : 1 \to A$ (es una flecha que va desde el objeto terminal hacia $A$). 

En teoría de tipos, "$x:A$" significa que $x$ es de tipo $A$. En Haskell usamos la siguiente notación: `x :: a`, para decir que `x` es un
término de tipo `a`. 

En lógica, dicha $x$ es la demostración de $A$, pues corresponde a la implicación
$\top \implies A$ (si $\top$ es verdadero, entonces $A$ es verdadero). 

Por lo tanto, `x :: A` y `x :: () -> A` denotan lo mismo. 

## Monoide

Ahora, recordemos lo que es un monoide,
concepto el cual vimos al hablar sobre
[[Type Classes]]. 

```Haskell
class Monoide m where
    x :: m -> m -> m
    u :: m
```

 Dado un tipo `m` y una función 
 `x :: m -> m -> m`, decimos que `m` es un monoide si cumple las siguientes condiciones:

- `x` es asociativa. 
- `x` tiene una unidad izquierda y otra derecha.

La asociatividad significa que:
```
(a `x` b) `x` c = a `x` (b `x` c)
```
(En otras palabras, que podemos omitir los paréntesis)

Tener unidades izquierda y derecha significa que hay `u :: m` tal que, para cualquier `a :: m`, se cumple lo siguiente.

```
u `x` a = a `x` u = a
```

Sabemos que `u :: m` y `u :: () -> m`
denotan lo mismo. Y también sabemos (gracias a
la currificación) que `x :: m -> m -> m` se puede escribir como `x :: (m, m) -> m`. 
Por lo tanto, nos podemos quedar con lo siguiente. 

```Haskell
class Monoide m where
    x :: (m, m) -> m
    u :: () -> m
```
## Comonoide 

Ahora, en teoría de categorías es muy común
hablar acerca de dualidad: en palabras muy simples, se trata de cambiar la dirección de las flechas, lo que antes era el origen ahora es el destino. En este caso ¿cuál sería el dual
del monoide? El comonoide. 

```Haskell
class Comonoide m where
    dividir :: m -> (m, m)
    destruir :: m -> ()
```

En Haskell, podemos implementar esto fácilmente para cualquier tipo.
```Haskell
instance Comonoide m where
    dividir m = (m, m)
    destruir m = ()
```

Se habla menos de los comonoides que de sus hermanos, los monoides, principalmente porque
en programación poder duplicar e ignorar un valor es algo que hacemos sin pensar.

```Haskell
f :: Int -> Int 
f x = x + x

g :: Int -> Int
g y = 42
```

Ni siquiera no nos lo pensamos dos veces a la hora de aplicar el argumento de una función dos veces, o de no aplicarlo en absoluto. Pero,
si quisiéramos ser explícitos, las funciones como esas podrían escribirse así:

```Haskell
f :: Int -> Int 
f x = let (x1, x2) = dividir x
      in x1 + x2

g :: Int -> Int
g y = let () = destruir y
      in 42
```
Como ya hemos reiterado, existen algunas situaciones en las que no queremos duplicar o descartar una variable. Tal es el caso cuando el argumento es un recurso externo, como un archivo, un puerto de red o un bloque de memoria. 

Un modelo de programación basado en una categoría cartesiana siempre presentará este problema.
La solución consiste en utilizar una categoría monoidal (cerrada) que no admita la duplicación ni la destrucción de objetos. Dicha categoría es donde viven los tipos lineales.
