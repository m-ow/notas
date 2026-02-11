Para acercarnos lo más posible a su presentación en la teoría, utilizaremos `strings` para el nombre de las variables. El principal problema con esto es que no podemos asumir la convención de Barendregt, tal como lo haríamos en papel.
```Haskell
type Nombre = String
data Expr
= Var Nombre
| App Expr Expr
| Lam Nombre Expr
```

| Cálculo λ                        | Haskell                                      |
| -------------------------------- | -------------------------------------------- |
| $xe$                             | `(App (Var "x") (Var "e"))`                  |
| $\lambda x.x \equiv \lambda y.y$ | `(Lam "x" (Var "x")) /= (Lam "y" (Var "y"))` |

Para lidiar con este problema vamos a tener que implementar una función que determine si dos términos son iguales renombrando el nombre de sus variables ligadas, en otras palabras, una función que determine su $\alpha$-equivalencia.
```Haskell
aeq :: Expr -> Expr -> Bool
```
Además, de una función que maneje el caso en que una sustitución entra en conflicto con los nombres de las variables libres, es decir, que evite cosas como: $(\lambda x.xy)[y := x] \to λx.xx$
y así, la sustitución solo proceda si la variable no se encuentra en el conjunto de variables libres de la expresión, y si lo hace, se cree una nueva variable en su lugar.
`if` $x \notin FV(a)$ `then` $(\lambda x.e)a \to e[x:=a]$
```Haskell
subst :: Nombre -> Expr -> Expr -> Expr
```

---
### Referencias
- [How to Implement the Lambda Calculus, Quickly](https://github.com/sweirich/lambda-n-ways/blob/main/doc/Part1.md)
- [Write You a Haskell | Substitution](https://smunix.github.io/dev.stephendiehl.com/fun/WYAH.pdf)
