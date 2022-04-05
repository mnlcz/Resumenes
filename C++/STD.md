# Indice
- [Indice](#indice)
- [RECORDATORIOS](#recordatorios)
  - [__FILE to _STD STRING___:](#file-to-std-string)
  - [*Type alias*](#type-alias)
- [Condicionales](#condicionales)
  - [_STD ALL_OF_:](#std-all_of)
  - [_STD ANY_OF_:](#std-any_of)
  - [_STD NONE_OF_:](#std-none_of)
- [Orden](#orden)
  - [_STD SORT_:](#std-sort)
  - [_STD STABLE_SORT_:](#std-stable_sort)
- [Búsqueda](#búsqueda)
  - [_STD BINARY_SEARCH_:](#std-binary_search)
  - [_STD LOWER_BOUND_:](#std-lower_bound)
  - [_STD UPPER_BOUND_:](#std-upper_bound)
  - [_STD FIND_:](#std-find)
  - [_STD FIND_IF_:](#std-find_if)
  - [_STD SEARCH_:](#std-search)
  - [_STD COUNT_:](#std-count)
  - [_STD COUNT_IF_:](#std-count_if)
- [Manipulación](#manipulación)
  - [_STD FOR_EACH_:](#std-for_each)
  - [_STD IOTA_:](#std-iota)
  - [_STD FILL_:](#std-fill)
  - [_STD GENERATE_:](#std-generate)
  - [_STD REVERSE_](#std-reverse)
  - [_STD REVERSE_COPY_:](#std-reverse_copy)
  - [_STD ROTATE_:](#std-rotate)
  - [_STD ROTATE_COPY_:](#std-rotate_copy)
  - [_STD COPY_](#std-copy)
  - [_STD COPY_IF_ (filter)](#std-copy_if-filter)
  - [*STD UNIQUE*](#std-unique)
  - [*STD REMOVE*](#std-remove)
- [Operaciones](#operaciones)
  - [*STD DISTANCE*](#std-distance)
  - [_STD ACCUMULATE_](#std-accumulate)
  - [_STD INNER_PRODUCT_](#std-inner_product)
  - [_STD TRANSFORM_](#std-transform)
  - [Diferencias con _for_each_](#diferencias-con-for_each)
- [Conjuntos](#conjuntos)
  - [_STD SET_UNION_:](#std-set_union)
  - [_STD SET_INTERSECTION_:](#std-set_intersection)
  - [_STD SET_DIFFERENCE_:](#std-set_difference)
  - [_STD SET_SYMMETRIC_DIFFERENCE_:](#std-set_symmetric_difference)
- [Estructuras](#estructuras)
  - [_STD SET_](#std-set)
  - [Métodos](#métodos)
  - [_STD MAP_](#std-map)
  - [Diferencias con _STD UNORDERED_MAP_](#diferencias-con-std-unordered_map)
  - [Ejemplo básico](#ejemplo-básico)
  - [Acceso](#acceso)
  - [Insertar](#insertar)
  - [Modificar](#modificar)
  - [Eliminar](#eliminar)
  - [Información](#información)
  - [Iterar](#iterar)

# RECORDATORIOS
- Los __iteradores__ son punteros, por lo tanto se pueden __desreferenciar__ para obtener el valor en su posición.

## __FILE to _STD STRING___:

```c++
ifstream archivo("../input.txt");
stringstream ss;
ss << archivo.rdbuf();
archivo.close();
string str = ss.str();
```

## *Type alias*

Ejemplo simple:

```c++
using flags = std::ios_base::fmtflags;
```

Ejemplo con ***templates***:

```c++
#include <array>

template<int S>
using INT_ARRAY = std::array<int, S>;

int main()
{
	INT_ARRAY<5> nums{1, 2, 3, 4};
}
```

# Condicionales

```c++
#include <algorithm>
```

## _STD ALL_OF_:

```c++
bool x = std::all_of(iterator, iterator, CONDICION);
```

## _STD ANY_OF_:

```c++
bool x = std::any_of(iterator, iterator, CONDICION);
```

## _STD NONE_OF_:

```c++
bool x = std::none_of(iterator, iterator, CONDICION);
```

# Orden

```c++
#include <algorithm>
```

## _STD SORT_:

```c++
std::sort(iterator, iterator);
std::sort(iterator, iterator, LAMBDA/funcion);
```

## _STD STABLE_SORT_:

La única diferencia que tiene con ___STD_SORT___ es que los elementos iguales NO CAMBIAN SU LUGAR.

```c++
/*
* stable_sort: mouse dog cat ant moth elephant --> dog cat ant moth mouse elephant
* sort: mouse dog cat ant moth elephant --> ant cat dog moth mouse elephant
*/
```

# Búsqueda

```c++
#include <algorithm>
```

## _STD BINARY_SEARCH_:

```c++
std::sort(iterator, iterator);
bool x = std::binary_search(iterator, iterator, VALOR_BUSCADO);
```

## _STD LOWER_BOUND_:

Devuelve un __iterador__ que apunta a la posición donde se encuentra el valor buscado o el primer elemento mayor.

```c++
std::sort(iterator, iterator);
ITERATOR x = std::lower_bound(iterator, iterator, VALOR_BUSCADO);
```

- Ejemplo desreferenciando:

```c++
cout << *(lower_bound(nums.begin(), nums.end(), 45)) << endl << endl;
```

- Ejemplo modificando el valor en el _iterator_:

```c++
// Se tiene
struct Enemigo
{
	string _nombre;
	bool _tieneSuperPoderes;
	int _vecesDerrotado;
	int _vecesGanadas;
};

// Funcion que busca a Mojojojo y le suma 1 a veces derrotado
void funcion(vector<Enemigo>& enemigos)
	{
		ordenarEnemigos(enemigos);
		auto it = lower_bound(enemigos.begin(), enemigos.end(), [](const auto& e) -> bool { return e._nombre == "Mojojojo"; });
		it->_vecesDerrotado++;
	}
```

- Ejemplo __trabajando con el índice__ del _lower_bound_. (ver línea del _// !!!_)

```c++
string getResponse(vector<int>& vec, int querie)
{
    string response;
    auto low = lower_bound(vec.begin(), vec.end(), querie);
    if (vec[low - vec.begin()] == querie)	// !!!
        cout << "Yes " << (low - vec.begin() + 1) << endl;
    else
        cout << "No " << (low - vec.begin() + 1) << endl;
    return response;
}
```

## _STD UPPER_BOUND_:

Devuelve un __iterador__ que apunta al primer elemento mayor al buscado.

```c++
std::sort(iterator, iterator);
ITERATOR x = std::upper_bound(iterator, iterator, VALOR_BUSCADO)
```

## _STD FIND_:

Devuelve un __iterador__, en caso de que no se encuentre devuelve un ITERATOR al final de la estructura.

```c++
ITERATOR x = find(iterator, iterator, VALOR);
bool estaEnArray = find(nums.begin(), nums.end(), 8) != nums.end();
```

## _STD FIND_IF_:

```c++
std::find_if(iterator, iterator, LAMBDA);
bool estaEnArray2 = find_if(nums.begin(), nums.end(), [](const auto n) { return n > 5; }) != nums.end();
```

## _STD SEARCH_:

Devuelve un __iterador__ que apunta a la secuencia buscada, caso contrario devuelve un ITERATOR al final de la estructura 1.

```c++
std::search(iterator1, iterator1, iterator2, iterator2);
bool estaEnArray3 = search(nums.begin(), nums.end(), nums2.begin(), nums2.end()) != nums.end();
```

## _STD COUNT_:

```c++
std::count(iterator, iterator, VALOR);
```

## _STD COUNT_IF_:

```c++
std::count_if(iterator, iterator, LAMBDA);
```

# Manipulación

```c++
#include <algorithm>
```

## _STD FOR_EACH_:

```c++
std::for_each(iterator, iterator, LAMBDA);
```

## _STD IOTA_:

```c++
#include <numeric>
```

Rellena inicialmente con VALOR, luego VALOR++.

```c++
std::iota(iterator, iterador, VALOR);
```

## _STD FILL_:

```c++
std::fill(iterator, iterator, VALOR);
```

## _STD GENERATE_:

Rellena con el resultado de las llamadas a LAMBDA.

```c++
std::generate(iterator, iterator, LAMBDA);
generate(nums.begin(), nums.end(), [i = 0]() mutable { i++; return i * 2; });
```

- ___mutable___ en lambdas permite que i (pasado por valor) pueda modificarse.

## _STD REVERSE_

```c++
std::reverse(iterator, iterator);
```

- En caso de no estar ordenado, mantiene el orden original y solo lo invierte.

## _STD REVERSE_COPY_:

Lo mismo que el anterior a diferencia de que guarda una copia de los elementos en el tercer ITERATOR.

```c++
std::reverse_copy(iterator, iterator, iterator);
```

- El tercer __iterator__ sería el ___.begin()___ de un nuevo vector.

## _STD ROTATE_:

Mueve el iterador del medio y todo lo que le sigue al inicio.

```c++
std::rotate(iterator, iterator, iterator);
// 1 2 3 4 5 --> rotate(begin, begin + 2, end) --> 3 4 5 1 2
```

## _STD ROTATE_COPY_:

Lo mismo que el anterior a diferencia de que guarda una copia de los elementos en el tercer ITERATOR.

```c++
std::rotate_copy(iterator, iterator, iterator, iterator);
```

## _STD COPY_

En caso de que el contenedor de donde se copia **pueda ser descartado** usar `std::move`.

```c++
std::copy(iterator, iterator, iterator)
```

## _STD COPY_IF_ (filter)

- __IMPORTANTE__: sirve para hacer un _filter_ a una colección.

```c++
std::copy_if(iterator, iterator, iterator, CONDICION)
```

Para _strings_ hay que usar ___std::back_inserter___:

```c++
std::copy_if(iterator, iterator, std::back_inserter(DESTINO), CONDICION});
```

También esta la versión `std::remove_if`.

## *STD UNIQUE*

Elimina las repeticiones y devuelve un **iterador** al **siguiente del último elemento**.

```c++
#include <algorithm> // std::unique
#include <iterator> // std::distance
std::vector<...>vec;
iterator = std::unique(vec.begin(), vec.end());
vec.resize(std::distance(vec.begin(), iterator));
```

Ejemplo:

```c++
#include <algorithm>
std::vector<int> myvector{10,20,20,20,30,30,20,20,10};
auto it = std::unique(myvector.begin(), myvector.end());
myvector.resize(std::distance(myvector.begin(), iterator));
```

## *STD REMOVE*

Elimina un elemento de una colección y devuelve **un iterador al siguiente del último elemento sin eliminar**.

```c++
#include <algorithm> // std::remove
#include <iterator>  // std::distance
iterator = std::remove(begin, end, VALOR);
vec.resize(std::distance(begin, iterator));
```

Ejemplo:

```c++
std::vector<int> nums{10,20,30,30,20,10,10,20};
iterator = std::remove(nums.begin(), nums.end(), 20);
nums.resize(std::distance(nums.begin(), iterator));
// 10 30 30 10 10
```

# Operaciones

## *STD DISTANCE*

Calcula la distancia entre iteradores.

```c++
#include <iterator>
std::distance(iterator, iterator)
```

Ejemplo:

```c++
std::list<int> mylist;
for (int i=0; i<10; i++) mylist.push_back (i*10);

std::list<int>::iterator first = mylist.begin();
std::list<int>::iterator last = mylist.end();

std::distance(first,last);
// 10
```

## _STD ACCUMULATE_

```c++
#include <numeric>			// std::accumulate
#include <functional>		// operaciones
std::accumulate(iterator, iterator, VALOR_INICIAL, OPERACION)		// OPERACION optativa
```

Algunas operaciones disponibles (__dentro de < > se puede especificar el tipo__):

- ___std::plus<>()___
- ___std::minus<>()___
- ___std::multiplies<>()___
- ___std::divides<>()___
- ___std::modulus<>()___

## _STD INNER_PRODUCT_

Multiplica 2 vectores.

```c++
int init = 100;
int series1[] = {10,20,30};
int series2[] = {1,2,3};

std::cout << "using default inner_product: ";
std::cout << std::inner_product(series1,series1+3,series2,init);
// using default inner_product: 240
```

## _STD TRANSFORM_

El equivalente al _map_:

```c++
std::transform(iterator, iterator, iterator2, OPERACION)
```

## Diferencias con _for_each_

- `std::transform` is the same as `map`. The idea is to apply a function to each element in between the two iterators and obtain a different container composed of elements resulting from the application of such a function.
- On the other hand, you execute `std::for_each` for the sole side effects. In other words, `std::for_each` closely resembles a plain range-based `for` loop.

# Conjuntos

```c++
#include <algorithm>
#include <iterator>
```

## _STD SET_UNION_:

Guarda en ITERADOR la __unión__ de los otros 2.

```c++
std::set_union(iterador1, iterador1, iterador2, iterador2, ITERADOR);
set_union(nums1.begin(), nums1.end(), nums2.begin(), nums2.end(), back_inserter(nums3));
```

- ___std::back_inserter___: constructs a back-insert iterator that inserts new elements at the end.

## _STD SET_INTERSECTION_:

```c++
std::set_intersection(iterador1, iterador1, iterador2, iterador2, ITERADOR);
set_intersection(nums1.begin(), nums1.end(), nums2.begin(), nums2.end(), back_inserter(nums4));
```

## _STD SET_DIFFERENCE_:

```c++
std::set_difference(iterador1, iterador1, iterador2, iterador2, ITERADOR);
set_difference(nums1.begin(), nums1.end(), nums2.begin(), nums2.end(), back_inserter(nums5));
```

## _STD SET_SYMMETRIC_DIFFERENCE_:

```c++
std::set_symmetric_difference(iterador1, iterador1, iterador2, iterador2, ITERADOR);
set_symmetric_difference(nums1.begin(), nums1.end(), nums2.begin(), nums2.end(), back_inserter(nums6));
```

# Estructuras

## _STD SET_

- Conjunto __ordenado__ de __elementos únicos__.
- ___O(log n)___: 
  - Inserción.
  - Eliminación.
  - Búsqueda.			

```c++
#include <set>

std::set<int> Set = {1, 4, 3, 2, 5, 1, 2, 5, 4, 3};
// Set => {1, 2, 3, 4, 5}
```

- Se puede elegir el tipo de orden. Por defecto es ascendente.

```c++
#include <functional>
std::set<int, std::greater<>> Set = {1, 2, 3, 4, 5, 1, 2, 3, 4, 5};

// por defecto se usa std::less
```

- Si se quiere usar tipos de dato abstracto __es necesario brindar una función de comparación para que el _set_ se mantenga ordenado__. Esto se lograría haciendo un _overloading_ del operador de comparación __<__, el cual es el operador por defecto para comparar (salvo que se haya especificado _std::greater_ como el ejemplo previo).

```c++
class Person
{
  public:
    float age;
    string name;
    
    bool operator<(const Person& p) const { return age < p.age; }
};

std::set<Person> Set = {{22, "Manuel"}, {42, "Carlos"}, {16, "Camila"}};
```

## Métodos

```c++
int length = s.size(); 						//Gives the size of the set.

s.insert(x); 								//Inserts an integer x into the set s.

s.find(x);									// Gets iterator to x. s.end() otherwise.

s.erase(val); 								//Erases an integer val from the set s.

set<int>::iterator itr = s.find(val); 		//Gives the iterator to the element val if it is found otherwise returns s.end() .
```

- Además cuenta con los clásicos métodos: _lower_bound_, _count_, entre otros.

## _STD MAP_

Es el equivalente al _Hash_ en _Ruby_. También esta su versión ___std::unordered_map___. 

## Diferencias con _STD UNORDERED_MAP_

- Al iterar, en caso de un ___map___, los valores se obtienen ordenados por la _clave_. Mientras que en un ___unordered_map___ se obtienen en un orden no arbitrario.
- ___unordered_map___ generalmente es __O(n)__.
- ___map___ generalmente es __O(log n)__.
- ___unordered_map___ es una versión moderna de __hash_map__.

## Ejemplo básico

- Los elementos dentro del _map_ son del tipo ___std::pair___, por lo que se podría acceder a la clave con el método _.first_ y al valor con _.second_

```c++
// include <map>

// std::map<KEY_TYPE, VALUE_TYPE>
std::map<char, int> Map =
{
    {'T', 7},
    {'S', 8},
    {'A', 1}
};
```

## Acceso

El acceso a un elemento es igual que con una _array_ a diferencia de que en lugar del _índice_ se pone la ___clave___.

```c++
Map['T'];
```

- En caso de que la clave no exista se devuelve el valor por defecto correspondiente al tipo de dato de la clave, en este caso sería __0__ .

## Insertar

Hay 3 formas (__la tercera es la mejor__):

```c++
// <NombreMap>[<clave>] = <valor>
Map['C'] = 2;

// <NombreMap>.insert(std::pair<KEY_TYPE, VALUE_TYPE>(<clave>, <valor>))
Map.insert(std::pair<char, int>('M', 22));

// <NombreMap>.insert(std::make_pair(<clave>, <valor>))
Map.insert(std::make_pair('N', 24));
```

## Modificar

```c++
map<string, int> students;
int mark;
cin >> mark;
students[name] += mark;			// !!
```

## Eliminar

```c++
// <NombreMap>.erase(<clave>)
Map.erase('C');

// Para borrar todo:
Map.clear():
```

## Información

```c++
Map.empty();					// Vacio?
Map.size();
Map.find(clave);				// iterator o, en caso de no encontrarlo, .end()
```

- Además cuenta con los clásicos métodos: _lower_bound_, _count_, entre otros.

## Iterar

Se utiliza el clásico _ranged for loop_.

```c++
for(auto& x : Map)
{
    cout << x.first << " " << x.second << endl;
}
```

