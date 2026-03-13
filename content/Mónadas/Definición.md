En programación funcional, una mónada se define mediante la siguiente type class.
```Haskell
class Monada m where
	return :: a -> m a
	(>>=) :: m a -> (a -> m b) -> m b
```
Recordemos que las type classes definen un conjunto de operaciones que un tipo debe implementar; son similares a las interfaces en la programación orientada a objetos y permiten el polimorfismo y la reutilización de código.