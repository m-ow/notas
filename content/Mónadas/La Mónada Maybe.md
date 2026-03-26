La mónada más simple es aquella que maneja cálculos con posibilidad de fallo.
```Haskell
divisionSegura :: Int -> Int -> Maybe Int
divisionSegura _ 0 = Nothing
divisionSegura x y = Just (x `div` y)

cabezaSegura :: [a] -> Maybe a
cabezaSegura [] = Nothing
cabezaSegura (x:_) = Just x
```
Ahora, si queremos usar estas funciones juntas, como sacar la cabeza de la lista y luego dividir el número `2` entre ese valor, podríamos hacer lo siguiente. Noten que si el primer elemento de la lista es un `0`, la división fallará devolviendo `Nothing`, lo cual demuestra la utilidad de esta mónada para manejar errores sin detener el programa entero.
```Haskell
calculoAnidado :: [Int] -> Maybe Int
calculoAnidado xs =
    case cabezaSegura xs of
        Nothing -> Nothing
        Just x  ->
            case divisionSegura 2 x of
                Nothing -> Nothing
                Just y  -> Just y
```
## La Operación Bind (`>>=`)
¿Podemos escribir lo anterior de una forma más sencilla? Claro que sí, utilizando la operación _bind_ (escrita como `>>=`), que representa el corazón de toda mónada. En el caso de `Maybe`, se interpreta así: _si el cálculo a su izquierda ha tenido éxito, se aplica la función de la derecha; si ha fallado, se propaga el error (Nothing)._
```Haskell
calculoBind :: [Int] -> Maybe Int
calculoBind xs =
    cabezaSegura xs >>= \x ->
    divisionSegura 2 x >>= \y ->
    return y
```
_(Para comprender mejor su funcionamiento, es un gran ejercicio ejecutar a mano algunos ejemplos de esta función)._
## La Notación Do
¿Podemos escribirlo de una forma todavía más clara? Sí, con la notación `do`.

La notación `do` es _azúcar sintáctico_ para hacer que el código con mónadas parezca imperativo.
```Haskell
calculoDo :: [Int] -> Maybe Int
calculoDo xs = do
    x <- cabezaSegura xs
    y <- divisionSegura 2 x
    return y
```
**Ojo:** Esto no es un permiso para escribir código imperativo cuando no sepamos hacerlo de forma funcional. Es simplemente aceptar que los seres humanos pensamos mejor en secuencias paso a paso. La sintaxis es más cómoda, pero la abstracción matemática se mantiene intacta.
### Tabla de traducción

| Si escribes esto en un bloque `do`...   | El compilador lo traduce a esto...         |
| :-------------------------------------- | :----------------------------------------- |
| `x <- expresion` <br> `resto_del_do`    | `expresion >>= \x ->` <br> `resto_del_do`  |
| `let x = expresion` <br> `resto_del_do` | `let x = expresion in` <br> `resto_del_do` |

