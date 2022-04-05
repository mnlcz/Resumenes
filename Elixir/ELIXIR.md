# Indice
- [Indice](#indice)
- [*.ex* y *.exs*](#ex-y-exs)
  - [Interpretar](#interpretar)
  - [Compilación](#compilación)
  - [Diferencias](#diferencias)
- [Intérprete](#intérprete)
  - [Ejecución](#ejecución)
  - [Ayuda](#ayuda)
  - [Información](#información)
  - [Autocompletar](#autocompletar)
  - [Incluir funciones propias](#incluir-funciones-propias)
- [Estructura de un proyecto](#estructura-de-un-proyecto)
- [Terminología y características](#terminología-y-características)
- [Estructura básica de un programa](#estructura-básica-de-un-programa)
- [Imprimir](#imprimir)
- [Tipos de dato](#tipos-de-dato)
  - [Chequeo de tipos](#chequeo-de-tipos)
  - [_Atoms_](#atoms)
  - [_Strings_](#strings)
    - [Imprimir](#imprimir-1)
    - [Concatenar](#concatenar)
    - [Comparar](#comparar)
    - [Algunos métodos](#algunos-métodos)
      - [_split_](#split)
- [Colecciones](#colecciones)
  - [_(Linked) Lists_](#linked-lists)
    - [Operaciones](#operaciones)
    - [_head_ y _tail_](#head-y-tail)
    - [_Charlists_](#charlists)
    - [Agregar elementos al inicio](#agregar-elementos-al-inicio)
    - [Equivalente a _include?_](#equivalente-a-include)
    - ["Iterar"](#iterar)
  - [Por comprensión](#por-comprensión)
  - [_Tuples_](#tuples)
    - [Tamaño](#tamaño)
    - [Generador](#generador)
    - [Acceso](#acceso)
    - [Eliminación](#eliminación)
    - [Inserción](#inserción)
    - [Agregar/modificar elemento](#agregarmodificar-elemento)
  - [Diferencias entre _Lists_ y _Tuples_](#diferencias-entre-lists-y-tuples)
  - [Rangos](#rangos)
  - [*Keyword lists*](#keyword-lists)
  - [*Maps*](#maps)
    - [Creación](#creación)
    - [Acceso](#acceso-1)
    - [Modificación](#modificación)
  - [Diferencias entre *keyword lists* y *map*](#diferencias-entre-keyword-lists-y-map)
  - [*Nested data structures*](#nested-data-structures)
    - [Acceso](#acceso-2)
    - [Modificación](#modificación-1)
- [Operaciones básicas](#operaciones-básicas)
  - [Redondeo](#redondeo)
- [Modulos](#modulos)
  - [Atributos](#atributos)
    - [Como documentación](#como-documentación)
      - [Uso de *Markdown*](#uso-de-markdown)
    - [Como constantes](#como-constantes)
  - [Reuso](#reuso)
    - [*alias*](#alias)
    - [*require*](#require)
    - [*import*](#import)
    - [*use*](#use)
    - [Multi *alias/import/require/use*](#multi-aliasimportrequireuse)
- [Funciones](#funciones)
  - [Anónimas](#anónimas)
    - [Atajo](#atajo)
  - [Con nombre](#con-nombre)
    - [*Overloading*?](#overloading)
    - [Single line](#single-line)
    - [*Capturing*](#capturing)
      - [Atajo para crear funciones](#atajo-para-crear-funciones)
    - [Parámetros por defecto](#parámetros-por-defecto)
    - [Guardas](#guardas)
- [*Structs*](#structs)
  - [Acceso](#acceso-3)
- [Operadores](#operadores)
  - [Concatenar](#concatenar-1)
  - [Condicionales](#condicionales)
  - [Comparación](#comparación)
  - [Pipe operator](#pipe-operator)
- [_Pattern matching_](#pattern-matching)
  - [Operador =](#operador-)
    - [_Matchear_ valores específicos](#matchear-valores-específicos)
  - [En _lists_](#en-lists)
    - [_head_ - _tail_](#head---tail)
  - [Operador ^](#operador--1)
  - [Variable _](#variable-_)
- [Estructuras de control](#estructuras-de-control)
  - [_case_](#case)
    - [Con variables extra](#con-variables-extra)
  - [_cond_](#cond)
  - [_if / unless_](#if--unless)
- [Bloques _do - end_](#bloques-do---end)
- [Recursividad](#recursividad)
  - [Ejemplo implementando un equivalente a *sum*](#ejemplo-implementando-un-equivalente-a-sum)
  - [Ejemplo implementando una función que multiplique por 2 los elementos](#ejemplo-implementando-una-función-que-multiplique-por-2-los-elementos)
- [*Enumerables*](#enumerables)
  - [Capture operator](#capture-operator)
  - [*all?*](#all)
  - [*any?*](#any)
  - [*chunk_every*](#chunk_every)
  - [*chunk_by*](#chunk_by)
  - [*count*](#count)
  - [*count_until*](#count_until)
  - [*each*](#each)
  - [*empty?*](#empty)
  - [*filter*](#filter)
  - [*find*](#find)
  - [*find_index*](#find_index)
  - [*frequencies*](#frequencies)
  - [*map*](#map)
  - [*map_every*](#map_every)
  - [*max*](#max)
  - [*min*](#min)
  - [*product*](#product)
  - [*reduce*](#reduce)
  - [*shuffle*](#shuffle)
  - [*sort*](#sort)
  - [*sum*](#sum)
  - [*take*](#take)
  - [*take_every*](#take_every)
  - [*uniq*](#uniq)
  - [*uniq_by*](#uniq_by)
- [*Generators*](#generators)
- [*IO*](#io)
  - [Input y Output](#input-y-output)
  - [*File*](#file)
  - [*Path*](#path)
- [*Sigils*](#sigils)
  - [*Regex*](#regex)
  - [*Strings*](#strings-1)
  - [*Charlists*](#charlists-1)
  - [*Wordlists*](#wordlists)
  - [Dias y horarios](#dias-y-horarios)
    - [*Date*](#date)
    - [*Time*](#time)
    - [*UTC DateTime*](#utc-datetime)

# *.ex* y *.exs*

- ***.ex es compilado***.
- ***.exs es interpretado***

## Interpretar

```bash
elixir archivo.exs
```

## Compilación

```bash
elixirc archivo.ex
```

La compilación permite que los *módulos* creados puedan ser utilizados luego.

1. Al compilar el archivo **se crea un archivo *.beam***, el cual contiene el *bytecode* del módulo en cuestión.
2. **Si se abre el intérprete en el mismo directorio del archivo compilado, el módulo estará disponible para su uso.**

## Diferencias

- Los archivos interpretados son recomendables para proyectos pequeños.
- Los archivos compilados se ejecutan más rápido.
- Los archivos interpretados se utilizan para ***scripting***.
- Los archivos interpretados se utilizan para ***tests***.
- Los archivos compilados se utilizan para la **aplicación principal** que se esté haciendo.
- Los archivos interpretados requieren la sentencia `c("archivo.exs")` para pasar su funcionalidad al iex.

# Intérprete

## Ejecución

```powershell
iex.bat
```

- ___Ctrl + C_ dos veces__ para cerrarlo.

Para abrir el __intérprete__ original (fuera de la terminal):

```powershell
iex.bat --werl
```

- Mejor experiencia que usándolo desde la terminal.

## Ayuda

Usando ___h___ mas una función y su _aridad_ (ver terminología) se puede obtener información.

<img src="..\img\image-20211130193508986.png" alt="image-20211130193508986" style="zoom:80%;" />

- ___h trunc/1___ funciona ya que está incluido en ___Kernel___. Generalmente se necesita __incluir el nombre del módulo__:

```erlang
h Kernel.trunc/1
```

## Información

Para obtener información de un elemento se utiliza la función ___i/1___.

<img src="..\img\image-20211130202231759.png" alt="image-20211130202231759" style="zoom:100%;" />

## Autocompletar

Expresión +`TAB` brinda muestra todas las opciones.

![image-20211206172723224](..\img\image-20211206172723224.png)

## Incluir funciones propias

Incluye el módulo en iex y ejecuta las sentencias fuera del mismo.

```elixir
c("programa.exs")
```

# Estructura de un proyecto

Generalmente el proyecto se divide en **3 carpetas**:

- ***_build***: contiene los elementos de compilación.
- ***lib***: contiene los ***.ex***.
- ***test***: contiene los programas de prueba (***los .exs***).

# Terminología y características

- ___arity___: cantidad de parámetros de una _función_.  Ejemplo: `trunc/1` refiere a la función _trunc_ que tiene _aridad_ 1.
- __Las estructuras de datos en _Elixir_ son _inmutables___.

# Estructura básica de un programa

```elixir
defmodule Solution do
  def print_hello(n) do
    1..n |> Enum.each(fn _ -> IO.puts "Hello" end)
  end
end

num = IO.gets("") |> String.trim |> String.to_integer
Solution.print_hello(num)
```

# Imprimir

```elixir
IO.puts "hola"				# Agrega newline
IO.write "hola"				# Sin newline
IO.inspect "hola"			# Equivalente al p en Ruby
```

# Tipos de dato

```elixir
1          		# integer
0x1F       		# integer
1.0        		# float
true       		# boolean
:atom      		# atom / symbol
"elixir"   		# string
[1, 2, 3]  		# list
{1, 2, 3}  		# tuple
```

## Chequeo de tipos

_Elixir_ tiene varias funciones para chequear el tipo de dato:

```elixir
is_boolean(true)		# true
is_boolean(1)			# false
is_integer(1)			# true
```

## _Atoms_

Equivalente a los _Symbols_ en _Ruby_.

- Son iguales si sus nombres coinciden: `:apple == :apple`.
- Los _booleanos_ y _nil_ son también _Atoms_.
- Suelen usarse para chequear el estado de una operación. Con _atoms_ como ___:ok___ y ___:error___.

## _Strings_

- Van entre " ".
- Al igual que _Ruby_ soportan _string interpolation_:

```elixir
nombre = "Manuel"
saludo = "hola #{nombre}"		# "hola Manuel"
```

- Pueden tener _line breaks_ usando el operador de escape _\n_.

### Imprimir

```elixir
IO.puts("hello world")
# => hola manuel
# => :ok
```

- ___IO.puts/1___ luego de imprimir la _string_ __devuelve el _atom :ok___.

### Concatenar

```elixir
"foo" <> "bar"					# => "foobar"
```

### Comparar

Se utiliza el operador ___===___, el cual chequea __el valor y el tipo__.

```elixir
"Hola" === "hola"				# => false
```

### Algunos métodos

```elixir
String.length("hello")									# => 5
String.upcase("hello")									# => "HELLO"
String.capitalize("hello")								# => "Hello"
String.contains?("hola que tal", "hola")				# => true
String.first("hola")									# => "h"
String.at("hola", 1)									# => "o"
String.slice("hola", 0, 2)								# => "ho"
String.reverse("hola")									# => "aloh"

age = IO.gets("edad: ") |> String.trim |> String.to_integer		# este ultimo recibe un Int de input
# .trim => chomp en Ruby
```

#### _split_

```elixir
"Elixir rocks" |> String.split()
```

# Colecciones

## _(Linked) Lists_

- Los valores pueden ser de cualquier tipo.

### Operaciones

- __IMPORTANTE__: las operaciones __no modifican la lista original__, sino que devuelven __una nueva__.

Para __concatenar__ se usa el operador ___++___.

```elixir
[1, 2, 3] ++ [4, 5, 6]		# => [1, 2, 3, 4, 5, 6]
```

Para __restar__ se usa el operador ___--___.

```elixir
[1, true, 2, false, 3, true] -- [true, false]
# => [1, 2, 3, true]
```

### _head_ y _tail_

Refieren al primer y al resto de elementos respectivamente. Se obtienen con las funciones ___hd/1___ y ___tl/1___.

```elixir
list = [1, 2, 3]
hd(list)				# 1
tl(list)				# [2, 3]
```

- Al recibir una __lista vacía__ ambas funciones fallan.

### _Charlists_

Es común crear una lista y obtener __un valor entre comillas simples__. Esto sucede ya que _Elixir_ interpreta los valores como caracteres imprimibles, en otras palabras, como una ___cadena de caracteres___.

```elixir
[11, 12, 13]
# => '\v\f\r'
```

__IMPORTANTE__:

```elixir
'hello' == "hello"		# false
```

### Agregar elementos al inicio

```elixir
[1, 2, 3]				# => [1, 2, 3]
[0 | list]				# => [0, 1, 2, 3]
```

### Equivalente a _include?_

```elixir
list1 = [1, 2, 3]
2 in list1				# => true
```

### "Iterar"

```elixir
words = ["hola", "que", "tal"]
Enum.each words, fn w ->
	IO.puts w
end
```

## Por comprensión

```elixir
double_list = for n <- [1,2,3], do: n * 2
even_list = for n <- [1,2,3,4], rem(n, 2) == 0, do: n
```

## _Tuples_

- Son el equivalente a las _Arrays_ en otros lenguajes. 
- Aceptan cualquier tipo de dato.
- **No son *enumerables***.

```elixir
{:ok, "hello"}
```

### Tamaño

```elixir
tuple_size {:ok, "hello"}								# 2
```

### Generador

```elixir
muchos_ceros = Tuple.duplicate(0, 10)					# => {0, 0, 0, 0, 0, 0, 0, 0, 0, 0}
```

### Acceso

```elixir
elem(tuple, 1)											# "hello"
```

### Eliminación

```elixir
otra_tupla = Tuple.delete_at(tuple, 0)					# Borra el primer elemento
```

### Inserción

```elixir
otra_tupla = Tuple.insert_at(tuple, 0, 1999)			# Inserta 1999 en la posición 1
```

### Agregar/modificar elemento

```elixir
# put_elem(TUPLA, INDEX, VALOR)
put_elem(tuple, 1, "world")
```

- __IMPORTANTE__: devuelve __una nueva tupla__, no modifica la original.

## Diferencias entre _Lists_ y _Tuples_

- ___Lists___: al ser una _linked list_ cada elemento tiene la dirección del próximo. Lo que hace que el acceso no se puede hacer mediante __índices__.
- ___Tuples___: son guardadas de manera __contigua__ en memoria. Puede accederse a cada elemento de manera muy rápida. Sin embargo __agregar o modificar elementos es más costoso que en una _List___.

## Rangos

```elixir
one_to_10 = 1..10
```

## *Keyword lists*

Son *lists* de *2 items tuples*. Representan una estructura de datos ***key - value***.

- ***keyword list***: una lista donde el primer elemento de la tupla es un *atom*.

```elixir
list = [a: 1, b: 2] 				# => [a: 1, b: 2]
```

**Características fundamentales**:

- Las ***keys*** deben ser ***atoms***.
- Las ***keys*** están ordenadas.
- Las ***keys*** no necesariamente deben ser únicas.

## *Maps*

La estructura de datos fundamental para guardar ***key - value***.

### Creación

```elixir
map = %{:a => 1, 2 => :b}
```

### Acceso

```elixir
map[:a]					# => 1
map[2]					# => :b
map[:c]					# => nil

# Cuando las keys son atoms
map = %{:a => 1, 2 => :b}
map.a					# => 1
```

### Modificación

```elixir
map = %{:a => 1, 2 => :b}				# => %{2 => :b, :a => 1}
%{map | 2 => "two"}						# => %{2 => "two", :a => 1}
```

## Diferencias entre *keyword lists* y *map*

- ***Map*** permite cualquier tipo de dato como ***key***.
- ***Map*** no está ordenado.
- ***Map*** son mucho mas eficaces en *pattern matching*.

## *Nested data structures*

```elixir
users = [
  john: %{name: "John", age: 27, languages: ["Erlang", "Ruby", "Elixir"]},
  mary: %{name: "Mary", age: 29, languages: ["Elixir", "F#", "Clojure"]}
]
```

### Acceso

```elixir
users[:john].age						# => 27	
```

### Modificación

```elixir
users = put_in users[:john].age, 31		# john: %{age: 31 .......
users = update_in users[:mary].languages, fn languages -> List.delete(languages, "Clojure") end
```

- ***update_in*** permite pasar una función.

# Operaciones básicas

```elixir
1 + 2
5 * 5
10 / 2			# siempre devuelve float
div(10, 2)		# devuelve integer
rem(10, 3)		# % = 1
1.0e-10			# notacion cientifica
```

## Redondeo

```elixir
round(3.58)		# 4
trunc(3.58)		# 3 => devuelve la parte entera
```

# Modulos

Se utilizan para agrupar funciones, se crean con ***defmodule***. Serían el equivalente al ***namespace*** en *C++*.

```elixir
defmodule Math do
	def sum(a, b) do
		a + b
	end
end
```

## Atributos

Son tratados como **constantes**.

```elixir
defmodule Example do
  @greeting "Hello"

  def greeting(name) do
    ~s(#{@greeting} #{name}.)
  end
end
```

### Como documentación

```elixir
defmodule MyServer do
  @moduledoc "My server code."
end
```

- `@moduledoc` - *provides documentation for the current module.*
- `@doc` - *provides documentation for the function or macro that follows the attribute.*
- `@spec` - *provides a typespec for the function that follows the attribute.*
- `@behaviour` - *(notice the British spelling) used for specifying an OTP or user-defined behaviour.*

#### Uso de *Markdown*

```elixir
defmodule Math do
  @moduledoc """
  Provides math-related functions.

  ## Examples

      iex> Math.sum(1, 2)
      3

  """

  @doc """
  Calculates the sum of two numbers.
  """
  def sum(a, b), do: a + b
end
```

### Como constantes

```elixir
defmodule MyServer do
  @initial_state %{host: "127.0.0.1", port: 3456}
  IO.inspect @initial_state
end
```

## Reuso

### *alias*

Permite darle un nombre a un determinado *módulo*.

```elixir
defmodule Stats do
  alias Math.List, as: List
  # In the remaining module definition List expands to Math.List.
end
```

Se puede limitar su *scope* al de una función.

```elixir
defmodule Math do
  def plus(a, b) do
    alias Math.List
    # ...
  end

  def minus(a, b) do
    # ...
  end
end
```

### *require*

Permite utilizar **macros** definidas en otro módulo.

![image-20211204231142521](..\img\image-20211204231142521.png)

### *import*

Sería el equivalente al `include` en otros lenguajes. Permite importar tanto **macros** como **funciones públicas** y, a diferencia de `require`, a la hora de utilizar dichas macros no es necesario utilizar el nombre completo.

```elixir
import List, only: [duplicate: 2]			# => List
duplicate(:ok, 3)							# => [:ok, :ok, :ok]
```

- `only:`: se utiliza para evitar importar todas las funciones. También está `except:`.
  - La sintaxis sería `only: [funcion: aridad]`.

También se puede limitar el *scope* a funciones:

```elixir
defmodule Math do
  def some_function do
    import List, only: [duplicate: 2]
    duplicate(:ok, 10)
  end
end
```

### *use*

Permite que otro módulo **"inyecte" código** en el módulo actual.

```elixir
defmodule AssertionTest do
  use ExUnit.Case, async: true

  test "always pass" do
    assert true
  end
end
```

### Multi *alias/import/require/use*

Se pueden importar, dar alias o requerir varios módulos a la vez.

```elixir
alias MyApp.{Foo, Bar, Baz}
```

# Funciones

## Anónimas

```elixir
variable = fn parametros -> cuerpo end
```

Ejemplo:

```elixir
add = fn a, b -> a + b end
```

- Se encierran entre ___fn___ y ___end___.

Para llamarla:

```elixir
add.(1, 2)			# 3
```

- __IMPORTANTE__: el punto luego del nombre.

```elixir
is_function(add)		# true
is_function(add, 2)		# (nombre, aridad) => true
```

### Atajo

```elixir
sum = &(&1 + &2)
sum.(2, 3)				# => 5
```

## Con nombre

```elixir
defmodule Math do
  def sum(a, b) do
    do_sum(a, b)
  end

  defp do_sum(a, b) do
    a + b
  end
end
```

- ***def*** para funciones comunes.
- ***defp*** para funciones **privadas** (no disponibles fuera del módulo).

### *Overloading*?

```elixir
defmodule Math do
  def zero?(0) do
    true
  end

  def zero?(x) when is_integer(x) do
    false
  end
end

IO.puts Math.zero?(0)         		# => true
IO.puts Math.zero?(1)         		# => false
IO.puts Math.zero?([1, 2, 3]) 		# => ** (FunctionClauseError)
IO.puts Math.zero?(0.0)       		# => ** (FunctionClauseError)
```

### Single line

```elixir
defmodule Math do
  def zero?(0), do: true
  def zero?(x) when is_integer(x), do: false
end
```

### *Capturing*

```elixir
Math.zero?(0)					# => true
fun = &Math.zero?/1				# => &Math.zero?/1
is_function(fun)				# => true
fun.(0)							# => true

# Capturar operadores
add = &+/2						# => &:erlang.+/2
add.(1, 2)						# => 3
```

#### Atajo para crear funciones

```elixir
fun = &(&1 + 1)
fun.(1)							# => 2
```

### Parámetros por defecto

Se usa el operador `\\`

```elixir
defmodule DefaultTest do
  def dowork(x \\ "hello") do
    x
  end
end

DefaultTest.dowork				# => "hello"
DefaultTest.dowork(123)			# => 123
```

**Cuando una función tiene varios parámetros por defecto es necesario crear una *firma* en donde se inicialicen**:

```elixir
defmodule Concat do
  # A function head declaring defaults
  def join(a, b \\ nil, sep \\ " ")

  def join(a, b, _sep) when is_nil(b) do
    a
  end

  def join(a, b, sep) do
    a <> sep <> b
  end
end
```

- *The leading underscore in `_sep` means that the variable will be ignored in this function; see [Naming Conventions](https://hexdocs.pm/elixir/main/naming-conventions.html#underscore-_foo).*

### Guardas

Consisten en hacer un overloading de funciones **con misma aridad** mediante una condición. En caso de que dicha condición no se cumple, no se tiene en cuenta esa versión de la función.

```elixir
defmodule Calculadora do
	def dividir(_a, b) when b == 0
		:infinito
	end
	def dividir(a, b)
		a/b
	end
end
```

# *Structs*

Son un tipo de ***map*** con *default* ***keys*** y ***values***.

- Deben ser definidas **dentro de un módulo**.
- Toman el nombre del módulo.
- Si no de especifica un valor *default* este será ***nil***.

```elixir
defmodule User do
	defstruct name: "John", age: 27
end

%User{}							# => User{age: 27, name: "John"}
%User{name: "Jane"}				# => %User{age: 27, name: "Jane"}
```

## Acceso

Igual que con un ***map***.

```elixir
john = %User{}				# => %User{age: 27, name: "John"}
john.name					# => "John"
```

# Operadores

## Concatenar

```elixir
# CONCATENAR
++						# Concatena listas
--						# Concatena listas
<>						# Concatena strings

```

## Condicionales

```elixir
# CONDICIONALES (1): siempre que se compare condiciones usar estos
or
and
not

# CONDICIONALES (2): aceptan otros tipos de dato ademas de bool (ej: nil, int, etc)
||
&&
!
```

## Comparación

```elixir
1 == 1.0				# => true
1 === 1.0				# => false
!=
!==
<=
>=
< 
> 
```

En _Elixir_ se pueden comparar distintos tipos de dato sin problema:

```elixir
1 < :atom				# => true
```

- Esto le permite a los algoritmos de ordenamiento trabajar sin dificultades de tipo.

__El orden de menor a mayor es:__ `number, atom, reference, function, port, pid, tuple, map, list, bitstring`

## Pipe operator

El operador **|>** pasa el resultado de una expresión a otra.

 `other_function() |> new_function() |> baz() |> bar() |> foo()` 

Ejemplos:

```elixir
"Elixir rocks" |> String.split()								# => ["Elixir", "rocks"]
"Elixir rocks" |> String.upcase() |> String.split()				# => ["ELIXIR", "ROCKS"]
"elixir" |> String.ends_with?("ixir")							# => true
```

- En caso de que la *arity* de una función sea mayor a 1 **usar paréntesis**.

# _Pattern matching_

## Operador =

El ___operador =___ en _Elixir_ se conoce como ___match operator___. Tiene un comportamiento un tanto distinto al resto de lenguajes:

```elixir
x = 1				# => 1
1 = x				# => 1
```

- Como se puede ver, la segunda sentencia parece algo extraña, en _Elixir_ esto es valido ya que __1 y _x_ tienen el mismo valor__.

Si se intenta lo mismo pero con valores distintos se producirá un error:

![image-20211202235127505](..\img\image-20211202235127505.png)

Este operador es muy útil cuando se quiere __desestructurar__ un tipo de dato más complejo, por ejemplo una _tuple_.

```elixir
{a, b, c} = {:hello, "world", 42}			# => {:hello, "world", 42}
a											# => :hello
b											# => "world"
```

En caso de que no se cumpla con la validez de datos a ambos lados habrá un error, uno usual es cuando una de las tuplas tiene distinto tamaño:

![image-20211202235511983](..\img\image-20211202235511983.png)

Otro error usual ocurre cuando se comparan _tuplas_ y _listas_:

![image-20211202235608831](..\img\image-20211202235608831.png)

### _Matchear_ valores específicos

```elixir
{:ok, result} = {:ok, 13}				# => {:ok, 13}
result									# => 13
```

- En este caso el lado izquierdo solo _matcheará_ con el derecho __si este último tiene como primer elemento al _atom :ok___.

![image-20211203000048652](..\img\image-20211203000048652.png)

## En _lists_

```elixir
[a, b, c] = [1, 2, 3]					# => [1, 2, 3]
a										# => 1
```

### _head_ - _tail_

```elixir
[head | tail] = [1, 2, 3]				# => [1, 2, 3]
head									# => 1
tail									# => [2, 3]
```

## Operador ^

Impide el _rebinding_ de una variable.

![image-20211203000725393](..\img\image-20211203000725393.png)

- Como se __pinneo__ a la variable ___x___ cuando tenia el valor ___1___ la segunda operación sería el equivalente a hacer `1 = 2`

## Variable _

Se utiliza cuando no interesa un valor en un determinado ___pattern matching___.

```elixir
[head | _] = [1, 2, 3]					# => [1, 2, 3]
head									# => 1
```

- Tratar de acceder a _ genera un error.

# Estructuras de control

## _case_

- Equivalente al ___case___ en _Ruby_ y al ___switch___ en _C++_.
- __Un error no traspasa, solo hace fallar la guarda en cuestión__.

Ejemplo:

```elixir
iex> case {1, 2, 3} do
	{4, 5, 6} -> "This clause won't match"
	{1, x, 3} -> "This clause will match and bind x to 2 in this clause"
	_ -> "This clause would match any value"
end
```

### Con variables extra

Se usa el operador __^__:

```elixir
x = 1
case 10 do
	^x -> "Won't match"
	_ -> "Will match"
end
```

## _cond_

A diferencia de _case_ que permite matchear distintos valores, ___cond___ __busca la primer condición que se cumpla__.

- Sería el equivalente al ___else if___.
- __TODO LO QUE NO SEA _NIL_ O _FALSE_ ES CONSIDERADO COMO _TRUE___.

```elixir
cond do
	2 + 2 == 5 -> "This will not be true"
	2 * 2 == 3 -> "Nor this"
	1 + 1 == 2 -> "But this will"
end
```

- Si todas las condiciones son falsas habrá un error. Por eso se recomienda agregar un ___true___ como condición final (sería el equivalente a un ___else___):

```elixir
iex> cond do
	2 + 2 == 5 -> "This is never true"
	2 * 2 == 3 -> "Nor this"
	true -> "This is always true (equivalent to else)"
end
```

## _if / unless_

```elixir
if true do
	"This works!"
end

unless true do
	"This will never be seen"
end

if nil do
	"This won't be seen"
	else
	"This will"
end
```

También se pueden usar en **asignaciones**.

```elixir
edad = 22
mensaje = if edad >= 18 do
	"Mayor de edad"
else
	"Menor de edad"	
end
```

# Bloques _do - end_

```elixir
if true, do: 1 + 2							# => 3
if false, do: :this, else: :that			# => :that
```

# Recursividad

Al tener *inmutabilidad*, los *loops* no pueden realizarse de la misma manera que en lenguajes como *C, Ruby, C++, etc*. En ***Elixir*** se *loopea* usando **recursividad**.

```elixir
defmodule Recursion do
  def print_multiple_times(msg, n) when n > 0 do
    IO.puts(msg)
    print_multiple_times(msg, n - 1)
  end

  def print_multiple_times(_msg, 0) do
    :ok
  end
end

Recursion.print_multiple_times("Hello!", 3)
# Hello!
# Hello!
# Hello!
:ok
```

## Ejemplo implementando un equivalente a *sum*

```elixir
defmodule Math do
  def sum_list([head | tail], accumulator) do
    sum_list(tail, head + accumulator)
  end

  def sum_list([], accumulator) do
    accumulator
  end
end

IO.puts Math.sum_list([1, 2, 3], 0)			 # => 6
```

## Ejemplo implementando una función que multiplique por 2 los elementos

```elixir
defmodule Math do
  def double_each([head | tail]) do
    [head * 2 | double_each(tail)]
  end

  def double_each([]) do
    []
  end
end
```

# *Enumerables*

Consisten en un conjunto de funciones aplicables a colecciones, **excepto a tuplas**. Para listar todo el módulo `Enum` pegar el siguiente extracto en el intérprete:

```elixir
Enum.__info__(:functions) |> Enum.each(fn({function, arity}) ->
IO.puts "#{function}/#{arity}"
end)
```

## Capture operator

Muchas funciones del módulo toman una función anónima como parámetro. La sintaxis usual sería la siguiente:

```elixir
Enum.map([1,2,3], fn number -> number + 3 end)			# =>[4, 5, 6]
```

Sin embargo esto se podría simplificar **utilizando el operador *&***:

```elixir
Enum.map([1,2,3], &(&1 + 3))							# => [4, 5, 6]
```

## *all?*

Evalúa si **todos los elementos** cumplen una determinada condición.

```elixir
Enum.all?(["foo", "bar", "hello"], fn(s) -> String.length(s) == 3 end)			# => false
Enum.all?(["foo", "bar", "hello"], fn(s) -> String.length(s) > 1 end)			# => true
```

## *any?*

Evalúa si **al menos un elemento** cumple una determinada condición.

```elixir
Enum.any?(["foo", "bar", "hello"], fn(s) -> String.length(s) == 5 end)			# => true
```

## *chunk_every*

Permite dividir la colección en pequeños grupos.

```elixir
Enum.chunk_every([1, 2, 3, 4, 5, 6], 2)				# => [[1, 2], [3, 4], [5, 6]]
```

## *chunk_by*

Permite dividir la colección respecto a otra métrica que no sea el tamaño.

```elixir
Enum.chunk_by(["one", "two", "three", "four", "five"], fn(x) -> String.length(x) end)
# => [["one", "two"], ["three"], ["four", "five"]]

Enum.chunk_by(["one", "two", "three", "four", "five", "six"], fn(x) -> String.length(x) end)
# => [["one", "two"], ["three"], ["four", "five"], ["six"]]
```

## *count*

Devuelve un **contador** que haga referencia a la cantidad de repeticiones en las que la función se cumple.

```elixir
Enum.count([1,2,3,4], fn x -> Integer.is_even(x) end)			# => 2
```

## *count_until*

Lo mismo que ***count*** a diferencia de que este se detiene cuando se llega al valor especificado.

```elixir
Enum.count_until(1..20, 5)				# => 5
```

## *each*

Permite **iterar** (!!!!!!!!!!!!!!!), una colección sin producir ningún valor.

```elixir
Enum.each(["one", "two", "three"], fn(s) -> IO.puts(s) end)
# one
# two
# three
# :ok
```

## *empty?*

Evalúa si la colección está vacía o no.

```elixir
Enum.empty?([])						# => true
Enum.empty?([1, 2, 3])				# => false
```

## *filter*

Filtra los elementos que no cumplan una determinada condición.

```elixir
Enum.filter([1, 2, 3, 4], fn(x) -> rem(x, 2) == 0 end)		# => [2, 4]
```

## *find*

Devuelve el **primer elemento** para el que la función sea *true*. Caso contrario devuelve el valor asignado al parámetro ***default***.

```elixir
# Enum.find(coleccion, default, funcion)
Enum.find([2, 4, 6], 0, fn x -> rem(x, 2) == 1 end)			# => 0
```

## *find_index*

Devuelve el **índice** del elemento que haga que la función sea *true*.

```elixir
Enum.find_index([2,4,6], fn x -> x == 2 end)				# => 0
Enum.find_index([2,4,6], fn x -> x == 5 end					# => nil
```

## *frequencies*

Devuelve un ***map*** donde cada *key* representa al elemento de la colección y el *value* la cantidad de repeticiones.

```elixir
Enum.frequencies(~w{ant buffalo ant ant buffalo dingo})
# => %{"ant" => 3, "buffalo" => 2, "dingo" => 1}
```

## *map*

Permite aplicar una función a cada elemento de la colección.

```elixir
Enum.map([0, 1, 2, 3], fn(x) -> x - 1 end)					# => [-1, 0, 1, 2]
```

## *map_every*

Equivalente al *map*, éste permite aplicar la función saltando de a ***n*** elementos.

```elixir
# Apply function every three items
iex> Enum.map_every([1, 2, 3, 4, 5, 6, 7, 8], 3, fn x -> x + 1000 end)
# => [1001, 2, 3, 1004, 5, 6, 1007, 8]
```

## *max*

Obtiene el máximo de la colección.

```elixir
Enum.max([5, 3, 0, -1])					# => 5
```

Tiene una versión ***max/2*** que permite especificar una función que le de el valor de *max* **si la colección esta vacía**.

```elixir
Enum.max([], fn -> :bar end)			# => :bar
```

## *min*

Obtiene el mínimo de la colección.

```elixir
Enum.min([5, 3, 0, -1])					# => -1
```

Tiene una versión ***min/2*** que permite especificar una función que le de el valor de *min* **si la colección esta vacía**.

```elixir
Enum.min([], fn -> :foo end)			# => :foo
```

## *product*

Devuelve el producto de toda la colección.

```elixir
Enum.product([2, 3, 4])					# => 24
```

## *reduce*

Aplica una función (**con el *accumulator***) a todos los elementos. Si no se especifica el ***accumulator*** toma el valor del primer elemento.

```elixir
Enum.reduce([1, 2, 3], 10, fn(x, acc) -> x + acc end)			# => 16
Enum.reduce([1, 2, 3], fn(x, acc) -> x + acc end)				# => 6
Enum.reduce(["a","b","c"], "1", fn(x,acc)-> x <> acc end)		# => "cba1"
```

## *shuffle*

Desordena la colección.

```elixir
Enum.shuffle([1, 2, 3])					# => [2, 1, 3]
```

## *sort*

Ordena la colección. La primera versión ordena utilizando el método de ***Erlang***, la segunda pide como parámetro extra una *sorting function*.

```elixir
Enum.sort([5, 6, 1, 3, -1, 4])						# => [-1, 1, 3, 4, 5, 6]

Enum.sort([%{:val => 4}, %{:val => 1}], fn(x, y) -> x[:val] > y[:val] end)
# => [%{val: 4}, %{val: 1}]
```

## *sum*

Suma todos los elementos.

```elixir
Enum.sum([1, 2, 3])			# => 6
```

## *take*

Toma una cantidad de elementos de la colección.

```elixir
Enum.take([1, 2, 3], 2)		# => [1, 2]
```

## *take_every*

Los mismo que ***take*** a diferencia de que no va de 1 en 1, sino de *n* en *n*.

```elixir
take_every(1..10, 2)		# => [1, 3, 5, 7, 9]
```

## *uniq*

Elimina duplicados.

```elixir
Enum.uniq([1, 2, 3, 2, 1, 1, 1, 1, 1])		# => [1, 2, 3]
```

## *uniq_by*

Permite especificar una función de comparación.

```elixir
Enum.uniq_by([%{x: 1, y: 1}, %{x: 2, y: 1}, %{x: 3, y: 3}], fn coord -> coord.y end)
# => [%{x: 1, y: 1}, %{x: 3, y: 3}]
```

# *Generators*

```elixir
for n <- [1, 2, 3, 4], do: n * n			# => [1, 4, 9, 16]
for n <- 1..4, do: n * n					# => [1, 4, 9, 16]
```

# *IO*

## Input y Output

```elixir
IO.puts("hello world")
IO.gets("yes or no? ")
```

## *File*

Este módulo contiene funciones que permiten abrir archivos como ***IO devices***.

```elixir
file = File.open!("newfile", [:read, :utf8])
```

- Si no se especifica *utf8* el archivo se abre en *binary mode*.
- **Se usa *open!* debido a que *open* devuelve siempre una tupla**.

```elixir
{:ok, file} = File.open("newfile")		# util para pattern matching y error handling	
```

Además de leer y modificar archivos, este módulo tiene otras funciones para trabajar con el sistema de archivos. Algunos ejemplos:

- `File.rm/1`: para borrar archivos.
- `File.mkdir/1`: para crear carpetas.
- `File.cp/2`: para copiar.
- `File.rm/1`: para borrar.

## *Path*

```elixir
Path.join("foo", "bar")				# => "foo/bar"
Path.expand("~/hello")				# => "/Users/jose/hello"
```

# *Sigils*

## *Regex*

```elixir
regex = ~r/foo|bar/					# => ~r/foo|bar/
"foo" =~ regex						# => true
"HELLO" =~ ~r/hello/				# => false
"HELLO" =~ ~r/hello/i				# => true
```

## *Strings*

Se utiliza el ***sigil*** `~s`.

```elixir
~s(this is a string with "double" quotes, not 'single' ones)
# => "this is a string with \"double\" quotes, not 'single' ones"
```

## *Charlists*

Se utiliza el ***sigil*** `~c`.

```elixir
~c(this is a char list containing 'single quotes')
# => 'this is a char list containing \'single quotes\''
```

## *Wordlists*

Se utiliza el ***sigil*** `~w`

```elixir
~w(foo bar bat)					# => ["foo", "bar", "bat"]
```

- *The `~w` sigil also accepts the `c`, `s` and `a` modifiers (for char lists, strings, and atoms, respectively), which specify the data type of the elements of the resulting list:*

  ```elixir
  ~w(foo bar bat)a			# => [:foo, :bar, :bat]
  ```

## Dias y horarios

### *Date*

```elixir
d = ~D[2019-10-31]				# => ~D[2019-10-31]
d.day							# => 31
```

### *Time*

```elixir
t = ~T[23:00:07.0]				# => ~T[23:00:07.0]
t.second						# => 7
```

### *UTC DateTime*

Agrupa `Date` y `Time`, teniendo en cuenta el *timezone* ***UTC***.

```elixir
dt = ~U[2019-10-31 19:59:03Z]								# => ~U[2019-10-31 19:59:03Z]
%DateTime{minute: minute, time_zone: time_zone} = dt		# => ~U[2019-10-31 19:59:03Z]
minute														# => 59
time_zone													# => "Etc/UTC"
```
