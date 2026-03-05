En Haskell tenemos una manera análoga de trabajar a como lo hacemos en el cálculo $\lambda$. Por ejemplo, si escribimos la abstracción $\lambda x . x + x$, esto en Haskell se vería de la siguiente manera:
```Haskell
\x -> x + x
```
Usamos una flecha `->` en lugar de un punto para denotar el cuerpo de la función, y una diagonal invertida `\` porque visualmente se asemeja a una $\lambda$.

Para la aplicación, el proceso se ve así:
```Haskell
ghci> (\x -> x + x) 6
12
```
Su reducción análoga en el cálculo $\lambda$ puro sería:
$$(\lambda x. x + x) 6 = (x + x) [x := 6] = 6 + 6 = 12$$
Ahora, quizá me digan: _¡Estás haciendo trampa! Estás usando la suma y los números de Haskell_. Y tienen razón, así que les mostraré los numerales de Church desde cero. 

| Número   | Cálculo $\lambda$                            |
| -------- | -------------------------------------------- |
| 0        | $\lambda f . \lambda x . x$                  |
| 1        | $\lambda f . \lambda x . f\,x$               |
| 2        | $\lambda f . \lambda x . f \, (f\,x)$        |
| 3        | $\lambda f . \lambda x . f \, (f \, (f\,x))$ |

```Haskell
let cero = \f -> \x -> x
let uno  = \f -> \x -> f x
let dos  = \f -> \x -> f (f x)
let tres = \f -> \x -> f (f (f x))

let churchToInt = \c -> c (+1) 0
-- La suma en el cálculo lambda
let sumar = \m -> \n -> \f -> \x -> m f (n f x) 
```

$$sumar \equiv \lambda m . \lambda n . \lambda f . \lambda x . m f (n f x)$$

```Haskell
ghci> churchToInt (sumar dos tres)
5 
```
