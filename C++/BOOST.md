# Indice
- [Indice](#indice)
- [Strings](#strings)
	- [Casteo](#casteo)
		- [_boost::lexical_cast_](#boostlexical_cast)
	- [Eliminación de caracter](#eliminación-de-caracter)
		- [_boost::algorithm::erase\__](#boostalgorithmerase_)
	- [Formato](#formato)
		- [_boost::algorithm::to\__](#boostalgorithmto_)
	- [Búsqueda substring](#búsqueda-substring)
		- [_boost::algorithm::find\__](#boostalgorithmfind_)
	- [Concatenación](#concatenación)
		- [_boost::algorithm::join_](#boostalgorithmjoin)
	- [Reemplazo](#reemplazo)
	- [Borrado de espacios, tabs](#borrado-de-espacios-tabs)
		- [_boost::algorithm::trim\__](#boostalgorithmtrim_)
	- [Condicionales](#condicionales)
		- [_boost::algorithm::is_any_of_](#boostalgorithmis_any_of)
		- [_boost::algorithm::is_digit_](#boostalgorithmis_digit)
		- [_boost::is_upper()_](#boostis_upper)
	- [Comparación](#comparación)
	- [Separación](#separación)
- [Archivos y directorios](#archivos-y-directorios)
	- [Directorios](#directorios)
		- [_boost::filesystem::path_](#boostfilesystempath)
			- [Información del directorio](#información-del-directorio)
			- [Información del archivo en el directorio](#información-del-archivo-en-el-directorio)
			- [Iterando un directorio](#iterando-un-directorio)
			- [Concatenando directorios](#concatenando-directorios)
			- [Conversor de notaciones](#conversor-de-notaciones)
	- [Archivos](#archivos)
		- [Errores](#errores)

# Strings

## Casteo

### _boost::lexical_cast_

Casteo a __string__ sin usar _stringstream_ directamente:

```c++
#include <boost/lexical_cast.hpp>
string s = boost::lexical_cast<string>(150) + " es una string";
```

## Eliminación de caracter

### _boost::algorithm::erase\__

```c++
#include <boost/algorithm/string.hpp>
string s = "Boost C++ Libraries";
boost::algorithm::erase_first_copy(s, "s");
boost::algorithm::erase_nth_copy(s, "s", 0);
boost::algorithm::erase_last_copy(s, "s");
boost::algorithm::erase_all_copy(s, "s");
boost::algorithm::erase_head_copy(s, 5);
boost::algorithm::erase_tail_copy(s, 9);
```

## Formato

### _boost::algorithm::to\__

```c++
#include <boost/algorithm/string.hpp>
string s = "Boost C++ Libraries";
boost::algorithm::to_upper_copy(s); // sin modificar la og;
boost::algorithm::to_upper(s);      // modifica la og
// lo mismo con to_lower(s)
```

## Búsqueda substring

### _boost::algorithm::find\__

```c++
#include <boost/algorithm/string.hpp>
string s = "Boost C++ Libraries";
boost::iterator_range<std::string::iterator> r = boost::algorithm::find_first(s, "C++");
cout << r << endl; // Imprime "C++"

// de esta manera se trabajaria con booleanos
if (boost::algorithm::find_first(s, "C++")) cout << "true";
```

## Concatenación

### _boost::algorithm::join_

```c++
#include <boost/algorithm/string.hpp>
vector<std::string> v{ "Boost", "C++", "Libraries" };
boost::algorithm::join(v, " ");
```

## Reemplazo

```c++
#include <boost/algorithm/string.hpp>
string str1="Hello  Dolly,   Hello World!"
boost::algorithm::replace_first(str1, "Dolly", "Jane");      // str1 == "Hello  Jane,   Hello World!"
boost::algorithm::replace_last(str1, "Hello", "Goodbye");    // str1 == "Hello  Jane,   Goodbye World!"
boost::algorithm::erase_all(str1, " ");                      // str1 == "HelloJane,GoodbyeWorld!"
boost::algorithm::erase_head(str1, 6);                       // str1 == "Jane,GoodbyeWorld!"
// tambien estan sus versiones _copy
```

## Borrado de espacios, tabs

### _boost::algorithm::trim\__

```c++
#include <boost/algorithm/string.hpp>
string str1 = "     hello world!     ";
string str2 = trim_left_copy(str1);   // str2 == "hello world!     "
string str3 = trim_right_copy(str1);  // str3 == "     hello world!"
trim(str1);                           // str1 == "hello world!"
```

## Condicionales

### _boost::algorithm::is_any_of_

```c++
#include <boost/algorithm/string.hpp>
string s = "--Boost C++ Libraries--";
auto x = boost::algorithm::trim_copy_if(s, boost::algorithm::is_any_of("-"));
```

### _boost::algorithm::is_digit_

Verifica si es un numero.

```c++
#include <boost/algorithm/string.hpp>
string s = "123456789Boost C++ Libraries123456789";
auto x = boost::algorithm::trim_copy_if(s, boost::algorithm::is_digit());
```

### _boost::is_upper()_

```c++
string s = "HAY MAYUSCULAS";
cout << boolalpha << any_of(s.begin(), s.end(), boost::is_upper());
```

## Comparación

```c++
#include <boost/algorithm/string.hpp>
string s = "Boost C++ Libraries";
boost::algorithm::starts_with(s, "Boost");
boost::algorithm::ends_with(s, "Libraries");
boost::algorithm::contains(s, "C++");
boost::algorithm::lexicographical_compare(s, "Boost");
```

## Separación

```c++
#include <boost/algorithm/string.hpp>
string s = "Boost C++ Libraries";
vector<std::string> vector;
boost::algorithm::split(vector, s, boost::algorithm::is_space());    // separa a partir de espacios
```

Para separar a partir de otro caracter que no sea espacio se puede usar `boost::algorithm::is_any_of;`. Por ejemplo:

```c++
string s = "Boost,C++,Libraries";
vector<std::string> vector;
boost::algorithm::split(vector, s, boost::algorithm::any_of(","));    // separa a partir de espacios
```

# Archivos y directorios

## Directorios

### _boost::filesystem::path_

Aclaración, __path__ trabaja con ___strings___, no manipula los archivos reales del sistema.

```c++
#include <boost/filesystem.hpp>
boost::filesystem::path p{ "D:\\Archivos" };
```

#### Información del directorio

```c++
cout << "Direccion: " << p.string() << endl;
cout << "Root: " << p.root_name() << endl;
cout << "Root directory: " << p.root_directory() << endl;
cout << "Root path: " << p.root_path() << endl;
cout << "Relative path: " << p.relative_path() << endl;
cout << "Parent path: " << p.parent_path() << endl;
cout << "Filename: " << p.filename();
```

#### Información del archivo en el directorio

```c++
boost::filesystem::path archivo{ "foto.jpg" };
cout << "Archivo " << archivo.string() << endl;
cout << "Nombre: " << archivo.stem() << endl;
cout << "Terminacion: " << archivo.extension();
```

#### Iterando un directorio

```c++
p = "C:\\Windows\\System";
for (const auto& pp : p)
	cout << pp << endl;
```

#### Concatenando directorios

Se utiliza el operador ___/=___

```c++
p = "C:\\";
p /= "Windows\\System";
```

#### Conversor de notaciones

Suponiendo que se tiene un directorio que utiliza "/", en vez de "\\":

```c++
p = "C:/Windows/System";
cout << p.make_preferred();
```

## Archivos

### Errores

Al trabajar con archivos reales es muy probable encontrarse con errores.  Para este tipo de conflictos _Boost_ tiene implementadas la excepción ___boost::filesystem::filesystem_error___.

```c++
p = "D:\\";
try
{
	boost::filesystem::file_status estado = boost::filesystem::status(p);
	cout << boolalpha << boost::filesystem::is_directory(estado) << endl;
}
catch (boost::filesystem::filesystem_error& e)
{
	cerr << e.what() << endl;
}
```

