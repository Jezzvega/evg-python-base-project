# Crear un proyecto base ✨

Esta es una guia de como setear tu proyecto de python con herramientas para ayudarte al desarrollo y despliegue rapido de paquetes.

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

pusheamos a github

```bash
git remote add origin https://github.com/Jezzvega/python-meetup-39.git
```

y agregamos un ``.gitignore`` de https://www.toptal.com/developers/gitignore

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

An extremely fast Python linter and code formatter, written in Rust.

Contiene isort, black y flake8

```bash
ruff check .
ruff check --fix .
ruff format .
```

### mypy

Mypy is a static type checker for Python.

```bash
mypy .
```

Pero aun con todas estas herramientas los desarrolladores nos lincharian al agregarles un paso extra a su flujo de trabajo.

Asi que trataremos de automatizar las cosas aun más!

## pre-commit

A framework for managing and maintaining multi-language pre-commit hooks.

### Instalación

```bash
poetry add pre-commit --group dev
```

### Configuración

creamos un archivo llamado ``.pre-commit-config.yaml`` y agregamos la siguiente configuracion:

```yml
repos:
-   repo: https://github.com/astral-sh/ruff-pre-commit
    rev: v0.1.7
    hooks:
    - id: ruff
      args: [ --fix ]
    - id: ruff-format
-   repo: https://github.com/pre-commit/mirrors-mypy
    rev: v1.7.1
    hooks:
    - id: mypy
      args: [--disallow-untyped-calls]
```

e instalamos los hooks:

```bash
pre-commit install
```

## first commit

```bash
git commit -m "first commit"
git push -u origin main
```

Ya tenemos nuestro primer release clean!
Ahora que hacemos?? ??  ?? ??

## python semantic release

Automatic Semantic Versioning for Python projects.

Esta herramienta nos resulve toodas las dudas de arriba automaticamente!

### Formato de mensaje de commit
Esta basada en Angular Commit Style, hay formatos mas complejos pero lo podemos resumir en **tipo** y **mensaje**:

```
<tipo>: <mensaje>
```

### Tipo
Debe ser uno de los siguientes:

* **feat**: A new feature
* **fix**: A bug fix
* **docs**: Documentation only changes
* **style**: Changes that do not affect the meaning of the code (white-space, formatting, missing
  semi-colons, etc)
* **refactor**: A code change that neither fixes a bug nor adds a feature
* **perf**: A code change that improves performance
* **test**: Adding missing or correcting existing tests
* **chore**: Changes to the build process or auxiliary tools and libraries such as documentation
  generation

### Configuración

Agregamos las siguientes lineas a nuestro archivo ``pyproject.toml``

```toml
[tool.semantic_release]
assets = []
commit_message = "{version}\n\nAutomatically generated by python-semantic-release"
commit_parser = "angular"
logging_use_named_masks = false
major_on_zero = true
tag_format = "v{version}"

version_toml = [
    "pyproject.toml:tool.poetry.version",
]

build_command = "pip install poetry && poetry build"

[tool.semantic_release.branches.main]
match = "(main|master)"
prerelease_token = "rc"
prerelease = false

[tool.semantic_release.changelog]
template_dir = "templates"
changelog_file = "CHANGELOG.md"
exclude_commit_patterns = []

[tool.semantic_release.changelog.environment]
block_start_string = "{%"
block_end_string = "%}"
variable_start_string = "{{"
variable_end_string = "}}"
comment_start_string = "{#"
comment_end_string = "#}"
trim_blocks = false
lstrip_blocks = false
newline_sequence = "\n"
keep_trailing_newline = false
extensions = []
autoescape = true

[tool.semantic_release.commit_author]
env = "GIT_COMMIT_AUTHOR"
default = "semantic-release <semantic-release>"

[tool.semantic_release.commit_parser_options]
allowed_tags = ["chore", "feat", "fix", "style", "refactor", "test"]
minor_tags = ["feat"]
patch_tags = ["fix"]

[tool.semantic_release.remote]
name = "origin"
type = "github"
ignore_token_for_push = false

[tool.semantic_release.remote.token]
env = "GH_TOKEN"

[tool.semantic_release.publish]
dist_glob_patterns = ["dist/*"]
upload_to_vcs_release = true
```

Esta herramienta nos crea un CHANGELOG.md pero ademas tenemos la opcion de crear una plantilla personalizada.

## Github Actions

en la ruta ``.github/workflows`` agregamos el archivo ``main.yml``

```yml
name: 'Semantic Release Versioning'
on:
  push:
    branches:
    - main
jobs:
  release:
    runs-on: ubuntu-latest
    concurrency: release
    permissions:
      id-token: write
      contents: write

    steps:
    - uses: actions/checkout@v4
      with:
        fetch-depth: 0

    - name: Python Semantic Release
      uses: python-semantic-release/python-semantic-release@master
      with:
        github_token: ${{ secrets.GH_TOKEN }}
```

### Commit and push

```bash
git commit -m "chore: add semantic release and github actions"
git push -u origin main
```

### Agregamos mas funciones

```bash
git commit -m "feat: random function"
git push -u origin main
```

## Siguientes pasos...

### Crear una plantilla usando Cookie Cutter

### Actions con validaciones

### Contruccion y distribucion del paquete.
