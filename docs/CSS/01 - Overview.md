# CSS

[Video explicativo](https://www.youtube.com/watch?v=TlJbu0BMLaY&t=1h42m25s)

[Video animaciones CSS](https://www.youtube.com/watch?v=RwjgfNX41TE)

Las siglas CSS vienen de "Cascading Style Sheets" que significa "Hojas de Estilo en Cascada". Y se utiliza para **describir la apariencia/diseño de un documento HTML**.

La palabra **cascada** se refiere a que las reglas se aplican en orden, y si hay una regla que se repite, la última tiene prioridad. Por ejemplo, si en un documento HTML se define un color de texto para un elemento, y luego se define otro color de texto para el mismo elemento, el último color de texto será el que se aplicará.

## Recursos útiles

- [Lenguaje CSS](https://lenguajecss.com/css)
- [MDN](https://developer.mozilla.org/es/docs/Web/CSS)
- [Curso google](https://web.dev/learn/css?hl=es)
- [CodiLink](https://codi.link/)

## Conceptos básicos

Los estilos se pueden definir en cualquier parte del archivo HTML, pero generalmente se debe hacer en el apartado `<style>` que se encuentra dentro de la etiqueta `<head>`.

### Comentarios en CSS

Un comentario en CSS se define con `/*` y `*/`. Por ejemplo:

```css
/* Esto es un comentario */
```

### Selectores y declaraciones

Un selector es una forma de seleccionar un elemento HTML para aplicarle estilos. Por ejemplo, si se quiere seleccionar todos los elementos `<p>` (parágrafos), se puede usar el selector `p`.

Una declaración es una forma de aplicar estilos a un selector. Por ejemplo, si se quiere aplicar un color de texto a todos los elementos `<p>`, se puede usar la declaración `color: red`.

```css
p { /* Selector */
    color: red; /* Declaración */
}
```

### Usar clases para definir estilos

Es recomendable agrupar elementos html en clases o grupos para aplicar estilos, se suelen usar las clases o ids para identificar los elementos. 

- Podemos definir el estilo de una **clase** con el selector `.<nombre_clase>`. 
    - Por ejemplo:

        ```html
        <a class="link" href="https://google.com"> Google </a>
        ```

        ```css
        .link {
            color: red;
        }
        ```
- Podemos definir el estilo de un **id** con el selector `#nombre_id`.
    > Nota: No es recomendable abusar de los ids para definir estilos.
    - Por ejemplo:

        ```html
        <a id="link" href="https://google.com"> Google </a>
        ```

        ```css
        #link {
            color: red;
        }
        ```

### Herencia

En CSS, muchas de las declaraciones se heredan, por lo que si se define una declaración en un selector padre, se aplicará a todos los elementos hijos.

No todas las declaraciones se heredan, por ejemplo, `border` no se hereda, por lo que si se define en un selector padre, no se aplicará a los elementos hijos. Pero hay formas forzar esta herencia con la declaración `inherit`.

```html
<div class="container">
    Este es el container
    <div class="child">
        Este es el hijo
    </div>
</div>
```

```css
.container {
    border: 1px solid red;
}

.child {
    border: inherit;
}
```

#### otras formas de herencia

- `initial`: establece el valor inicial de la declaración.
- `unset`: establece el valor inicial de la declaración si es una propiedad heredada, o `inherit` si no lo es.
- `inherit`: establece el valor del selector padre.
- `revert`: establece el valor del selector padre si es una propiedad heredada, o `initial` si no lo es.

### pseudo-clases

Las `pseudo-clases` se usan para seleccionar elementos en un estado específico. Por ejemplo, si se quiere realizar algun cambio en el estilo cuando se tiene el ratón sobre dicho elemento, se puede usar la pseudo-clase `:hover`.

```css
p:hover {
    color: red;
}
```

### pseudo-elementos

Los pseudo-elementos se usan para seleccionar una parte específica de un elemento. Por ejemplo, si se quiere seleccionar el primer parágrafo de un elemento `<p>`, se puede usar el pseudo-elemento `::first-line`.

```css
p::first-line {
    color: red;
}
```

#### Tipos de pseudo-clases

| Pseudo-clases | Descripción |
| --- | --- |
| `:hover` | Selecciona el elemento cuando se tiene el ratón sobre él. |
| `:active` | Selecciona el elemento cuando se está haciendo clic sobre él. |
| `:focus` | Selecciona el elemento cuando tiene el foco. Por ejemplo cuando tenemos seleccionado para rellenar un formulario. |
| `:checked` | Selecciona el elemento cuando está marcado. Por ejemplo cuando tenemos seleccionado un checkbox. |
| `:disabled` | Selecciona el elemento cuando está deshabilitado. |
| `:enabled` | Selecciona el elemento cuando está habilitado. |
| `:empty` | Selecciona el elemento cuando está vacío. |
| `:first-child` | Selecciona el elemento cuando es el primer hijo. |
| `:last-child` | Selecciona el elemento cuando es el último hijo. |
| `:nth-child(n)` | Selecciona el elemento cuando es el n- simo hijo. |
| `:nth-last-child(n)` | Selecciona el elemento cuando es el n- simo hijo contando desde el final. |
| `:nth-of-type(n)` | Selecciona el elemento cuando es el n- simo hijo de su tipo. |
| `:nth-last-of-type(n)` | Selecciona el elemento cuando es el n- simo hijo de su tipo contando desde el final. |
| `:only-child` | Selecciona el elemento cuando es el único hijo. |
| `:only-of-type` | Selecciona el elemento cuando es el único hijo de su tipo. |
| `:not(selector)` | Selecciona el elemento cuando no coincide con el selector. |
| `:target` | Selecciona el elemento cuando es el objetivo de la URL. |
| `:lang(language)` | Selecciona el elemento cuando tiene el idioma especificado. |
| `:root` | Selecciona el elemento raíz del documento. |
| `:scope` | Selecciona el elemento que es el ámbito actual. |
| `:is(selector)` | Selecciona el elemento cuando coincide con el selector. |
| `:has(selector)` | Selecciona el elemento cuando tiene un hijo que coincide con el selector. |

### Selectores combinados

Cada selector puede combinarse con otros selectores para seleccionar elementos de manera más precisa.

En el siguiente ejemplo, sólo el primer párrafo es el que aplica el color rojo.

```html
<p class="description">
    Hola <strong class="bold"> Mundo </strong>
</p>
<p class="secondary">
    Hola <strong class="bold"> Mundo </strong>
</p>
```
```css
.description .bold {
    color: red;
}
```

#### Aplicar estilos al hijo directo

Usando el símbolo `>` se puede definir que un estilo se aplique sólo al hijo directo.

```html
<article>
    <p>Hola <strong class="bold"> Mundo </strong></p>
    <footer>
        <p>
            Hola <strong class="bold"> Mundo </strong>
        </p>
    </footer>

</article>
```

```css
article p { /* Aplica color a ambos párrados */
    color: red;
}

article > p { /* Aplica color sólo al párrafo en el primer nivel (hijo directo)*/
    color: red;
}
```

#### Aplicar estilos a los hermanos siguientes

Usando el símbolo `~` se puede definir que un estilo se aplique sólo a los hermos siguientes, esto no aplica a los elementos anteriores.

```html
<article>

    <span>Texto en negro</span>
    <p>Párrafo</p>
    <span>Texto en rojo</span>
    <span>Texto en rojo</span>
    <code>Código</code>
    <span>Texto en rojo</span>

</article>

<span>Texto en negro porque está fuera del article, por lo que el párrafo anterior no es hermano.</span>

<p>Párrafo</p>
<span>Texto en rojo</span>
```

```css
p ~ span { /* Aplica color sólo a los span que hay tras un párrafo (hermanos siguientes)*/
    color: red;
}
```

#### Aplicar estilo al hermano directamente siguiente

Usando el símbolo `+` se puede definir que un estilo se aplique sólo al hermano directamente siguiente.

```html
<span>Texto en negro</span>
<p>Párrafo</p>
<span>Texto en rojo</span>
<span>Texto en negro</span>
<span>Texto en negro</span>

<p>Párrafo</p>
<code>Código</code>
<span>Texto en rojo</span>
<span>Texto en negro</span>
```

```css
p + span { /* Aplica color sólo al span que está tras un párrafo (hermano directamente siguiente)*/
    color: red;
}
```

### Cascada

Este es uno de los puntos más problemáticos con CSS y es el que implica que se puedan sobreescribir las variables de estilo. CSS sigue una regla de cascada, es decir, que el último estilo que se defina es el que se aplicará.

```css
p {
    font-size: 16px;
    color: red;
    font-weight: bold;
}

p {
    color: blue;
}
```

En este caso, el color del párrafo será azul, ya que es el último estilo que se define, pero mantendrá las modificaciones de la fuente.

#### Fallback

Esta es una característica que se usa cuando queremos definir un estilo que quizás no está listo en todos los operadores (experimentales o muy nuevos). Para comprobar si un navegador soporta una función, se puede usar la web [Can I Use](https://caniuse.com).

En el ejemplo siguiente, el color del párrafo será el que se genere con `oklch`, pero si ese método no está disponible para un navegador dará error, pero gracias al `fallback` usará el siguiente color que sería azul.

```css
p {
    font-size: 16px;
    color: red;
    font-weight: bold;
}

p {
    color: blue;
    color: oklch(100% 0.1 19);
}
```

En este caso, el color del párrafo será azul, ya que es el último estilo que se define, pero mantendrá las modificaciones de la fuente.

#### Nivel del estilo

CSS tiene una regla para definir niveles y así aplicar el estilo del selector con mayor nivel de "especificación".

El nivel de "especificación" se define con una regla X.Y.Z, donde dependiendo del selector, dicho nivel es diferente. Se puede usar la web [Specificity Calculator](https://specificity.keegan.st) para calcular el nivel de "especificación".

En el siguiente ejemplo se aplicaría el estilo definido en `.text` al ser el selector con mayor nivel.

```css
.text {
    color: red;
}
/*
Nivel 0.1.0
0 - IDs
1 - Classes, attributes and pseudo-classes
0 - Elements and pseudo-elements
*/

p {
    color: blue;
}

/*
Nivel 0.0.1
0 - IDs
0 - Classes, attributes and pseudo-classes
1 - Elements and pseudo-elements
*/
```

En el caso de que dos selectores tengan el mismo nivel de especificación, se aplicará el último estilo que se defina.

#### Comprobar especificación en el navegador

Mediante las herramientas de desarrollo del navegador, en el panel de Estilos es posible identificar qué reglas de estilos se están sobrescribiendo. Además, al situar el puntero sobre un selector, se indica su especificidad.

##### Estilo en línea

Es posible definir los estilos directamente en el código HTML, y este tipo de estilos tiene la máxima especificidad.

```html
<p style="color: red;">Hola Mundo</p>
```

Pero cabe destacar que esto no es una buena práctica.

##### Importancia

Al definir una variable como importante con `!important`, se indica que es la más importante y por lo tanto se aplicará, independientemente de la especificidad.

```css
p {
    color: blue !important;
}
```

En caso de tener más de una regla con `!important`, se aplicará la última siguiendo el orden de las reglas de especificación.

Pero cabe destacar que esto no es una buena práctica, a excepción de cuando se está sobrescribiendo un estilo de un framework, librería o momentos puntuales.

### Unidades

#### Píxeles

La unidad más utilizada son los píxeles definidos con el símbolo `px`.

> Nota: Estamos hablando de píxeles, pero no píxeles reales como tal, es una unidad relativa que dependiendo de la resolución de la pantalla agrupará más o menos píxeles reales.

```html
<div class="container">
</div>
```
```css
.container {
    width: 250px;
    height: 250px;
    color: red;
}
```

#### Porcentaje

La unidad porcentaje se define con el símbolo `%` y se refiere al 100% del valor del elemento padre.

```html
<div class="container">
    <div class="item"></div>
</div>
```
```css
.container {
    width: 250px;
    height: 250px;
    color: red;
}

.item {
    width: 50%;
    height: 50%;
    color: blue;
}
```

#### Viewport

La unidad Viewport se define con los símbolos `vw` y `vh`, estos valores indican el porcentaje de ancho y alto de la ventana del navegador.

```html
<div class="container">
</div>
```
```css
.container {
    width: 250vw;
    height: 50vh;
    color: red;
}
```

### Modelo de la caja

El modelo de la caja es el que define cómo se dibujan los elementos en la página.

Dependiendo del elemento HTML usará por defecto estilo en línea o estilo en bloque.

#### Estilo en línea

Los nuevos elementos se comportan como texto, no se puede modificar alto ni ancho, y se añadirá a la derecha del anterior.

```html
<span>Hola Mundo</span>
<span>Hola Mundo</span>
```

#### Estilo en bloque

Los nuevos elementos se comportan como bloque, se puede modificar alto y ancho, y se añadirá debajo del anterior.

```html
<div>Hola Mundo</div>
<div>Hola Mundo</div>
```

##### Padding

Indica el relleno, es decir, el espacio alrededor del contenido. Se puede definir en general o en una sóla dirección.

```css
.container {
    padding: 10px; /*Mismo padding en todas las direcciones*/
    padding: 10px 20px; /*Arriba/Abajo Derecha/Izquierda*/
    padding: 10px 20px 5px 0px; /*Arriba Derecha Abajo Izquierda*/
}
```

##### Border

Indica el borde, es decir, el contorno alrededor del padding y del contenido. 

```css
.container {
    border: 1px solid red; 
}
```

##### Box-Sizing

Box-sizing indica cómo se añade el border y el padding al ancho y alto del elemento.

- **Contenido por defecto (content-box)**: width/height no incluyen padding ni border. El tamaño total = width + padding + border.

- **border-box**: width/height incluyen padding y border. Útil para que el ancho visual no crezca al añadirlos.

```css
.container{
    box-sizing: border-box;
}
```


## Otros conceptos

### Colores en CSS

En CSS, los colores se pueden definir de varias maneras:

- Usando el nombre del color (por ejemplo, `red`)
- Usando el código hexadecimal (por ejemplo, `#FF0000`)
- Usando el código RGB (por ejemplo, `rgb(255, 0, 0)`)
- Usando el código RGB con transparencia (por ejemplo, `rgb(255 0 0 / 50%)`)
- Usando el código HSL (por ejemplo, `hsl(0, 100%, 50%)`)
- Usando el código oklch (por ejemplo, `oklch(100% 0.1 19)`)
    - oklch (lightness, chroma, hue) permite representar una gama más amplia de colores que los anteriores.
    - Es bastante nuevo, quizás no todos los navegadores lo soporten.
    - Existen calculadoras para ayudarnos a elegir colores en oklch.

```css
p {
    color: red;
    color: #FF0000;
    color: rgb(255, 0, 0);
    color: rgba(255 0 0 / 50%);
    color: hsl(0, 100%, 50%);
    color: oklch(100% 0.1 19);
}
```

### Current color

Este valor permite referirse al color que se está usando en el selector actual. Por ejemplo, si se quiere aplicar el mismo color de texto que se está usando en el selector actual, se puede usar el valor `currentColor`.

```css
p {
    color: red;
    border-color: currentColor;
}
```

### Definir font-family

`font-family` es una declaración que se usa para definir la fuente que se va a usar en el documento. 

Es una declaración que se hereda, por lo que si se define en el selector `body`, se aplicará a todos los elementos. 

Se pueden definir más de una fuente y el sistema intentará usar la primera que esté disponible. Por defactor se suele dejar la fuente `sans-serif` como última opción, ya que suele ser las más universal.

Por ejemplo:

```css
body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}
```

### Borde VS contorno

El `border` es el contorno que se dibuja alrededor de un elemento, mientras que el `outline` es el contorno que se dibuja alrededor del `border`. 

El `outline` no ocupa espacio, por lo que no afecta al tamaño del elemento.

```css
p {
    border: 1px solid red;
    outline: 1px solid red;
}
```

