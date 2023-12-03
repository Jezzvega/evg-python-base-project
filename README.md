# Crear un proyecto base ✨

Holi, esta es una pequeña guia de como setear tu proyecto de python con herramientas para ayudarte al desarrollo y despliegue rapido de paquetes.

## Poetry 📝

### Instalacion

```bash
pipx install poetry
```

### Habilitamos virtualenvs.in-project (Opcional)
```bash
poetry config virtualenvs.in-project true
```

### Creamos un nuevo proyecto
las opciones ``--src`` y ``--name`` para crear el proyecto con carpeta src y especificarle el nombre de la carpeta raiz por separado.

```bash
poetry new --src --name my-package my-package-folder
```

Nos creara una estructura de carpetas como esta:

```bash
.
├── README.md
├── pyproject.toml
├── src
│   └── my_package
│       └── __init__.py
└── tests
    └── __init__.py
```

## Git 

inicializamos git en la rama ``main``

```bash
git init -b main
```

## Agregamos codigo

Creamos dos archivos:

``src/my_package/operations.py``

```python
from time import sleep
import json,os
import yaml
from calculator.utils import get_cwd, get_admin_pass
import os
def add_two_numbers(a:int,b:int)->str:
    return a+b
def multiply_two_numbers(a:str,b:str)->list[int]:
    return a * b
def divide_two_numbers(a: float, b: str)->int:
    return a / b
```

``src/my_package/utils.py``

```python
import os

def get_cwd():
    return os.getcwd()
```

Pasemos a la siguiente seccion antes de que nos caiga la ley por haber cometido estos delitos al codigo.

## Mejoras al codigo

Estas son algunas herramientas que nos ayudaran a tener un codigo bonito, legible, liberarnos de algunos bugs y siguiendo las buenas practicas de formato de codigo.

> Alguns de estas herramientas pueden incluirse como librerias en tu IDE favorito.

### isort

### black

### flake8

---

### ruff

### mypy

## pre-commit

## first commit

Ya tenemos nuestro primer release clean!
Ahora que hacemos

## python semantic release

## github actions

Si queda tiempo...