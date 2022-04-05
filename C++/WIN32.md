# Apuntes Win32 API
## Indice
- [Apuntes Win32 API](#apuntes-win32-api)
  - [Indice](#indice)
  - [Windows Coding Conventions](#windows-coding-conventions)
    - [Enteros](#enteros)
    - [Booleano (int)](#booleano-int)
    - [Punteros](#punteros)
    - [Hungarian notation](#hungarian-notation)
  - [Strings](#strings)
  - [Window](#window)
    - [Parent y Owner Windows](#parent-y-owner-windows)
    - [Windows handles](#windows-handles)
  - [WinMain](#winmain)
  - [Creando mi primera ventana](#creando-mi-primera-ventana)
    - [Windows Classes](#windows-classes)
    - [Creando la ventana](#creando-la-ventana)
  - [Windows Messages](#windows-messages)
  - [Message Loop](#message-loop)
  - [Posted Messages versus Sent Messages](#posted-messages-versus-sent-messages)
  - [Writing the Window Procedure](#writing-the-window-procedure)
  - [Default message handling](#default-message-handling)
  - [Evitar cuellos de botellas en el window procedure](#evitar-cuellos-de-botellas-en-el-window-procedure)

## Windows Coding Conventions
### Enteros
- _BYTE_: 8 bits, unsigned.
- _DWORD_: 32 bits, unsigned.
- _INT32_: 32 bits, signed.
- _INT64_: 64 bits, signed.
- _LONG_: 32 bits, signed.
- _LONGLONG_: 64 bits, signed.
- _UINT32_: 32 bits, unsigned.
- _UINT64_: 64 bits, unsigned.
- _ULONG_: 32 bits, unsigned.
- _ULONGLONG_: 64 bits, unsigned.
- _WORD_: 16 bits, unsigned.

### Booleano (int)
- _BOOL_: es un _typedef_ para un ___entero___, sus valores estan definidos de la siguiente manera:
```C++
#define FALSE 0
#define TRUE 1
```
A pesar de la definición de _TRUE_, la mayoria de funciones que retornan un valor _BOOL_ retornan cualquier numero
distinto de 0 para indicar _verdadero_. Por lo tanto:
```C++
// Right way.
BOOL result = SomeFunctionThatReturnsBoolean();
if (result) 
{ 
    ...
}

// Wrong!
if (result == TRUE)
{
...
}
```

### Punteros
La mayoria de las definiciones vienen en forma de _pointer-to-X_. Suelen tener el prefijo _P_.
Para algunos casos hay multiples formas de escribirlo:
```C++
RECT*  rect;  // Pointer to a RECT structure.
LPRECT rect;  // The same
PRECT  rect;  // Also the same.
```
Sabiendo que _P_ refiere a puntero, vale aclarar que _LP_ refiere a long pointer, esta distinción era importante
en epocas pasadas pero en la actualidad no es relevante.
Algunos punteros:
- _DWORD_PTR_
- _INT_PTR_
- _LONG_PTR_
- _ULONG_PTR_
- _UINT_PTR_

### Hungarian notation
Notación utilizada, se caracteriza por agregarle prefijos a las variables para brindarle mayor información.


## Strings
Windows trabaja con Unicode, usando UTF-16. Los caracteres de UTF-16 son llamados ___wide characters___,
para distinguirlos de los caracteres de 8 bits. El compilador de _C++_ los conoce como _wchar_t_.
```C++
typedef wchar_t WCHAR;
```
Para definir una _string literal_ (secuencia de caracteres terminada en null) se pone una __L__ antes del literal:
```C++
wchar_t a = L'a';
wchar_t *str = L"hello";
```
Algunos _typedef_:
- _CHAR_: char
- _PSTR_ o _LPSTR_: char*
- _PCSTR_ o _LPCSTR_: const char*
- _PWSTR_ o _LPWSTR_: wchar_t*
- _PCWSTR_ o _LPCWSTR_: const wchar_t*

## Window
Las ventanas son un papel crucial en Windows, valga la redundancia, sin embargo las ventanas no son solo
el recuadro del programa (llamado _main window_), tambien son los botones y otros elementos de la interfaz de
usuario o __UI__. Se puede pensar a una ventana como una construcción (en programación) que posee las siguientes
caracteristicas:
- Ocupa cierto espacio en la pantalla.
- Puede ser o no visible en cierto momento.
- Sabe como dibujarse a si misma.
- Responde a eventos del usuario o del sistema operativo.

### Parent y Owner Windows
La _application window_ es ___parent___ de la _control window_. La __parent window__ le otorga ciertas
caracteristicas a la __child__, como por ejemplo el tamaño o la posición. <br>
Una ___owner window___ siempre aparece en el centro de su _owner_, también desaparece cuando esta es minimizada.

### Windows handles
Las ventanas son _objetos_, pero no son __clases__ de __C++__. El programa referencia una ventana usando un valor
llamado ___handle___. En esencia es solo un numero, sin embargo, el sistema operativo lo utiliza para identificar al objeto.
El tipo de dato para un ___window handle___ es __HWND__. <br>
Las funciones que retornan __window handle__ son las que crean la ventana: _CreateWindow_ y _CreateWindowEx_. <br>
Para realizar una operacion con una ventana usualmente se llama a una funcion que reciba un __HWND__ como parámetro. Por ejemplo:
```C++
BOOL MoveWindow(HWND hWnd, int X, int Y, int nWidth, int nHeight, BOOL bRepaint);
```
__IMPORTANTE__: Los ___handle___ NO son punteros.

## WinMain
Todos los programas de _Windows_ tienen un entry-point denominado ___WinMain___ o ___wWinMain___:
``` C++
int WINAPI wWinMain(HINSTANCE hInstance, HINSTANCE hPrevInstance, PWSTR pCmdLine, int nCmdShow);
```
Parametros:
- ___hInstance___: "handle to instance". El SO lo utiliza para identificar el ejecutable (EXE) cuando se carga en memoria.
Es necesario para realizar funciones como: cargar iconos, bitmaps, etc.
- ___hPrevInstance___: irrelevante. Se utilizaba en versiones viejas de _Windows_, en la actualidad su valor es siempre 0.
- ___pCmdLine___: contiene los argumentos de la linea de comandos en formato _Unicode string_.
- ___nCmdShow___: flag que indica si la ventana va a estar minimizada, maximizada o normal.
- ___WINAPI___: calling convention.

La función retorna un ___entero___. Dicho valor no será útil para el SO pero el programador puede usarlo para otro programa. <br>
La diferencia entre ___wWinMain___ y ___WinMain___. Es que usan _UNICODE_ y _ANSI_ respectivamente.

## Creando mi primera ventana
### Windows Classes
Una _window class_ define una serie de comportamientos o caracteristicas que un conjunto de ventanas podrían tener en comun. Por ejemplo:
En un grupo de botones, que ciertos botones tengan el mismo comportamiento cuando el usuario clickea. <br>
A la información que es unica por cada ventana se la llama ___instance data___. <br>
Cada ventana debe asociarse a una _window class_. __ACLARACIÓN__: no son clases de _C++_, son mas o menos como una estructura de datos que el SO
usa internamente. Para registrar una _window class_ se rellena la estructura __WNDCLASS__:
```C++
// Register the window class.
const wchar_t CLASS_NAME[]  = L"Sample Window Class";

WNDCLASS wc = { };

wc.lpfnWndProc   = WindowProc;
wc.hInstance     = hInstance;
wc.lpszClassName = CLASS_NAME;
```
- ___lpfnWndProc___: puntero a una función llamada _window procedure_. Dicha función define los comportamientos de la ventana.
- ___hInstance___: _handle_ a la instancia de la aplicacion. Este valor se obtiene del parametro _hInstance_ de ___wWinMain___.
- ___lpszClassName___: string que identifica el nombre de la _window class_.
Si se va a usar algún _Windows control_ el nombre de la clase debe ser adecuado, por ejemplo: la _window class_ para un botón sería _"Button"_. <br>
La estructura _WNDCLASS_ tiene otros parámetros, estos pueden ser 0. Ver la documentación para mas info. <br>
Luego pasarle la direccion de memoria de _WNDCLASS_ a la funcion _RegisterClass_, la cual registrará la clase con el SO:
```C++
RegisterClass(&wc);
```
### Creando la ventana
Para crear una nueva instancia de una ventana se utiliza la función _CreateWindowEx_:
```C++
HWND hwnd = CreateWindowEx(
    0,                              // Optional window styles.
    CLASS_NAME,                     // Window class
    L"Learn to Program Windows",    // Window text
    WS_OVERLAPPEDWINDOW,            // Window style

    // Size and position
    CW_USEDEFAULT, CW_USEDEFAULT, CW_USEDEFAULT, CW_USEDEFAULT,

    NULL,       // Parent window    
    NULL,       // Menu
    hInstance,  // Instance handle
    NULL        // Additional application data
    );

if (hwnd == NULL)
{
    return 0;
}
```
- El primer parámetro permite especificar un comportamiento opcional como puede ser que la ventana sea transparente. Para
que se comporte normalmente se le asigna el valor 0.
- __CLASS_NAME__: es el nombre de la _window class_. Define el tipo de clase que se está creando.
- El _window text_ se utiliza de diferentes formas según el tipo de ventana. Si la ventana tiene _title bar_, el nombre se
mostrará en la parte superior.
- El _window style_ son una serie de _flags_ que definen el _look and feel_ de la ventana. En conjunto estas _flags_ le dan
a la ventana el _title bar_, el borde, el menu, los botones de Minimizar y Maximizar, etc.
- Para el tamaño y posición se pone __CW_USEDEFAULT__ para utilizar los valores por defecto.
- Setea una _parent_ o _owner window_. En caso de no necesitarlo poner NULL.
- El siguiente parámetro define un menu para la ventana, en este caso como no se usa se pone NULL.
- ___hInstance___: explicado previamente.
- El último parámetro es un puntero a un tipo de dato arbitrario _void*_. Este valor se puede usar para pasar una estructura de datos
al _window procedure_.
  
_CreateWindowEx_ retorna un _handle_ a la nueva ventana o 0 en caso de que la función falle. Para mostrar la ventana, se pasa el _handle_
como parámetro a la función _ShowWindow_:
```C++
ShowWindow(hwnd, nCmdShow);
```
- ___hwnd___: es el _handle_ que retorna _CreateWindowEx_.
- ___nCmdShow___: es el parámetro que se utiliza para minimizar o maximizar la ventana.

## Windows Messages
Para poder estructurar el programa para que reciba todas las ordenes que el usuario desee _Windows_ usa ___mensajes___ para comunicarse con el sistema operativo. Dicho mensaje es un __valor numérico__ que representa un determinado __evento__ (clicks, teclas, etc). Un ejemplo del mensaje que se recibiría si el usuario presiona el click izquierdo del mouse:
```C++
#define WM_LBUTTONDOWN    0x0201
```
Algunos mensajes tienen información asociada a ellos. Por ejemplo _WM_LBUTTONDOWN_ incluye las coordenadas XY del cursor del mouse.
## Message Loop
Una aplicación común y corriente recibe miles de mensajes mientras esta funcionando. Para que esto funcione correctamente se necesita un ___loop___ que reciba los mensajes y los envíe a destino. <br>
El sistema operativo crea una ___queue___ para los mensajes de una aplicación determinada. Si bien esta cola no se puede manipular a gusto (ya que está oculta), es posible obtener un mensaje llamando a la funcion ___GetMessage___:
```C++
MSG msg;
GetMessage(&msg, NULL, 0, 0);
```
Esta funcion remueve el primer mensaje de la cola, en caso de que la cola este vacía la función se bloquea hasta que haya un mensaje (este bloqueo no afecta al funcionamiento del programa, ya que si no hay mensajes, el programa no tiene nada por hacer). <br>
Con respecto a los parametros:
- ___MSG___: si la funcion se ejecuta correctamente, la información del mensaje se guardará en la estructura _MSG_.

El resto de parámetros permiten filtrar los mensajes que se obtienen de la cola, por lo general siempre serán 0. <br> <br>
Si bien la estructura ___MSG___ contiene información del mensaje, nunca se la va a utilizar directamente, en cambio, se utilizara mediante otras 2 funciones:
```C++
TranslateMessage(&msg); 
DispatchMessage(&msg);
```
- ___TranslateMessage___: traduce tecleados del usuario en caracteres. Siempre se la llama antes de _DispatchMessage_.
- ___DispatchMessage___: le dice al sistema operativo que busque el _window handle_ de la ventana y la invoque.

Ejemplo:
1. The operating system puts a WM_LBUTTONDOWN message on the message queue.
2. Your program calls the GetMessage function.
3. GetMessage pulls the WM_LBUTTONDOWN message from the queue and fills in the MSG structure.
4. Your program calls the TranslateMessage and DispatchMessage functions.
5. Inside DispatchMessage, the operating system calls your window procedure.
6. Your window procedure can either respond to the message or ignore it.

 <br>
El loop para los mensajes quedaría de la siguiente manera:

```C++
MSG msg = { };
while (GetMessage(&msg, NULL, 0, 0) > 0)
{
    TranslateMessage(&msg);
    DispatchMessage(&msg);
}
```
___GetMessage___ retorna valores distintos a 0, por lo tanto, para salir del loop se utiliza la función ___PostQuitMessage___, la cual pone el mensaje _WM_QUIT_ en la cola de mensajes.<br><br>
_WN_QUIT_ es un mensaje especial, lo que hace es darle el valor 0 a _GetMessage_:
```C++
PostQuitMessage(0);
```

## Posted Messages versus Sent Messages
La diferencia entre _posted messages_ y _sent messages_ es que los ___sent messages___ __NO VAN A LA MESSAGE QUEUE__. 

## Writing the Window Procedure
La funcion _DispatchMessage_ llama al window procedure de la ventana correspondiente al mensaje. El _window procedure_ tiene la siguiente firma:
```C++
LRESULT CALLBACK WindowProc(HWND hwnd, UINT uMsg, WPARAM wParam, LPARAM lParam);
```
- ___hwnd___: _handle_ a la ventana.
- ___uMsg___: message code. Por ej: _WM_SIZE_.
 ___wParam___ y ___lParam___: información adicional del mensaje, varia segun el message code.
 - ___LTRESULT___: un entero que representa la respuesta del programa a un determinado mensaje. _CALLBACK_ es una calling convention.

 Usualmente un _window procedure_ es simplemente un gran _switch_ que engloba los mensajes a los que se busque responder.
 ```C++
switch (uMsg)
{
    case WM_SIZE: // Handle window resizing

    // etc
}
 ```
 Como se mencionó previamente, los parametros _lParam_ y _wParam_ contienen información adicional acerca del mensaje. Ambos son __enteros__. El significado de cada uno depende del message code _uMsg_. Para cada mensaje hay que revisar la info en MSDN para castear los parametros al tipo de dato adecuado, usualmente son valores numericos o punteros a struct. <br>
 Por ejemplo, la documentación para _WM_SIZE_ dice lo siguiente:
 - ___wParam___: flag que indica si la ventana fue minimizada, maximizada o resized.
 - ___lParam___: contiene el alto y ancho de la ventana en valores de 16 bit, guardados en un numero de 32 o 34 bits. Para obtener esta informacion se utilizan una serie de macros que se verán luego.
 
 Un _window procedure_ suele ser muy extenso, debido a la cantidad de mensajes que hay. Una buena practica para mantener el codigo modular es poner la lógica que responda a los mensajes en diferentes funciones, separadas del _switch_. Por ejemplo (acá van las macros que se mencionó previamente):
 ```C++
LRESULT CALLBACK WindowProc(HWND hwnd, UINT uMsg, WPARAM wParam, LPARAM lParam)
{
    switch (uMsg)
    {
    case WM_SIZE:
        {
            int width = LOWORD(lParam);  // Macro to get the low-order word.
            int height = HIWORD(lParam); // Macro to get the high-order word.

            // Respond to the message:
            OnSize(hwnd, (UINT)wParam, width, height);
        }
        break;
    }
}

void OnSize(HWND hwnd, UINT flag, int width, int height)
{
    // Handle resizing
}
 ```
 Como se puede ver, la función _OnSize_ se extrajo del _switch_ para mantenerlo mas limpio y corto. <br>
 Las macros _LOWORD_ y _HIWORD_ obtienen los valores en 16-bit de la altura y anchura. El _window procedure_ extrae esos valores y se los pasa a la funcion _OnSize_.

 ## Default message handling
Si en el _window procedure_ no se trabaja con X mensaje en particular, se pasan los parámetros del mensaje directamente a la función _DefWindowProc_. Esta función realiza la acción predeterminada para el mensaje, la cual varía según el tipo de mensaje.
```C++
return DefWindowProc(hwnd, uMsg, wParam, lParam);
```
## Evitar cuellos de botellas en el window procedure
While your window procedure executes, it blocks any other messages for windows created on the same thread. Therefore, avoid lengthy processing inside your window procedure. For example, suppose your program opens a TCP connection and waits indefinitely for the server to respond. If you do that inside the window procedure, your UI will not respond until the request completes. During that time, the window cannot process mouse or keyboard input, repaint itself, or even close. <br>
Instead, you should move the work to another thread, using one of the multitasking facilities that are built into Windows:
- Create a new thread.
- Use a thread pool.
- Use asynchronous I/O calls.
- Use asynchronous procedure calls.