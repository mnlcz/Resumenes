# Indice
- [Indice](#indice)
- [Stack](#stack)
  - [Código inicial](#código-inicial)
  - [Comentarios](#comentarios)
- [GHCI](#ghci)
- [Import](#import)
- [Tipos de datos](#tipos-de-datos)
  - [Declaración](#declaración)
- [Funciones](#funciones)
  - [Funciones con múltiples parámetros](#funciones-con-múltiples-parámetros)
- [Clases](#clases)
- [Clases de tipos](#clases-de-tipos)
- [Parámetros de diferente tipo](#parámetros-de-diferente-tipo)
- [Funciones ultra genéricas](#funciones-ultra-genéricas)
- [Composición de funciones](#composición-de-funciones)
- [Funciones matemáticas](#funciones-matemáticas)
  - [Sumando](#sumando)
  - [Resta, multiplicación y división](#resta-multiplicación-y-división)
  - [Número negativo](#número-negativo)
- [Operadores de prefijo e infijo](#operadores-de-prefijo-e-infijo)
- [Operadores logicos](#operadores-logicos)
- [Listas](#listas)
  - [Algunas operaciones](#algunas-operaciones)
- [Pattern Matching](#pattern-matching)
- [Tuplas](#tuplas)
  - [Funciones __fst__ y __snd__](#funciones-fst-y-snd)
  - [Deconstruir tuplas](#deconstruir-tuplas)
# Stack
```Powershell
stack new my-project
cd my-project
stack setup
stack build
stack exec my-project-exe
```
* __stack new__ crea un nuevo directorio para el proyecto.

* __stack setup__ descarga el compilador de ser necesario.

* __stack build__ buildea el proyecto.

* __stack exec__ ejecuta el proyecto.

* __stack install < nombre >__ para instalar un ejecutable.
 
## Código inicial
 ```Haskell
main :: IO ()
main = return ()
 ```

 ## Comentarios
 ```Haskell
-- Comentario en una linea.

{-
Comentario
multilinea.
-}
 ```

 # GHCI
 ```Powershell
stack ghci
 ```  
 Se puede utilizar ___:t___ para ver como trabaja una determinada función.
 ```
:t (+)
 ```
Se puede utilizar ___:info___ para ver información sobre _typeclass_.
```
:info Num
```

# Import
Vendría a ser el __#include__ en ___C++___.
```Haskell
import Data.List
import System.IO
```

# Tipos de datos
___Haskell___ tiene inferencia de tipos, la variable "detecta" el tipo de dato que se le asigna.
* __Int__: -2^63 hasta 2^63.
* __Integer__: sin límite concreto, solo la memoria del sistema.
* __Float/Double__: decimales.
* __Bool__: True o False.
* __Char__: 'a'.
* __String__: "Hola".
* __Tuple__: listas de diferentes tipos de datos.

## Declaración 
```Haskell
siempre5 :: Int
siempre5 = 5
```

# Funciones
```Haskell
funcNombre parametro1 parametro2 = operacion (valor de retorno)
```
En ___Haskell___ se evalúa todo en forma de funciones matemáticas. ¿Cuál es el __dominio__ y cuál es la __imágen__?
```Haskell
funcionSiguiente :: Int -> Int
```
La función tiene como __dominio__ un entero y como __imágen__ otro entero. Eso se refiere a que ___recibe___ Int y ___retorna___ Int.

## Funciones con múltiples parámetros
Se escribe la cantidad de parámetros a elección, el último tipo hace referencia al valor de ___retorno___.
```Haskell
funcionMultiplesParametros :: Int -> String -> Char -> Bool
```

# Clases
* __Num__: Int y Floar, utilizado para __SUMA__, __RESTA__ y _MULTIPLICACIÓN_.

* __Eq__: Comparables, ya sea __DISTINTO (/=)__ o __IGUAL (==)__. Casi todo es comparable, menos las _funciones_.

* __Ord__: Comparables y Ordenables (menor a mayor), se utilizan los operadores __<__, __>__, __<=__, __>=__, etc. _INT_, _FLOAT_, _CHAR_, _STRING_. 

* __Show__: Convertible a __STRING__, se pueden _mostrar en consola_ mediante la función ___show___.

# Clases de tipos
Existen funciones que trabajan con una gama mas amplia de tipos de datos, por ejemplo: la función (+) sirve para __Int__, __float__, etc.
Definición del tipo:
```Haskell
(+) :: Num a => a -> a -> a
```
* La suma de 2 variables retorna un valor de __cualquier tipo__. RESTRINGIDO POR ___NUM___, es decir, cualquier tipo dentro de ___Num___.

* 'a' se refiere a una ___variable de tipo___, hace referencia al tipo en cuestion, en este caso, al haber todas 'a' se refiere a que se trabaja con un tipo de dato en común.

* '=>' se refiere a una __RESTRICCIÓN__, en este caso ___Num___

# Parámetros de diferente tipo
Para casos en los que haya mas de un tipo se utilizan múltiples __variables de tipo__, por ejemplo: 'a' o 'b'.
```Haskell
funcion :: (Ord a, Show b) => a -> a -> b -> Bool
funcion x y z = x > y || show z == "hola"
```

# Funciones ultra genéricas
Sus parámetros tienen __variables de tipo__ sin ninguna restricción. Un ejemplo es la función ___id___:
```Haskell
id :: a -> a
id x = x
```
# Composición de funciones
En ___Haskell___ es muy normal definir funciones en base al resultado que devuelven otras. Por ejemplo:
```Haskell
dobleDelSiguiente numero = doble(siguiente numero)
```
Al ser esto tan utilizado, existe una manera mas corta de escribir este comportamiento, y se vería de la siguiente manera:
```Haskell
dobleDelSiguiente = doble.siguiente
```

# Funciones matemáticas

## Sumando
```Haskell
sumandoNumeros = sum [1..1000]      -- Sumando desde 1 a 1000.
suma = 5 + 4
```

## Resta, multiplicación y división
```Haskell
resta = 5 - 4
multiplicacion = 5 * 4
division = 5 / 4
```

## Número negativo
```Haskell
numeroNegativo = 5 + (-4)       -- Tiene que ir entre paréntesis.
```

# Operadores de prefijo e infijo
```Haskell
modulo = mod 5 4        -- Prefijo.
modulo2 = 5 `mod` 4     -- Infijo.
```

# Operadores logicos
```Haskell
verdaderoYfalso = True && False
verdaderoOfalso = True || False
noVerdadero = not (True)
```

# Listas
Una _lista_ está compuesta por una __cabeza__ y una __cola__.
- __cabeza__: primer elemento.
- __cola__: lista compuesta por los elementos restantes.
Aclaración: ___x___ hace referencia a la __cabeza__ y ___xs___ a la __cola__.
## Algunas operaciones
```Haskell
numerosPrimos = [3, 5, 7, 11]
concatenarLista = numerosPrimos ++ [13, 17, 19, 23, 29]
otraFormaLista = 1 : 2 : 3 : 4 : 5 : []
multiLista = [[3, 5, 7], [11, 13, 17]]
concatenarLista2 = 2 : concatenarLista
tamanioLista = length numerosPrimos
reversa = reverse numerosPrimos
estaVacia = null numerosPrimos
segundoValor = numerosPrimos !! 1
primero = head numerosPrimos
ultimo = last = numerosPrimos
todosMenosPrimero = tail numerosPrimos
todosMenosUltimo = init numerosPrimos
primeros3 = take 3 numerosPrimos
eliminar3 = drop 3 numerosPrimos
estaEnLaLista = 7 `elem` numerosPrimos
maximo = maximum numerosPrimos
minimo = minimum numerosPrimos
agregarEntre = intercalate " " ["hola", "que", "tal?"]
producto = procuct numerosPrimos
muchos = take 10 (repeat 2)
muchos2 = replicate 10 3
ciclo = take 10 (cycle [1, 2, 3, 4, 5])
-- ..............................................................
generarListaDesde = [0..10]
generarListaPar = [2, 4..20]
listaCaracteres = ['A', 'C'..'Z']
-- ..............................................................
listaPorDos = [x * 2 | x <- [1..10]]        -- Multiplica x por 2 y crea una nueva lista.
divisiblePor9y13 = [x | x <- [1..500], x `mod` 13 == 0, x `mod` 9 == 0]
listaOrdenada = sort [5, 8, 6, 9, 2, 1]
sumarElementosListas = zipWith (+) [1, 2, 3, 4, 5] [6, 7, 8, 9, 10]     -- resultado = [7, 9, 11, 13, 15]
filtrarLista = filter (> 5) [2, 4, 6, 8, 10]
paresHasta20 = takeWhile (<= 20) [2, 4..]
mapTest = map times4 [1, 2, 3, 4, 5]        -- map aplica una funcion a toda una lista (hacer de cuenta que times4 fue declarada)

-- ..............................................................

-- Explicacion:
-- [operacion | variable <- [rango]]

pow3List = [3^n | n <- [1..10]]     -- Toma los valores de 1 al 10, los guarda en n y realiza la operacion.
tablaMultiplicar = [[x * y | y <- [1..10]] | x <- [1..10]]
paisesAsia paises = [nombrePais pais | pais <- paises, estaEnAsia pais]
```

# Pattern Matching
```Haskell
esCero 0 = True
esCero _ = False        -- Variable anonima
```

# Tuplas
```Haskell
-- Cantidad fija de elementos.
(1, 3)
(1, "test", 45)
```
El tipo de una __tupla__ depende de sus elementos. Por ejemplo:
```Haskell
(2, 4)                  -- (Num a, Num b) => (a, b)
("Hector", 24, 75)      -- Num a, Num b => (String, a , b)
```
## Funciones __fst__ y __snd__
Devuelven el _primer_ y _segundo_ elemento respectivamente. ___SOLO___ para __tuplas__ de 2 valores. Ejemplo:
```Haskell
sumaDeParOrdenado parOrdenado = fst parOrdenado + snd parOrdenado
```
## Deconstruir tuplas
```Haskell
sumaDeParOrdenado (x,y) = x + y
```