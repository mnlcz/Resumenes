# Indice
- [Indice](#indice)
- [Compilar y ejecutar](#compilar-y-ejecutar)
  - [Gradle](#gradle)
  - [Native](#native)
- [Variables](#variables)
  - [Inicializacion](#inicializacion)
    - [Desestructuracion](#desestructuracion)
  - [Null safety](#null-safety)
    - [Assertion](#assertion)
    - [Filtrar valores null](#filtrar-valores-null)
    - [Safe nullable type access](#safe-nullable-type-access)
    - [Check null (elvis operator)](#check-null-elvis-operator)
- [Casteo y chequeo de tipos](#casteo-y-chequeo-de-tipos)
  - [Type conversion entre tipos numericos](#type-conversion-entre-tipos-numericos)
    - [Conversion a Short y Byte](#conversion-a-short-y-byte)
  - [Type conversion entre Strings y Booleanos](#type-conversion-entre-strings-y-booleanos)
- [Control flow](#control-flow)
  - [*when*](#when)
  - [Asignación](#asignación)
- [Funciones](#funciones)
  - [Aclaraciones](#aclaraciones)
  - [Ejemplos](#ejemplos)
  - [Parámetros](#parámetros)
    - [*vararg*](#vararg)
    - [Parámetros con nombre](#parámetros-con-nombre)
    - [Parámetros por defecto](#parámetros-por-defecto)
  - [Referencias a funciones](#referencias-a-funciones)
  - [Funciones *inline*](#funciones-inline)
  - [*Lambdas*](#lambdas)
    - [Multiples argumentos](#multiples-argumentos)
    - [Un único argumento](#un-único-argumento)
  - [Funciones *operator*](#funciones-operator)
  - [Funciones como parámetro](#funciones-como-parámetro)
- [Colecciones](#colecciones)
  - [Inicializar con valores arbitrarios](#inicializar-con-valores-arbitrarios)
  - [Arrays](#arrays)
    - [Array vacío](#array-vacío)
  - [Listas](#listas)
    - [Mostrar una lista](#mostrar-una-lista)
    - [Comparar](#comparar)
    - [Operaciones basicas](#operaciones-basicas)
    - [Copiar listas](#copiar-listas)
  - [Matrices](#matrices)
    - [Acceso](#acceso)
    - [Imprimir](#imprimir)
  - [Map](#map)
    - [Recorrido](#recorrido)
  - [Manipulación](#manipulación)
    - [*map()*](#map-1)
    - [*filter()*](#filter)
- [Iteracion](#iteracion)
  - [*repeat*](#repeat)
  - [*for*](#for)
  - [Iteracion por indices](#iteracion-por-indices)
    - [Rangos](#rangos)
      - [Basicos](#basicos)
      - [*downTo*](#downto)
      - [*step*](#step)
      - [*until*](#until)
  - [*forEach*](#foreach)
  - [*forEachIndexed*](#foreachindexed)
- [User input](#user-input)
  - [readLine](#readline)
    - [*to*](#to)
    - [Leer multiples valores en una linea](#leer-multiples-valores-en-una-linea)
  - [readln](#readln)
  - [Scanner](#scanner)
  - [Leer cantidad indefinida de valores](#leer-cantidad-indefinida-de-valores)
  - [Leer elementos de una lista](#leer-elementos-de-una-lista)
- [OOP](#oop)
  - [Clases](#clases)
    - [Propiedades](#propiedades)
    - [Constructores](#constructores)
      - [Alternativa a constructores secundarios](#alternativa-a-constructores-secundarios)
    - [Getters y setters](#getters-y-setters)
    - [Herencia](#herencia)
    - [Data classes](#data-classes)
  - [Visibilidad](#visibilidad)
- [Type alias](#type-alias)
  - [Basico](#basico)
  - [Funciones](#funciones-1)
  - [Generics](#generics)

# Compilar y ejecutar

Para compilar:

```powershell
kotlinc myCode.kt -include-runtime -d myCode.jar
```

Para ejecutar:

```powershell
java -jar myCode.jar
```

## Gradle
```powershell
gradle init
gradle build
gradle run
```

## Native
```powershell
konanc -o hello hello.kt
```

# Variables

## Inicializacion
La asignación en *Kotlin* se divide en 2 tipos:

- `val`: variables que solo se pueden asignar una vez.

- `var`: las variables de toda la vida.

La sintaxis es la siguiente:

```kotlin
val num: Int = 13
```

Vale aclarar que dependiendo de la situación, es posible omitir el tipo de dato.

Otra característica un poco diferente a otros lenguajes es que por defecto los tipos en *Kotlin* **no son *nullables***. Para que una variable soporte `null` es necesario especificarlo en el tipo de dato con el operador `?`.

```kotlin
val nombre: String? = null
```
### Desestructuracion
Es posible inicializar mutliples valores en una línea:
```kotlin
val (a, b, c) = arrayOf(0, 0, 0)
```
- También funciona con `listOf` y cualquier otra estructura similar.


## Null safety

### Assertion
El operador `!!` convierte el valor *nullable* a una versión *not-null* del mismo tipo. **En caso de que el valor sea null se producirá la excepción:** `KotlinNullPointerException`. Es una forma de decirle al compilador de *Kotlin* que uno está seguro de que dicho valor no será null.
```kotlin
val message: String? = null
println(message!!) //KotlinNullPointerException thrown, app crashes
```

### Filtrar valores null
```kotlin
val a: List<Int?> = listOf(1, 2, 3, null)
val b: List<Int> = a.filterNotNull()
```

### Safe nullable type access
Para acceder valores *nullable* es necesario utilizar operadores especiales `?.` y `let`.

- `?.` permite utilizar los métodos del valor como si no fuera nullable:
```kotlin
val string: String? = "Hello World!"
print(string.length) // Compile error: Can't directly access property of nullable type.
print(string?.length) // Will print the string's length, or "null" if the string is null.
```
- `let` es similar al operador previo, permite interpretar un valor nullable como uno not null:
```kotlin
nullable?.let { notnull ->
notnull.foo()
notnull.bar()
}
```
### Check null (elvis operator)

Similar al operador `??` de *C#*,  en *Kotlin* sería con el operador `?:`:

```kotlin
val nombre: String? = null
val nombre2 = nombre ?: "Es null"
```

- Si *nombre* es *null*, *nombre2* pasa a valer *"Es null"*, caso contrario tomaría el valor de *nombre*.

# Casteo y chequeo de tipos

Para chequear el tipo basta con usar el operador `is`:

```kotlin
if (obj is String) 
{
    print(obj.length)
}
```

Para castearlo explicitamente se usa el operador `as`:

```kotlin
val x: String = y as String
```

## Type conversion entre tipos numericos

```kotlin
val num: Int = 100
val res: Double = sqrt(num.toDouble())

val num2: Int = 100
val bigNum: Long = num2.toLong() // 100

val d: Double = 12.5
val n: Long = d.toLong() // 12

val d: Double = 10.2
val n: Long = 15
val res1: Int = d.toInt() // 10
val res2: Int = n.toInt() // 15
```

### Conversion a Short y Byte

```kotlin
val floatNumber = 10f
val doubleNumber = 1.0

val shortNumber = floatNumber.toShort() // avoid this
val byteNumber = doubleNumber.toByte()  // avoid this

val shortNumber = floatNumber.toInt().toShort() // correct way
val byteNumber = doubleNumber.toInt().toByte()  // correct way
```

## Type conversion entre Strings y Booleanos

```kotlin
val n = 8     // Int
val d = 10.09 // Double
val c = '@'   // Char
val b = true  // Boolean

val s1 = n.toString() // "8"
val s2 = d.toString() // "10.09"
val s3 = c.toString() // "@"
val s4 = b.toString() // "true"

val n = "8".toInt() // Int
val d = "10.09".toDouble() // Double
val b = "true".toBoolean() // Boolean

val b1 = "false".toBoolean() // false
val b2 = "tru".toBoolean()   // false
val b3 = "true".toBoolean()  // true
val b4 = "TRUE".toBoolean()  // true
```

# Control flow

Los clasicos *if - else* son iguales al resto de lenguajes. Las diferencias se dan en los *one liner statements* y que ***Kotlin* no soporta el operador ternario**.

## *when*

Es el equivalente al `switch` de toda la vida. La sintaxis es la siguiente.

```kotlin
when(num)
{
    13 -> println("CASE 13")
    else -> println("Default case")
}
```
Puede hacer chequeos más complejos que el *switch* de toda la vida.
```kotlin
val names = listOf("John", "Sarah", "Tim", "Maggie")
when (x) {
    in names -> print("I know that name!")
    !in 1..10 -> print("Argument was not in the range from 1 to 10")
    is String -> print(x.length) // Due to smart casting, you can use String-functions here
}
```
También se puede usar para reemplazar un *if* que contenga muchos *else if*:
```kotlin
if (str.length == 0) {
    print("The string is empty!")
} else if (str.length > 5) {
    print("The string is short!")
} else {
    print("The string is long!")
}

// Usando when
when {
    str.length == 0 -> print("The string is empty!")
    str.length > 5 -> print("The string is short!")
    else -> print("The string is long!")
}
```
Ejemplo: chequeando si un num es par o impar.
```kotlin
val number = readln().toInt()
when {
  number % 2 == 0 -> println("$number is even")
  number % 2 != 0 -> println("$number is odd")
}
```

## Asignación

Las estructuras de control se pueden utilizar tambien para asignar una variable.

```kotlin
val num = 13
val nroFav = if(num == 13) 13 else -1
```

Ejemplo con la estructura `when`:

```kotlin
val nroFav2 = when(num)
{
    13 -> 13
    else -> -1
}
```

# Funciones

## Aclaraciones

- Para las funciones que no devuelven nada **no hay que especificar el valor de retorno**. Este de fondo es `Unit`, el cual seria el equivalente al `void` de otros lenguajes.

- Por convención, se utiliza **camelCase**.

## Ejemplos

La sintaxis es la siguiente:

```kotlin
fun devuelveString(): String
{
    // ...
}

fun noDevuelveNada()
{
    // ...
}

fun funcionDeUnaLinea() = "Hola Kotlin"

fun funcionConParametros(nombre: String) = "Hola $nombre"
```

- Para las definiciones en una linea generalmente no hace falta especificar el tipo de retorno.

## Parámetros

### *vararg*

Además de los parámetros comunes existen los parámetros con `vararg`, estos se utilizan con colecciones y permiten recibir una cantidad variable de elementos, incluso 0. 

```kotlin
fun saludar1(personas: List<String>)
{
    personas.forEach { println(it) }
}

fun saludar2(vararg personas: String)
{
    personas.forEach { println(it) }
}
```

- `saludar2` permitiría su llamada sin ningun parámetro, mientras que `saludar1` necesitaria una lista vacía.

- En caso de que se quiera pasar una colección a `vararg` basta con utilizar el operador `*`.
  
  ```kotlin
  val nombres = arrayOf("Manuel", "Nicolas")
  saludar2(*nombres)
  ```

### Parámetros con nombre

```kotlin
fun saludar(saludo: String, persona: String) = println("$saludo $persona")

saludar(saludo = "Hola", persona = "Manuel")
saludar(persona = "Nicolas", saludo = "Hola")
```

### Parámetros por defecto

```kotlin
fun saludar(saludo: String = "Hola", persona: String = "Kotlin") = println("$saludo $persona")

saludar()
```
## Referencias a funciones
Para referenciar funciones se utiliza el operador `::`. Esto es util cuando se requiera pasar una función como parámetro sin necesidad de llamarla. Por ejemplo:
```kotlin
fun addTwo(x: Int) = x + 2
listOf(1, 2, 3, 4).map(::addTwo)
```
## Funciones *inline*
Son equivalentes a las macros en *C*. La llamada a una función de este tipo "pega" el cuerpo de la misma en el lugar de la llamada. Vale aclarar que estas funciones solo pueden ser accedidas en el scope en el que se las define. Ejemplo:
```kotlin
inline fun sayMyName() = println("My name is Manuel")
```
## *Lambdas*
- No requieren `return`. **La última sentencia se convierte en el valor de retorno**.
- Si el único argumento de una función es una *lambda*, **se pueden omitir los parentesis**. Ejemplo:
  ```kotlin
    listOf(1, 2, 3, 4).map { it + 2 }
  ```
### Multiples argumentos
```kotlin
{ argumentOne:String, argumentTwo:String ->
    "$argumentOne - $argumentTwo"
}
```
### Un único argumento
```kotlin
{ "Your name is $it" }
```

## Funciones *operator*
Vendría a ser el equivalente al *operator overloading* de *C++*. Una pequeña diferencia es que en *Kotlin* a la hora de definir ese overload no se utiliza el operador en si, sino una especie de alias del mismo. Algunos alias:
- `plus(b)`: X + b
- `minus(b)`: X - b
- `times(b)`: X * b
- `div()`: X / b
- `not()`: X!

La lista completa se puede ver en [Operator overloading](https://kotlinlang.org/docs/operator-overloading.html).

Un ejemplo de un overloading:
```kotlin
data class IntListWrapper (val wrapped: List<Int>) {
    operator fun get(position: Int): Int = wrapped[position]
}
```

## Funciones como parámetro
```kotlin
// Takes no parameters and returns anything
() -> Any?
// Takes a string and an integer and returns ReturnType
(arg1: String, arg2: Int) -> ReturnType
```

# Colecciones

Es importante aclarar que las colecciones por defecto **SON INMUTABLES**, es decir, no se pueden quitar ni agregar elementos. Para solucionar esto existe la version mutable de estas colecciones, por ejemplo: `mutableListOf`.

## Inicializar con valores arbitrarios
```kotlin
val x = Array<Int>(9) {0}
// [0, 0, 0, 0, 0, 0, 0, 0, 0]

val numbers = IntArray(5) { 10 * (it + 1) }
// [10, 20, 30, 40, 50]
```

## Arrays

```kotlin
val nombres: Array<String> = arrayOf("Manuel", "Nicolas", "Federico")
```

- El tipo no es necesario, en este caso lo puse para que se entienda mejor.

### Array vacío

```kotlin
val emptyArray = emptyArray<Int>()
```

## Listas

```kotlin
val nombres = listOf("Manuel", "Nicolas", "Federico")
```

### Mostrar una lista
Se puede utilizar `joinToString()`
```kotlin
val southernCross = mutableListOf("Acrux", "Gacrux", "Imai", "Mimosa")
println(southernCross.joinToString())   //  Acrux, Gacrux, Imai, Mimosa
```
- `joinToString()` tiene un parametro opcional: el **separator**.
  ```kotlin
  println(southernCross.joinToString(" -> "))   //  Acrux -> Gacrux -> Imai -> Mimosa
  ```

### Comparar
Se pueden utilizar los operadores `==` y `!=`.
```kotlin
val firstList = mutableListOf("result", "is", "true")
val secondList = mutableListOf("result", "is", "true")
val thirdList = mutableListOf("result")

println(firstList == secondList)  //  true
println(firstList == thirdList)   //  false
println(secondList != thirdList)  //  true
```

### Operaciones basicas
- `add`: agregar un elemento
- `+=`: agregar un elemento
- `addAll`: agrega todos los elementos de otra lista
- `remove`: quita un elemento
- `removeAt`: quita un elemento dado un indice
- `clear`: quita todos los elementos

### Copiar listas
No existe una funcion para copiar listas existentes. Esto se puede hacer mediante `toMutableList`:
```kotlin
val list = mutableListOf(1, 2, 3, 4, 5)
val copyList = list.toMutableList()

print(copyList) // [1, 2, 3, 4, 5]
```

## Matrices
```kotlin
val mutList2D = mutableListOf(
    mutableListOf<Int>(0, 0, 0, 0),
    mutableListOf<Int>(0, 0, 0, 0),
    mutableListOf<Int>(0, 0, 0, 0)
)
```
- Las matrices literales se representan con los saltos de linea acordes para una buena visibilidad.
- Las listas internas no necesariamente tienen que tener el mismo tamaño:
  ```kotlin
  val mutList2D = mutableListOf(
    mutableListOf<Int>(0),
    mutableListOf<Int>(1, 2),
    mutableListOf<Int>(3, 4, 5)
  )
  ```
- Las listas internas pueden ser de distinto tipo:
  ```kotlin
  val mutListOfStringAndInt2D = mutableListOf(
    mutableListOf<String>("Practice", "makes", "perfect"),
    mutableListOf<Int>(1, 2)
  )
  ```

### Acceso
El acceso a un elemento es como en la mayoria de lenguajes, usando el operador `[][]`.

### Imprimir
Se hace un `println` con la **lista principal**.
```kotlin
val mutListOfChar2D = mutableListOf(
  mutableListOf<Char>('k'),
  mutableListOf<Char>('o', 't'),
  mutableListOf<Char>('l', 'i', 'n')
)

println(mutListOfChar2D)    // [[k], [o, t], [l, i, n]]
```
En caso de que se quiera usar ``joinToString` es necesario aclarar los indices de las sub-listas.
```kotlin
val mutListString = mutableListOf(
    mutableListOf<String>("A", "R", "R", "A", "Y")
  )
print(mutListString[0].joinToString())    // A, R, R, A, Y
```

## Map

Equivalente a *Hash* en otros lenguajes.

```kotlin
val map = mapOf("ARG" to "Argentina", "BR" to "Brasil", "EEUU" to "Estados Unidos")
```

### Recorrido

```kotlin
map.forEach { key, value -> println("$key -> $value")}
```

## Manipulación
### *map()*
Se utiliza para transformar una colección.
```kotlin
val numbers = listOf(1, 2, 3, 4, 5, 6, 7, 8, 9, 0)
val numberStrings = numbers.map { "Number $it" }
```
### *filter()*
```kotlin
val numbers: List<Int> = listOf(1, 2, 3, 4, 5, 6, 7)
val evenNumbers = numbers.filter { it % 2 == 0 }
```

# Iteracion
## *repeat*
Sea `n` la cantidad de repeticiones:
```kotlin
repeat(3)
{
  println("hola")
}
```
## *for*

Es bastante similar al `foreach` de *C#*.

```kotlin
val nombres = arrayOf("Manuel", "Nicolas", "Federico")
for(n in nombres)
{
    / ...
}
```

## Iteracion por indices
Equivalente al *for loop* generico en lenguajes como *C, C++, JAVA*.
```kotlin
val daysOfWeek = mutableListOf("Sun", "Mon", "Tues", "Wed", "Thur", "Fri", "Sat")

for (index in daysOfWeek.indices){
  println("$index: ${daysOfWeek[index]}")
}
```

### Rangos
Pequeña aclaración necesaria: la palabra clave `in` sirve para obtener un valor booleano que represente si *X* elemento se encuentra en *Y* rango.
```kotlin
println(5 in 5..15)  // true
println(12 in 5..15) // true
println(15 in 5..15) // true
println(20 in 5..15) // false
```
- Para ignorar el limite por derecha:
```kotlin
val withinExclRight = c in a..b - 1 // a <= c && c < b
```
- Para negar la existencia en el rango:
```kotlin
val notWithin = 100 !in 10..99 // true
```
Los rangos también pueden ser utilizados con caracteres:
```kotlin
println('b' in 'a'..'c') // true
println('k' in 'a'..'e') // false

println("hello" in "he".."hi") // true
println("abc" in "aab".."aac") // false
```

#### Basicos
```kotlin
for (i in 1..4) print(i) // prints "1234"
```
#### *downTo*
```kotlin
for (i in 4 downTo 1) print(i) // prints "4321"
```
#### *step*
```kotlin
for (i in 1..4 step 2) print(i) // prints "13"
for (i in 4 downTo 1 step 2) print(i) // prints "42"
```
#### *until*
```kotlin
for (i in 1 until 10) { // i in [1, 10), 10 is excluded
println(i)
}
```

## *forEach*

Bastante similar a *Ruby*.

```kotlin
val nombres = arrayOf("Manuel", "Nicolas", "Federico")
nombres.forEach { println(it) }
```

- Por defecto el iterador correspondiente al elemento se llama `it`. Para cambiarle el nombre sería de la siguiente manera
  
  ```kotlin
  nombres.forEach { nombre -> println(nombre) }
  ```

## *forEachIndexed*

Similar al `for(int i = 0 ...)`  de *C++* o al `each_with_index` de *Ruby*.

```kotlin
nombres.forEachIndexed { index, nombre -> println("Nombre: $nombre Indice: $index") }
```

# User input

## readLine
Es el método original para la lectura de datos. **SIN EMBARGO, SE ESTA BUSCANDO QUE `readln` LO REEMPLACE POR COMPLETO. OPTAR SIEMPRE POR ESTE ULTIMO!!!**
```kotlin
println("Please enter your favorite color.")
val myColor = readLine()
```
- Vale aclarar que el tipo de `myColor` es `String?`.

Para "forzar" el tipo `String` se utiliza el operador `!!`:
```kotlin
println("Please enter your favorite color.")
val myColor = readLine()!!
```
### *to*
```kotlin
println("Print any number: ") 
val number = readLine()!!.toInt()   

println("The earth is flat. Print true or false:")
val answer = readLine()!!.toBoolean()
```
### Leer multiples valores en una linea
Suponiendo que el input sea `23 59 59`. Este se podría leer en una línea de la siguienta manera:
```kotlin
val (hrs, min, sec) = readLine()!!.split(" ")
```

## readln
- Método de preferencia para la lectura de datos **no nulos**.
- Se creo para evitar el uso del operador `!!` en el método `readLine`.
- **Siempre** devuelve una version **not nullable** de `String`.
- Se busca que en el futuro este método y su version `readlnOrNull` reemplacen por completo a `readLine`
```kotlin
println("What is your nickname?")
val nickname = readln()
println("Hello, $nickname!")
```

## Scanner

Para lectura más compleja se puede utilizar la clase `Scanner` de *Java*. Algunos de los métodos más comunes son:

- `nextDouble()`

- `next()`

- `nextInt()`

- `nextBoolean()`

## Leer cantidad indefinida de valores
Para esto se utiliza `Scanner` con un *while loop*.
```kotlin
import java.util.*

fun main() {
    val scanner = Scanner(System.`in`)
    while (scanner.hasNext()) {
        val next = scanner.next()
        println(next)
    }
}

// INPUT: Kotlin is a modern language
/* OUTPUT:
          Kotlin
          is
          a
          modern
          language
*/
```

## Leer elementos de una lista
```kotlin
val size = readln().toInt()
val mutList: MutableList<Int> = mutableListOf()
for (i in 0 until size) {
  mutList.add(readln().toInt())
}
```

# OOP

## Clases

Sintaxis base:

```kotlin
class Persona constructor()
```

- En caso de que el contructor no tenga parametros se puede omitir, quedaría así:
  
  ```kotlin
  class Persona
  ```

### Propiedades

Forma 1:

```kotlin
class Persona(_nombre: String, _apellido: String)
{
    val nombre: String = _nombre
    val apellido: String = _apellido

}
```

Forma 2 (**recomendable**):

```kotlin
class Persona(val nombre: String, val apellido: String)
```

### Constructores

El constructor principal es el que se encuentra ya sea explicito o no en la firma de la clase. En caso de que se quieran constructores secundarios:

```kotlin
class Persona(val nombre: String, val apellido: String)
{
    construcor(): this.("Manuel", "Lopez Cosmitz")
}
```

- El constructor secundario llama al primario con X valores.

- Los bloques `init` **simpre se ejecutan antes que los constructores secundarios**. 

#### Alternativa a constructores secundarios

Es posible en la gran mayoria de los casos optar por **parámetros por defecto**. Ejemplo:

```kotlin
class Persona(val nombre: String = "Sin nombre", val apellido: String = "Sin apellido")
```

### Getters y setters

Se generan automaticamente por el compilador, dependiendo si la propiedad es `var` o `val`. Para sobreescribirlos:

```kotlin
class Persona(val nombre: String = "Sin nombre", val apellido: String = "Sin apellido")
{
    var apodo: String? = null
        set(valor)
        {
            field = value
            println("Nuevo apodo: $value")
        }
}
```

- `field` corresponde a la propiedad en cuestión. Si se omite su asignación perdería sentido el *setter*.

### Herencia

Para que una clase pueda ser heredada es necesario especificarlo con la palabra clave `open` en la definición de la misma.

```kotlin
open class SerVivo()
{
    val patas: Int = 4
}
```

La herencia se realiza con el operador `:`

```kotlin
class Humano : SerVivo()
```

Para sobreescribir una propiedad o un método es necesario utilizar la palabra clave `override`. Sin embargo, para las propiedades también hay que usar `open` al igual que con las clases. Todo junto se vería así:

```kotlin
open class SerVivo()
{
    open val patas: Int = 4
}


class Humano : SerVivo()
{
    override val patas: Int
        get() = 2        
}
```

### Data classes
Las data clases son utiles cuando se necesita representar un conjunto de información simple en una clase. Una de sus caracteristicas fundamentales es la posibilidad de comparar 2 objetos de esta clase segun sus propiedades.
```kotlin
data class Persona(val nombre: String, val apellido: String, val edad: Int)
```
Las data clases tienen la capacidad de **desestructurarse** en sus respectivos elementos. Continuando con el ejemplo previo:
```kotlin
val persona = Persona("Manuel", "Lopez Cosmitz", 23)
val (nombre, apellido, edad) = persona
```

## Visibilidad

Por defecto la visibilidad de todo es `public`. Los distintos modificadores son:

- `public`

- `internal`

- `private`

- `protected`

# Type alias
Se utiliza la palabra clave `typealias`.

## Basico
```kotlin
typealias STRING = String
```

## Funciones
```kotlin
typealias StringValidator = (String) -> Boolean
typealias Reductor<T, U, V> = (T, U) -> V
```

## Generics
```kotlin
typealias Parents = Pair<Person, Person>
typealias Accounts = List<Account>
```