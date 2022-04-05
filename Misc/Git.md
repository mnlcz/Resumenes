# Indice
- [Indice](#indice)
- [Comandos](#comandos)
  - [Init](#init)
  - [Add](#add)
  - [Remove](#remove)
  - [Status](#status)
  - [Commit](#commit)
    - [Método 1](#método-1)
    - [Método 2](#método-2)
  - [Push](#push)
  - [Pull](#pull)
  - [Clone](#clone)
  - [Config](#config)
- [.gitignore](#gitignore)
- [Branches](#branches)
  - [Crear](#crear)
  - [Cambiar de branch](#cambiar-de-branch)
  - [Ejemplo](#ejemplo)
  - [Combinar branches](#combinar-branches)
- [Repositorio remoto](#repositorio-remoto)
  - [Remote](#remote)
  - [Crear repositorio](#crear-repositorio)
  - [Repositorio existente](#repositorio-existente)

# Comandos
## Init
```
git init
```
Se crea una carpeta _.git_ en la dirección actual. Esta carpeta permite usar los comandos de git dentro del proyecto.
## Add
```
git add <file>
git add *.<filetype>
git add .
```
Se agregan los archivos a la _staging area_, para luego hacer el _commit_.

## Remove
```
git rm –-cached <file>
```
Elimina un archivo del _staging area_, es decir, deja de trackearlo.
> –cached will only remove files from the index. Your files will still be there.

## Status
```
git status
```
Brinda información del area de trabajo, qué archivos estan en el _staging area_, cuales son ignorados, etc.

## Commit
### Método 1
```
git commit
```
Envía los elementos del _staging area_ al repositorio local. 
<br>
1. Abre Vim para agregar un mensaje.
2. `:wq`
### Método 2
```
git commit -m "Mensaje"
```
De esta manera se omite el paso de abrir Vim.

## Push
```
git push
```
Envía el repositorio local a un repositorio remoto (GitHub). Es necesario validar las credenciales ya sea usando la contraseña o una SSH Key.

## Pull
```
git pull
```
Descarga el contenido del repositorio remoto y actualiza el repositorio local.

## Clone
```
git clone
```
Descarga un repositorio y crea una carpeta del mismo en la dirección actual.

## Config
```
git config --local user.name ' '
git config --global
git config --system
```
Se utiliza principalmente para configurar el nombre de usuario y su mail, las opciones _local_, _global_ y _system_ representan los scopes de la configuración, siendo el primero para el proyecto, el segundo para el usuario del sistema operativo y el tercero para todo el sistema operativo. <br>
<br>
Para ver los valores actuales usar el comando:

```
git config --list
```

# .gitignore
Es un archivo en el cual se ponen los nombre de los archivos o carpetas que no se quieren agregar. Para crearlo se utiliza el siguiente comando:
```bat
touch .gitignore
```
Ejemplo de como se vería el archivo:
```
archivo.txt
/carpeta1
*.pdf
```

# Branches
Situación: grupo trabajando en un proyecto grande, se me asigna la tarea de agregar una funcionalidad. Sería mala idea tocar la versión principal del proyecto, ya que podría haber otras personas trabajando con esa versión en particular. Para esto se utilizan __branches__, yo crearía una branch para trabajar mi funcionalidad. Cuando termine de implementarla, combino mi branch personal con la __master branch__.

## Crear
```
git branch <nombre>
```
## Cambiar de branch
```
git checkout <nombre>
```
<br> 

Al cambiar de branch, todos los cambios que se hagan dentro de la misma no se veran reflejados en otra.

## Ejemplo
Creo una branch para mi funcionalidad:
```
git branch Funcionalidad
git checkout Funcionalidad
```
Implemento la funcionalidad:
```
touch funcionalidad.cpp
```
Edito algun archivo del proyecto:
```
vim main.cpp
```
Agrego los archivos modificados a la _staging area_:
```
git add .
```
Hago el commit de los cambios:
```
git commit -m "Agregada la funcionalidad"
```
<br>

Ahora supongamos que quisiera volver a la __master branch__ por algún motivo:
```
git checkout master
```
Al hacer el cambio, el archivo _funcionalidades.cpp_ desaparecerá del proyecto, al igual que los cambios en _main.cpp_, ya que estos fueron creados y modificados en una branch distinta.
<br>

## Combinar branches
Supongamos que ya terminé la implementación de la funcionalidad y quiero combinar la branch funcionalidad con la __master branch__. ACLARACIÓN: hay que estar en la __master branch__.
```
git merge <nombre>
```
Para el ejemplo sería:
```
git merge Funcionalidad
```

# Repositorio remoto
## Remote
```
git remote
git remote -v
git remote add <shortname> <url>
git remote remove <nombre>
git remote rename <nombre-viejo> <nombre-nuevo>
```
> __-v__: Shows URLs of remote repositories <br>
> __add__: Creates a new connection to a remote repository. The "shortname" you provide can later be used instead of the URL when referencing the remote. <br>
>__remove__: Disconnects the remote from the local repository. 

## Crear repositorio
```
cd Path/to/project
git init
git add README.md
git commit -m "Primer commit"
git remote add origin https://github.com/mnlcz/nombre-repositorio.git
git push -u origin master
```

## Repositorio existente
```
git remote add origin https://github.com/mnlcz/nombre-repositorio.git
git push -u origin master
```
