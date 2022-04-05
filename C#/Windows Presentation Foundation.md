# Overview - Aclaraciones

## Estructura

Los proyectos **WPF** consisten a grandes rasgos de un archivo ***XAML***, el cuál define la pantalla, su estructura y diseño; y los archivos ***C#***.

Dentro de ***MainWindow.xaml.cs*** es donde se define la lógica del programa, un ejemplo sería el comportamiento de algún botón.

## Algunas recomendaciones

Dentro de *Visual Studio* se puede ver la ventana de herramientas, la cual contiene todos los elementos que se le pueden poner a la ventana. Si bien es posible arrastrar cada elemento a la misma **es recomendable diseñar la ventana utilizando el *XAML***.

Cada elemento de la ventana tiene unas determinadas **propiedades**, estas se pueden editar en la ventana *Propiedades* o directamente desde el *XAML*.

## Arrastrar vs *XAML*

¿Por qué se recomienda diseñar directamente en el *XAML?* La respuesta es evidente cuando se ve el comportamiento de cada elemento al arrastrarlo a la ventana. Se puede ver que ni bien se lo suelta en la misma aparece el código equivalente el *XAML* hardcodeado, las propiedades de *Height* y *Width* toman el valor equivalente al lugar donde se soltó el elemento inicialmente, a su vez se agregan otras propiedades que por ahí inicialmente no se necesitan.

No es necesario deshechar por completo la herramienta de *Visual Studio*, se puede continuar arrastrando los elementos pero no hay que olvidarse de borrar las propiedades que se generan en el *XAML*.
