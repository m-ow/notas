Existe más de una forma de definir qué es una mónada: Una preferida por programadores (`Monada1`) y otra preferida por matemáticos (`Monada2`).
```Haskell
import Prelude hiding (Monad, return, (>>=), fmap, join)

class Monada1 m where
    return :: a -> m a
    (>>=) :: m a -> (a -> m b) -> m b

class Monada2 m where
    fmap :: (a -> b) -> m a -> m b
    return' :: a -> m a
    join :: m (m a) -> m a

miFmap :: Monada1 m => (a -> b) -> m a -> m b
miJoin :: Monada1 m => m (m a) -> m a


miBind :: Monada2 m => m a -> (a -> m b) -> m b
```

**De programador a matemático:** Demuestra que con `Monada1` puedes construir las operaciones de la `Monada2`.
- Implementa `fmap` usando únicamente `>>=` y `return`.
- Implementa `join` usando únicamente `>>=`.

**De matemático a programador:** Demuestra que con `Monada2` puedes construir las operaciones de la `Monada1`.
- Implementa `(>>=)` utilizando únicamente `fmap` y `join`.

 > _Pista:_ Utiliza lambdas ($\lambda x$) en tus respuestas. Recuerda que en Haskell es `\x -> ...`




