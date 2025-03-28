# DevContainer

DevContainer es una herramienta que nos permite crear un entorno virtual de trabajo dentro de docker.

Sólo hace falta tener instalado docker engine.

## Pre-requisitos

Si estamos usando VSCode hay que instalar la extension `Dev Containers`.

## Crear configuración para devcontainer

1. Abrir buscador de VSCode y buscar `Add Dev Container Configuration Files`.
2. Lo normal es selecionar `Add configuration to workspace`.
3. Seleccionamos la plantilla que queramos utilizar.

Tras este punto, al esperar unos segundos, debería aparecer un nuevo archivo de configuración con el nombre `.devcontainer/devcontainer.json`.

## Iniciar contenedor

1. Abajo a la izquierda (botón con símbolo `><`), o desde el buscador de VSCode, ejecutar `Reopen in container`.
2. Tras unos segundos se abrirá una nueva ventana donde estarás ya dentro del contenedor, dentro de tu entorno virtual.

## Otras curiosidades de devcontainer

* Cada cambio que se haga dentro del archivo de configuración de devcontainer necesitará que se reinicie el contenedor para aplicar los cambios.
* Puedes instalar extensiones VSCode dentro del contenedor de devcontainer. Esto añadirá dicha extensión al archivo de configuración.
* Cada vez que se quiera habilitar un puerto del contenedor aparecerá una ventana para dar el permiso, pero también se puede hacer directamente desde el archivo de configuración.

    ```json
    {
        "name": "Python 3 - Test devcontainer",
        //(...)
        // Use 'forwardPorts' to make a list of ports inside the container available locally.
        "forwardPorts": [8000, 5001],
        "portsAttributes": {
            "8000": {
                "label": "API Port",
                "protocol": "https"
            }
        }
    }
    ```

* Ejecutar comando justo al lanzar el contenedor.

    ```json
    {
        "name": "Python 3 - Test devcontainer",
        //(...)
        "postStartCommand": "python app/main.py"
    }
    ```

* Es posible usar otros archivos DockerFile o docker-compose propios para crear el contenedor.

    ```json
    {
        "name": "Python 3 - Test devcontainer",
        //"image": "mcr.microsoft.com/devcontainers/python:1-3.12-bullseye",
        "build":{
            "dockerfile": "Dockerfile"
        }
        //(...)
    }
    ```

    ```json
    {
        "name": "Python 3 - Test devcontainer",
        //"image": "mcr.microsoft.com/devcontainers/python:1-3.12-bullseye",
        "dockerComposeFile": "docker-compose.yml"
        //(...)
    }
    ```

* Se pueden tener múltiples configuraciones para un mismo repo, por ejemplo una configuración para dev y otra para debug. Por lo que al tratar de iniciar el contenedor nos preguntará cuál queremos iniciar.

    ```bash
    .
    └── .devcontainer
        ├── debug
        │   └── devcontainer.json
        └── dev
            └── devcontainer.json
    ```
