# Ruff


Ruff es una herramienta de linting y formateo para Python, escrita en Rust, que destaca por su velocidad y eficiencia. Unifica múltiples funcionalidades (como Flake8, isort, pyupgrade, entre otras) en una sola CLI, permitiendo escribir código más limpio, consistente y libre de errores, y es hasta 100 veces más rápida que alternativas tradicionales.


**Características principales:**

*   **Velocidad:** Su mayor ventaja. Realiza el linting y formateo en segundos, incluso en grandes bases de código.
*   **Todo en uno:** Reemplaza a un gran conjunto de linters y formateadores (Flake8, isort, pydocstyle, pyupgrade, autoflake, etc.).
*   **Compatible con Flake8:** Implementa la mayoría de las reglas de Flake8 y sus plugins más populares.
*   **Autocorrección (Autofix):** Puede corregir automáticamente muchos de los errores que detecta.
*   **Configuración sencilla:** Utiliza `pyproject.toml` para la configuración, alineándose con los estándares modernos de Python.
*   **Integración con editores:** Se integra fácilmente con editores de código populares como VS Code, PyCharm, etc.
*   **Caché Inteligente:** Guarda los resultados del linting para evitar re-analizar archivos no modificados, acelerando aún más las ejecuciones subsecuentes.

## ¿Por qué es beneficioso usar Ruff?

Integrar Ruff en tu flujo de trabajo de Python aporta numerosas ventajas:

1.  **Rendimiento Excepcional:**
    *   Reduce drásticamente el tiempo de espera en los procesos de Integración Continua/Despliegue Continuo (CI/CD) y en el desarrollo local.
    *   Permite ejecutar el linter con más frecuencia sin interrumpir el flujo de trabajo.

2.  **Simplificación del Herramental (Tooling):**
    *   En lugar de gestionar múltiples dependencias y configuraciones para diferentes linters y formateadores, solo necesitas Ruff.
    *   Esto simplifica el archivo `pyproject.toml` o `requirements.txt` y reduce la complejidad del entorno de desarrollo.

3.  **Consistencia del Código Mejorada:**
    *   Al aplicar un conjunto unificado de reglas de linting y formateo, Ruff asegura que el código sea más homogéneo en todo el proyecto, facilitando la lectura y el mantenimiento.

4.  **Detección Temprana de Errores y "Code Smells":**
    *   Ayuda a identificar errores comunes, posibles bugs y malas prácticas de programación antes de que lleguen a producción.

5.  **Modernización del Código:**
    *   Incluye reglas y autocorrecciones para actualizar la sintaxis de Python a versiones más modernas (similar a `pyupgrade`).

6.  **Mejora de la Productividad del Desarrollador:**
    *   La velocidad y la capacidad de autocorrección permiten a los desarrolladores centrarse más en la lógica de negocio y menos en el formato y los errores triviales.
    *   Reduce la fricción durante las revisiones de código, ya que muchos problemas de estilo se resuelven automáticamente.

7.  **Fácil Adopción:**
    *   Su compatibilidad con Flake8 facilita la migración desde configuraciones existentes.
    *   La configuración es intuitiva y bien documentada.

8.  **Comunidad Activa y Desarrollo Continuo:**
    *   Ruff es un proyecto de código abierto con una comunidad creciente y un desarrollo muy activo, lo que asegura la incorporación de nuevas funcionalidades y la corrección de errores de forma rápida.

## Instalación

```bash
pip install ruff
```

### Instalación desde uv

`uv` es un gestor de herramientas, similar a `pip`, pero para rápido y que ayuda a manejar módulos de python de una manera más sencilla.

```bash
uv tool install ruff
```

## Usos

### check

Comprueba el código y devuelve por consola los errores.

```bash
ruff check <file or path>
```

Algunos argumentos útiles:

* `--fix` para corregir los errores.
* `--diff` para mostrar los cambios que se van a hacer.
* `--show-source` para mostrar el código fuente.
* `--show-position` para mostrar la posición del error.

### format

```bash
ruff format <file or path>
```

Algunos argumentos útiles:

* `--check` para comprobar si el código está formateado.
* `--diff` para mostrar los cambios que se van a hacer.
* `--watch` para vigilar los cambios en el archivo y formatearlos automáticamente.

## Configuración

Se puede configurar Ruff para cada proyecto a través de un archivo `pyproject.toml` en el directorio raíz del proyecto.

```toml
(...)

[tool.ruff]
line-length = 120
lint.extend-ignore = ["E501"]
lint.extend-select = ["PTH"]
``` 

Por defecto ruff utilizará la configuración que se encuentra en el archivo `ruff.toml`. Que se puede encontrar por defecto en:

Linux:
```bash
~/.config/ruff/ruff.toml
```

Windows:
```bash
C:\Users\<username>\AppData\Local\ruff\ruff.toml
```

## Integración con VSCode

Se puede integrar Ruff con VSCode para que se ejecute automáticamente al guardar el archivo.

1. Instalar ruff extension en VSCode.
2. Añadir la siguiente configuración en el archivo `settings.json` de VSCode.

```json
{
    "[python]": {
        "editor.formatOnSave": true,
        "editor.defaultFormatter": "charliermarsh.ruff",
        "editor.formatOnSave": true,
        "editor.codeActionsOnSave": {
            "source.organizeImports": "explicit"
            //"source.fixAll": "explicit" // better to see errores
        }
    },
    "python.linting.enabled": true, // not tested
    "python.linting.provider": "ruff", // not tested
    "python.analysis.inlayHints": {
        "variableTypes": true,
        "parameterTypes": true, // not tested
        "parameterName": true, // not tested
        "functionReturnType": true
    }
}
```

## Referencias

* [Ruff Documentation](https://docs.astral.sh/ruff/)
* [Ruff Rules](https://docs.astral.sh/ruff/rules/)
* [Ruff GitHub](https://github.com/astral-sh/ruff)
* [Ruff Video Tutorial](https://www.youtube.com/watch?v=828S-DMQog8&ab_channel=CoreySchafer)    
