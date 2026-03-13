Las mónadas deben cumplir tres leyes: identidad izquierda, identidad derecha y asociatividad. Estas no son sugerencias.

| Ley                 | Haskell                                     | Matemáticas                                               |
| ------------------- | ------------------------------------------- | --------------------------------------------------------- |
| Identidad izquierda | `return a >>= f = f a`                      | $\eta (a) \star f = f(a)$                                 |
| Identidad derecha   | `m >>= return = m`                          | $m \star \eta = m$                                        |
| Asociatividad       | `(m >>= f) >>= g = m >>= (\x -> f x >>= g)` | $(m \star f) \star g = m \star (\lambda x. f(x) \star g)$ |

Una mónada es las tres leyes anteriores y nada más. Y... si eso es todo lo que es una mónada, entonces seguramente muchas cosas forman una mónada, ¿no? Sí, efectivamente, en particular, el tipo lista forma una mónada:
```Haskell
instance Monada [] where
	return x = [x]
	xs >>= f = concat (map f xs)
```

> Notemos que lo único que el compilador hace es verificar que las operaciones tengan los tipos correctos, no estamos demostrando que las leyes se cumplan, así que tal vez este ejemplo no sea correcto (que sí lo es). El punto aquí es que en Haskell (y en la mayoría de los demás lenguajes) puedes afirmar que tu mónada sigue las leyes pero no puedes demostrarlo (cosa que en LEAN y Rocq sí), el compilador de Haskell se limita a creer en tu palabra.

---

Veamos una forma distinta de enunciar estas leyes.

Consideremos el operador (`>=>`) definido por
```Haskell
(>=>) :: Monada m => (a -> m b) -> (b -> m c) -> (a -> m c)
(>=>) f g x = f x >>= g
```

Este operador es llamado composición de Kleisli y como podemos observar, es muy similar la composición de funciones. Con él, podemos escribir las mismas leyes de una forma más clara.

| Ley                 | Haskell                             | Matemáticas                                 |
| ------------------- | ----------------------------------- | ------------------------------------------- |
| Identidad izquierda | `return >=> f = f`                  | $\eta \circ f = f$                          |
| Identidad derecha   | `f >=> return = f`                  | $f \circ \eta = f$                          |
| Asociatividad       | `(f >=> g) >=> h = f >=> (g >=> h)` | $(f \circ g) \circ h = f \circ (g \circ h)$ |

Recordemos que cualquier conjunto de valores con una operación binaria asociativa y un
elemento de identidad se denomina monoide, así, en términos de `(>=>)`, las tres leyes nos dicen que `(>=>)` es asociativo con `return` como la identidad.