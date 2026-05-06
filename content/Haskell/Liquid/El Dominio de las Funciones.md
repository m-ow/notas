### Funciones Totales y Parciales 

- Una función *total* es aquella que está definida para todos los valores posibles de su tipo de entrada.

- Una función *parcial* es aquella que no está definida para ciertos valores de su tipo de entrada.

**Ejemplos de funciones parciales:**

```Haskell
ghci> head []
*** Exception: Prelude.head: empty list

ghci> 9 `div` 0
*** Exception: divide by zero
```
### El Dominio Matemático

En matemáticas, si tienes una función $f:A \to B$, el dominio (el conjunto $A$) es el conjunto de todos los valores de entrada válidos para los cuales la función está definida y produce un resultado válido en el codominio ($B$).

Por ejemplo, para la función matemática de la división $f(x) = \frac{9}{0}$:

- El dominio no son "todos los números reales".

- El dominio es $\mathbb{R}∖ \{0\}$ (Todos los números reales, excluyendo el cero).

- Si le pasamos un 0, la función simplemente "no tiene sentido" matemático. Está fuera de su dominio.

### El Problema del Dominio en Haskell
Cuando escribimos la siguiente función:
```Haskell
dividir :: Int -> Int -> Int
dividir x y = x `div` y
```

El compilador de Haskell lee esta firma y asume que el dominio del segundo argumento son todos los valores de tipo `Int`. Es decir, Haskell está afirmando que la función puede recibir cualquier entero, incluyendo el cero, y devolverá un entero válido.

Pero esto es una mentira, como acabamos de ver. 

### La Solución: Liquid Haskell 

Aquí es donde entra Liquid Haskell. Su trabajo principal es **permitirnos alinear el dominio del código con el dominio matemático real**.

Liquid Haskell nos permite tomar un tipo (como `Int`) y "refinarlo" usando lógica.

```Haskell
{-@ dividir :: Int -> {v:Int | v /= 0} -> Int @-}
dividir :: Int -> Int -> Int
dividir x y = x `div` y
```

Al escribir `{v:Int | v /= 0}`, estamos diciendo que el dominio real de este segundo argumento no es todo `Int`. Es el subconjunto de los enteros que cumplen la condición de ser diferentes de cero. 

Al restringir el dominio explícitamente en el tipo, nuestra función vuelve a ser una función total (es válida para todos los elementos).

Ahora, si alguien intenta escribir `dividir 9 0` en un archivo verificado por Liquid Haskell, el código será rechazado en tiempo de compilación, no en tiempo de ejecución.