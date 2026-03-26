Recordemos cómo están definidos `Either` y `Maybe`:
```Haskell
data Maybe a = Nothing | Just a
data Either e a = Left e | Right a
```
Notemos que tanto `e` como `a` pueden representar cualquier tipo concreto (`Bool`, `String`, `Int`, etc.). Esto lo podemos confirmar viendo sus _kinds_ (tipos de los tipos) en GHCi:
```Haskell
ghci> :k Either
Either :: * -> * -> *
ghci> :k Maybe
Maybe :: * -> *
```
_(Donde `*` representa el tipo de todos los tipos concretos)._

En este sentido, ¿cuál será la firma de nuestra clase `Monada`?
```Haskell
ghci> :k Monada
Monada :: (* -> *) -> Constraint
```
La salida es una restricción de clase (`Constraint`), la cual es la salida de todas las _Type Classes_ en Haskell.
```Haskell
ghci> :k Monada Maybe
Monada Maybe :: Constraint
ghci> :k Monada (Either e)
Monada (Either e) :: Constraint
```
**Nota sobre cómo leer las firmas:**
> En Haskell, los tipos concretos siempre empiezan con mayúscula (`Int`, `String`, `Bool`). Cuando vemos letras en minúscula (como `a`, `b`, `m`, `x`), estamos viendo variables de tipo.
> La regla de oro es:
> - **Letras diferentes** (ej. `a -> b`): Los tipos _pueden_ ser diferentes (ej. de `Int` a `String`), pero no están obligados a serlo.
> - **Letras repetidas** (ej. `a -> a`): El compilador exigirá estrictamente que ambos lugares sean ocupados por _exactamente_ el mismo tipo.

---
## Ejemplo Completo con la Mónada Nativa
A partir de este punto, dejamos de usar nuestra clase `Monada` de juguete y pasamos a usar la clase `Monad` nativa de Haskell.
Aquí tenemos un programa que:
1. Toma una función para buscar elementos en una lista.
2. Toma una lista.
3. Devuelve una tupla con el primero, tercero, quinto y séptimo elemento.
```Haskell
primeroTerceroQuintoSeptimo :: Monad m
                               => ([a] -> Int -> m a)
                               -> [a]
                               -> m (a, a, a, a)
primeroTerceroQuintoSeptimo buscar xs = do
    primero <- buscar xs 0
    tercero <- buscar xs 2
    quinto  <- buscar xs 4
    septimo <- buscar xs 6
    return (primero, tercero, quinto, septimo)
```
Lo que aparece antes de la flecha doble (`=>`) es la restricción de clase. Se lee como: _"Para cualquier tipo `m`, siempre que `m` sea una instancia de `Monad`, este programa funcionará"_.

Como `Either` y `Maybe` son mónadas, podemos crear funciones de búsqueda que satisfagan la firma esperada `([a] -> Int -> m a)`:
```Haskell
buscarMaybe :: [a] -> Int -> Maybe a
buscarMaybe xs i
    | i < length xs = Just (xs !! i)
    | otherwise     = Nothing

buscarEither :: [a] -> Int -> Either String a
buscarEither xs i
    | i < length xs = Right (xs !! i)
    | otherwise     = Left ("Error: Indice " ++ show i ++ " fuera de rango.")
```
**Conclusión:** Así como `Maybe` te indica que algo ha fallado pero no te dice por qué, `Either` te permite incluir el motivo exacto del fallo, propagándolo de forma segura por todo el programa.