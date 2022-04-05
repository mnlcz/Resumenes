# Indice
- [Indice](#indice)
- [_C++ REGEX_](#c-regex)
  - [Uso](#uso)
  - [_Matches_](#matches)
  - [_Search_](#search)
    - [Método 1: _while_](#método-1-while)
    - [Método 2: _iterator_](#método-2-iterator)
    - [Index](#index)
  - [Rangos](#rangos)
    - [Atajos](#atajos)
      - [Variantes negativas](#variantes-negativas)
    - [Importante](#importante)
  - [Modificadores](#modificadores)
  - [Condicionales](#condicionales)

# _C++ REGEX_

## Uso

Ejemplo básico:

```c++
#include <regex>

std::string s = "Perro gato pajaro caballo";
std::regex r("gato");
```

## _Matches_

Los __matches__ se guardan en una variable de tipo ___std::smatch___.

```c++
std::smatch matches;
```

## _Search_

```c++
std::regex_search(<string>, <smatch>, <regex>);
```

### Método 1: _while_

```c++
std::string s;
std::smatch matches;
std::regex r();
while(std::regex_search(s, matches, r))
{
    std::cout << "Hay match? " << matches.ready() << std::endl;
    std::cout << "No hay matches? " << matches.empty() << std::endl;
    std::cout << "Cantidad de matches: " << matches.size() << std::endl;
    std::cout << "Primer match: " << matches.str(1) << std::endl;
}
```

### Método 2: _iterator_

```c++
std::string s;
std::regex r();
std::sregex_iterator matchActual(s.begin(), s.end(), r);
std::sregex_iterator matchFinal;
while(matchActual != matchFinal)
{
    std::smatch match = *matchActual;
    std::cout << match.str() << std::endl;
    matchActual++;
}
```

### Index

El índice de un match se puede obtener con el método ___position___.

```c++
if (regex_search(tag, match, reg))
{
	index = match.position();
}
```

## Rangos

- __[0-9]__: números del 0 al 9.
- __[a-z]__: letras de la _a_ a la _z_ (minúsculas).
- __[A-Z]__: letras de la _a_ a la _z_ (mayúsculas).
- __[^a-z]__: _negated range_.

Ejemplo:

```c++
std::regex r("[0-5]");
```

### Atajos

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
- __^__: _beginning of line_.
- __$__: _end of line_.

## Condicionales

```c++
std::regex r("gato|perro");
```

