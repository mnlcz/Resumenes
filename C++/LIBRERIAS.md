# Agregando librerias con CMake en Clion

## Pasos previos

Primero se debe crear un subdirectorio _source_ en el proyecto, en el cuál irá el _main.cpp_ con su respectivo _CMakeLists.txt_. Vale aclarar que además del _CMakeLists_ mencionado, debe haber otro mas en la carpeta raíz del proyecto.

### _CMakeLists.txt_ raíz

Este archivo debe contener inicialmente lo siguiente (ejemplo con SFML):

```cmake
cmake_minimum_required(VERSION 3.20)
project(SFML)

set(CMAKE_CXX_STANDARD 23)
add_subdirectory(source)
```

### _CMakeLists.txt_ source

Inicialmente debe contener lo siguiente:

```cmake
cmake_minimum_required(VERSION 3.20)
add_executable(SFML main.cpp)
```

## Descargando la librería

_Git bash_ en la carpeta raíz. Luego:

```bash
git init
git submodule add https://github.com/SFML/SFML SFML
```

Esto descargará la librería en un directorio llamado SFML dentro del proyecto.

## Agregando la librería a _CMake_

Agregar el subdirectorio nuevo en el _CMakeLists.txt_ raíz:

```cmake
add_subdirectory(SFML)
```

Agregando el include:

```cmake
target_include_directories(${PROJECT_NAME} PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/SFML/include)
```

## Linkeando

Por último el _linkeo_, para esto se modifica el _CMakeLists.txt_ source:

```cmake
target_link_libraries(${PROJECT_NAME} sfml)
```

# _CMakeLists.txt_ finales

## _CMakeLists.txt_ raíz

```cmake
cmake_minimum_required(VERSION 3.20)
project(SFML)

set(CMAKE_CXX_STANDARD 23)
add_subdirectory(SFML)
add_subdirectory(source)
target_include_directories(${PROJECT_NAME} PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/SFML/include)
```

## _CMakeLists.txt_ source

```cmake
cmake_minimum_required(VERSION 3.20)
add_executable(SFML main.cpp)
target_link_libraries(${PROJECT_NAME} sfml)
```

