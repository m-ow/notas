El cálculo lambda consiste de tres sencillos términos y todas las combinaciones que podemos hacer de ellos recursivamente.  Esto es, en la notación de Backus-Naur:
```
term ::=
var (Una variable) |
term term (Una aplicación) |
λ var . term (Una abstracción lambda)
```

La primera operación básica del cálculo lambda es la aplicación: Sean $F, A \in \Lambda$ (dos elementos de todo el conjunto de términos λ) notemos que, en la expresión: $FAF$ representa un algoritmo y $A$ representa la entrada que toma ese algoritmo, por lo tanto, $FA$, puede ser visto de dos formas: como el proceso en si de calcular $FA$, o bien como el resultado final de ese proceso.

La segunda operación básica es la λ abstracción: Recordemos que una función se define como una correspondencia que asigna exactamente un elemento de un conjunto a cada elemento de otro, por lo que si $f(x) = e$ , la función $f$ evaluada en $x$ siempre tendrá el valor $e$, ahora, si utilizamos esta nueva notación, decimos que un término λ liga su variable, es decir, si $e$ es una expresión que contiene o depende de $x$, entonces, $\lambda x . e$ denota a la función que toma una variable $x$ y devuelve $e$ ($x \mapsto e$, o bien $f = \lambda x . e$).

¿Por qué esta necesidad de usar funciones en programación? ¿Por qué no usar lenguajes imperativos? Con los ciclos `for` y `while` que ya conocemos. Bueno... porque un aspecto fundamental en todas las Matemáticas es el razonamiento ecuacional, según el cual si
$a = f (x)$, entonces la expresión $g(f (x), f (x))$ es igual a $g(a, a)$, en otras palabras, siempre podemos sustituir a las funciones por los valores que calculan. La idea central de la programación funcional es estructurar nuestros programas de tal manera que podamos razonar sobre ellos como un sistema de ecuaciones, tal y como hacemos en Matemáticas.

---
### Referencias
- [Introduction to Lambda Calculus | Application and abstraction](https://www.cse.chalmers.se/research/group/logic/TypesSS05/Extra/geuvers.pdf)
- [Write You a Haskell | Concepts](https://smunix.github.io/dev.stephendiehl.com/fun/WYAH.pdf)