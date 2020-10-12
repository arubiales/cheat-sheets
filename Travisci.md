#TRAVISCI

Pasos para hacer que funcione:

1. Creamos en el repositorio raiz, un archivo llamado ".travis.yml". Este archivo le dira a Travis como tiene que correr los tests y tiene varias secciones. Sería así:

```
language: python
python:
  - "3.7" #Versión
  - "nightly" #La última versión disponible
install: #Las cosas que debemos instalar a la hora de correr el test
  - "pip install pytest" #instalamos pytest
  - "pip install -r requirements.txt" #instalamos todas las bibliotecas
script: pytest tests/ #Lo que queremos que corra la terminal
```