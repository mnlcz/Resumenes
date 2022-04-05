# [Christmas Challenge](https://tryhackme.com/christmas)

# [Advent of Cyber 3 (2021)](https://tryhackme.com/room/adventofcyber3)

# Día 1

## Objetivos

1. ¿Qué es una vulnerabilidad __IDOR__?
2. ¿Cómo encuentro dicha vulnerabilidad?
3. Completar el desafío.

## 1) Vulnerabilidad __IDOR__

_Insecure Direct Object Reference_: 

- Se da cuando el atacante obtiene acceso a información o acciones que no debería.
- __Suele ocurrir cuando un servidor web recibe _input_ del usuario y no valida los permisos__.

## 2) ¿Cómo encontrarla?

Ese ___input del usuario___ del que se habló previamente suele encontrarse en 3 lugares:

### ___Query Component___ 

Información pasada a partir de la _URL_. Ejemplo:

![image-20211201143526717](..\img\image-20211201143526717.png)

A partir de esa _URL_ se puede obtener la siguiente información:

- __Protocolo__: _https://_
- __Dominio__: _website.thm_
- __Página__: _/profile_
- __Componente Query__: _id=23_

### _Post Variables_

Examinando las _forms_ de un sitio web se pueden obtener campos que pueden ser vulnerables a un _IDOR exploit_. 

Ejemplo: ___form que actualiza la contraseña del usuario___.

![image-20211201143951739](..\img\image-20211201143951739.png)

- __El _user_id_ se está pasando al servidor como un campo oculto__.
- Cambiar _user_id_ podría resultar en cambiarle la contraseña a otro usuario.

### _Cookies_

Las _cookies_ se utilizan para mantenerse _logeado_ en una página. El proceso generalmente consiste en enviar una ___session_id___ que el servidor utiliza para validar la sesión.

- __La _session_id_ suele ser una _string_ de caracteres random__. Por ejemplo: __5db28452c4161cf88c6f33e57b62a357__
  - __Programadores novatos suelen poner la información en la _cookie_ directamente__.
    - Cambiar esta _cookie_ podría, por ejemplo, mostrar información de otro usuario.

![image-20211201144542567](..\img\image-20211201144542567.png)

## 3) Desafío

Consiste en una página de mentiras que muestra unos juguetes defectuosos.

![image-20211201144910218](..\img\image-20211201144910218.png)

Tiene otras páginas:

- _Builds_: juguetes y sus partes.
- _Inventory_: lista de items.
- ___Your Activity___: perfil de _McSkidy_.

### Vulnerabilidad

La _URL_ de _Your Activity_ presenta información relevante:

```http
https://inventory-management.thm/activity?user_id=11
```

- ___user_id = 11___
  - Ese valor corresponde al _id_ de _McSkidy_.

### Usando esa vulnerabilidad

Procedo a cambiar los _user_id_ hasta encontrar el perfil del que podría ser el responsable.

- El enunciado menciona valores entre _1-20_.

#### Culpable

El ___user_id = 9___ es el del responsable:

![image-20211201145353930](..\img\image-20211201145353930.png)

### Resolución

#### _Santa's position in the company_

![image-20211201145709003](..\img\image-20211201145709003.png)

#### _McStocker's position in the company_

![image-20211201145815190](..\img\image-20211201145815190.png)



#### _Flag_

![image-20211201145500706](..\img\image-20211201145500706.png)

## Conclusión

Para mas información acerca de ataques ___IDOR___ se recomienda https://tryhackme.com/room/idor

# Día 2

## Objetivos

1. Entender la tecnología y como se comunican los servidores web.
2. Entender qué son las ___cookies___ y para qué sirven.
3. Aprender a manipular ___cookies___.
4. Resolver el desafío.

## 1) _HTTP(S)_

Para que una computadora se comunique con un servidor web es necesario un protocolo intermediario. Este protocolo se llama ___HTTP___ (_Hypertext Transfer Protocol_), algunas de sus caracteristicas son:

- Las ___HTTP request___ son similares a una _request_ [TCP](https://es.wikipedia.org/wiki/Protocolo_de_control_de_transmisión).

- Es un protocolo _stateless_, en otras palabras, no se podría distinguir la request entre 2 usuarios.

- Cuando se crea una _request_ se incluye siempre el ___method___ y ___target___ _header_. 

  - ___target header___: especifica que se va a obtener del servidor.
  - ___method header___: especifica cómo se va a obtener lo del _target header_.
  - Para obtener información de un servidor se suele utilizar el ___GET method___.
  - Para enviar información a un servidor se suele utilizar el ___POST method___. Ej: mandar información de login.

  Ejemplo:

  ```http
  GET / HTTP/1.1
  Host: tryhackme.com
  User-Agent: Mozilla/5.0 Firefox/87.0
  Referer: https://tryhackme.com/
  ```

  - __El servidor respondera con un _status code___. El _status code_ más común para representar que todo salió bien es `HTTP 200 OK`. 

  Un ejemplo de una respuesta sería:

  ```html
  HTTP/1.1 200 OK
  Server: nginx/1.15.8
  Date: Wednesday, 24 Nov 2021 13:34:03 GMT
  Content-Type: text/html
  Content-Length: 98
  
  <html>
  <head>
      <title>Advent of Cyber</title>
  </head>
  <body>
      Welcome To Advent of Cyber!
  </body>
  </html>
  ```

## 2) _Cookies_

Como se mencionó en el apartado previo __el protocolo _HTTP_ es _stateless___, esto a grandes rasgos quiere decir que no hay forma de distinguir entre _requests_ de distintos usuarios. Para resolver este problema se utilizan las ___cookies___. Algunas características:

- Son pequeños extractos de información, también conocido como __metadata__.
- Permiten identificar al usuario y sus permisos.
- Se les puede asignar cualquier nombre y valor.

Un ejemplo:

![image-20211202113100398](..\img\image-20211202113100398.png)

### Componentes de una _cookie_

![image-20211202113404288](..\img\image-20211202113404288.png)

Los componentes son agrupados de a __pares__. Los más relevantes son el par ___name-value___ (el nombre de la _cookie_ y el valor de ese nombre) y ___attribute-value___ (define el atributo de la _cookie_ y el valor del mismo). Un ejemplo de la sintáxis de `Set-Cookie`:

```html
Set-Cookie: <cookie-name>=<cookie-value>; Domain=<domain-value>; Secure; HttpOnly
```

## 3) Manipular _cookies_

Consiste en modificar una _cookie_ para obtener un comportamiento distinto al esperado por el programador original. Esto es posible ya que __las _cookies_ se guardan localmente en cada computadora__.

### Pasos previos

1. Abrir las _developer tools_ en el navegador. `F12` o `Ctrl + Shift + I`
2. En _Edge_ ir a _Aplicación_, luego al apartado _Almacenamiento_ y por último _Cookies_.

![image-20211202114415344](..\img\image-20211202114415344.png)

Ejemplo de como se vería en _Wikipedia_:

![image-20211202114538062](..\img\image-20211202114538062.png)

### Pasos para manipular

![image-20211202115133385](..\img\image-20211202115133385.png)

## 4) Desafío

1. Me pide abrir la página en cuestión en una nueva pestaña, procedo.

![image-20211202115315612](..\img\image-20211202115315612.png)

2. Me registro y verifico las _cookies_.

![image-20211202115433892](..\img\image-20211202115433892.png)

![image-20211202115458203](..\img\image-20211202115458203.png)

3. Abro las _Dev Tools_.

   ![image-20211202115531841](..\img\image-20211202115531841.png)

4. Para decodear la _cookie_ la página me recomienda [CyberChef](https://gchq.github.io/CyberChef/)

![image-20211202120702997](..\img\image-20211202120702997.png)

Como se ve en la imagen, el tipo de _encoding_ es _[Hexadecimal](https://en.wikipedia.org/wiki/Hexadecimal)_ y el formato de _output_ es _[JSON](https://es.wikipedia.org/wiki/JSON)_.

5. Manipulo la _cookie_ de manera tal que `username = admin`. Usando _CyberChef_:

![image-20211202121143592](..\img\image-20211202121143592.png)

6. Pego el _output_ encodeado en la _cookie_.
7. Reinicio la página para ver los cambios.

![image-20211202121348793](..\img\image-20211202121348793.png)

# Día 3

## Objetivos

1. Entender qué es _content discovery_ y cuáles son sus usos.
2. Entender qué son las *default cretendials*.
3. Resolver el desafío.

## 1) _Content discovery_

- __Content__: _assets and inner workings of the application that we are testing. Files, folders, etc_. Este contenido no necesariamente está disponible para el público general.

Ejemplo: se tiene un blog personal donde se suben recetas, si bien se quiere que el público pueda verlas, la modificación solo debería ser posible para el *admin*.

### Usos

- Permite encontrar cosas que de otra forma no serían posibles, como por ejemplo:
  - ***Config files***.
  - ***Passwords***.
  - ***Management systems***.

Estos elementos se pueden encontrar debido a que se encuentran localizados en una carpeta o en un archivo. **Esto último conlleva a que se pueda buscar determinada información mediante la terminación de un archivo**. Por ejemplo `.txt`.

### Como encontrar

Se puede hacer manualmente buscando nombres como ***admin*** o ***passwords.txt***. Sin embargo, la forma más eficaz es utilizando herramientas orientadas a este tipo de trabajo. Un ejemplo es ***Dirbuster***.

#### *Dirbuster*

- **INPUT**: un archivo que contenga una lista con las palabras que se quieran buscar.

![image-20211203140959739](..\img\image-20211203140959739.png)

Ejemplo:

![image-20211203141018903](..\img\image-20211203141018903.png)

- ***Dirbuster*** buscará en *santascookies.thm* las carpetas listadas con los nombres presentes en el *mywordlist.txt*.
  - Para buenas ***wordlists*** ver [SecLists](https://github.com/danielmiessler/SecLists).

## 2) *Default credentials*

Son credenciales listas para que el usuario pueda usar la plataforma, **se espera que sean modificadas**.

- Para buenas ***default credentials*** ver [SecLists/Passwords/Default-Credentials](https://github.com/danielmiessler/SecLists/tree/master/Passwords/Default-Credentials).

Algunos ejemplos:

![image-20211203141640808](..\img\image-20211203141640808.png)

## 3) Desafío

1. Inicio la *VM* brindada por *TryHackMe*.

2. Me pide usar una *common wordlist* para obtener el contenido de `http://10.10.139.244 `.

   ![image-20211203142058595](..\img\image-20211203142058595.png)

   - Recomienda usar *common.txt*.

3. Uso ***Dirbuster***:

   ![image-20211203142425370](..\img\image-20211203142425370.png)

4. Abro en el navegador la dirección *admin*.

   ![image-20211203142659775](..\img\image-20211203142659775.png)

5. Pruebo las *default credentials*

   - Se sabe que el usuario es *administrator*. Pruebo contraseñas básicas.

     ![image-20211203143010383](..\img\image-20211203143010383.png)

     - La clave era ***administrator***.

6. Busco el valor de la *flag*.

   ![image-20211203143236683](..\img\image-20211203143236683.png)

# Día 4

## Objetivos

1. Entender qué es la ***autenticación*** y dónde se utiliza.
2. Entender qué es ***fuzzing***.
3. Entender qué es ***Burp Suite*** y cómo se puede usar para logearse.
4. Resolver el desafio.

## 1) Autenticación

Se entiende como *autenticación* al proceso de **validad la identidad de un usuario**, confirmando si es la persona que dice ser. Algunos ejemplos de autenticación:

- ***User*** y ***password***.
- ***Token authentication***: texto encriptado.
- ***Biometric authentication***: huellas digitales, *retina data*, etc.

## 2) *Fuzzing*

Consiste en *testear* automaticamente un elemento de una página web hasta que la aplicación devuelva **una vulnerabilidad o información valiosa**.

Ejemplo: *rather than testing a login form for a valid set of credentials one-by-one (which is exceptionally time-consuming), we can use a tool for fuzzing this login form to test hundreds of credentials at a much faster rate and more reliably.*

## 3) *Burp Suite*

Es un programa que se utiliza para *testing*.

![image-20211204142213017](..\img\image-20211204142213017.png)

### Configurar

1. Entrar a una dirección con el navegador.

2. **Proxy** -> **Intercept** -> **Intercept ON**.

   ![image-20211204142419860](..\img\image-20211204142419860.png)

3. Activar la extensión ***FoxyProxy*** en el navegador.

   ![image-20211204142533978](..\img\image-20211204142533978.png)

4. Asignarle a los datos de login un valor inicial.

   ![image-20211204142651153](..\img\image-20211204142651153.png)

5. En *Burp Suite* aparecerá lo siguiente.

   ![image-20211204144006546](..\img\image-20211204144006546.png)

6. Click derecho + *Sent to intruder*.

   ![image-20211204144102286](..\img\image-20211204144102286.png)

7. **Intruder -> Positions -> Clear**

   ![image-20211204144221782](..\img\image-20211204144221782.png)

8. Se selecciona el parámetro al que se le quiera hacer fuerza bruta y se presiona **Add**.

   ![image-20211204144452278](..\img\image-20211204144452278.png)

9. **Payloads** -> **Payload Options** -> **Load** para cargar nuestro txt con las contraseñas.

   ![image-20211204144724336](..\img\image-20211204144724336.png)

10. Ataco.

    ![image-20211204145129781](..\img\image-20211204145129781.png)

11. Obtengo una lista.

    ![image-20211204145145799](..\img\image-20211204145145799.png)

12. **TODAS LAS PASSWORD QUE FALLEN VAN A TENER UNA *LENGTH* IGUAL**. Para ver cuál es la buena basta con ordenar por tamaño.

    ![image-20211204145252298](..\img\image-20211204145252298.png)

## 4) Desafío

Los primeros puntos del desafío consisten en configurar correctamente ***Burp Suite***. Los últimos piden obtener la contraseña (lo cuál se puede ver en el apartado *3) Burp Suite*) y la *flag*.

- **Contraseña**: `cookie`
- ***Flag***: `THM{SANTA_DELIVERS}`

# Día 5

## Objetivos

1. Entender qué es una ***XSS vulnerability***.
2. Qué tipos existen.
3. Completar el desafío.

## 1) *XSS vulnerability*

***Cross-Site scripting*** más conocido como *XSS* corresponde a un ***injection attack*** donde se utiliza ***malicious JavaScript***.

- Se inyecta el *JavaScript* en la web esperando que otros usuarios lo ejecuten.
- Algunas de las cosas que se pueden obtener con este tipo de ataque son: 
  - Obtener las ***cookies*** del objetivo.
  - Tomar control se la sesión del objetivo.
  - Correr un ***keylogger***.

## 2) Tipos

### *DOM*

*Document Object Model* es una interfaz de programación para documentos *HTML* y *XML*. Representa una página para que el programa pueda modificar su estructura, estilo y contenido.

En este tipo de ***XSS*** el *JavaScript* se ejecuta **directamente en el navegador** cuando se recibe un *input* o una interacción con el usuario.

### *Reflected*

Ocurre cuando la información de un usuario en una ***request HTTP*** se incluye en la página web sin ningun tipo de validación. Un ejemplo sería cuando se da un mensaje de error con información valiosa en la URL: `**https://website.thm/login?error=Username%20Is%20Incorrect**`. Este mensaje podría reemplazarse por código *JavaScript* que se ejecute cuando el usuario visite la página.

### *Stored*

Como su nombre indica, el ***XSS*** se encuentra en el sitio web, por ejemplo, en una base de datos. Este se ejecuta cuando otro usuario visita la página.

- Es peligroso por la gran cantidad de victimas que puede afectar.

### *Blind*

Similar al *stored*, se encuentra en el sitio web, la diferencia es que en este caso no se puede la *paylod working*, ni testearlo previamente. Ejemplo: *contact form*.

## 3) Desafío

1. Accedo a la *URL* brindada en el enunciado, la cuál me lleva a un foro en el cuál se da el problema de la sustitución de la palabra *Christmas* por *Buttmas*.

2. Inicio sesión con la información brindada en el enunciado.

   ![image-20211205223237790](..\img\image-20211205223237790.png)

3. Voy a configuración e intento cambiar la contraseña de McSkidy. Al hacerlo noto que esta información se muestra sin encriptar en la *URL*: `https://10-10-91-15.p.thmlabs.com/settings?new_password=pass123`.

4.  Entro a un hilo e intento comentar: `hello <u>world</u>`.

   ![image-20211205223523356](..\img\image-20211205223523356.png)

5. Comento la siguiente sentencia para cambiar la contraseña

<script>fetch('/settings?new_password=pass123');</script>

- `fetch`: hace una ***request*** a la *URL* especificada.
- `<script>`: permite ejecutar *JavaScript*.

![image-20211205223736483](..\img\image-20211205223736483.png)

6. Inspecciono la página con la *DevTool* para verificar si el ***XSS*** está funcionando.

   ![image-20211205224009922](..\img\image-20211205224009922.png)

7. Al estar corriendo sin problemas, el ***XSS*** hace que cada usuario que acceda al hilo **sufrirá el cambio de su contraseña tal como se muestra en el script**.

8. Cierro sesión e intento logearme con el usuario `grinch` y con la clave especificada en el script (`pass123`).

   ![image-20211205224317288](..\img\image-20211205224317288.png)

9. Deshabilito el plugin para obtener la *flag*.

   ![image-20211205224355899](..\img\image-20211205224355899.png)

# Día 4

 ## Objetivos

1. Entender qué es una ***Local File Inclusion (LFI) vulnerability***.
2. Entender cuáles son sus usos.
3. Entender cómo identificar, testear y utilizar.
4. Completar el desafío.

## 1) *LFI*

Es un tipo de vulnerabilidad que permite al atacante **incluir** y **leer** archivos en el servidor. 

- Suceden debido a la falta de seguridad por parte del otro desarrollador.
- Ocurren cuando el *input* del usuario no es filtrado.
- En el *PHP* las siguientes funciones pueden ser las causantes:
  - `include`
  - `require`
  - `include_once`
  - `require_once`

## 2) Usos

Una vez que se encuentra una vulnerabilidad ***LFI*** es posible leer información protegida si se cuenta con los permisos necesarios. Otros usos pueden ser:

- Filtrar información clasificada.
- Realizar una *[Ejecución arbitraria de código](https://es.wikipedia.org/wiki/Ejecución_arbitraria_de_código)* en el servidor.

## 3) Identificación, testeo y utilización

### Identificación

Los atacantes están interesados en los **parámetros *HTTP*** para poder manipular el *input* e inyectar el ataque. Generalmente se busca ***entry points***:

- Parámetros `GET` o`POST`. Los parámetros son *strings* dentro de la *URL* utilizadas para obtener información y realizar determinadas acciones dependiendo del *input* del usuario.

  ![image-20211206174324185](..\img\image-20211206174324185.png)

Una vez encontrado el *entry point* es necesario entender como se procesará esta información dentro de la aplicación. Luego se puede empezar a testear vulnerabilidades concretas ya sea manualmente o con herramientas automatizadas. Un ejemplo de un código *PHP* vulnerable a ***LFI***:

```php
<?PHP 
	include($_GET["file"]);
?>
```

### Testeo

Con el *entry point* se puede comenzar a testear la lectura de archivos locales con información importante. Un ejemplo de archivos importantes en *Linux*:

```
/etc/issue
/etc/passwd
/etc/shadow
/etc/group
/etc/hosts
/etc/motd
/etc/mysql/my.cnf
/proc/[0-9]*/fd/[0-9]*   (first number is the PID, second is the filedescriptor)
/proc/self/environ
/proc/version
/proc/cmdline
```

#### Pasos

1. Identifico el ***entry point*** o el **parámetro *HTTP***.

2. Empiezo a testear **incluyendo** archivos del sistema en la página y evalúo cómo responde. Algunos ejemplos de inclusiones:

   1. `/etc/passwd`: para *Linux OS*.
   2. `..`: para retroceder un directorio.
   3. `....//`: para sobrepasar filtros.

   Ejemplo:

```http
http://example.thm.labs/page.php?file=/etc/passwd http://example.thm.labs/page.php?file=../../../../../../etc/passwd http://example.thm.labs/page.php?file=../../../../../../etc/passwd%00 http://example.thm.labs/page.php?file=....//....//....//....//etc/passwd http://example.thm.labs/page.php?file=%252e%252e%252fetc%252fpasswd
```

### Uso

La utilización de un ***LFI*** suele estar limitada por la configuración del servidor web. En caso de tratarse de una aplicación web *PHP*, se podría usar un ***PHP-supported Wrapper***. *PHP* provee varios métodos para la transmición de información.

#### *PHP filter*

El ***PHP filter wrapper*** se utilizar para leer el contenido *PHP* de la página actual. Esto generalmente no es posible ya que los archivos *PHP* suelen ejecutarse sin mostrar su contenido. Sin embargo, usando *filter* se puede obtener dichos archivos *php* en otro formato, por ej: `base64` o `ROT13`.

Ejemplo tratanto de leer `/etc/passwd` utilizando ***PHP filter wrapper***:

```http
http://example.thm.labs/page.php?file=php://filter/resource=/etc/passwd
```

1. Trato de leer `index.php` utilizando el *filter*. Esto no funcionará ya que el servidor tratará de ejecutar dicho código, para solucionarlo utilizo `base64` o `ROT13` como output.

   ```http
   http://example.thm.labs/page.php?file=filter/read=string.rot13/resource=/etc/passwd http://example.thm.labs/page.php?file=php://filter/convert.base64-encode/resource=/etc/passwd
   ```

2. El resultado en `base64` se vería de la siguiente manera:

   ```
   cm9vdDp4OjA6MDpyb290Oi9yb290Oi9iaW4vYmFzaApkYWVtb246eDox******Deleted
   ```

3. Para traducir esto se puede utilizar [Base64 Decode and Encode - Online](https://www.base64decode.org/).

#### *PHP DATA*

Se utiliza para incluir texto o información "encodeada" en `base64`. También se utiliza para incluir imágenes en la página actual.

Ejemplo encodeando `"AoC3 is fun!"` para luego incluirlo en la página usando ***wrapper data***:

```bash
echo "AoC3 is fun!" | base64 QW9DMyBpcyBmdW4hCg==
```

Para decodearlo sería:

```bash
echo "QW9DMyBpcyBmdW4hCg==" | base64 --decode AoC3 is fun!
```

Ya encodeado se procede a incluir la información en la página vulnerable:

```http
http://example.thm.labs/page.php?file=data://text/plain;base64,QW9DMyBpcyBmdW4hCg==
```

#### Log files

El atacante debe incluir código maligno en los ***log files***. Dicho código se ejecutará cuando se haga la *request*.

## 4) Desafío

1. Entro a la *URL* (sin el login) que me brinda el enunciado.

   ![image-20211207174430695](..\img\image-20211207174430695.png)

2. En la *URL* del item previo `https://10-10-240-228.p.thmlabs.com/index.php?err=error.txt` se puede ver la vulneravilidad `err`.

3. Modifico el valor `err` para poder acceder a la flag en `/etc/flag`:

   ![image-20211207174637717](..\img\image-20211207174637717.png)

4. Uso ***PHP filter*** para ver el código *PHP* correspondiente a la flag.

   `https://10-10-240-228.p.thmlabs.com/index.php?err=php://filter/convert.base64-encode/resource=index.php`

   ![image-20211207175247092](..\img\image-20211207175247092.png)

5. Usando [Base64 Decode and Encode - Online](https://www.base64decode.org/) obtengo:

   ![image-20211207175349219](..\img\image-20211207175349219.png)

6. Realizo la misma técnica pero con `./includes/creds.php`.

   `https://10-10-240-228.p.thmlabs.com/index.php?err=php://filter/convert.base64-encode/resource=./includes/creds.php`

7. Luego de traducirlo obtengo entre otras cosas:

   ```php
   $USER = "McSkidy";
   $PASS = "A0C315Aw3s0m"
   ```

8. Me logeo con la info obtenida.

   ![image-20211207180755179](..\img\image-20211207180755179.png)

9. `https://10-10-240-228.p.thmlabs.com/index.php?err=/app_access.log` para obtenter el nombre del webserver.
