Vamos a empezar ocultando las siguientes funciones (`return` y `>>=`) para usar nuestra propia implementación con fines didácticos. Más adelante, cuando usemos funciones avanzadas, dejaremos de ocultarlas para usar la implementación nativa de Haskell.
```Haskell
import Prelude hiding (Monad, return, (>>=))

class Monada m where
    return :: a -> m a
    (>>=)  :: m a -> (a -> m b) -> m b
```
Recordemos que una mónada es, en esencia, **2 operaciones junto con 3 leyes**. El compilador de Haskell se limita a verificar las firmas de las operaciones; es decir, no necesitamos demostrar formalmente las leyes para convencer al compilador de que un tipo cumple con ser una mónada (cosa que en lenguajes como Rocq u otros asesores de pruebas sí podemos y debemos hacer).

A continuación, definimos cómo se comportan estas operaciones para los tipos `Maybe` y `Either`:
```Haskell
instance Monada Maybe where
    return x = Just x
    Nothing >>= _ = Nothing
    Just x  >>= f = f x

instance Monada (Either e) where
    return x = Right x
    Left err >>= _ = Left err
    Right x  >>= f = f x
```
## Pureza y Efectos Secundarios
Sabemos que una **función pura** es aquella que, dada la misma entrada, siempre produce la misma salida sin causar ningún efecto secundario o colateral. Un efecto se refiere a cualquier interacción con el mundo exterior.

Haskell es un lenguaje de programación funcional puro, lo que significa que sus funciones no tienen efectos secundarios. Sin embargo, los programas en la vida real _necesitan_ efectos. Pueden fallar, modificar una variable, leer datos introducidos por un usuario o lanzar misiles. Si llamas a la función `getUsername()`, esperas que consulte a una base de datos, no que inicie una guerra nuclear.

La cuestión aquí es: **¿Cómo representamos los efectos de manera que sepamos, solo a partir de los tipos, lo que podría hacer una función?** Las mónadas son una respuesta (no la única) para escribir programas con efectos secundarios en un lenguaje que carece de ellos. La programación funcional no prohíbe los efectos, pero exige que sean **sinceros y transparentes**. La firma de una función debe describir todo lo que el programa puede hacer, en lugar de limitarse solo a los argumentos que espera y el tipo de resultado que devuelve.