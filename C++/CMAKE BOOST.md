# Usar _BOOST_ con _CMAKE_

En el _CMakeLists.txt_ del proyecto:

```cmake
cmake_minimum_required (VERSION 3.8)
set(CMAKE_CXX_STANDARD 23)

include_directories($ENV{BOOST}) 
add_executable (CMakeTarget "Actividad 11.cpp" "Actividad 11.h")
link_libraries($ENV{BOOST_LIBS})
```

- Se asume que las __variables de entorno__ _BOOST_ y _BOOST_LIBS_ ya están definidas.

