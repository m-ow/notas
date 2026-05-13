Requisitos Previos (Haskell y Cabal)

`curl --proto '=https' --tlsv1.2 -sSf https://get-ghcup.haskell.org | sh`

Z3

Debian/Ubuntu

`sudo apt-get install z3`

Arch 

`sudo pacman -S z3`

Fedora

`sudo dnf install z3`


El proyecto
```
mkdir practicas-lh
cd practicas-lh
cabal init
```

Una vez que ejecuten el comando, la terminal les irá haciendo preguntas. Lo más importante es elegir Ejecutable (Executable) para que puedan correr su archivo `Main.hs` fácilmente.

Agregar la Dependencia en Cabal

Abran el archivo con extensión .cabal que se generó en su carpeta y ubiquen la sección build-depends. Agreguen el paquete liquidhaskell:

```
executable practicas-lh
    main-is:          Main.hs

    build-depends:    base >= 4.14 && < 5,
                      liquidhaskell
    default-language: Haskell2010
````

Abran su archivo fuente principal (por ejemplo, Main.hs). Para indicarle al compilador que utilice el motor de verificación en ese archivo específico, deben incluir lo siguiente en la primera línea de código:

`{-# OPTIONS_GHC -fplugin=LiquidHaskell #-}`



