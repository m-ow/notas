En Haskell es muy fácil escribir una función si sabemos de antemano sus tipos concretos.
```Haskell
ejemplo :: Bool -> Integer
ejemplo False = 0
ejemplo True = 1
```
Como solo hay dos valores habitando el tipo `Bool`, podemos asignarles fácilmente valores de tipo `Integer` para definir la función de manera exhaustiva.

Pero, ¿qué pasa si queremos generalizar y definir una función para tipos completamente arbitrarios?

```Haskell
ejemplo :: a -> b
ejemplo = error "No hay forma de implementar esto sin romper las reglas"
```
¿Por qué no podemos implementar una función total que cumpla con esa firma? La respuesta proviene de uno de los resultados teóricos más hermosos de nuestra disciplina.

La pregunta de fondo es: **Dado un tipo cualquiera, ¿bajo qué condiciones existe un valor que tenga ese tipo?** Es decir, ¿cuándo un tipo está _poblado_?. Como vimos en el ejemplo `a -> b`, la respuesta es que no siempre lo está.