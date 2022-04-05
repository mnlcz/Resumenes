# Indice
- [Indice](#indice)
- [Crear carpeta/archivo](#crear-carpetaarchivo)
- [Copiar líneas entre elementos](#copiar-líneas-entre-elementos)
- [Ver información de un objeto](#ver-información-de-un-objeto)
- [Abrir una página web](#abrir-una-página-web)
- [Recibir contraseñas como input](#recibir-contraseñas-como-input)
- [*HTTP* request](#http-request)
- [Filtrar resultados](#filtrar-resultados)
- [Uso de métodos](#uso-de-métodos)

# Crear carpeta/archivo

```powershell
New-Item -Path . -Name carpeta -ItemType Directory
New-Item -Path . -Name archivo -ItemType File
```

# Copiar líneas entre elementos

```powershell
Get-Content main.cpp -First 5 | Out-File otro.cpp
```

# Ver información de un objeto

```powershell
Get-ChildItem | Get-Member
```

# Abrir una página web

```powershell
Start-Process https://www.youtube.com/
```

# Recibir contraseñas como input

```powershell
$ClaveSegura = Read-Host -Prompt "PASS: " -AsSecureString
$ClaveLegible = [Runtime.InteropServices.Marshal]::PtrToStringAuto([Runtime.InteropServices.Marshal]::SecureStringToBSTR($ClaveSegura))
```

- Sin hacer la conversión no es posible la lectura de la clave recibida.

# *HTTP* request

```powershell
$Response = Invoke-WebRequest -URI https://www.bing.com/search?q=how+many+feet+in+a+mile
$Response.InputFields | Where-Object {
    $_.name -like "* Value*"
} | Select-Object Name, Value

name       value
----       -----
From Value 1
To Value   5280
```

# Filtrar resultados

Para este ejemplo se filtra el output del comando `Get-Process` para ver únicamente el proceso ***notepad***.

```powershell
Get-Process | Where-Object { $_.name -eq "notepad" }
```

- `$_`: refiere a **cada objeto que viene proveniente del |**.

Otra forma:

```powershell
Get-Process -Name notepad
```

# Uso de métodos

```powershell
(Get-Process | Where-Object { $_.name -eq "notepad" }).Kill()
```

