Un ejemplo de cómo funcionan la abstracción y la aplicación juntas es el siguiente.
$$(\lambda x.2 ∗ x + 1)3 = 2 ∗ 3 + 1$$
Todo lo anterior denota a la función $x \mapsto 2 ∗ x + 1$ aplicando $3$ como argumento y dando $7$ como resultado. En general, tenemos:

$$(\lambda x. M) N = M [x := N]$$

Donde $[x := N]$ denota la sustitución de $x$ (esto es, la variable ligada) **por** $N$; en este caso, de todas las $x$ que puede haber en $M$. Lo anterior es llamado *reducción* y es el único axioma esencial del cálculo $\lambda$, del que resulta toda su compleja teoría.

Es importante notar que, si una misma variable aparece tanto libre como ligada en una misma expresión, entonces, la sustitución $[x := N]$ solo se realiza cuando $x$ aparece libre. Ejemplo:

$$yx(\lambda x.x)[x := N ] \equiv yN (\lambda x.x).$$

Por lo tanto, de ahora en adelante vamos a asumir que, las variables ligadas que aparezcan en una determinada expresión son diferentes de las libres (convención de Barendregt). Esto se puede lograr al renombrar las variables ligadas, por ejemplo, $\lambda x.x$ se puede escribir como $\lambda z.z$, ya que representan el mismo algoritmo, $(\lambda x.x)a = a = (\lambda z.z)a$. Así, vamos a identificar como equivalentes a todas las expresiones que solo difieran en el nombre de sus variables ligadas.

---
### Referencias
- [Introduction to Lambda Calculus | Free and bound variables](https://www.cse.chalmers.se/research/group/logic/TypesSS05/Extra/geuvers.pdf)
- [Write You a Haskell | Lambda Calculus](https://smunix.github.io/dev.stephendiehl.com/fun/WYAH.pdf)
