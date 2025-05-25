# Elementos HTML

HTML consta de más de 100 elementos, pero todos ellos se definen con etiquetas.

```html
<nombre_etiqueta>Contenido de la etiqueta</nombre_etiqueta>
```

* Toda la línea anterior sería un **elemento HTML**. 
* Mientras que `<nombre_etiqueta>` es la **etiqueta HTML**.
    * Dentro de las etiquetas podemos distinguir **etiqueta de apertura** y **etiqueta de cierre**. Estas etiquetas siempre deben coincidir dentro de un mismo elemento.

Existen 3 tipos de elementos dentro de HTML:
* Elementos normales (o contenedores).
* Elementos reemplazables.
* Elementos vacíos.

> Documentación sobre elementos HTML: https://developer.mozilla.org/en-US/

## Elementos normales

Son aquellos que tienen una etiqueta de apertura y una de cierre, y pueden contener texto, otros elementos o ambos.

### Títulos

#### Título de la web

Los títulos son elementos HTML que permiten establecer un título, se definen con la etiqueta `<title>` en el `head`. Este es el nombre que aparece en la pestaña del navegador y en el buscador de google.

```html
<title>Contenido de Título</title>
```

> Es importante para el SEO ya que es el nombre que aparece en el buscador de google.

#### Títulos de contenido

Los títulos son elementos HTML que permiten dar un título a un documento HTML. Estos títulos van del h1 al h6, donde el h1 es el título más importante y el h6 es el título menos importante.

```html
<h1>Contenido de h1</h1>
<h2>Contenido de h2</h2>
<h3>Contenido de h3</h3>
<h4>Contenido de h4</h4>
<h5>Contenido de h5</h5>
<h6>Contenido de h6</h6>
```

### Párrafo

Los párrafos son elementos HTML que permiten establecer un párrafo de texto, se definen con la etiqueta `<p>`.

```html
<p>Contenido de párrafo</p>

```

### Modificadores de texto
#### Strong

Enfatiza un texto, se definen con la etiqueta `<strong>`.

```html
<strong>Contenido de strong</strong>
```

### Énfasis

Enfatiza un texto, se definen con la etiqueta `<em>`.
```html

<em>Contenido de em</em>
```

#### small

```html
<small>Contenido de small</small>
```

### Lista desordenada

Las listas desordenadas son elementos HTML que permiten establecer una lista de elementos, se definen con la etiqueta `<ul>`. Cada elementos de la lista se representa con la etiqueta `<li>`.

```html
<ul>
    <li>Contenido de li 1</li>
    <li>Contenido de li 2</li>
    <li>Contenido de li 3</li>
</ul>
```

Nota: Hay casos en los que no hace falta la etiqueta de cierre. Como los `<li>`. Pero **no es recomendable**, luego en el navegador automáticamente se cierran.

```html
<ul>
    <li>Contenido de li 1
    <li>Contenido de li 2
    <li>Contenido de li 3
</ul>
```

### Lista ordenadas

Las listas ordenadas son elementos HTML que permiten establecer una lista de elementos, se definen con la etiqueta `<ol>`. Cada elementos de la lista se representa con la etiqueta `<li>`.

```html
<ol>
    <li>Contenido de li 1</li>
    <li>Contenido de li 2</li>
    <li>Contenido de li 3</li>
</ol>
```

#### Enumeración

Dentro de las listas ordenadas podemos elegir cómo queremos que se enumere con el atributo `type`.

* Ordenar numérico: `type="1"`
* Ordenar alfabético en mayúsculas: `type="A"`
* Ordenar alfabético en minúsculas: `type="a"`
* Ordenar en romanos en mayúsculas: `type="I"`
* Ordenar en romanos en minúsculas: `type="i"`

```html
<ol type="I">
    <li>Contenido de li 1</li>
    <li>Contenido de li 2</li>
    <li>Contenido de li 3</li>
</ol>
```

#### Inversión

Podemos indicar si queremos invertir el orden de la lista con el atributo `reversed`.

```html
<ol reversed>
    <li>Contenido de li 1</li>
    <li>Contenido de li 2</li>
    <li>Contenido de li 3</li>
</ol>
```

#### Inicio

Podemos indicar el número desde el que queremos que comience la lista con el atributo `start`.

```html
<ol start="5">
    <li>Contenido de li 1</li>
    <li>Contenido de li 2</li>
    <li>Contenido de li 3</li>
</ol>
```

### Formularios

Los formularios son elementos HTML que permiten establecer un formulario, se definen con la etiqueta `<form>`.

```html
<form method="post" action="/<url donde se deben enviar los datos>">
    <fieldset>
        <legend>Información personal</legend>
        <div>
            <label for="name">Nombre:</label>
            <input type="text" name="name" id="name" placeholder="Introduce tu nombre" required pattern="[a-zA-Z ]+">
        </div>
        <!-- Otra forma quitando el for del label, sin usar id ni el div -->
        <label style="display: block;">Apellidos:
            <input type="text" name="apellidos" placeholder="Introduce tus apellidos">
            <input name="telefono" type="number" placeholder="Introduce tu teléfono" required pattern="[0-9]{9}">
        </label>
        <div>
            <datalist id="languages-list">
                <option value="HTML">
                <option value="CSS">
                <option value="JavaScript">
            </datalist>
            <label>
                <input type="text" name="language" id="language" list="languages-list">
            </label>
        </div>
    </fieldset>
    <button type="submit">Enviar</button>
</form>
```

### Desplegables

Los desplegables son elementos HTML que permiten establecer un desplegable, se definen con la etiqueta `<details>`.

```html
<details>
    <summary>Contenido desplegable</summary>
    <p>Contenido del desplegable</p>
</details>
```

### Tablas

Las tablas son elementos HTML que permiten establecer una tabla, se definen con la etiqueta `<table>`.

```html
<table>
    <thead>
        <tr>
            <th>Contenido de th</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Contenido de td</td>
        </tr>
    </tbody>
</table>
```

### Script

Los scripts son elementos HTML que permiten establecer un script de javascript, se definen con la etiqueta `<script>`.

```html
<script>
    console.log("Contenido del script");
</script>
```

### Dialog

Un dialog (Diálogo) es un elemento HTML que permite establecer un diálogo, se definen con la etiqueta `<dialog>`.

Es una especia de modal, es decir, un cuadro de diálogo que se muestra en una ventana modal. Normalmente se debe abrir cuando se hace clic en un botón.

```html
<dialog open>
    <h1>Contenido del diálogo</h1>
    <p>Contenido del diálogo</p>
    <button>Botón</button>
</dialog>
```

Ejemplo de usar dialog con javascript:

```html
<button id="show-dialog">Abrir diálogo</button>
<dialog id="custom-dialog">
    <h1>Contenido del diálogo</h1>
    <p>Contenido del diálogo</p>
    <button id="close-dialog">Cerrar diálogo</button>
</dialog>

<script>
    window.show-dialog.addEventListener("click", () => {
        window.custom-dialog.showModal();
    });
    window.close-dialog.addEventListener("click", () => {
        window.custom-dialog.close();
    });
</script>
```

## Elementos reemplazables

Son elementos cuyo contenido es reemplazado por recursos externos, como imágenes, videos, iframes, etc. No necesitan etiqueta de cierre.

### Imágenes

Las imágenes son elementos HTML que permiten establecer una imagen, se definen con la etiqueta `<img>`. 

```html
<img src="ruta_de_la_imagen" alt="Texto alternativo">
```

Si añadimos la imagen dentro de un anchor es posible descargar la imagen. Sólo funciona con recursos que tengas dentro de tu dominio.

```html
<a download href="ruta_de_la_imagen">
    <img
        src="ruta_de_la_imagen"
        alt="Texto alternativo"
    >
</a>
```

Posibles atributos de la etiqueta img:

* `src`: ruta de la imagen
* `alt`: texto alternativo
* `width`: ancho de la imagen
* `height`: alto de la imagen
* `title`: título de la imagen
* `loading`: indica cómo se debe cargar la imagen
    * `loading="lazy"`: indica que la imagen se debe cargar de forma diferida.
    * `loading="eager"`: indica que la imagen se debe cargar de forma inmediata.
    * `loading="auto"`: indica que la imagen se debe cargar de forma automática.
* `style`: indica el estilo de la imagen. Ejemplos:
  * `style="width: 100%"`: indica que la imagen se debe ajustar al ancho de su contenedor.
  * `style="height: 100%"`: indica que la imagen se debe ajustar al alto de su contenedor.
  * `style="aspect-ratio: 16 / 9"`: indica que la imagen se debe ajustar al aspecto de su contenedor.

### Videos

Los videos son elementos HTML que permiten establecer un video, se definen con la etiqueta `<video>`.

```html
<video {atributos...} src="ruta_del_video"></video>
```

Posibles atributos de la etiqueta video:

* `controls`: muestra los controles del video
* `autoplay`: reproduce el video automáticamente
* `loop`: repite el video
* `muted`: silencia el video
* `poster`: muestra una imagen mientras se carga el video

### Audios

Los audios son elementos HTML que permiten establecer un audio, se definen con la etiqueta `<audio>`.

```html
<audio {atributos...} src="ruta_del_audio"></audio>
```

Posibles atributos de la etiqueta audio:

* `controls`: muestra los controles del audio
* `autoplay`: reproduce el audio automáticamente
* `loop`: repite el audio
* `muted`: silencia el audio

### Iframes

Un iframe (Inline Frame) es un elemento HTML que permite insertar un documento HTML dentro de otro. El contenido del iframe se carga de forma independiente al documento que lo contiene. Se definen con la etiqueta `<iframe>`.


```html
<iframe src="ruta_del_iframe"></iframe>
```

Posibles atributos de la etiqueta iframe:

* `width`: ancho del iframe
* `height`: alto del iframe
* `title`: título del iframe
* `allow`: indica qué funcionalidades se permiten al iframe.

### Input

Los inputs son elementos HTML que permiten al usuario escribir información, se definen con la etiqueta `<input>`.

```html
<input type="text" name="name" id="name">
```

Se puede cambiar el atributo `type` para crear diferentes tipos de campos:

* `text`: campo de texto
* `password`: campo para contraseñas
* `email`: campo para correos electrónicos
* `number`: campo para números
* `datetime-local`: campo para fechas y horas
* `checkbox`: casilla de verificación


### Enlaces

Los enlaces son elementos HTML que permiten establecer un enlace, se definen con la etiqueta `<a>`. Suele ir dentro de nav.

```html
<a href="https://www.google.com">Contenido de enlace</a>
```

Por defecto los enlaces se abren en la misma pestaña. Si queremos que el enlace se abra en una nueva pestaña, debemos añadir el atributo `target="_blank"`. También es recomendable eliminar la referencia de origen con `rel="noopener noreferrer"`.

* `noopener`: impide que el enlace abra en una pestaña nueva. De esta forma, el nuevo enlace no puede acceder a la ventana actual.
* `noreferrer`: impide que el enlace abra en una pestaña nueva y, además, no envia el referer (información sobre la página actual). De esta forma, el nuevo enlace no puede acceder a la ventana actual ni tener información sobre la página que lo llama. En navegadores modernos no es necesario añadirlo ya que por defecto lo hace.

```html
<a href="https://www.google.com" target="_blank" rel="noopener noreferrer">Contenido de enlace</a>
```

También es posible hacer enlaces internos dentro de la misma web, pero para ello debemos definir con un identificador dichos puntos.

```html
<a href="#section_1">Contenido de enlace</a>

...

<section id="section_1">
    <h2>Encabezado de section</h2>
    <p>Contenido de section</p>
</section>
```

### Elementos semánticos

Este grupo de elementos son muy importantes para conseguir un archivo HTML con buena semántica, es decir, con una buena estructura para generar la página web.

#### aside

El elemento `<aside>` es un elemento HTML que se utiliza para definir contenido tangencialmente relacionado con el contenido principal de la página web. Se utiliza para definir contenido que no sea fundamental para la comprensión principal de la página web.

```html
<aside>
    <h2>Encabezado de aside</h2>
    <p>Contenido de aside</p>
</aside>
```

#### section

El elemento `<section>` es un elemento HTML que se utiliza para definir una sección de contenido en una página web. Se utiliza para definir contenido que sea fundamental para la comprensión principal de la página web.

```html
<section>
    <h2>Encabezado de section</h2>
    <p>Contenido de section</p>
</section>
```

#### nav

El elemento `<nav>` es un elemento HTML que se utiliza para definir una sección de navegación en una página web. Se utiliza para definir contenido que sea fundamental para la comprensión principal de la página web.

```html
<nav>
    <h2>Encabezado de nav</h2>
    <p>Contenido de nav</p>
</nav>
```

#### footer

El elemento `<footer>` es un elemento HTML que se utiliza para definir el pie de página de una página web. Se utiliza para definir contenido que sea fundamental para la comprensión principal de la página web.

```html
<footer>
    <h2>Encabezado de footer</h2>
    <p>Contenido de footer</p>
</footer>
```

#### main

El elemento `<main>` es un elemento HTML que se utiliza para definir el contenido principal de una página web, sólo puede haber un main por página web. Se utiliza para definir contenido que sea fundamental para la comprensión principal de la página web. Es el contenido que hace la página web única.

```html
<main>
    <h2>Encabezado de main</h2>
    <p>Contenido de main</p>
</main>
```

#### article

El elemento `<article>` es un elemento HTML que se utiliza para definir un artículo en una página web. Se utiliza para definir contenido que sea fundamental para la comprensión principal de la página web.

```html
<article>
    <h2>Encabezado de article</h2>
    <p>Contenido de article</p>
</article>
```

#### span

El elemento `<span>` es un elemento HTML que se utiliza para definir un elemento en una página web. Se utiliza para definir contenido que sea fundamental para la comprensión principal de la página web. Se usa para separar estilo de una parte de la web cuando no afecta a la semántica de la página.

```html
<span>Contenido de span</span>
```

#### div

El elemento `<div>` es un elemento HTML que se utiliza para definir un elemento en una página web. Se utiliza para definir contenido que sea fundamental para la comprensión principal de la página web. Se usa para agrupar elementos que no afectan a la semántica de la página.

```html
<div>
    <h2>Encabezado de div</h2>
    <p>Contenido de div</p>
</div>
``` 

## Elementos vacíos

Un elemento vacío en HTML es aquel que no tiene contenido y no requiere etiqueta de cierre

### Metadatos

Los metadatos son elementos HTML que permiten establecer metadatos, se definen con la etiqueta `<meta>`. Usando la etiqueta metadatos se pueden definir muchos metadatos de la página web. No todos los metadatos son elementos vacíos, pero sí la mayoría.

```html
<!-- Especifica la codificación -->
<meta charset="UTF-8">
<!-- Define la descripción de la página -->
<meta name="description" content="Descripción de la página">
<!-- Ajusta el ancho de la página al tamaño del dispositivo -->
<meta name="viewport" content="width=device-width">
<!-- Propiedades para open graph (Para SEO) -->
<meta property="og:title" content="Título de la página en open graph">
<meta property="og:description" content="Descripción de la página en open graph">
<meta property="og:image" content="Ruta de la imagen en open graph">
<!-- Propiedades para redireccionar por idiomas (Para SEO) -->
<link rel="alternate" href="https://www.<same_url>/en" hreflang="en-GB">
<link rel="alternate" href="https://www.<same_url>/es" hreflang="es-ES">
<!-- Propiedad para redireccionar a la principal (Para SEO, Ej sin www) -->
<link rel="canonical" href="https://<same_url>">
<!-- Estilos en línea -->
<style>
    body {
        background-color: blue;
        /* background-color: #09f; */
    }
</style>
<!-- Más posible metadatos ... -->
```

> `description` es importante para el SEO ya que es la descripción que suele aparecer (no siempre) en los resultados de búsqueda de google.

### Link

Los link son elementos HTML que permiten establecer un enlace, se definen con la etiqueta `<link>`. Es la etiqueta que se usa para definir el `favicon`, es decir, el icono que aparece en la pestaña del navegador.

```html
<link rel="icon" type="image/jpg" href="/docs/HTML/im-logo-1.jpg">
```

### Salto de línea

Los saltos de línea son elementos HTML que permiten establecer un salto de línea, se definen con la etiqueta `<br>`.

```html
<br>
```

## Atributos

Los atributos son pares de clave valor que se agregan a las etiquetas HTML para proporcionar más información sobre el elemento. Se definen escribiendo el nombre del atributo seguido del valor entre comillas.

Existen dos tipos de atributos:

### Atributos globales

Los atributos globales son aquellos que se pueden utilizar en cualquier elemento HTML. Algunos de los atributos globales son:

* `class`: Indica una o varias clases CSS que se aplican al elemento. También sirve para identificar elementos, pero este id se puede   repetir.
* `id`: Indica el **identificador único** del elemento. Este id se puede repetir.
* `style`: Indica el estilo CSS que se aplica al elemento.
* `title`: Indica el título del elemento.
* `role`: Indica el rol del elemento. Se utiliza para mejorar la accesibilidad de la página web.

### Atributos específicos

Los atributos específicos son aquellos que solo se pueden utilizar en determinados elementos HTML. Algunos de los atributos específicos son:

* `src` en `<img>`: Indica la ruta de la imagen a mostrar.
* `href` en `<a>`: Indica la ruta del enlace.
* `type` en `<input>`: Indica el tipo de campo de formulario.
* `alt` en `<img>`: Indica el texto alternativo de la imagen en caso de no poderse mostrar.
* `hidden` en `<img>`: Indica que la imagen no se muestra.

#### Tipos de href

Dentro de href se pueden definir varios tipos de enlaces:

* Enlaces internos: Enlaces que apuntan a una página dentro de la misma web.

    ```html
    <a href="/">Inicio</a>
    ```

* Enlaces externos: Enlaces que apuntan a una página fuera de la misma web.

    ```html
    <a href="https://www.google.com">Google</a>
    ```

* Enlaces de correo: Enlaces que apuntan a un correo electrónico.

    ```html
    <a href="mailto:info@example.com">info@example.com</a>
    ```

* Enlaces de teléfono: Enlaces que apuntan a un número de teléfono.

    ```html
    <a href="tel:+34913333333">+34 913 33 33 33</a>
    ```

* Enlace a whatsapp: Enlaces que apuntan a whatsapp.

    ```html
    <a href="whatsapp://send?text=Hola&phone=+34666666666">WhatsApp</a>
    ```

* Enlaces de mapa: Enlaces que apuntan a un mapa.

    ```html
    <a href="https://www.openstreetmap.org/#map=18/40.4085/-3.69196">Madrid</a>
    ```
