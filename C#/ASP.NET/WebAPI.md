# Indice
- [Indice](#indice)
- [Crear el proyecto](#crear-el-proyecto)
- [Algunos elementos del proyecto](#algunos-elementos-del-proyecto)
- [Test](#test)
- [*.NET REPL*](#net-repl)
  - [Instalacion](#instalacion)
  - [Conectarse a la *WebAPI*](#conectarse-a-la-webapi)
  - [Explorar los directorios](#explorar-los-directorios)
  - [Requests](#requests)
  - [Salir](#salir)
- [Analizando el controlador por defecto](#analizando-el-controlador-por-defecto)
    - [`ControllerBase`](#controllerbase)
    - [`Attributes`](#attributes)
    - [`Actions`](#actions)
- [*Models*](#models)
  - [Creando un modelo](#creando-un-modelo)
- [*Services*](#services)
  - [Creando un servicio](#creando-un-servicio)
- [*Controllers*](#controllers)
  - [Creando un controlador](#creando-un-controlador)
  - [Implementando `GET`](#implementando-get)
    - [`GET` multiple](#get-multiple)
    - [`GET` single](#get-single)
    - [Posibles respuestas (`ActionResult`)](#posibles-respuestas-actionresult)
      - [`OK`](#ok)
      - [`NotFound`](#notfound)
  - [Implementando `POST`](#implementando-post)
    - [Posibles respuestas (`IActionResult`)](#posibles-respuestas-iactionresult)
      - [`CreatedAtAction`](#createdataction)
      - [`BadRequest`](#badrequest)
  - [Implementando `PUT`](#implementando-put)
    - [Posibles respuestas (`IActionResult`)](#posibles-respuestas-iactionresult-1)
      - [`NoContent`](#nocontent)
      - [`BadRequest`](#badrequest-1)
  - [Implementando `DELETE`](#implementando-delete)
    - [Posibles respuestas (`IActionResult`)](#posibles-respuestas-iactionresult-2)
      - [`NoContent`](#nocontent-1)
      - [`NotFound`](#notfound-1)
  - [Probando el controlador](#probando-el-controlador)
    - [`GET`](#get)
      - [Single `GET`](#single-get)
      - [`GET` mediante `id` existente](#get-mediante-id-existente)
      - [`GET` mediante `id` NO existente](#get-mediante-id-no-existente)
    - [`POST`](#post)
    - [`PUT`](#put)
      - [Verificando que se haya agregado correctamente](#verificando-que-se-haya-agregado-correctamente)
    - [`DELETE`](#delete)

# Crear el proyecto
```ps1
dotnet new webapi
```
# Algunos elementos del proyecto
- `Controllers`: contiene las clases con los métodos *HTTP*. **GET, POST,** etc.
- `Program.cs`: entry point.

# Test
```ps1
dotnet run 
```
- En la consola se mostrará la dirección con el puerto correspondiente.
- En caso de tener otras páginas se puede hacer: `https://localhost:{PORT}/nombrePagina`

# *.NET REPL*
## Instalacion
```ps1
dotnet tool install -g Microsoft.dotnet-httprepl
```
## Conectarse a la *WebAPI*
```ps1
httprepl https://localhost:{PORT}
```
O ya dentro del *REPL*:
```ps1
connect https://localhost:{PORT}
```
## Explorar los directorios
Se pueden usar los clasicos comandos `ls` y `cd`

## Requests
```ps1
get
```
Un ejemplo de una response:
```json
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Date: Fri, 02 Apr 2021 17:31:43 GMT
Server: Kestrel
Transfer-Encoding: chunked
[
  {
    "date": 4/3/2021 10:31:44 AM,
    "temperatureC": 13,
    "temperatureF": 55,
    "summary": "Sweltering"
  },
  {
    "date": 4/4/2021 10:31:44 AM,
    "temperatureC": -13,
    "temperatureF": 9,
    "summary": "Warm"
  },
  // ..
]
```
## Salir
```ps1
exit
``` 
# Analizando el controlador por defecto
```csharp
using Microsoft.AspNetCore.Mvc;

namespace ContosoPizza.Controllers;

[ApiController]
[Route("[controller]")]
public class WeatherForecastController : ControllerBase
{
    private static readonly string[] Summaries = new[]
    {
        "Freezing", "Bracing", "Chilly", "Cool", "Mild", "Warm", "Balmy", "Hot", "Sweltering", "Scorching"
    };

    private readonly ILogger<WeatherForecastController> _logger;

    public WeatherForecastController(ILogger<WeatherForecastController> logger)
    {
        _logger = logger;
    }

    [HttpGet(Name = "GetWeatherForecast")]
    public IEnumerable<WeatherForecast> Get()
    {
        return Enumerable.Range(1, 5).Select(index => new WeatherForecast
        {
            Date = DateTime.Now.AddDays(index),
            TemperatureC = Random.Shared.Next(-20, 55),
            Summary = Summaries[Random.Shared.Next(Summaries.Length)]
        })
        .ToArray();
    }
}
```
### `ControllerBase`
Un controlador es una clase pública con uno o más métodos denominados ***actions***. Esta clase hereda de `ControllerBase`, está ultima contiene un conjunto de funcionalidades estandar para manejar las requests.
<br>
Las *actions* corresponden a los endpoints HTTP. Por ejemplo: una request `GET` a `https://localhost:{PORT}/weatherforecast` ejecutaría el método `Get()` de la clase `WeatherForecastController`.

### `Attributes`
```csharp
[ApiController]
[Route("[controller]")]
public class WeatherForecastController : ControllerBase
```
- `[ApiController]`: enables opinionated behaviors that make it easier to build web APIs.
- `[Route]` defines the routing pattern `[controller]`. The `[controller]` token is replaced by the controller's name (case-insensitive, without the Controller suffix).
  - `[Route]` puede contener un path. Ejemplo: `api/[controller]`

### `Actions`
Para este caso hay una sola acción:
```csharp
[HttpGet(Name = "GetWeatherForecast")]
public IEnumerable<WeatherForecast> Get()
```
- Este atributo relaciona la request `GET` con el método `Get()`.

# *Models*
Antes de empezar a implementar la API es necesario contar con un "contenedor" al cual se le puedan realizar distintas operaciones.
<br>
Para el ejemplo del proyecto *ContosoPizza* un modelo necesario sería el de una **Pizza**.
- Este modelo representaria las caracteristicas y una pizza.

## Creando un modelo
Continuando con el ejemplo de la pizza.
```ps1
ni Models -ItemType Directory
ni Pizza.cs
```
Dentro de `Pizza.cs` se define dicha clase, una posible implementación serpia la siguiente:
```csharp
namespace ContosoPizza.Models;

public class Pizza
{
    public int Id { get; set; }
    public string? Name { get; set; }
    public bool IsGlutenFree { get; set; }
}
```
# *Services*
Para practicar se puede usar un servicio local denominado `in-memory caching service`. En el mundo real es recomendable utilizar una base de datos como por ejemplo *SQL Server* con el *Entity Framework Core*.
## Creando un servicio
Continuando con el ejemplo de las pizzas:
```ps1
ni Services -ItemType Directory
ni PizzaService.cs
```
Una posible implementación sería:
```csharp
using ContosoPizza.Models;

namespace ContosoPizza.Services;

public static class PizzaService
{
    static List<Pizza> Pizzas { get; }
    static int nextId = 3;
    static PizzaService()
    {
        Pizzas = new List<Pizza>
        {
            new Pizza { Id = 1, Name = "Classic Italian", IsGlutenFree = false },
            new Pizza { Id = 2, Name = "Veggie", IsGlutenFree = true }
        };
    }

    public static List<Pizza> GetAll() => Pizzas;

    public static Pizza? Get(int id) => Pizzas.FirstOrDefault(p => p.Id == id);

    public static void Add(Pizza pizza)
    {
        pizza.Id = nextId++;
        Pizzas.Add(pizza);
    }

    public static void Delete(int id)
    {
        var pizza = Get(id);
        if(pizza is null)
            return;

        Pizzas.Remove(pizza);
    }

    public static void Update(Pizza pizza)
    {
        var index = Pizzas.FindIndex(p => p.Id == pizza.Id);
        if(index == -1)
            return;

        Pizzas[index] = pizza;
    }
}
```

# *Controllers*
Recordando: un controlador es una clase con métodos conocidos como *acciones*. Estas acciones corresponden a los *HTTP endpoints*.
## Creando un controlador
Continuando con el ejemplo de las pizzas.
```ps1
sl Controllers
ni PizzaController.cs
```
- Por convención los controladores tienen como sufijo *Controller*.

Una plantilla posible sería la siguiente:
```csharp
using ContosoPizza.Models;
using ContosoPizza.Services;
using Microsoft.AspNetCore.Mvc;

namespace ContosoPizza.Controllers;

[ApiController]
[Route("[controller]")]
public class PizzaController : ControllerBase
{
    public PizzaController()
    {
    }

    // GET all action

    // GET by Id action

    // POST action

    // PUT action

    // DELETE action
}
```
Recordando algunos atributos:
- `[Route]` define el mapeo al token `[controller]`.
  - Como la clase se llama `PizzaController`, este controlador se encargara de la request `https://localhost:{PORT}/pizza`.

## Implementando `GET`
### `GET` multiple
```csharp
[HttpGet]
public ActionResult<List<Pizza>> GetAll() => PizzaService.GetAll();
```
- Con esta request el cliente puede obtener todas las pizzas a partir de la API.

### `GET` single
```csharp
[HttpGet("{id}")]
public ActionResult<Pizza> Get(int id)
{
    var pizza = PizzaService.Get(id);

    if(pizza == null)
        return NotFound();

    return pizza;
}
```

- Responds only to the HTTP `GET` verb, as denoted by the `[HttpGet]` attribute.
- Requires that the id parameter's value is included in the URL segment after `pizza/`. Remember, the controller-level `[Route]` attribute defined the `/pizza` pattern.
- Queries the database for a pizza that matches the provided `id` parameter.

### Posibles respuestas (`ActionResult`)
#### `OK`
- `HTTP status code: 200`

Se daría cuando existe un producto con el `id` brindado en la request.

#### `NotFound`
- `HTTP status code 404`
  
Se daría cuando se pide un producto con un `id` no existente en memoria.

## Implementando `POST`
Para que un usuario pueda agregar un nuevo item se tiene que implementar la accion `POST` usando el atributo `[HttpPost]`.
<br>
Una vez que se pasa un item como parámetro *ASP.NET Core* lo convierte automaticamente a un **objeto**, siguiendo el ejemplo previo sería a una pizza.
```csharp
[HttpPost]
public IActionResult Create(Pizza pizza)
{            
    PizzaService.Add(pizza);
    return CreatedAtAction(nameof(Create), new { id = pizza.Id }, pizza);
}
```
- The first parameter in the `CreatedAtAction` method call represents an action name. The `nameof` keyword is used to avoid hard-coding the action name. `CreatedAtAction` uses the action name to generate a location HTTP response header with a URL to the newly created pizza, as explained in the previous unit.

### Posibles respuestas (`IActionResult`)
#### `CreatedAtAction`
- `HTTP status code: 201`

Se daría cuando la pizza se agrega sin problemas.

#### `BadRequest`
- `HTTP status code: 400`

Se daría cuando el cuerpo del objeto `pizza` no es válido.


## Implementando `PUT`
Para modificar un elemento, en este caso una pizza, es necesario implementar la acción `PUT` con el atributo `[HttpPut]`, el cual tomara como parámetro un `id`. La accion contara con los parametros `id` y un objeto `pizza` a modificar.
```csharp
[HttpPut("{id}")]
public IActionResult Update(int id, Pizza pizza)
{
    if (id != pizza.Id)
        return BadRequest();
           
    var existingPizza = PizzaService.Get(id);
    if(existingPizza is null)
        return NotFound();
   
    PizzaService.Update(pizza);           
   
    return NoContent();
}
```
- Requires that the `id` parameter's value is included in the URL segment after `pizza/`.
- Returns `IActionResult` because the `ActionResult` return type isn't known until *runtime*. The `BadRequest`, `NotFound`, and `NoContent` methods return `BadRequestResult`, `NotFoundResult`, and `NoContentResult` types, respectively.

### Posibles respuestas (`IActionResult`)
#### `NoContent`
- `HTTP status code: 204`

Se daría cuando la pizza pudo modificarse correctamente.

#### `BadRequest`
- `HTTP status code: 400`

Se daría cuando los `id` no *matchean*. O cuando el cuerpo del objeto `pizza` no es válido.


## Implementando `DELETE`
Para que el usuario pueda eliminar una pizza dado un `id` es necesario implementar la acción `DELETE` con el atributo `[HttpDelete]`.
```csharp
[HttpDelete("{id}")]
public IActionResult Delete(int id)
{
    var pizza = PizzaService.Get(id);
   
    if (pizza is null)
        return NotFound();
       
    PizzaService.Delete(id);
   
    return NoContent();
}   
```

### Posibles respuestas (`IActionResult`)
#### `NoContent`
- `HTTP status code: 204`

Se daría cuando la pizza se pudo eliminar correctamente.

#### `NotFound`
- `HTTP status code: 404`

Se daría cuando el `id` brindado no existe.

## Probando el controlador
```ps1
httprepl https://localhost:{PORT}
ls
cd Pizza
```

### `GET`
#### Single `GET`
```ps1
get
```
Respuesta:
```json
HTTP/1.1 200 OK
  Content-Type: application/json; charset=utf-8
  Date: Fri, 02 Apr 2021 21:55:53 GMT
  Server: Kestrel
  Transfer-Encoding: chunked

  [
      {
          "id": 1,
          "name": "Classic Italian",
          "isGlutenFree": false
      },
      {
          "id": 2,
          "name": "Veggie",
          "isGlutenFree": true
      }
  ]
```

#### `GET` mediante `id` existente
```ps1
get 1
```
Respuesta:
```json
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Date: Fri, 02 Apr 2021 21:57:57 GMT
Server: Kestrel
Transfer-Encoding: chunked

{
    "id": 1,
    "name": "Classic Italian",
    "isGlutenFree": false
}
```

#### `GET` mediante `id` NO existente
```ps1
get 5
```
Respuesta
```json
HTTP/1.1 404 Not Found
Content-Type: application/problem+json; charset=utf-8
Date: Fri, 02 Apr 2021 22:03:06 GMT
Server: Kestrel
Transfer-Encoding: chunked

{
    "type": "https://tools.ietf.org/html/rfc7231#section-6.5.4",
    "title": "Not Found",
    "status": 404,
    "traceId": "00-ec263e401ec554b6a2f3e216a1d1fac5-4b40b8023d56762c-00"
}
```

### `POST`
```ps1
post -c "{"name":"Hawaii", "isGlutenFree":false}"
```
Respuesta:
```json
HTTP/1.1 201 Created
Content-Type: application/json; charset=utf-8
Date: Fri, 02 Apr 2021 23:23:09 GMT
Location: https://localhost:{PORT}/Pizza?id=3
Server: Kestrel
Transfer-Encoding: chunked

{
    "id": 3,
    "name": "Hawaii",
    "isGlutenFree": false
}
```

### `PUT`
```ps1
put 3 -c  "{"id": 3, "name":"Hawaiian", "isGlutenFree":false}"
```
Respuesta:
```json
HTTP/1.1 204 No Content
Date: Fri, 02 Apr 2021 23:23:55 GMT
Server: Kestrel
```
#### Verificando que se haya agregado correctamente
```ps1
get 3
```
Respuesta:
```json
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Date: Fri, 02 Apr 2021 23:27:37 GMT
Server: Kestrel
Transfer-Encoding: chunked

{
    "id": 3,
    "name": "Hawaiian",
    "isGlutenFree": false
}
```

### `DELETE`
```ps1
delete 3
```
Respuesta:
```json
HTTP/1.1 204 No Content
Date: Fri, 02 Apr 2021 23:30:04 GMT
Server: Kestrel
```