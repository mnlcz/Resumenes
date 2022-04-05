# Recordatorios

- Para ver como se _parsea_ un archivo separado por _newlines_  ver __la resolución del día 2 del _Advent of Code___.

# Comentarios

```ruby
# Linea simple 
=begin
  Comentarios
  Multi
  Linea
=end 
```
# Proyectos utilizando _bundle_
Para crear un proyecto ruby utilizando el gestor _bundle_ se usa el comando:
```
bundle gem nombreProyecto
```
Se creara una carpeta con el nombre elegido, dentro de la misma habra una carpeta lib,
en la cual se encuentra el archivo .rb.
Para correr el proyecto se utiliza el comando:
 ```
 ruby nombreProyecto.rb
 ```
 Para agregar __gems__ se edita el __Gemfile__:
 ```ruby
source "https://rubygems.org"

gem 'byebug'
gem 'httparty'
gem 'nokogiri'
 ```
# Intérprete

```bash
irb			# opcion 1
pry			# opcion 2
```

## Clear

_CTRL + L_

## Documentación

```bash
pry
ri Prime
ri Prime#prime?
```

# Print

```ruby
print "bla bla bla bla"
puts "bla bla bla"
p "bla bla bla"
```
La diferencia entre ambos es que ___puts___ agrega una salto de linea al final. ___p___ se suele utilizar para debug, ya que imprime el "_raw material_" por decirlo de alguna manera.

# Variables
Sin declarar el tipo de dato:
```ruby
  nombre_personaje = "Manuel"
  edad = 21
  puts ("Nombre: " + nombre_personaje + "Edad: " + edad)
```
## Constantes
Todas las variables que empiecen con mayúscula son tratadas como constantes. Usar uppercase para nombrarlas:
```ruby
VARIABLE_CONSTANTE = 50
```

## Estáticas

```ruby
class Personas
   @@cantidad = 0
    def initialize(nombre)
    	@nombre = nombre
        @@cantidad += 1
    end
end
```

Otro ejemplo:

```ruby
class Machine
  @@users = Hash.new
  
  def initialize(username, password)
    @username = username
    @password = password
    @@users[username] = password
    @files = Hash.new
  end
  
  def create(filename)
    time = Time.now
    @files[filename] = time
    puts "#{filename} was created by #{@username} at #{time}."
  end
  
# IMPORTANTE: ver como se llama el método.    
  def Machine.get_users
    @@users
  end
end
```

## Globales

```ruby
$variable_global = 10
```

# Casteo

Se puede castear el tipo de dos maneras: usando los métodos <to_> o haciendo:
```ruby
<TIPO>(<valor_para_convertir>)
String(125.6)
Integer(2.34)
```
## Enteros y Strings
```ruby
Integer("123")     # => 123
Integer("0xFF")    # => 255
Integer("0b100")   # => 4
Integer("0555")    # => 365
```

# Tipos de datos
NO SE DECLARAN, es solo para saber:
```ruby
Strings: nombre = Manuel
Entero: edad = 21
Float: numeroFloat = 0.5
Booleano: esHombre = true
Nil (una especie de void, no tiene valor): ejemploNil = nil 
```
# _% literals_

- `%w()` array of strings
- `%r()` regular expression.
- `%q()` string
- `%x()` a shell command (returning the output string)
- `%i()` array of symbols (Ruby >= 2.0.0)
- `%s()` symbol
- `%()` (without letter) shortcut for `%Q()`

# Strings

## Inicializar

```ruby
my_string = "Hello." 							# create a string from a literal
my_empty_string = String.new					# create an empty string
my_copied_string = String.new(my_string) 		# copy a string to a new variable
```

## Tipos

```ruby
str1 = 'hola'									# tipo mas basico
str2 = "hola"									# permite concatenar y caracteres especiales
str3 = <<-HERE
Esta es una string
muy larga,
ocupa varias lineas.
HERE
```

## Concatenar

```ruby
def strike(word)
  "<strike>#{word}</strike>"
end
strike("Meow!") # => "<strike>Meow!</strike>"
```

## Métodos útiles

```ruby
frase = "Esto es una string"
puts frase.upcase()
puts frase.length()
puts frase.include?("Esto")
puts frase[0]
puts frase[0,3]                			 		# Caracteres del índice 0 hasta el 2.
puts frase.index("s")
```

### Encoding

```ruby
puts frase.encoding.name						# => UTF-8
```

- Para ver los otros 100 _encodings_ que soporta _Ruby_: `puts Encoding.list`

#### Cambiar encoding

```ruby
"ñññ".encode("utf-8")							# Preferible
"ñññ".force_encoding("utf-8")					# Ultimo recurso
```

### _chomp_ y _chomp!_

```ruby
"Hello World!  \r\n".chomp						# => "Hello World!  "
"Hello World!".chomp("orld!")					# => "Hello W"
"hello \n there".chomp							# => "hello \n there"
```

### _chop_ y _chop!_

Remueve el __último caracter__.

```ruby
"string\n".chop									# => "string"
"string".chop									# => "strin"
```

### _tr_ y _tr!_

Devuelve una copia de la _string_, modificando el las ocurrencias del primer parámetro por el segundo. __Generalmente se usa para modificar caracteres__. Para modificar palabras utilizar _gsub_.

```ruby 
tr(p1, p1)
```

Ejemplo:

```ruby
"hello".tr('el', 'ip')      	# => "hippo"
"hello".tr('aeiou', '*')    	# => "h*ll*"
"hello".tr('a-y', 'b-z')    	# => "ifmmp"
```

### _strip_ y _strip!_

Elimina espacios, tabs, etc.

```ruby
"    hello    ".strip   		# => "hello"
"\tgoodbye\r\n".strip   		# => "goodbye"
```

### _gsub_ y _gsub!_

```ruby
string = "We have many dogs"
string.gsub("dogs", "cats")		# => "We have many cats"
```

Se puede utilizar para eliminar palabras:

```ruby
string = "abccc"
string.gsub("c", "")			# => "ab"
```

También funciona con _regex_:

```ruby
string = "We have 3 cats"
string.gsub(/\d+/, "5")			# => "We have 5 cats"
"hello".gsub(/[aeiou]/, '*')	# => "h*ll*"
```

Y con bloques:

```ruby
# Asumir que existe strike/1
text.gsub(/#{pattern}/) {|match| strike(match)}
```

### _each_

![image-20211201132143017](C:\Users\manuc\AppData\Roaming\Typora\typora-user-images\image-20211201132143017.png)

# Números

```ruby
2 + 2
4 - 2
5 * 2
4/2
2**2
10 % 3
num = 20
puts ("Imprimiendo num" + num.to_s)   # Para imprimir num hay que convertirlo a string.
num.abs()                             # Valor absoluto del numero.
num.round()                           # Redondear.
numero = 20.1
numero.ceil()                         # "techo" = 21
numero.floor()                        # "piso" = 20
```

## Decimales

### _format_

Un uso de _format_ sería especificar __la cantidad de decimales__ a mostrar:

```ruby
format('%.6f', 0.0)					# => "0.000000"
```

# Clase _Math_

```ruby
Math.sqrt(num)
Math.log(1)
```
También posee seno coseno, etc.

# Clase _Time_ y _Date_

## _Date_

```ruby
require 'date'
```

- _day_
- _month_
- _year_

```ruby
Date.today # Current date
Date.new   # Returns a negative date
Date.today + 1  # Adds one day
```

### Formating

Se utiliza el método ___strptime___.

```ruby
Date.strptime("3 of September", "%d of %B") # 2015-09-03
```

### Constantes

La clase _Date_ tiene constantes que pueden resultar útiles:

```ruby
Date::MONTHNAMES # (index 0 = nil)
Date::DAYNAMES.rotate(1)
```

- __Los índices de los meses empiezan en 1__.
- __La semana empieza el Domingo, con _rotate_ se puede lograr que empiece el Lunes__.

## _Time_

```ruby
require 'time'
```

- _hours_
- _minutes_
- _seconds_

```ruby
Time.now
# 2018-10-19 15:43:20 +0200

Time.new(2018, 1, 1)
# 2018-01-01 00:00:00 +0100

Time.at(15000000000)
# 2017-07-14 04:40:00 +0200
```

### Componentes

```ruby
t = Time.now

puts t.day
puts t.month
puts t.hour
```

### Formating

Se utiliza el método ___strftime___.

```ruby
time = Time.new

time.strftime("%d/%m/%Y")        # "05/12/2015"
time.strftime("%k:%M")           # "17:48"
time.strftime("%I:%M %p")        # "11:04 PM"
time.strftime("Today is %A")     # "Today is Sunday"
time.strftime("%d of %B, %Y")    # "21 of December, 2015"
time.strftime("Unix time is %s") # "Unix time is 1449336630"
```

- Más info en [Strftime](http://strftime.net/)

### Suma y resta

```ruby
# Add ten seconds
time = Time.new + 10
```

## Parseo

El método ___parse___ tratará de parsear cualquier _string_. Vale aclarar que puede devolver resultados sin sentido.

```ruby
Date.parse("10/10/2010")  # -> 2010-10-10
Date.parse("September 3") # -> 2015-09-03
Date.parse("May I have a cup of coffee, please?") # -> 1 of May
```

## _DateTime_

Es una _subclass_ de _Date_, permite guardar horas además de fechas.

```ruby
require 'date'

DateTime.superclass
# Date

DateTime.now
# DateTime: 2018-10-15T16:06:39+02:00
```

# Floats

```ruby
10/7            # = 1
10/7.0          # = 1.4285...
```

# Input
```ruby
puts "Ingrese un numero: "
num1 = gets
puts ("El numero es: " + num1)
```
El ___gets___ genera tambien un _newline_ cuando el usuario presiona _Enter_, por lo tanto, si yo agregara texto luego de la variable num1 en el ___puts___, habría una _newline_ entre medio, para solucionar esto se realiza la siguiente modificación:
```ruby
puts "Ingrese un numero: "
num2 = gets.chomp()   # Elimina el newline.
puts ("El numero es: " + num2 + "bla bla bla")
```

# Colecciones

## Iteración

Además de los clásicos _each_, _while_ y _for_, _Ruby_ cuenta con una clase __Enumerable__, la cuál más formas de iterar.

### _each_with_index_

```ruby
colors = ['red', 'green', 'blue']
colors.each_with_index { |item, index| p "#{index}:#{item}" }
```

## Operaciones

### _map_

```ruby
[1,2,3].map { |x| 2*x }		# => [2, 4, 6]
```

También se la llama ___collect___:

```ruby
{:a=>1, :b=>2, :c=>3}.collect { |key, value| 2*value }
# => [2, 4, 6]
```

### _sum_

```ruby
[1, 2, 3].sum				# => 6
```

### _inject_/_reduce_

Acumula el valor de una __operación__ aplicada a la colección y a un __valor inicial__ dado.

```ruby
(5..10).inject(:+)			# => 45
(5..10).inject(1, :*)		# => 151200
```

Ejemplo usando bloques:

```ruby
(5..10).inject { |sum, n| sum + n }		#=> 45
```

## Agrupar

### _group_by_

Devuelve un __Hash__ representando los resultados del bloque. Ejemplo: agrupar los elementos pares.

```ruby
(1..5).group_by {|x| x % 2 == 0}
# => {false=>[1, 3, 5], true=>[2, 4]}
```

Ejemplo usando __custom keys__:

```ruby
marks = { "Ramesh": 23, "Vivek": 40, "Harsh": 88, "Mohammad": 60 }
marks.group_by do |_, value|
    if value < 30
      'Failed'
    else
      'Passed'
	end
end

# => {"Failed"=>[[:Ramesh, 23]], "Passed"=>[[:Vivek, 40], [:Harsh, 88], [:Mohammad, 60]]}
```

- La __key__ en caso de que no cumpla el bloque es ___Failed___.
- La __key__ en caso de que cumpla el bloque es ___Passed___.

# Array

```ruby
numeros = Array[1, 2, 3, 4, 5]
```
A diferencia de las arrays en _C++_, en Ruby se pueden almacenar valores de distinto tipo. Algunos métodos utiles para arrays son los clasicos de __length__, __sort__, etc. <br>
El metodo __sort__ ordena la array (tiene que ser de un tipo solo) alfabeticamente o numericamente.
Se pueden obtener los elementos al igual que en _C++_, usando los indices. Sin embargo, en Ruby tambien se pueden ingresar indices inversos, es decir, el indice -1, el cual
empezaria por el final de la array.

## Inicializar

### Vacío

```ruby
arr1 = Array.new
arr2 = []
```

### _nil_

```ruby
arr1 = Array.new(1)
arr2 = [nil]
```

### Múltiples elementos iguales

```ruby
# .new(length, value)
arr1 = Array.new(2, 10)
```

### _Array_ de _Strings_ o _Char_

```ruby
arr = %w[b c]
```

### Array de _Symbols_

```ruby
arr = %i[s1 s2]
```

## Índices

### Básicos

```ruby
arr = [9, 5, 1, 2, 3, 4, 0, -1]
arr[4]						# => 3
arr.at(4)					# => 3
```

#### Inverso

```ruby
arr = [9, 5, 1, 2, 3, 4, 0, -1]
arr[-1]						# => -1
```

#### Múltiples

```ruby
arr = [9, 5, 1, 2, 3, 4, 0, -1]
arr.take(3)					# => [9, 5, 1]
arr.drop(3)					# => [2, 3, 4, 0, -1]
```

### Rangos

```ruby
arr = [9, 5, 1, 2, 3, 4, 0, -1]
arr[1..3]					# => incluye ambos índices
# [5, 1, 2]

arr[1...3]					# => excluye al último índice
# [5, 1]
```

#### Etc

Desde un índice en particular hasta un tamaño de _array_.

```ruby
arr = [9, 5, 1, 2, 3, 4, 0, -1]
arr[1,4]					#  => [5, 1, 2, 3]
```

## Insertar elementos

### _push_

Agrega __al final__.

```ruby
x = [1,2]
x.push(3)					# => [1,2,3]
```

__Aclaración__: recordar que _push_ tiene un "atajo" y es el operador __<<__.

```ruby
x = [1,2]
x << 3						# => [1,2,3]
```

### _insert_

Agrega uno o más elementos a partir de un índice en concreto.

```ruby
.insert(<indice>, <elemento/s>)
```

Ejemplo:

```ruby
x = [1,2]
x.insert(1, 5, 6, 7) 		# => [1, 5, 6, 7, 2]
```

### _unshift_

Agrega __al inicio__.

```ruby
x = [1,2,3]
x.unshift(10, 20, 30)		# => [10, 20, 30, 1, 2, 3]
```

## Eliminar elementos

### _pop_

Elimina __el último elemento__.

```ruby
arr = [5, 6, 5, 4, 3, 1, 2, 5, 4, 3, 3, 3]
arr.pop						# => borra el 3
```

### _shift_

Elimina __el primer elemento__.

```ruby
arr = [5, 6, 5, 4, 3, 1, 2, 5, 4, 3, 3, 3]
arr.pop						# => borra el 5
```

### _delete_at_

Elimina el elemento __en el índice brindado__.

```ruby
arr = [5, 6, 5, 4, 3, 1, 2, 5, 4, 3, 3, 3]
arr.delete_at(2)			# => borra el 5
```

### _delete_

Elimina __todas las ocurrencias__ del elemento.

```ruby
arr = [5, 6, 5, 4, 3, 1, 2, 5, 4, 3, 3, 3]
arr.delete(5)				# => borra todos los 5
```

## Obtener elementos (sin modificar _array_)

### _select_

El método ___select___ permite obtener un subconjunto de un _array_ que cumpla una determinada condición.

```ruby
arr = [3, 4, 2, 1, 2, 3, 4, 5, 6]
arr.select {|a| a > 2}		#  => [3, 4, 3, 4, 5, 6]
```

- La variante ___select!___ modifica al _array_.

### _reject_

Lo contrario al ___select___, obtiene el subconjunto que __no cumple__ con la condición.

```ruby
arr = [3, 4, 2, 1, 2, 3, 4, 5, 6]
arr.reject {|a| a > 2}		# => [2, 1, 2]
```

- La variante ___reject!___ modifica al _array_.

### _drop_while_

Elimina elementos hasta que la condición dentro del bloque sea __falsa__.

```ruby
arr = [3, 4, 2, 1, 2, 3, 4, 5, 6]
arr.drop_while {|a| a > 1}	# => [1, 2, 3, 4, 5, 6]
```

## Obtener elementos (modificando _array_)

### _delete_if_

```ruby
arr = [3, 4, 2, 1, 2, 3, 4, 5, 6]  
arr.delete_if {|a| a < 2} 	# => [3, 4, 2, 2, 3, 4, 5, 6]  
```

### _keep_if_

```ruby
arr = [3, 4, 2, 1, 2, 3, 4, 5, 6]
arr.keep_if {|a| a < 4}  	# => [3, 2, 2, 3]
```

# Matriz

## Inicializar

```ruby
arr_2d = Array.new(row) { Array.new(col,default_value) }
```

## Reemplazar elemento

Supongase que `num` es el valor que se quiere reemplazar de la matriz.

```ruby
@tablero.each { |x| x.map! { |y| y == num ? -1 : y } }
```

- Se chequea si el valor está en la matriz, en caso de que esté se lo reemplaza.

# Hash

## Inicializar

__La forma más moderna de un _Hash_ es la siguiente__:

```ruby
provincias =
{
	ID: "Valor"
}
```

- A pesar de tener : al final, ___ID___ sigue siendo un símbolo.

Otra forma:

```ruby
provincias =
{
  :ID => "Valor"
}
```
También se puede escribir de la siguiente manera:
```ruby
provincias =
{
  "ID" => "Valor"
}
```
## Acceso

Para acceder a un valor basta con llamar al hash con la clave:

```ruby
puts provincias[:ID]
```

## _select_

El método _select_ permite filtrar al _Hash_. Lo que hace es dado un bloque con un criterio de filtro devuelve un nuevo _Hash_ con los elementos que lo cumplan.

```ruby
grades = 
{ 
  alice: 100,
  bob: 92,
  chris: 95,
  dave: 97
}
 
grades.select { |name, grade| grade <  97 }
# ==> { :bob => 92, :chris => 95 }
```

## _each_\_

Se puede iterar por las _claves_ o los _valores_ usando:

- ___.each_key___
- ___.each_value___

## _keys_

Devuelve un _array_ con las claves.

```ruby
hash = { id: 150}
claves = hash.keys			# => [:id]
```

- La primer clave se podría obtener de la siguiente manera:

```ruby
primero = hash.keys.first
```

## _values_

Devuelve un _array_ con los valores.

```ruby
hash = { id: 150}
valores = hash.values		# => [150]
```

# Métodos

```ruby
def nombre_metodo
  puts "Esto es un método"
end
```
Si el método no recibe parámetros se omiten los paréntesis tanto en la declaración como en la llamada.
## Parametros
```ruby
def nombre_metodo(parametro1, parametro2)
  puts (parametro1 + parametro2)
end

nombre_metodo("test1", "test2")
```
### Default

Se pueden asignar __valores por defecto__ para los parametros, por ejemplo:

```ruby
def info_persona(nombre, trabajo = "no tiene")
  puts (nombre + trabajo)
end

info_persona("Manuel")
```

### Opcionales

Se usa el operador __*__ luego del parámetro.

```ruby
def chocolate(number_of_milk_packets*)
  “This will be good with #{number_of_milk_packets} packets”
end
```

### _Keyword_

Permite __pasar los argumentos en cualquier orden__ algo que con parámetros _default_ y _opcionales_ no se podía.

```ruby
def write(file:, data:, mode: "ascii")
end

write(data: 123, file: "test.txt")
```

- __Permiten englobar las características tanto de los parámetros _default_ tanto como los opcionales__

  ```ruby
  def foo(x, str: "foo", num: 424242)
    [x, str, num]
  end
  ```

### Gran cantidad de parámetros

Usando el operador __*__ se puede definir un parámetro "múltiple", el cual se comportaría como una especie de _array_.

```ruby
def sum(first, *rest)
    rest.reduce(first) { |o, x| o + x }
end

sum(1)
sum(1, 2)
sum(1, 2, 3)
```

#### Usando _Hash_

El operador __**__ permite pasar un _Hash_ como parámetro.

```ruby
def foo(str: "foo", num: 424242, **options)
  [str, num, options]
end

foo #=> ['foo', 424242, {}]
foo(check: true) # => ['foo', 424242, {check: true}]
```

## Return

Funciona igual que en _C++_, la unica diferencia es que se puede retornar mas de 1 valor. En estos casos, a la hora de la llamada, se trata al _return_ type como un array. 
```ruby
def suma_multiplicacion(numero)
  suma = numero + 2
  multiplicacion = numero * 2
  return suma, multiplicacion
end

suma = suma_multiplicacion(5)[0]
multiplicacion = suma_multiplicacion(5)[1]
```

## Yield

___yield___ permite "pasarle el control" a un bloque en la llamada del método.

```ruby
def double(n)
  yield(n)
end

double(5) { |n| n*2 }
```

- En este caso se ve que __yield__ le "da" el parámetro _n_ al bloque y este lo multiplica.

# Condicionales

## if, elsif, else
```ruby
if <condicion>
  puts "if statement"
elsif
  puts "elsif statement"
else
  puts "else statement"
end
```
## unless

```ruby
llueve = true
puts "salir" unless llueve
```

## Operadores

```ruby
# Recomendados (más fuertes):
&& 
||
!
? :   # Operador ternario.
# Variante (más débil):
and
or
not
```
## Case
Pensarlo como un _switch_ de _C++_:
```ruby
case <valor_a_comparar>
when <caso>
  # ...
when <caso2>
  # ...
else
  # default.
end
```

# Loops
## while
```ruby
while <condicion>
  # ...
end
```
## each
```ruby
<array>.each do |<variable>|
  # ...
end
```
## for
```ruby
for <variable> in <donde_se_busca>
  # ...
end
```
## times

```ruby
<numero>.times do |<variable>|
  # ...
end
```

## upto, downto

```ruby
95.upto(100) { |num| print num, " " }
# Prints 95 96 97 98 99 100
```

También funciona con letras:

```ruby
"L".upto("P") { |letra| puts letra }
# L M N O P
```

# Archivos

```ruby
File.open(<nombre>, <modo>) do |<variable_archivo>|
  # ...
end
```

## Modos
```ruby
"r"     # Solo lectura, comienza al inicio del archivo.
"r+"    # Lectura y escritura, comienza al inicio del archivo.
"w"     # Solo escritura, sobreescribe o crea el archivo.
"w+"    # Lectura y escritura, sobreescribe o crea el archivo.
"a"     # Solo escritura, crea el archivo o comienza al final del mismo.
"a+"    # Lectura y escritura, crea el archivo o comienza al final del mismo.
"b"     # Binario.  
```

## Algunos metodos de lectura
```ruby
File.open(<nombre>, "r") do |<variable_archivo>|
  puts <variable_archivo>.read()
  puts <variable_archivo>.readline()
  puts <variable_archivo>.readchar()
  for linea in <variable_archivo>.readline()
    puts linea
  end
end
```

## Algunos metodos de escritura
```ruby
File.open(<nombre>, "r+") do |<variable_archivo>|
  <variable_archivo>.write("\nAgregando texto al archivo.")
  <variable_archivo>.readline()     # Envia el cursor a la siguiente linea.
  <variable_archivo>.write("Modificando la segunda linea")
end
```

# Clases
Definición:
```ruby
class Nombre
  # ...
end
```

## Atributos
Hay 2 formas para declarar atributos, la primera es utilizando _@_ para definir las variables:
```ruby
class Nombre
  def initialize (x , y, z)
    @atributo1 = x
    @atributo2 = y
    @atributo3 = z  
  end
end
```
En caso de querer acceder a esas variables va a haber que crear manualmente los getters. 

La otra forma es utilizando ___attr_accessor___ o sus variantes ___attr_writer___, ___attr_reader___, las cuales generan automaticamente los getters y setters:
```ruby
class Nombre
  attr_accessor :atributo1, :atributo2
end
```
### Variantes attr
```ruby
attr_accessor   # Genera getters y setters.
attr_reader     # Genera getters.
attr_writer     # Genera setters.
```

Ejemplo:

```ruby
class Person
  attr_reader :name
  attr_writer :job
  def initialize(name, job)
    @name = name
    @job = job
  end
end
```

## Constructor

El constructor tiene el nombre de _initialize_, a diferencia de _C++_ donde compartia el nombre con la clase. Para llamarlo se utiliza ___.new___
```ruby
objeto = Clase.new(<parametros>)
```

## Visibilidad

Por defecto los métodos son __public__. La sintaxis se ve de la siguiente manera:

```ruby
class ClassName
  # Some class stuff
  public
  def public_method
    # public_method stuff
  end
end
```

- Todo lo que viene luego de ___public___ es público.

Agregando un método privado quedaría de la siguiente manera:

```ruby
class ClassName
  # Some class stuff
 
  public
  # Public methods go here
  def public_method; end
 
  private
  # Private methods go here
  def private_method; end
end
```

# Herencia

```ruby
class DerivedClass < BaseClass
  # Some stuff!
end
```

Ejemplo:

```ruby
class ApplicationError
  def display_error
    puts "Error! Error!"
  end
end

class SuperBadError < ApplicationError
end

err = SuperBadError.new
err.display_error
```

## Override

```ruby
class Creature
  def initialize(name)
    @name = name
  end
  
  def fight
    return "Punch to the chops!"
  end
end

class Dragon < Creature
  def fight
    "Breathes fire!"
  end
end
```

## Super

Cuando se usa ___super___ dentro de un método, eso le dice  _Ruby_ que busque un método con el mismo nombre en la __superclass__.

```ruby
class Creature
  def initialize(name)
    @name = name
  end
  
  def fight
    return "Punch to the chops!"
  end
end

class Dragon < Creature
  def fight
    puts "Instead of breathing fire..."
    super
  end
end
```

- __IMPORTANTE__: cada clase tiene 1 _superclass_.

## Ejemplo extra

```ruby
class Message
  @@messages_sent = 0
  def initialize(from, to)
    @from = from
    @to = to
    @@messages_sent += 1
  end
end

class Email < Message
  def initialize(from, to) super; end
end

my_message = Message.new("yo", "vos")
```

# Módulos

Un módulo puede definirse como una caja de herramientas que contiene una serie de _métodos_ y _constantes_. Algunas características de los módulos:

- No pueden crear clases.
- No pueden crear instancias.
- Usan __constantes__ en vez de __variables__.

Pensarlos como los _header_ y los _namespaces_ en _C++_.

```ruby
module Circle

  PI = 3.141592653589793
  
  def Circle.area(radius)
    PI * radius**2
  end
  
  def Circle.circumference(radius)
    2 * PI * radius
  end
end
```

## Acceso

Para acceder a los elementos de un módulo se utiliza el _operador de resolución de scope_ ___::___

```ruby
puts Math::PI
```

- Imprime el valor de ___PI___ que se encuentra en el __modulo__ _Math_.

## _require_

Para incluir módulos en el _.rb_ se utiliza la keyword ___require___:

```ruby
require 'module'
```

## _include_

Sería el equivalente al ___using namespace___ de _C++_. Permite evitar la utilización del operador ___::___

```ruby
class Angle
  include Math
  attr_accessor :radians
  
  def initialize(radians)
    @radians = radians
  end
  
  def cosine
    cos(@radians)
  end
end

acute = Angle.new(1)
acute.cosine
```

## _extend_

A diferencia de _include_ que permite la utilización de métodos del módulo para las instancias de la clase, ___extend___ permite que la clase en sí utilice dichos métodos. Pensarlo de manera estática, se pueden usar los métodos sin instanciar la clase.

```ruby
module ThePresent
  def now
    puts "It's #{Time.new.hour > 12 ? Time.new.hour - 12 : Time.new.hour}:#{Time.new.min} #{Time.new.hour > 12 ? 'PM' : 'AM'} (GMT)."
  end
end

class TheHereAnd
  extend ThePresent
end

TheHereAnd.now
```

# Mixins

Un mixin se trata de la conjunción entre las clases y los módulos. Éste consiste en la utilización de un módulo en más de una clase, generalmente dicho módulo cuenta con métodos generales para las clases en cuestión.

```ruby
module Action
  def jump
    @distance = rand(4) + 2
    puts "I jumped forward #{@distance} feet!"
  end
end

class Rabbit
  include Action
  attr_reader :name
  def initialize(name)
    @name = name
  end
end

class Cricket
  include Action
  attr_reader :name
  def initialize(name)
    @name = name
  end
end

peter = Rabbit.new("Peter")
jiminy = Cricket.new("Jiminy")

peter.jump
jiminy.jump
```

Como se ve en el ejemplo, el módulo cuenta con la definición del método ___jump___. Dicho método es utilizado posteriormente en ambas clases, con solo incluir el módulo se evita una redefinición del método, en otras palabras, escribirlo mas de una vez.

# Error handling

```ruby
begin
  # aca va el código que podría llegar a tener errores. Ej:
  num = 10/0
rescue
  puts "Division por cero"    # Si se da el error se ejecuta.
end
```

## Multiples errores:
```ruby
begin
  num = 10/0
  array["hola"]
rescue ZeroDivisionError    # Especifico el error que va a "atrapar".
  puts "Division por cero"
rescue TypeError
  puts "Tipo incorrecto"
end
```

## Variables
Se puede asignar el error a una variable de la siguiente manera:
```ruby
begin
  num = 10/0
  array["hola"]
rescue ZeroDivisionError => variable1
  puts variable1
rescue TypeError => variable2
  puts variable2
end
```
Especificar el error de esta manera (utilizando variables) permite que a la hora de hacer un print del error, este sea mas detallado, ya que se imprime el error en si y no una descripcion creada por el programador.

LINEA 358

# Notaciones y atajos

## Inlines

```ruby
# if
puts "hola" if true
puts "hola" unless false

# ternario
true ? (puts "hola") : (puts "chau")

# case
case language
   when "JS" then puts "Javascript"
   when "Py" then puts "Python"
   when "RB" then puts "Ruby"
   else puts ":("
end
      
# while - until
puts "true" while true
puts "false" unless true
    
# expresion
class Dragon < Creature; end
    
# metodo
def bark; puts "Woof!" end
```

## Asignación condicional

Permite que si un objeto ya se inicializó no se pueda reescribir, y en caso de que no se haya inicializado lo haga.

```ruby
num_fav = nil
num_fav ||= 13
num_fav ||= 5
num_fav ||= 9
puts num_fav			# ==> 13
```

## Llamada y respuesta

Con el método ___respond_to?___ se puede verificar si un objeto puede recibir ese método. _respond_to?_ toma como parámetro un __símbolo__ correspondiente al método que se quiere verificar.

```ruby
[1, 2, 3].respond_to?(:push)		# true
[1, 2, 3].respond_to?(:to_sym)		# false
```

## Atajos a métodos usuales

### _.push!_

```ruby
[1, 2, 3] << 4						# ==> [1, 2, 3, 4]
"Yukihiro " << "Matsumoto"			# ==> "Yukihiro Matsumoto"
```

### .to_s

```ruby
age = 22
"I am #{age} years old."
```

## Imprimir con el operador ternario

```ruby
puts 1 < 2 ? "One is less than two" : "One is not less than 2"		# ==> One is less than two
```

# Bloques

Ejemplos:

```ruby
5.times { puts "Hola" }
```

## _.collect (map)_ y _.collect! (map!)_

Aplica una función a todos los elementos de un array. Tanto _collect_ como _map_ hacen lo mismo.

```ruby
my_nums = [1, 2, 3]
my_nums.collect! { |num| num ** 2 }
# ==> [1, 4, 9]

# ==

my_nums = [1, 2, 3]
my_nums.map! { |num| num ** 2 }
# ==> [1, 4, 9]
```

## Forma simplificada

Se pueden simplificar los bloques cuando se aplica una función:

```ruby
positive = arr.count(&:positive?)
```

# Procs

Los __procs__ vendrían a ser como las lambdas con nombre en _C++_.

```ruby
test_proc1 = Proc.new { bloque }
# o
test_proc2 = Proc.new do |x|
    # ...
end
```

Ejemplo:

```ruby
multiples_of_3 = Proc.new do |n|
  n % 3 == 0
end
```

## Pasaje a métodos

Para pasar el _proc_ a algún método se utiliza ___&___. Este operador lo que hace es __convertir a bloque__ a lo que se le ponga luego.

```ruby
cube = Proc.new { |x| x ** 3 }
[4, 5, 6].map!(&cube)
# ==> [64, 125, 216]
```

## Llamar al Proc

```ruby
test = Proc.new { # does something }
test.call
# does that something!
```

## Symbol to Proc

Usando el operador ___&___ se pueden convertir _símbolos_ a _procs_.

```ruby
strings = ["1", "2", "3"]
nums = strings.map(&:to_i)
# ==> [1, 2, 3]
```

# Lambdas

```ruby
->(param) { operacion }					# Ruby version >= 1.9
lambda { |param| operacion }			# Ruby version <= 1.8
```

## Llamada

Se puede utilizar ___call___ o ___.___

```ruby
test.call(param)						# Preferible esta formaPr
test.(param)
```

## Pasaje a métodos

Se hace igual que los _procs_, utilizando el operador ___&___:

```ruby
symbol_filter = lambda { |i| i.is_a? Symbol }
symbols = my_array.select(&symbol_filter)
```

## Aplicación Parcial

Consiste en crear una ___lambda___ que recibe un parámetro y __devuelve otra _lambda___ que realiza alguna operación con el parámetro.

```ruby
multiply_function = -> (number) do
   -> (another_number) do
       number * another_number
   end
end

doubler = multiply_function.(2)			# => en este momento es tipo PROC
tripler = multiply_function.(3)			# => en este momento es tipo PROC

puts doubler.(4)						# => 2(number) * 4(another_number) = 8
puts tripler.(4)						# => 12		
```

- _In the above example, the lambda will take *number* as a parameter, and return a lambda. When you call this lambda with *another_number*, it will return the product of the two._

## _Currying_

Es una técnica que consiste en __convertir una función__ que recibe __múltiples parámetros__ en una __secuencia de funciones que reciban solo 1 parámetro__.

![image-20211201114911504](C:\Users\manuc\AppData\Roaming\Typora\typora-user-images\image-20211201114911504.png)

Ejemplo:

```ruby
multiply_numbers = -> (x,y) do
    x*y
end

doubler = multiply_numbers.curry.(2)		# PROC => doubler/1 -> n * 2
tripler = multiply_numbers.curry.(3)		# PROC => tripler/1 -> n * 3

puts doubler.(4)    						# => 8
puts tripler.(4)    						# => 12
```

# Diferencias bloques lambdas y procs

- Un __bloque__ es un extracto de código entre un ___do___, ___end___ o ___{}___. No es un objeto pero se lo puede pasar a métodos como ___.each__ o ___.select___.
- Un ___proc___ es un bloque al que se lo puede guardar y reutilizar.
- Una ___lambda___ es un ___proc___ al que solo le interesa la cantidad de parámetros que obtiene (a diferencia de los _proc_). En caso de que no se respete la cantidad de parámetros se tirará un error. La ___lambda___ además a la hora de _return_ vuelve al scope de donde fue llamada, a diferencia de los ___proc___ que salen completamente.

# Métodos para colecciones

## _.select_

Filtra según el bloque brindado.

```ruby
arr.select { bloque }
arr.select(&proc)
```

## _.each_

```ruby
nums = (1..5).to_a
nums.each { |x| puts x }
```

## _.map!_

```ruby
nums.map! { |x| x*2 }
```

## *.slice*

Permite dividir la colección en **subcolecciones**, en otras palabras **ARRAY TO MATRIX**.

```ruby
(1..25).each_slice(5).to_a
# => [[1, 2, 3, 4, 5], [6, 7, 8, 9, 10], [11, 12, 13, 14, 15], [16, 17, 18, 19, 20], [21, 22, 23, 24, 25]]
```

# _lazy_

__Wikipedia__: _[**Lazy**](https://en.wikipedia.org/wiki/Lazy_evaluation) evaluation is an evaluation strategy that delays the assessment of an expression until its value is needed._

__Ruby Doc__: _[Enumerator::Lazy](https://ruby-doc.org/core-2.7.0/Enumerator/Lazy.html) is a special type of [Enumerator](https://ruby-doc.org/core-2.7.0/Enumerator.html), that allows constructing chains of operations without evaluating them immediately, and evaluating values on as-needed basis._

- Permite mejorar la performance evitando cálculos innecesarios. 

- Gracias a esto se pueden crear __estructuras de datos _"infinitas"___.

```ruby
power_array = -> (power, array_size) do 
    1.upto(Float::INFINITY).lazy.map { |x| x**power }.first(array_size) 
end
```

- _In this example, `lazy` avoids needless calculations to compute *power_array*._

- ___.first(array_size)___ es donde se realiza el cálculo real, el cual corresponde a, en este caso, el tamaño del _array_.

Ejemplo: Imprimir los primeros ___n___ _[Palindromic numbers](https://en.wikipedia.org/wiki/Palindromic_number)_ que sean __primos__.

```ruby
p Prime.each.lazy.select { |x| String(x) == String(x).reverse }.first(gets.to_i)
```

