Consideremos el siguiente problema: Escribir la función identidad en el cálculo lambda con tipos simples ($\lambda \to$).

Sea $\alpha$ un tipo arbitrario, tenemos: $$f \equiv \lambda x : \alpha . x$$Sin embargo, notemos que el siguiente término no es legal, es decir, **no es tipable**:
$$f \, 3 \equiv (\lambda x : \alpha . x) \, 3$$
¿Por qué? Porque $3 : \text{Nat}$ y $\alpha \not\equiv \text{Nat}$. Si queremos lograr lo anterior, necesitamos una función específica para los naturales:
$$(\lambda x : \text{Nat} . x) \, 3 \to_{\beta} 3$$
¿Y para `Bool`? Bueno...
$$(\lambda x : \text{Bool} . x) \, \text{False} \to_{\beta} \text{False}$$
Se requiere definir una función para cada tipo. En conclusión: **no existe una función identidad universal**, al menos no en $\lambda \to$.
### La solución: El Sistema F
¿Cómo se resuelve este problema en el Sistema F? El truco es agregar otra abstracción al frente:
$$\lambda \alpha : * . \lambda x : \alpha . x$$
Donde el símbolo $*$ denota al *tipo de todos los tipos* (kind).
Ahora, no olvidemos que todo término tiene un tipo y sin duda, el término anterior también lo tiene:
$$(\lambda \alpha : * . \lambda x : \alpha . x) : \forall \alpha : * . \alpha \to \alpha$$
Lo anterior lo podemos representar en Haskell como:
```Haskell
identidad :: a -> a
identidad = \x -> x 
```
La razón por la que tenemos un programa mucho más limpio y fácil de leer es porque en Haskell, tras bambalinas, se ejecuta una serie de algoritmos para reconstruir los tipos que no estamos anotando. Sin embargo, podemos ser más explícitos en el tipo de la función:
```Haskell
identidad :: forall a. a -> a
identidad = \x -> x
```
Lo cual se acerca mucho más a la forma en que leemos el símbolo $\forall$.
¿Y qué hay de "$*$"? Bueno, eso lo podemos explorar en GHCi:
```Haskell
ghci> :t 'a'  
'a' :: Char  
   
ghci> :t True  
True :: Bool  
   
ghci> :t "HOLA"  
"HOLA" :: String
```
Si preguntamos por el _kind_ (la categoría de los tipos):
```Haskell
ghci> :k Bool 
Bool :: *

ghci> :k Char
Char :: * 

ghci> :k String 
String :: *
```
> Observación sobre las [notas de clase](https://sites.google.com/ciencias.unam.mx/lenguajes2/material) 
> En las notas teóricas del curso sobre el Sistema F, tenemos $(\Lambda \alpha . \lambda x : \alpha . x) : \forall \alpha . \alpha \to \alpha$ en lugar de usar $\lambda \alpha : *$. Esto es porque la profesora está evitando hablar de *kinds* ($*$) a propósito para simplificar la notación.

De esta manera, hemos conseguido nuestra función identidad **polimórfica**, en otras palabras, un programa general que funciona para todos los tipos.
Pero ¿qué tal si yo quiero que sea monomórfica? Es decir, un programa especializado que solamente funcione para un tipo. Sencillo, aplicamos el tipo:
$$(\lambda \alpha : * . \lambda x : \alpha . x) \, \text{Nat} \to_{\beta} \lambda x : \text{Nat} . x$$
$$(\lambda \alpha : * . \lambda x : \alpha . x) \, \text{Bool} \to_{\beta} \lambda x : \text{Bool} . x$$
De esta manera podemos notar que, nuestra función general no es la función identidad en sí misma, sino una con el potencial de serlo; **una función identidad en potencia**.

---
A todo lo anterior se le llama **polimorfismo paramétrico**, donde "paramétrico" es una forma elegante de decir que todas nuestras funciones se van a comportar *exactamente de la misma manera* para todos los tipos.

Pero ¿qué tal si yo quiero un comportamiento *diferente* para cada tipo de entrada? Por ejemplo, consideremos una función con la siguiente firma (donde `a` puede denotar cualquier tipo):
```Haskell
f :: a -> a -> a
f a1 a2 = case (typeOf a1) of
            Int  -> a1 + a2
            Bool -> a1 && a2
            _    -> a1
```
**La cruda verdad es que en Haskell no hay manera de hacer algo como lo anterior.** No es posible preguntar qué tipo tiene un valor y decidir qué hacer basado en la respuesta en tiempo de ejecución. Una de las razones es porque Haskell elimina los tipos (_type erasure_) cuando el compilador los verifica exitosamente, por lo que durante la ejecución del programa ya no existe esa información para tomar decisiones.

¿Pero entonces por qué podemos hacer esto?
```Haskell
ghci> (-7) + 5.12 -- Un entero negativo más un número racional
-1.88
```
En matemáticas tenemos la total libertad de sumar $\frac{1}{7} + \pi$ (un racional más un irracional), pero en nuestras computadoras estas cosas se representan de maneras muy distintas por motivos de eficiencia o precisión. No es lo mismo sumar en binario puro que sumar números de punto flotante. ¿Cómo es posible que un mismo operador `(+)` haga cosas tan diferentes en la memoria de la computadora?

Por un lado, el polimorfismo paramétrico nos permite crear funciones que implementan _el mismo_ algoritmo sin importar el tipo. Por otro lado, en el **polimorfismo ad-hoc**, el comportamiento cambia en función del tipo específico. Haskell aborda el polimorfismo ad-hoc mediante un sistema llamado **[[Type Classes]]**.