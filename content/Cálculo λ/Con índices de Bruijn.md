Recordemos que el principal axioma del cálculo $\lambda$ es:

$$
(\lambda x.M )N = M [x := N ] \quad \forall \, M,N \in \Lambda
$$

Sin embargo, una computadora no puede identificar los términos $\alpha$-equivalentes tal y como lo hacemos con nuestra mente, se necesita del siguiente axioma en su lugar.
$$
\lambda x.M = \lambda y.M [x := y], \quad
\text{siempre que $y$ no aparezca en $M$.}
$$
Una manera de ahorrarnos el lidiar con la $\alpha$-equivalencia es usar índices de Bruijn, donde, en lugar de nombrar a las variables, usamos números para representar la distancia que hay entre una variable ligada y su respectiva lambda.
```Haskell
type Ind = Int
data Expr
= Var Ind
| App Expr Expr
| Lam Expr
```

| **Cálculo λ (con nombres)**      | **Cálculo λ (de Bruijn)**    | **Haskell**                      |
| -------------------------------- | ---------------------------- | -------------------------------- |
| $\lambda x. x$                   | $\lambda 0$                  | `(Lam (Var 0))`                  |
| $\lambda x. \lambda y. x$        | $\lambda (\lambda 1)$        | `(Lam (Lam (Var 1)))`            |
| $\lambda x.x \equiv \lambda y.y$ | $\lambda 0 \equiv \lambda 0$ | `(Lam (Var 0)) == (Lam (Var 0))` |
