# Indice
- [Indice](#indice)
- [Selectores](#selectores)
  - [Selector universal](#selector-universal)
  - [Selectores de tipo](#selectores-de-tipo)
  - [Selectores por clase](#selectores-por-clase)
  - [Selectores por ID](#selectores-por-id)
  - [Selectores por atributo](#selectores-por-atributo)
  - [Selectores descendientes](#selectores-descendientes)
  - [Selectores por pseudoclases](#selectores-por-pseudoclases)
- [Especificidad](#especificidad)
  - [Metodología BEM](#metodología-bem)
- [Medidas](#medidas)
  - [Relativas](#relativas)
    - [EM (por defecto:1em=16px).](#em-por-defecto1em16px)
    - [Viewport](#viewport)
  - [Fijas](#fijas)
- [Normalize](#normalize)
  - [Cambios importantes para _normalize.css_](#cambios-importantes-para-normalizecss)
- [Propiedades](#propiedades)
  - [Texto](#texto)
    - [Tipografías externas](#tipografías-externas)

# Selectores
Sintaxis:
```css
selector
{
    propiedad: valor;
}
```
## Selector universal
Selecciona todos los elementos. Por ejemplo:
```css
*
{
    color: red;
}
```
## Selectores de tipo
Selecciona el elemento que se quiera modificar:
```css
h1
{
    color: red;
}
```
## Selectores por clase
Se agrega al tag de _HTML_ un atributo _class="nombre-clase"_. Luego se selecciona dicha clase en _CSS_:
```css
.nombre-clase
{
    color: red;
}
```
## Selectores por ID
Muy similares a los selectores por clase, la principal diferencia sería que idealmente los _ID_ deberían ser únicos por elemento:
```css
#id-elemento
{
    color: red;
}
```
## Selectores por atributo
Como su nombre indica, selecciona al elemento en base a un atributo, el cual puede ser creado por el programador:
```css
[atributo="si"]
{
    color: red;
}
```
## Selectores descendientes
Afectan a un tag "hijo" especificado. Supóngase que se tiene un _h2_ con un _p_ adentro:
```css
h2 p
{
    color: red;
}
```
Los _h2_ que no tengan _p_ no se verán afectados. Vale aclarar que también se pueden utilizar con __selectores por clase__ y con más de un "hijo":
```css
.nombre-clase p b
{
    color: red;
}
```
## Selectores por pseudoclases
Ejemplo que modifique el color cuando el mouse se posiciona sobre dicho elemento:
```css
h2:hover
{
    color: yellow;
}
```

# Especificidad
El concepto de especificidad soluciona el problema de la redefinición de estilos basándose en un sistema jerárquico. Dicho de otras palabras, supóngase que se tiene lo siguiente:
```css
b
{
    color: red;
}
b
{
    color: blue;
}
b
{
    color: green;
}
```
En este caso se esta modificando al tag _b_ 3 veces, a diferencia de la lógica de los lenguajes de programación donde la "asignación" predominante (la salida) es la última, en _CSS_ lo que sucede es que el tag _b_ va a ser primero rojo, luego azul y por ultimo verde, todo en un periodo de tiempo tan corto que no se puede visualizar. <br>
El sistema jerárquico que se mencionó previamente vendría a poner un poco de orden en todo este tema, ya que suponiendo que se haga el mismo cambio del ejemplo previo pero el de color azul tiene mas poder jerárquico que el rojo, este último se verá perjudicado. <br>
De más jerarquía a menor, el sistema se vería así:

1. ___!important___
2. ___estilos en línea___
3. ___identificadores___
4. ___clases___, ___pseudoclases___ y ___atributos___
5. ___elementos___ y ___pseudoelementos___

Supongase que se tiene una clase dentro de un _h1_ llamada ___clase-h1___:
```css
.clase-h1
{
    color: red;
}
h1
{
    color: blue;
}
```
El resultado final será red, ya que las clases tienen más poder jerárquico. Para que sea blue habría que utilizar el ___!important___, ya que tiene más jerarquía que las clases:
```css
.clase-h1
{
    color: red;
}
h1
{
    color: blue !important; 
}
```

## Metodología BEM

Permite una mejor organización para evitar conflictos utilizando nombres de clases. Se tiene:

```html
<div class="form">
        <input type="text">
        <input type="password">
</div>
```

La aplicación de la metodología se da respetando la siguiente regla de nombramiento:

```html
<div class="form">
        <input type="text" class="form__input">
        <input type="password" class="form__input">
</div>
```

Ahora supóngase que se tiene varios input y se quiera estilizar uno de ellos, la referencia en _CSS_ sería de la siguiente manera:

```css
.form__input:first-child
{
    color: red;
}
```

La otra forma sería manteniendo la regla de nombramiento previa pero agregando una característica extra que diferencie al elemento que se quiera referir:

```html
<div class="form">
        <input type="text" class="form__input--text">
        <input type="password" class="form__input">
</div>
```

```css
.form__input--text
{
    color: red;
}
```

Resumiendo, la regla de nombramiento quedaría de la siguiente manera:

```html
class="bloque-div__tipo--especificador"
```

Un caso particular sería al tener un bloque dentro de otro, ya sea un _div_ dentro de otro o un _h2_ dentro de un _p_ por poner otro ejemplo. En estos casos vale aclarar que hay que utilizar un _-_, ya que no se debe agregar más de un __:

```html
<div class="contact-form">
    <p class="contact-form__p">
        <h2 class="contact-form__p-h2"></h2>
    </p>
</div>
```

# Medidas

## Relativas

- Dependen de un objeto.
- Son variables.
- Útiles para adaptar la página a móviles (_responsive design_).

### EM (por defecto:1em=16px).

```css
.contact-form
{
    font-size: 20px;
}
```

La definición que se le dio en tamaño a la caja contenedora, "modifica" el valor default de _1em_, por lo que cuando se refiera posteriormente a medidas en _em_, estas sean relativas a la asignación que se dio inicialmente:

```css
.contact-form__h2
{
    font-size: 5em
}
```

En este caso _5em = 100px_. <br>

Vale aclarar que esta relación no solo se da con la propiedad _font-size_, sino que se comparte con todas las propiedades que se manejen con medidas.

### Viewport

Refieren a las dimensiones del dispositivo. Por ejemplo:

```css
.contact-form
{
    font-size: 25px;
    background-color: black;
    width: 100vw;
}
```

- __100 _vw___: viewport width. Refiere al ancho total de la pantalla. En caso de que fuera _50 vw_ sería la mitad del ancho.
- El mismo comportamiento se da con ___vh___ (viewport height).

## Fijas

- No varían.
- _Pixeles, milímetros, centímetros, etc_.

```css
.contact-form__h2
{
    font-size: 50px;
}
```

# Normalize

El navegador tiene ciertas propiedades por defecto, como el color de la letra, el margen, entre otras. Para resetear estos estilos se utiliza el __normalize__.

```htt
https://necolas.github.io/normalize.css/
```

Se puede descargar y pegar el archivo en el proyecto o directamente poner:

```html
<link rel="stylesheet" href="https://necolas.github.io/normalize.css/">
```

## Cambios importantes para _normalize.css_

- Para que las imágenes en dispositivos móviles se ajuste al 100% y no se pase.

```css
img {
  border-style: none;
  max-width: 100%;
}
```

- Para que el tamaño de las cajas sea constante al valor que se quiere y no varíe según las propiedades.

```css
*{
   box-sizing: border-box;
   padding: 0;
   margin: 0;
 }
```

# Propiedades

## Texto

- ___font-size___: cambia el tamaño de la letra.

```css
font-size: 2em;
```

- ___font-family___: tipografía.

```css
font-family: Georgia;
```

---

### Tipografías externas

```htt
https://fonts.google.com/
```

1. Copiar el link. Ej: <link href="https://fonts.googleapis.com/css2?family=**Roboto:wght@100**&display=swap" rel="stylesheet">
2.  Pegarlo en el _head_ del ___html___.
3.  Para implementarlo copiar el _font-family_ que aparece en la página. Ej: 

```cs
font-family: 'Roboto', sans-serif;
```

La segunda font representa el "plan b" en caso de no encontrar la primera.

---

- ___line-height___: 1 corresponde al alto que ocupa una letra normalmente (0.5 para arriba y 0.5 para abajo), comenzando desde el centro.

```css
line-height: 1.9;
```

- ___font-weight___: grosor. Ej: ___normal___, ___800___, ___etc___.

```css
font-weight: normal;
```

- ___display___: corresponde al comportamiento de la caja, _inline_ (ocupa un espacio en la línea, por el el tag ___b___) o _block_ (ocupa toda la línea, por ej el tag ___h2___).

```css
display: inline;
```



