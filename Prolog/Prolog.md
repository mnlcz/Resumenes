# Indice
- [Indice](#indice)
- [_SWI-Prolog_](#swi-prolog)
- [Comentarios](#comentarios)
- [Algunos items y tips sobre el lenguaje](#algunos-items-y-tips-sobre-el-lenguaje)
- [Operadores y otras utilidades](#operadores-y-otras-utilidades)
- [Predicados](#predicados)
  - [Predicados inversibles](#predicados-inversibles)
- [Reglas](#reglas)
  - [Reglas con multiples características](#reglas-con-multiples-características)
- [Variables](#variables)
  - [Variable anónima](#variable-anónima)
- [Variable libre](#variable-libre)
- [_forall_](#forall)
- [_is_](#is)
- [Redefinición de predicados](#redefinición-de-predicados)
- [Tuplas](#tuplas)
- [Functores](#functores)
  - [Functores en funciones auxiliares](#functores-en-funciones-auxiliares)
  - [Pattern matching con functores](#pattern-matching-con-functores)
  - [Polimorfismo con functores](#polimorfismo-con-functores)
- [_findall_](#findall)

# _SWI-Prolog_
Se puede ejecutar mediante el ejecutable o dentro de la consola con los comandos _swipl_:
```powershell
swipl --help
swipl --version
```
Lo mismo sucede con el intérprete, se puede usar el ejecutable o la terminal, con el comando:
```powershell
swipl
```
# Comentarios
Se utiliza ___%___:
```prolog
% Comentario
```
Para multilinea:
```prolog
/* Comentario
multi
linea */
```
# Algunos items y tips sobre el lenguaje
- En Prolog, todas las cosas que preguntamos están implícitamente cuantificadas mediante un operador de existencia. Eso significa que da lo mismo si hay una sola o múltiples formas de probar algo: a Prolog sólo le importa que exista al menos una prueba para responder _yes_.

- Una __consulta__ se puede hacer utilizando una variable en lugar de un __átomo__ en un __predicado__. La variable tomará el valor del átomo faltante:
```prolog
grupo(argentina, b).
grupo(argentina, X).    % X = b
```
- También se puede usar a la variable anónima dentro de una consulta:
```prolog
abuelo(carlos, juan).
abuelo(carlos, adrian).

abuelo(_, Nieto).       % Nieto toma el valor de juan y adrian.
```
- En _Prolog_ siempre se ingresan __consultas__, no expresiones.
# Operadores y otras utilidades
```prolog
x < y             % Menor que
x > y             % Mayor que
x >= y            % Mayor o igual
x <= y            % Menor o igual
x = y             % Cito: succeeds if X and Y unify (match) in the Prolog sense
x \= y            % Cito: succeeds if X and Y do not unify; i.e. if not (X = Y)
x =:= y           % Igual valor
x =\= y           % No son iguales
x == y            % Idéntico (mismo nombre de variable)
x \== y           % No son idénticos

x + y             % Suma
x - y             % Resta
x * y             % Multiplicación
x / y             % División
x ** y            % Potenciación
x // y            % División de enteros
x mod y           % Módulo

nl                % New line
not(_)            % Negacion, recibe por parámetro una consulta
forall(_, _)      % Ver apartado forall
is(_, _)          % Ver apartado is
max(_, _)         % Obtiene el mayor
between(_, _, _)  % Si el parámetro 3 se encuentra entre el primero y el segundo 
round(_)          % Redondea al entero más cercano    
```

# Predicados
Son expresiones que definen cierta caracteristica:
```prolog
loves(romeo, juliet).
```
- Los ___atomos___ (romeo y julieta en este caso) van en __minúscula__.
- Al final de la expresión va un punto.
## Predicados inversibles
```prolog
hermanoVersion2(Uno, Otro) :-
   padre(Uno, Padre),  % acá padre sirve de generador de Uno
   padre(Otro, Padre), % acá padre sirve de generador de Otro
   Uno \= Otro.        % y asi logramos que Uno y Otro lleguen instanciados
```
Para predicados __no inversibles__, la ultima linea iría como primera:
```prolog
hermanoVersion2(Uno, Otro) :-
   Uno \= Otro,
   padre(Uno, Padre),
   padre(Otro, Padre). 
```
# Reglas
Las reglas se utilizan cuando se quiere definir un _fact_ a partir de otros. Para definir una __regla__ se utiliza ___:-___, que seria el equivalente al ___if___ de otros lenguajes. Por ejemplo:
```prolog
happy(albert).
runs(albert) :-
    happy(albert).
```

## Reglas con multiples características
En caso de que se quiera agregar mas de un predicado para definir una regla, se utilizan las comas (___,___), que vendrían a ser el equivalente al ___AND___ de otros lenguajes. Ejemplo (usando variables):
```prolog
personajePopular(Personaje) :-
   personajeDeNovela(Personaje),
   personajeDePelicula(Personaje).
```
# Variables
- Su primera letra siempre va en mayúscula.
- Se escriben __DESPUÉS__ de un _hecho_ o _fact_. 
Ejemplo:
```prolog
personajeSurrealista(dali).
personajeSurrealista(Personaje) :-
  apareceEnPinturaSurrealista(Personaje).
```
## Variable anónima
Sirve para chequear la existencia de un predicado sin importar el valor del mismo.
```prolog
hombre(carlos).
hombre(fabian).
hombre(manuel).

hombre(_).      % Devuelve true
```
Esto es útil también para cuando se quiere utilizar un predicado pero ignorando el valor de uno de sus átomos. Por ejemplo (ejercicio 2.4 en _Mumuki_):
```prolog
empleado(ventas, maria, gerente).
empleado(ventas, juan, cadete).
empleado(ventas, roque, cadete).
empleado(compras, nora, gerente).
empleado(compras, pedro, cadete).
empleado(administracion, felipe, gerente).
empleado(administracion, ana, cadete).
empleado(administracion, hugo, cadete).

trabajaEn(Departamento, Persona) :-
  empleado(Departamento, Persona, _).
```

# Variable libre
Variable que se utiliza __sin ser instanciada previamente__. Extracto del ejercicio 3.4 de _Mumuki_:
```prolog
destinosProximos(Destino1, Destino2) :-
  quedaEn(Destino1, Zona),
  quedaEn(Destino2, Zona).
```

# _forall_
Es un predicado que recibe 2 parámetros, los cuales son consultas y pueden ser predicados.
- __"Cuando a todos los que les pasa lo primero, les pasa lo segundo."__
- Emplea variables libres.

Ejemplo 1:
```prolog
 esTierno(Persona):- forall(leGusta(Persona, Alimento), dulce(Alimento)).
```
En otras palabras, _"Una persona es tierna ... si todas las cosas que le gustan ... son dulces"_.
<br>
<br>
Ejemplo 2 (_Mumuki_): <br>
Escribí un predicado destinoAccesible/1 que nos diga si llegan fácil todas las personas que quieran ir a éste. Recordá que existe el predicado llegaFacil/2 que relaciona una persona y un destino.
```prolog
destinoAccesible(Destino) :-
  forall(quiereIr(Persona, Destino), llegaFacil(Persona, Destino)).
```
También se puede usar _forall_ para verificar existencia. Por ejemplo:
```prolog
% Asumir que mujer() ya existe.
reunion(mabel, viernes).
reunion(ana, viernes).
reunion(adrian, viernes).
reunion(pablo, viernes).

forall(reunion(_, viernes), mujer(_)).    % yes
```

# _is_
Recibe 2 parámetros, un _valor_ y una _expresión_, luego evalúa si al realizar la expresión se obtiene el valor:
```prolog
is(4, 2+2).     % yes
cuadrado(Numero, Cuadrado) :-
  is(Cuadrado, Numero**2).
```
También se puede usar _is_ para darle un valor a una variable mediante una operación (puede ser una función) y evaluarla:
```prolog
Numero is 4 + 4.

area(B, H, A) :-
  A is B*H.

puntajeEquilibrioEscoba(Competidor, Puntaje) :-
  metrosEscoba(Competidor, M),
  Puntaje is round(M/3 * 1).
```

# Redefinición de predicados
Para evitar conflictos en casos en que el predicado tenga distintos comportamientos segun el parámetro, se puede realizar más de una definición del mismo, así como se hacía cuando se le asignaban valores a lo _pattern matching_.
```prolog
esAlcoholica(Bebida) :-
  esWhisky(Bebida).
esAlcoholica(Bebida) :-
  esVino(Bebida).
```

# Tuplas
Permiten tratar a un conjunto ordenado como si fueran uno solo:
- un personaje, de la que sabemos edad y sexo: _(15, sexo)_
- un helado, del que sabemos peso y temperatura: _(1, -3)_

Estos se dice que tienen ___aridad___ = 2. Un ejemplo de __aridad__ 3:
- una dirección, de la que sabemos nombre, calle y altura: _(utn, medrano, 951)_

Ejemplo:
```prolog
personaje(jonSnow, (18, hombre)).
personaje(sansa, (15, mujer)).
```
La consulta se vería así:
```prolog
? personaje(jonSnow, Personaje).
Personaje = (18, hombre).

?  personaje(jonSnow, (Edad, Sexo)).
Edad = 18,
Sexo = hombre.

?  personaje(Quien, (15, _)).
Quien = sansa.
```
# Functores
Son similares a las tuplas, la diferencia es que tienen un nombre:
```prolog
personaje(arya, stark(14, mujer)).
personaje(cersei, lannister(34, mujer)).
```
__IMPORTANTE__: no debe haber espacio entre el nombre y los paréntesis.

Los _functores_ también pueden ser _matcheados_ contra __patrones__:
```prolog
personaje(sansa, stark(15, mujer)).
personaje(tyrion, lannister(30, hombre)).

esLannister(Nombre) :-
  personaje(Nombre, lannister(_, _)).
```

__IMPORTANTE__: los nombres de los _functores_ no pueden ser variables.

## Functores en funciones auxiliares
```prolog
esAdulto(stark(Edad, _)) :-
  Edad > 15.
esAdulto(lannister(Edad, _)) :-
  Edad > 15.
  
personajeAdulto(Nombre) :-
  personaje(Nombre, Pje),
  esAdulto(Pje).
```

## Pattern matching con functores
Ejercicio 8 de functores en _Mumuki_: Agregar una cláusula a esPersonajePeligroso/1 de forma que un Guardia de la Noche sea peligroso sólo si tiene un lobo. No se debe utilizar el predicado personaje para esto.

La base de conocimientos es la siguiente:
```prolog
personaje(jon,nightwatch(23,lobo(ghost))).
personaje(sam,nightwatch(23)).
% etc
```
Resolución:
```prolog
esPersonajePeligroso(nightwatch(_, lobo(_))).
```
__IMPORTANTE: EL FUNCTOR NO SOLO SE IDENTIFICA POR SU NOMBRE, TAMBIÉN LO HACE POR SU ARIDAD.__

## Polimorfismo con functores
```prolog
esPersonajePeligroso(lannister(Oro)) :-
  Oro > 300.
esPersonajePeligroso(stark(_, _)).
esPersonajePeligroso(nightwatch(_, lobo(_))).
```
Otro ejemplo:
```prolog
cuantoSabePersonaje(lannister(_), mucho).
cuantoSabePersonaje(stark(_, _), poco).
cuantoSabePersonaje(nightwatch(_), poco).
cuantoSabePersonaje(nightwatch(_, lobo(_)), nada).
```

# _findall_
