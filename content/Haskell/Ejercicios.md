Tratemos de demostrar los siguientes teoremas construyendo programas que compilen.

Ejercicio 1: Identidad $A \implies A$ "Dado que asumimos A, concluimos A".
```Haskell
identidad :: a -> a
identidad a = a
```
Ejercicio 2: Constante** $A \implies B \implies A$
```Haskell
constante :: a -> b -> a
constante a b = a
```
Ejercicio 3: Modus Ponens $(A \implies B) \implies A \implies B$
```Haskell
modusPonens :: (a -> b) -> a -> b
modusPonens f x = f x
```
Ejercicio 4: Silogismo Hipotético $(A \implies B) \implies (B \implies C) \implies (A \implies C)$
```Haskell
transitividad :: (a -> b) -> (b -> c) -> (a -> c)
transitividad f g x = g (f x)
-- También válido usando composición:
-- transitividad f g = g . f
```
Ejercicio 5: Conmutatividad de la Conjunción $A \land B \implies B \land A$****
```Haskell
conjConmuta :: (a, b) -> (b, a)
conjConmuta (x, y) = (y, x)
```
Ejercicio 6: El problema de la Negación y Lógica Intuicionista
En Haskell, el tipo `Void` (que no tiene constructores) representa lo falso ($\bot$).
```Haskell
data Falso -- Tipo sin constructores
type Not a = a -> Falso

{-# LANGUAGE EmptyCase #-}
exFalso :: Falso -> a
exFalso x = case x of {}
```
¿Qué pasa con el Tercio Excluso ($A \lor \neg A$) o la Doble Negación?
```Haskell
-- dobleNeg :: Not (Not a) -> a
-- dobleNeg = ???

-- tercioExcluso :: Either a (Not a)
-- tercioExcluso = ???
```
Nos quedaremos atrapados. ¿Por qué? Porque lenguajes como Haskell (y asesores de pruebas como Rocq o LEAN) operan bajo lógica _constructivista_. Si afirmas que algo existe o es verdadero, tienes que construir el algoritmo para obtenerlo.
### Resumen

| Proposición            | Tipo         |
| ---------------------- | ------------ |
| Implicación $A \to B$  | `a -> b`     |
| Conjunción $A \land B$ | `(a, b)`     |
| Disyunción $A \lor B$  | `Either a b` |
| Verdadero ($\top$)     | `()`         |
| Falso ($\bot$)         | `Void`       |
| Negación $\neg A$      | `a -> Void`  |

| **Reglas en Deducción Natural** | **Operaciones en Haskell**                  |
| ------------------------------- | ------------------------------------------- |
| Intro $\to$                     | `\x -> ...`                                 |
| Elim $\to$                      | `f x`                                       |
| Intro $\land$                   | `(x, y)`                                    |
| Elim $\land$                    | `fst p`, `snd p`                            |
| Intro $\lor$                    | `Left x`, `Right y`                         |
| Elim $\lor$                     | `case x of {Left a -> ...; Right b -> ...}` |
