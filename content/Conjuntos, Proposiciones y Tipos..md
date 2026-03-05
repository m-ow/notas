A lo largo de nuestros semestres en la Facultad, hemos notado la profunda conexión entre la teoría de conjuntos y la lógica proposicional.

| Conjuntos        | Proposiciones        |
| ---------------- | -------------------- |
| $x \in A$        | $P(x)$               |
| $x \in A^c$      | $\neg P(x)$          |
| $x \in A \cup B$ | $P(x) \lor Q(x)$     |
| $x \in A \cap B$ | $P(x) \land Q(x)$    |
| $A \subset Q$    | $P(x) \implies Q(x)$ |

Podemos pensar en una proposición lógica $P(x)$ como el conjunto de todos los elementos que hacen que dicha proposición sea verdadera.

De la misma manera, en programación funcional entendemos los tipos como conjuntos. Si $42 \in \mathbb{Z}$, decimos que $42 : \text{Integer}$
```Haskell
ejemplo :: Integer
ejemplo = 42
```

Si las proposiciones se comportan como conjuntos, y los tipos también, ¿las proposiciones son tipos? **La respuesta es sí**.

Las _proposiciones_ matemáticas pueden considerarse tipos. Para las proposiciones `P` y `Q`, los elementos `p : P` y `q : Q` son las _demostraciones_ (o pruebas) de que estas proposiciones son verdaderas. Una proposición falsa es simplemente un tipo vacío (sin habitantes).

A esta correspondencia se le conoce como el **Isomorfismo de Curry-Howard**:

> "Los tipos son teoremas, los programas son demostraciones".

Así, el verificador de tipos (Type Checker) de Haskell se convierte en nuestro verificador de demostraciones matemáticas. El flujo es el siguiente:
1. Traducimos la proposición al tipo correspondiente en Haskell.
2. Escribimos una función (creamos un valor) que tenga ese tipo.
3. Si el código compila, ¡nuestra demostración es válida!