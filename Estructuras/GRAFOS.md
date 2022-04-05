# Definición

A grandes rasgos un __grafo__ es un conjunto de ___nodos___ y ___aristas___.

## Nodos

Visualizar a un _nodo_ como a un circulo con información dentro.

<img src="..\img\image-20211129135808416.png" alt="image-20211129135808416" style="zoom:67%;" />

## Aristas

Son las __conexiones__ entre _nodos_.

<img src="..\img\image-20211129135859467.png" alt="image-20211129135859467" style="zoom:65%;" />

# Tipos

![image-20211129140151496](..\img\image-20211129140151496.png)

- Los grafos _dirigidos_ tienen __aristas__ con __direcciones__. Ej: _a -> c_
  - Si estoy en el _nodo c_ no puedo volver hacia el _nodo a_.

# Terminología

Siguiendo con el mismo _grafo dirigido_ previo. 

- Si estoy en el _nodo a_, el _nodo b_ y _nodo c_ son __vecinos__. Ya que son __accesibles__ desde el _nodo a_.
- Que un __grafo__ sea __dirigido__ se refiere a que las _aristas_ tienen __sentido__.
- Una ___arista___ es una __conexión__ entre ___nodos___.
- Un __ciclo__ en recorrido entre ___nodos___ que termine en la posición donde comenzó.
- Un __grafo acíclico__ es uno que no tiene __ciclos__.

# Implementación

## Lista de adyacencias

La implementación varia según el lenguaje, generalmente se utiliza un __Hash Map__.

```json
{
    a: [b, c],
    d: [d],
    c: [e],
    d: [],
    e: [b],
    f: [d]
}
```

- En ___C++___ se utilizaría `std::unordered_map`.
- En ___Ruby___ se utilizaría `Hash`.
- En ***C#*** se utilizaría `Dictionary`.

Las claves serían los **nodos** mientras que los valores serían una _array_ que represente los __vecinos__ del nodo en cuestión.

Es importante aclarar que a pesar de que un _nodo_ no tenga vecinos, __debe tener su propia clave dentro del _Hash Map___. Ver _d_.

### *C++*

```cpp
#include <unordered_map>
#include <vector>
template<typename T1, typename T2>
using GRAFO = std::unordered_map<T1, std::vector<T2>>;

int main()
{
    GRAFO g = 
    {
        {'a', {'c', 'b'}},
        {'b', {'d'}},
        {'c', {'e'}},
        {'d', {'f'}},
        {'e', {}},
        {'f', {}}
    };
}
```

### *C#*

```csharp
public class Grafo<T_NODO> where T_NODO: notnull
{
    public readonly Dictionary<T_NODO, List<T_NODO>> Elementos;    

    public Grafo(Dictionary<T_NODO, List<T_NODO>> grafo)
    {
        this.Elementos = grafo;
    }
}
```

# Recorrido

## Teoría

### _DFS_

_Depth first search_.

1. Elijo mi _nodo inicial_.

<img src="..\img\image-20211129154903975.png" alt="image-20211129154903975" style="zoom:80%;" />

2. Sigo con los _vecinos_, puedo ir a cualquiera.

<img src="..\img\image-20211129155104714.png" alt="image-20211129155104714" style="zoom:80%;" />

3. Sigo __en profundidad__ con el recorrido, tengo que ir al ___nodo d___ antes del ___nodo c___.

<img src="..\img\image-20211129160201462.png" alt="image-20211129160201462" style="zoom:80%;" />

4. Como ya termine el recorrido en profundidad correspondiente al __vecino b__, debo hacer lo mismo con el __vecino c__.

<img src="..\img\image-20211129160330208.png" alt="image-20211129160330208" style="zoom:80%;" />

5. Como se puede ver, en este caso se hará un recorrido duplicado ya que ya se recorrió el _nodo b_, esto es completamente normal en este algoritmo.

<img src="..\img\image-20211129160504221.png" alt="image-20211129160504221" style="zoom:80%;" />

- También es normal que queden _nodos_ sin recorrer, ya que, al ser un _grafo dirigido_ existe esa posibilidad.

### _BFS_

_Breadth first search_.

1. Elijo mi _nodo inicial_.

<img src="..\img\image-20211129160739568.png" alt="image-20211129160739568" style="zoom:80%;" />

2. Sigo con los _vecinos_, puedo ir a cualquiera primero.

<img src="..\img\image-20211129160814523.png" alt="image-20211129160814523" style="zoom:80%;" />

3. A diferencia de _DFS_ en este algoritmo tengo que continuar con el otro __vecino__ del _nodo_ anterior, en este caso del _nodo inicial_.

<img src="..\img\image-20211129160928510.png" alt="image-20211129160928510" style="zoom:80%;" />

4. Luego se repite el algoritmo hasta llegar al final.

### Diferencias entre _DFS_ y _BFS_

La diferencia se puede visualizar claramente cuando se trata de recorrer un _grafo_ bastante grande.

Comenzando por ___DFS___ el recorrido se vería así:

- Se arranca eligiendo un _nodo inicial_ y una dirección, por ejemplo hacia la derecha.

<img src="..\img\image-20211129161243959.png" alt="image-20211129161243959" style="zoom:80%;" />

- Se mantiene esa dirección hasta llegar al límite, una vez ahí elijo otra distinta y aplico la misma regla.

<img src="..\img\image-20211129161428191.png" alt="image-20211129161428191" style="zoom:80%;" />

- Se sigue con el algoritmo hasta el final.

<img src="..\img\image-20211129161525742.png" alt="image-20211129161525742" style="zoom:80%;" />

En cambio el algoritmo ___BFS___ se vería de la siguiente manera:

- Se arranca con el _nodo inicial_ y sus __vecinos__.

<img src="..\img\image-20211129161653703.png" alt="image-20211129161653703" style="zoom:80%;" />

- Luego con los __vecinos__ de los __vecinos__ hasta llegar al final.

<img src="..\img\image-20211129161733092.png" alt="image-20211129161733092" style="zoom:80%;" />

- Como se puede ver, el recorrido del ___BFS___ tiene a mantener un balance en las direcciones que explora. A diferencia de ___DFS___ que tiene a favorecer una dirección.

## Implementación con ayuda visual

A grandes rasgos la gran diferencia a la hora de implementar ambos algoritmos es que:

- ___DFS___ utiliza un __Stack__.
- ___BFS___ utiliza una __Queue__.

### _DFS_

<img src="..\img\image-20211129162420984.png" alt="image-20211129162420984" style="zoom:80%;" />

- Mantengo en una variable el _nodo actual_, obtenido mediante ___pop()___.

<img src="..\img\image-20211129162545094.png" alt="image-20211129162545094" style="zoom:80%;" />

- Agrego los __vecinos__ al _stack_.

<img src="..\img\image-20211129162621179.png" alt="image-20211129162621179" style="zoom:80%;" />

- ___pop()___ y piso el valor _current_.

<img src="..\img\image-20211129162738171.png" alt="image-20211129162738171" style="zoom:80%;" />

- Nuevamente agrego los __vecinos__ de _current_ al _stack_.

<img src="..\img\image-20211129162837768.png" alt="image-20211129162837768" style="zoom:80%;" />

- Continuo aplicando el mismo algoritmo

<img src="..\img\image-20211129162921168.png" alt="image-20211129162921168" style="zoom:80%;" />

<img src="..\img\image-20211129163045531.png" alt="image-20211129163045531" style="zoom:80%;" />

<img src="..\img\image-20211129163117362.png" alt="image-20211129163117362" style="zoom:80%;" />

<img src="..\img\image-20211129163134603.png" alt="image-20211129163134603" style="zoom:80%;" />

<img src="..\img\image-20211129163154872.png" alt="image-20211129163154872" style="zoom:80%;" />

<img src="..\img\image-20211129163213975.png" alt="image-20211129163213975" style="zoom:80%;" />

- El algoritmo termina cuando __el _stack_ está vacío__.

### _BFS_

<img src="..\img\image-20211129163428980.png" alt="image-20211129163428980" style="zoom:80%;" />

- El __primero__ en la _cola_ se vuelve el _current_.

<img src="..\img\image-20211129163505712.png" alt="image-20211129163505712" style="zoom:80%;" />

- Agrego los __vecinos__ a la _cola_.

<img src="..\img\image-20211129163552142.png" alt="image-20211129163552142" style="zoom:80%;" />

- El primero en la _cola_ pisa nuevamente a _current_.

<img src="..\img\image-20211129163634570.png" alt="image-20211129163634570" style="zoom:80%;" />

- Agrego los __vecinos__.

<img src="..\img\image-20211129163711933.png" alt="image-20211129163711933" style="zoom:80%;" />

- Continuo con el algoritmo hasta terminar.

<img src="..\img\image-20211129163747839.png" alt="image-20211129163747839" style="zoom:80%;" />

<img src="..\img\image-20211129163803964.png" alt="image-20211129163803964" style="zoom:80%;" />

<img src="..\img\image-20211129163821552.png" alt="image-20211129163821552" style="zoom:80%;" />

<img src="..\img\image-20211129163828791.png" alt="image-20211129163828791" style="zoom:80%;" />

<img src="..\img\image-20211129163846859.png" alt="image-20211129163846859" style="zoom:80%;" />

<img src="..\img\image-20211129163902966.png" alt="image-20211129163902966" style="zoom:80%;" />

## Implementación en _Ruby_

### _DFS_

Hay 2 formas, 1 que es la básica y otra que usa recursividad.

#### Forma básica

Ejemplo imprimiendo los nodos:

```ruby
nodo_inicial = grafo.keys.first
def print_dfs(grf, first)
  stack = [first]
  until stack.empty?
    current = stack.pop
    puts current
    grf[current.to_sym].each { |vecino| stack.push(vecino) }
  end
end
```

##### Paso a paso

1. Se obtiene el la __clave__ del _nodo inicial_.
2. Se crea el ___Stack___ con la __clave__ inicial.
3. Se _loopea_ hasta el final del algoritmo, que como se dijo anteriormente, es cuando el _stack_ está vacío.
4. Se guarda el valor del ___pop()___ en _current_.
5. Se realiza la operación deseada en _current_, en este caso imprimir.
6. Se agregan __todos los vecinos__ de _current_ al _stack_.

#### Forma recursiva

```ruby
nodo_inicial = grafo.keys.first
def print_dfs_recursivo(grf, current)
  puts current
  grf[current.to_sym].each { |vecino| print_dfs_recursivo(grf, vecino) }
end
```

- No requiere __condición de corte explícita__,  ya que termina automáticamente cuando se llega a una __clave__ que no tenga __valores__.
- No aplicable de la misma manera en ***C++***.

##### Paso a paso

1. Se realiza la operación deseada con _current_ el cual, inicialmente, será el _nodo inicial_.
2. Se itera en los __vecinos__ de _current_ llamando a la misma función, salvo que, a diferencia del comienzo, en este caso el _current_ será el __vecino__.

### _BFS_

```ruby
nodo_inicial = grafo.keys.first
def print_bfs(grf, first)
  queue = Queue.new
  queue << first
  until queue.empty?
    current = queue.pop
    puts current
    grf[current.to_sym].each { |vecino| queue << vecino }
  end
end
```

#### Paso a paso

1. Se obtiene el la __clave__ del _nodo inicial_.
2. Se crea la ___Queue___ con la __clave__ inicial.
3. Se _loopea_ hasta que la _queue_ esté vacía.
4. Se guarda el valor del ___pop()___ en _current_.
5. Se realiza la operación deseada en _current_, en este caso imprimir.
6. Se agregan __todos los vecinos__ de _current_ a la _queue_.

## Implementación en *C++*

### *DFS*

Ejemplo para un grafo como el del ejemplo previo:

```cpp
#include <stack>
#include <unordered_map>
#include <vector>

using GRAFO = std::unordered_map<char, std::vector<char>>;
using STACK = std::stack<char>;

void DFS(GRAFO& g, char first)
{
    STACK stack;
    stack.push(first);
    while(!stack.empty())
    {
        char current = stack.top();
        stack.pop();
        cout << current << " ";
        for(auto& vecino : g[current])
            stack.push(vecino);
    }
}
```

- Como se puede ver, a diferencia de *Ruby* el método `pop` en *C++* no devuelve el elemento eliminado. Por lo tanto previamente a eliminarlo hay que usar `top`.

### *BFS*

Ejemplo para un grafo como el del ejemplo:

```cpp
#include <queue>
#include <unordered_map>
#include <vector>

using GRAFO = std::unordered_map<char, std::vector<char>>;
using QUEUE = std::queue<char>;

void BFS(GRAFO& g, char primero)
{
    QUEUE queue;
    queue.push(primero);
    while(!queue.empty())
    {
        char current = queue.front();
        queue.pop();
        cout << current << " ";
        for(auto& vecino : g[current])
            queue.push(vecino);
    }
}
```

- Al igual que con ***DFS***, el método `pop` no devuelve nada, por lo que hay utilizar primero `front`.

## Implementación en *C#*

### *DFS*

```csharp
public void DFS(T_NODO primerElemento, Action<T_NODO> operacion)
{
    Stack<T_NODO> stack = new();
    stack.Push(primerElemento);
    while(stack.Count != 0)
    {
        var current = stack.Pop();
        operacion(current);
        foreach(var vecino in this.Elementos[current])
            stack.Push(vecino);
    }
}
```

- `Action`: pensarlo como el equivalente al `std::function` de *C++* con un return ***void***.
- Sería una implementación génerica para operaciones muy básicas. Para hacerlo mas robusto se me ocurre un overload en el que se use `Func`.

### *BFS*

```csharp
public void BFS(T_NODO primerElemento, Action<T_NODO> operacion)
{
    Queue<T_NODO> queue = new();
    queue.Enqueue(primerElemento);
    while(queue.Count != 0)
    {
        var current = queue.Dequeue();
        operacion(current);
        foreach(var vecino in this.Elementos[current])
            queue.Enqueue(vecino);
    }
}
```

# Métodos comunes para grafos

## ¿Hay recorrido?

### Teoría

<img src="..\img\image-20211129204848386.png" alt="image-20211129204848386" style="zoom:80%;" />

- Se necesitan 3 parámetros ___grafo___, ___origen___ y ___destino___. En caso de que se trabaje con clases el parámetro del grafo no es necesario.
- El objetivo es __verificar si existe un recorrido desde _origen_ hasta _destino___.
- Para implementarlo se puede utilizar tanto ___DFS___ como ___BFS___.

Como ejemplo se utilizará ___DFS___:

<img src="..\img\image-20211129205227328.png" alt="image-20211129205227328" style="zoom:80%;" />

1. Se recorre el _grafo_.
2. __Se evalúa si el _grafo_ actual es el _destino___.
3. Se continúa hasta encontrar el _destino_ o hasta que termine el algoritmo.
4. Se devuelve el ___bool___.

<img src="..\img\image-20211129205543776.png" alt="image-20211129205543776" style="zoom:80%;" />

### Complejidad espacial y temporal

Sea ___n = nodos___ y ___e = aristas___:

- Complejidad temporal: __O(e)__.
- Complejidad espacial: __O(n)__.

### Implementación en _Ruby_

#### _DFS_

```ruby
def hay_recorrido_dfs?(grafo, origen, destino)
  return true if origen == destino

  grafo[origen.to_sym].each { |vecino| return true if hay_recorrido_dfs?(grafo, vecino, destino) }
  false
end
```

#### _BFS_

```ruby
def hay_recorrido_bfs?(grafo, origen, destino)
  queue = Queue.new
  queue << origen
  until queue.empty?
    current = queue.pop
    return true if current == destino

    grafo[current.to_sym].each { |vecino| queue << vecino }
  end
end
```

### Implementación en *C#*

```csharp
public bool HayRecorrido(T_NODO origen, T_NODO destino)
{
    Stack<T_NODO> stack = new();
    stack.Push(origen);
    while(stack.Count != 0)
    {
        var current = stack.Pop();
        if(EqualityComparer<T_NODO>.Default.Equals(current, destino)) return true;s

        foreach(var vecino in this.Elementos[current])
            stack.Push(vecino);
    }
    return false;
}
```

- En este caso se está utilizando el algoritmo **DFS**.

- Se utiliza `EqualityComparer<>` ya que *C#* no permite utilizar el operador **=** con *Generics*.
