# Indice
- [Indice](#indice)
  - [Ejemplo básico](#ejemplo-básico)
    - [Forma 1:](#forma-1)
    - [Forma 2:](#forma-2)
      - [Importante](#importante)
  - [Variables](#variables)
  - [Caracteres](#caracteres)
  - [Rangos](#rangos)
    - [Atajos a rangos usuales](#atajos-a-rangos-usuales)
      - [Variantes negativas](#variantes-negativas)
    - [Importante](#importante-1)
  - [Modificadores](#modificadores)
  - [Búsqueda exacta](#búsqueda-exacta)
  - [Condicionales](#condicionales)
  - [Capturar grupos](#capturar-grupos)
    - [_Non-capturing groups_](#non-capturing-groups)
      - [_Named groups_](#named-groups)
  - [Mirar atrás y adelante](#mirar-atrás-y-adelante)
  - [Clase _REGEX_](#clase-regex)
  - [Flags _REGEX_](#flags-regex)
    - [Uso](#uso)
  - [Formato de _REGEX_](#formato-de-regex)
  - [_Ruby REGEX_](#ruby-regex)
    - [_.scan_](#scan)
    - [_.gsub_](#gsub)
    - [Etc](#etc)

## Ejemplo básico

### Forma 1:

```ruby
# Busca la palabra gatos
"Te gustan los gatos?" =~ /gatos/
```

- Devuelve el __índice__ de la palabra buscada.
- Devuelve ___nil___ en caso de no encontrar la palabra.

### Forma 2:

Otra forma de realizar la misma búsqueda es utilizando el método ___match___:

```ruby
if "Te gustan los gatos?".match(/gatos/)
    puts 'Exito'
end
```

#### Importante

El método ___match___ devuelve un objeto _MatchData_. Esta clase tiene [métodos](https://ruby-doc.org/core-2.4.0/MatchData.html) bastante útiles. Si solo interesa el valor __booleano__ de la expresión hay que utilizar ___match?___.

## Variables

```ruby
var = "Value"
str = "a test Value"
p str.gsub( /#{var}/, 'foo' )   # => "a test foo"
```

## Caracteres

Una _character class_ permite definir un __rango__ o una __lista__ de caracteres a buscar. Por ejemplo __[aeiou]__ para buscar las vocales.

```ruby
def tiene_vocal(str)
    str =~ /[aeiou]/
end

tiene_vocal("test")		# => 1
tiene_vocal("xd")		# => nil
```

## Rangos

Se pueden usar rangos a la hora de buscar múltiples caracteres. Esto evita que se tenga que escribir los caracteres uno por uno. Ejemplos:

- __[0-9]__: números del 0 al 9.
- __[a-z]__: letras de la _a_ a la _z_ (minúsculas).
- __[A-Z]__: letras de la _a_ a la _z_ (mayúsculas).
- __[^a-z]__: _negated range_.

Ejemplo:

```ruby
def tiene_numero(str)
    str =~ /[0-9]/
end

tiene_numero("The year is 2015")		# => 12
tiene_numero("Test")					# => nil
```

### Atajos a rangos usuales

- __\w__: es equivalente a __[0-9a-zA-Z_]__.
- __\d__: es equivalente a __[0-9].__
- __\s__: es equivalente a cualquier tipo de _espacio_ (__tabs, espacio, newline__).

#### Variantes negativas

- __\W__: todo lo que no sea __[0-9a-zA-Z_].__
- __\D__: todo lo que no sea un __número__.
- __\S__: todo lo que no sea un __espacio__.

### Importante

El punto __.__ simboliza __todo sin contar newlines__. Por lo tanto, si se quiere buscar el punto literal, va a ser necesario usar el caracter de escape. 

## Modificadores

- __+__: 1 o más.
- __*__: 0 o más.
- __?__: 0 o 1. 
- __.__: todo excepto _newline_.
- __{n}__: n veces. Se puede utilizar para restringir la cantidad de caracteres que tiene una cadena. Ej: __/w{4}/__.
- __{n,}__: n o más.
- __{n,m}__: n a m veces.[]

Ejemplo: evaluar si corresponde a una dirección IP.

```ruby
# Note that this will also match some invalid IP address
# like 999.999.999.999, but in this case we just care about the format.

def ip_address?(str)
  # We use !! to convert the return value to a boolean
  !!(str =~ /^\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}$/)
end

ip_address?("192.168.1.1")  # returns true
ip_address?("0000.0000")    # returns false
```

## Búsqueda exacta

Se utilizan los modificadores:

- __^__: _beginning of line_.
- __$__: _end of line_.

```ruby
# We want to find if this string is exactly four letters long, this will
# still match because it has more than four, but it's not what we want.
"Regex are cool".match /\w{4}/

# Instead we will use the 'beginning of line' and 'end of line' modifiers
"Regex are cool".match /^\w{4}$/

# This time it won't match. This is a rather contrived example, since we could just
# have used .size to find the length, but I think it gets the idea across.
```

## Condicionales

```ruby
"Te gustan los gatos?" =~ /gatos|perros|aves/
```

## Capturar grupos

Se puede capturar una parte de un _match_ para usarlo más tarde. Para capturar se encierra el elemento entre paréntesis __( )__. Ejemplo: parseando un _LOG FILE_.

```ruby
Line = Struct.new(:time, :type, :msg)
LOG_FORMAT = /(\d{2}:\d{2}) (\w+) (.*)/
def parse_line(line)
  line.match(LOG_FORMAT) { |m| Line.new(*m.captures) }
end
parse_line("12:41 INFO User has logged in.")
```

Como se aclaró el inicio del documento, el método ___match___ devuelve un objeto _MatchData_. 

- Los elementos capturados se acceden mediante el método ___capture___.
- Si no se quiere usar ___capture___ se puede tratar al objeto ___MatchData___ como un __Array__, donde el índice __[0]__ corresponde al __match entero__, y los siguientes índices a los __match capturados__.

Ejemplo:

```ruby
m = "John 31".match /\w+ (\d+)/
m[1]								# => 31
```

### _Non-capturing groups_

Permiten agrupar expresiones sin perder performance.

- __(?: ...)__

#### _Named groups_

- __(?\<nombre>...)__

Ejemplo:

```ruby
m = "David 30".match(/(?<name>\w+) (?<age>\d+)/)
m[:age]			# => 30
m[:name]		# => "David"
```

## Mirar atrás y adelante

El motor de _Regex_ de _Ruby_ permite revisar si hay un ___match___ antes o después:

- __(?=pat)__: _positive lookahead_.
- __(?<=pat)__: _positive lookbehind_.
- __(?!pat)__: _negative lookahead_.
- __(?<!pat)__: _negative lookbehind_.

Ejemplo: hay algún número que tenga al menos una letra antes?

```ruby
def number_after_word?(str)
  !!(str =~ /(?<=\w) (\d+)/)
end
number_after_word?("Grade 99")
```

## Clase _REGEX_

Las expresiones regulares en _Ruby_ son instancias de la clase ___Regexp___. Si bien la clase no se usa directamente es bueno saberlo.

Un posible uso es cuando se quiere crear un ___regex___ desde una ___string___:

```ruby
regexp = Regexp.new("a")
```

Otra forma:

```ruby
regexp = %r{\w+}
```

## Flags _REGEX_

- __g__: global. Busca en toda la _string_ (no solo primera ocurrencia).
- __i__: case insensitive.
- __m__: _multiline_.
- __x__: ignore _whitespace_.

### Uso

```ruby
"abc".match?(/[A-Z]/i)
```

## Formato de _REGEX_

Utilizando la opción ___x___ se puede separar una expresión en varias líneas:

```ruby
LOG_FORMAT = %r{
  (\d{2}:\d{2}) # Time
  \s(\w+)       # Event type
  \s(.*)        # Message
}x
```

## _Ruby REGEX_

Las expresiones regulares se pueden usar con varios métodos de _Ruby_, algunos ejemplos:

- ___scan___
- ___split___
- ___gsub___

### _.scan_

Matcheando todas las palabras:

```ruby
"this is some string".scan(/\w+/)
# => ["this", "is", "some", "string"]
```

Extrayendo los números:

```ruby
"The year was 1492.".scan(/\d+/)
# => ["1492"]
```

### _.gsub_

__Convirtiendo en mayúscula la primer letra de todas las palabras:__

```ruby
str = "lord of the rings"

str.gsub(/\w+/) { |w| w.capitalize }
# => "Lord Of The Rings"
```

### Etc

Validando una dirección de _email_:

```ruby
email = "test@example.com"

email.match?(/\A[\w.+-]+@\w+\.\w+\z/)
# true
```

