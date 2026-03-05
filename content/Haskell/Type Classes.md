Antes de hablar sobre Type classes, quiero que observemos el siguiente comportamiento:
```Haskell
ghci> ([1,2] ++ [3,4]) ++ [5,6] 
[1,2,3,4,5,6]

ghci> [1,2] ++ ([3,4] ++ [5,6]) 
[1,2,3,4,5,6]

ghci> [1,2,3] ++ [] 
[1,2,3]

ghci> 3 + 0 
3
```
- **Listas con concatenación (`++`)**: El orden de agrupación no importa y concatenar una lista vacía `[]` no cambia el resultado.
- **Números con suma (`+`)**: El orden en que sumas no importa y sumar el cero `0` no cambia el resultado.
#### Definición
En Matemáticas, a cualquier estructura que consista en un conjunto de elementos (como números o listas) y una operación binaria sobre ese conjunto (como `+` o `++`) se le llama **Monoide** si satisface dos propiedades clave:
- **Elemento neutro**: Si tomas cualquier elemento $x$ y aplicas la operación binaria con el elemento neutro, obtendrás $x$ de vuelta.
- **Asociatividad**: Reacomodar los paréntesis en una ecuación no cambiará el resultado.

¿Cómo representamos a los Monoides en Haskell? Con ayuda de las Type classes. **Una type class NO es un tipo, es una proposición acerca de un tipo:**
```Haskell
class Monoide a where
    neutro :: a
    op :: a -> a -> a
```
Existe un tipo `a` del cual podemos demostrar las proposiciones `a -> a -> a` y `a`. Mediante un `instance` podemos implementar o "demostrar" ese conjunto de proposiciones para un tipo en específico:
```Haskell
instance Monoide [a] where
    neutro = []
    op = (++)

instance Monoide Int where
    neutro = 0
    op = (+)
```
Veamos cómo el mismo operador (`op`) puede ser usado para dos tipos distintos.
```Haskell
ghci> op [1,2,3] [4,5,6]
[1,2,3,4,5,6]

ghci> op (4 :: Int) 2
6
```
Pero, ¿qué hay de la operación de suma de la que hablamos hace un momento? Si revisamos el tipo de la función original de Haskell, veremos una sintaxis particular con una flecha doble `=>`:
```Haskell
ghci> :t (+)
(+) :: Num a => a -> a -> a
```
El `Num a` que aparece antes de la `=>` es una **restricción** (una restricción de type class). Podemos leerlo como: _"Para cualquier tipo `a`, siempre que `a` sea un ejemplar de `Num`, el operador `(+)` puede funcionar"_.

Si el polimorfismo paramétrico es una promesa incondicional de que una función funcionará para cualquier tipo, una función polimórfica es una **promesa restringida**: funcionará para cualquier tipo, _siempre y cuando_ ese tipo sea un ejemplar de la clase requerida.
```Haskell
colap :: Monoide a => [a] -> a
colap [] = neutro
colap (x:xs) = x `op` colap xs
```
Una restricción de clase como `Monoide a =>` es una **obligación de demostración**; es decir, el compilador debe encontrar pruebas de que el tipo `a` que le estás pasando realmente es un Monoide.