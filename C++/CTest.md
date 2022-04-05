# _CTest_

```cmake
enable_testing()
add_executable (tester "Test.cpp")
add_test(Tester tester)
```

- En el directorio donde se encuentra el ejecutable de ___Test.cpp___:

```powershell
ctest
```

