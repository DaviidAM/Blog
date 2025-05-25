# HTML

HTML (`Lenguaje de Marcado de Hipertexto`, del inglés `HyperText Markup Language`) es el componente más fundamental de la Web.

En una página web confluyen muchos elementos y tecnologías diferentes: por ejemplo, para interactuar con la web se utiliza JavaScript (el más común), y para modificar la apariencia del contenido se emplea CSS. Sin embargo, **HTML se enfoca únicamente en definir/describir la estructura y el contenido que vemos en la web**, es decir, en establecer elementos como títulos, imágenes, párrafos, enlaces, y otros componentes básicos.

## Archivos HTML

Un archivo HTML es un archivo de texto que contiene el código HTML. Los archivos html deben acabar con la extensión `.html`.

Por defecto, es común usar el nombre `index.html` para el archivo html que contiene la información de la página principal de una página web.

### Comentarios

Los comentarios son texto que no se muestra en la página web. Se definen con la etiqueta `<!-- -->`.

```html
<!-- Esto es un comentario -->
```

### Campos mandatorios

Los campos mandatorios en HTML son unas etiquetas que tenemos que añadir siempre que creamos un nuevo archivo html.

#### DOCTYPE

El DOCTYPE es un texto que indica al navegador qué versión de HTML se está utilizando. Se definen con la etiqueta `<!DOCTYPE html>`.

```html
<!DOCTYPE html>
```

#### Bloque html

El bloque html es el contenedor/etiqueta principal de un documento HTML. Se definen con la etiqueta `<html>`.

```html
<html>

</html>
```

Este bloque principal html se divide en 2 partes:

* `head`.
* `body`.

##### Head

El bloque head es el contenedor principal de un documento HTML que contiene **información que no se ve en la página web**. Se definen con la etiqueta `<head>`. Dentro del bloque head se definen los títulos, los enlaces a los archivos css, los enlaces a los archivos js, y los metadatos.

```html
<head>

</head>
```

##### Body

El bloque body es el contenedor principal de un documento HTML que contiene la **información que se ve en la página web**. Se definen con la etiqueta `<body>`. Dentro del bloque body se definen los elementos que se ven en la página web como títulos, párrafos, imágenes, enlaces, y otros componentes básicos.

```html
<body>

</body>
```
