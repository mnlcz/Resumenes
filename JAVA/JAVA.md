# Indice
- [Indice](#indice)
- [Compilacion](#compilacion)
- [Ejecucion](#ejecucion)
- [Variables](#variables)
  - [_final_ (constantes)](#final-constantes)
- [Tipos](#tipos)
  - [Primitivos](#primitivos)
  - [Wrapper Class (Referencias)](#wrapper-class-referencias)
    - [Conversión](#conversión)
- [Clases](#clases)
  - [Constructor](#constructor)
    - [Constructor overloading](#constructor-overloading)
  - [Métodos](#métodos)
    - [_toString()_](#tostring)
- [_switch_](#switch)
- [Strings](#strings)
  - [Concatenar](#concatenar)
  - [_equals()_](#equals)
  - [Algunos métodos útiles](#algunos-métodos-útiles)
- [Array](#array)
  - [_Arrays.toString()_](#arraystostring)
  - [_length()_](#length)
  - [_String[] args_](#string-args)
- [ArrayList](#arraylist)
  - [_add_](#add)
  - [_assortment_](#assortment)
  - [_size_](#size)
  - [Accediendo a X índice (_get()_)](#accediendo-a-x-índice-get)
  - [Modificando a X índice (_set()_)](#modificando-a-x-índice-set)
  - [Borrando a X índice (_remove()_)](#borrando-a-x-índice-remove)
  - [Obtener índice de X valor (_indexOf()_)](#obtener-índice-de-x-valor-indexof)
- [For](#for)
  - [Borrando un elemento](#borrando-un-elemento)

# Compilacion

```power
javac Archivo.java
```

- Genera ___Archivo.class___.

# Ejecucion

```powershell
java Archivo
```

- Ejecuta ___Archivo.class___.

# Variables

## _final_ (constantes)

```java
final int yearBorn = 1968;
```

Si se quisiera modificar dicha variable se daría el siguiente error:

```powershell
error: cannot assign a value to final variable yearBorn
```

# Tipos

## Primitivos

- ___int___
- ___double___
- ___boolean___
- ___char___

Entre otros.

## Wrapper Class (Referencias)

Las _wrapper class_ son "versiones" de los tipos primitivos. Son __clases__, por lo tanto cuentan con la gran ventaja de poseer __métodos__ útiles dependiendo del tipo, por ejemplo:

```java
Boolean valor = true;
valor.booleanValue();
valor.toString();
```

Algunas _wrapper class_:

- ___Integer___
- ___Boolean___
- ___Character___
- ___String___

La desventaja que poseen es que requieren mas trabajo del compilador, ya que este debe hacer la __conversión__ a tipos primitivos.

### Conversión

El compilador hace una conversión entre __primitivo__ y __wrapper__.

- ___autoboxing___: de _primitivo_ a _wrapper_.
- ___unboxing___: de _wrapper_ a _primitivo_.

# Clases

Ejemplo:

```java
public class Store {
  // instance fields
  String productType;
  int inventoryCount;
  double inventoryPrice;
  
  // constructor method
  public Store(String product, int count, double price) {
    productType = product;
    inventoryCount = count;
    inventoryPrice = price;
  }
  
  // main method
  public static void main(String[] args) {
    Store lemonadeStand = new Store("lemonade", 42, .99);
    Store cookieShop = new Store("cookies", 12, 3.75);
    
    System.out.println("Our first shop sells " + lemonadeStand.productType + " at " + lemonadeStand.inventoryPrice + " per unit.");
    
    System.out.println("Our second shop has " + cookieShop.inventoryCount + " units remaining.");
  }
}
```

## Constructor

```java
public class Car {
 
  public Car() {
    // instructions for creating a Car instance
  }
 
  public static void main(String[] args) {
    // Invoke the constructor
    Car ferrari = new Car(); 
  }
}
```

### Constructor overloading

```java
public class Car {
  String color;
  int mpg;
  boolean isElectric;
 
  // constructor 1
  public Car(String carColor, int milesPerGallon) {
    color = carColor;
    mpg = milesPerGallon;
  }
  // constructor 2
  public Car(boolean electricCar, int milesPerGallon) {
    isElectric = electricCar;
    mpg = milesPerGallon;
  }
}
```

## Métodos

```java
class Car {
 
  String color;
 
  public Car(String carColor) {
    color = carColor;
  }
 
  public void startEngine() {
    System.out.println("Starting the car!");
    System.out.println("Vroom!");
  }
 
  public static void main(String[] args){
    Car myFastCar = new Car("red");
    // Call a method on an object 
    myFastCar.startEngine();
    System.out.println("That was one fast car!");
  }
}
```

### _toString()_

```java
class Car {
 
    String color;
 
    public Car(String carColor) {
        color = carColor;
    }
 
    public static void main(String[] args){
        Car myCar = new Car("red");
        System.out.println(myCar);
    }
 
   public String toString(){
       return "This is a " + color + " car!";
   }
}
```

# _switch_

```java
switch (course) {
  case "Algebra": 
    // Enroll in Algebra
  case "Biology": 
    // Enroll in Biology
  case "History": 
    // Enroll in History
  case "Theatre":
    // Enroll in Theatre
  default:
    System.out.println("Course not found");
}
```

# Strings

## Concatenar

Se utiliza el operador ___+___.

## _equals()_

Se utiliza para comparar _Strings_:

```java
String nombre1 = "Manuel";
String nombre2 = "Carlos";
if(nombre1.equals(nombre2)) System.out.println("Son iguales");
```

## Algunos métodos útiles

- ___length()___
- ___concat()___
- ___indexOf()___
- ___charAt()___
- ___subString()___
- ___toUpperCase() / toLowerCase()___

# Array

```java
double[] prices;												// sin inicializar
double[] prices = {13.15, 15.87, 14.22, 16.66};					// inicializando
```

_Arrays_ vacías:

```java
String[] menuItems = new String[5];
```

- ___new___: inicializa los elementos dependiendo del tipo de dato:
  - ___int = 0___ 
  - ___double = 0.0___
  - ___boolean = false___
  - ___Reference = null___

## _Arrays.toString()_

- ___import java.util.Arrays___: incluye _Arrays.toString()_, entre otros métodos útiles.

```java
import java.util.Arrays;
int[] lotteryNumbers = {4, 8, 15, 16, 23, 42};
String betterPrintout = Arrays.toString(lotteryNumbers);
System.out.println(betterPrintout);
```

## _length()_

Indica el tamaño de la _Array_. Independientemente si los elementos son _null_.

```java
String[] menuItems = new String[5];
System.out.println(menuItems.length);							// length -> 5
```

## _String[] args_

```java
public class HelloYou {
  public static void main(String[] args) {
    System.out.println("Hello " + args[0]);  
  }
}
```

```powershell
java HelloYou Laura
```

# ArrayList

```java
import java.util.ArrayList;
```

Equivalente al ___std::vector___ de _C++_.

- Solo guardan __referencias__.

```java
// Declaring:
ArrayList<Integer> ages;

// Initializing:
ages = new ArrayList<Integer>();

// Declaring and initializing in one line:
ArrayList<String> babyNames = new ArrayList<String>();
```

## _add_

Equivalente al ___push_back___ en otros lenguajes.

```java
ArrayList<Car> carShow = new ArrayList<Car>();
carShow.add(ferrari);
```

Se puede aclarar el índice en el cual se quiere agregar:

```java
// Insert object porsche at index 2
carShow.add(2, porsche);
```

- Los elementos que estaban después del índice 2 se correrán a la derecha. En otras palabras __tendrán índice + 1__.

## _assortment_

Es un _ArrayList_ especial que puede guardar distintos tipos de datos.

```java
ArrayList assortment = new ArrayList<>();
assortment.add("Hello"); 		// String
assortment.add(12); 			// Integer
assortment.add(ferrari); 		// reference to Car
// assortment holds ["Hello", 12, ferrari]
```

## _size_

Indica la cantidad de elementos dentro del _ArrayList_.

```java
ArrayList<String> shoppingCart = new ArrayList<String>();
 
shoppingCart.add("Trench Coat");
System.out.println(shoppingCart.size());
// 1 is printed
shoppingCart.add("Tweed Houndstooth Hat");
System.out.println(shoppingCart.size());
// 2 is printed
shoppingCart.add("Magnifying Glass");
System.out.println(shoppingCart.size());
// 3 is printed
```

## Accediendo a X índice (_get()_)

La sintaxis usada en _Arrays_ no funcionará en este caso. Para _ArrayList_ se usa el método _get()_:

```java
<ArrayList>.get(<index>);
```

Ejemplo:

```java
ArrayList<String> shoppingCart = new ArrayList<String>();
 
shoppingCart.add("Trench Coat");
shoppingCart.add("Tweed Houndstooth Hat");
shoppingCart.add("Magnifying Glass");
 
System.out.println(shoppingCart.get(2));		// -> "Magnifying Glass"
```

## Modificando a X índice (_set()_)

```java
<ArrayList>.set(<index>, <valor>);
```

Ejemplo:

```java
ArrayList<String> shoppingCart = new ArrayList<String>();
 
shoppingCart.add("Trench Coat");
shoppingCart.add("Tweed Houndstooth Hat");
shoppingCart.add("Magnifying Glass");
 
shoppingCart.set(0, "Tweed Cape");
```

## Borrando a X índice (_remove()_)

```java
<ArrayList>.remove(<index>);
// o
<ArrayList>.remove(<valor>);		// ELIMINA LA PRIMER OCURRENCIA DE <valor>
```

Ejemplo 1:

```java
ArrayList<String> shoppingCart = new ArrayList<String>();
 
shoppingCart.add("Trench Coat");
shoppingCart.add("Tweed Houndstooth Hat");
shoppingCart.add("Magnifying Glass");
 
shoppingCart.remove(1);
// shoppingCart now holds ["Trench Coat", "Magnifying Glass"]
```

Ejemplo 2:

```java
ArrayList<String> shoppingCart = new ArrayList<String>();
 
shoppingCart.add("Trench Coat");
shoppingCart.add("Tweed Houndstooth Hat");
shoppingCart.add("Magnifying Glass");
 
shoppingCart.remove("Trench Coat");
// shoppingCart now holds ["Tweed Houndstooth Hat", "Magnifying Glass"]
```

## Obtener índice de X valor (_indexOf()_)

```java
<ArrayList>.indexof(<valor>);
```

Ejemplo:

```java
// detectives holds ["Holmes", "Poirot", "Marple", "Spade", "Fletcher", "Conan", "Ramotswe"];
System.out.println(detectives.indexOf("Fletcher"));			// --> 4
```

# For

El equivalente al _ranged for loop_ de _C++_ se lo llama __enhanced for loop__:

```java
for (String inventoryItem : inventoryItems) 
{
  // Print element value
  System.out.println(inventoryItem);
}
```

## Borrando un elemento

```java
for (int i = 0; i < lst.size(); i++) {
  if (lst.get(i) == "value to remove"){
    // remove value from ArrayList
    lst.remove(lst.get(i));
    // Decrease loop control variable by 1
    i--;    
  }
}
```

