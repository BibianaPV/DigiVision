# Trabajo 2

Este repositorio contiene el segundo trabajo de digivision para vision por computador
en este realizamos validaciones de imagenes sinteticas, registro y mezcla de imagenes
asi como medir distancias con base a un referente dentro de un canvas de tres imagenes 
combinados, utilizando metodos de segmentacion.

## Requisitos

- Un Editor de codigo que tenga soporte de python y archivos tipo ipynb
- Python 3.11
- jupyter_client          8.6.3
- jupyter_core            5.8.1
- jupyterlab_widgets      3.0.16
- matplotlib              3.10.6
- matplotlib-inline       0.1.7
- numpy                   2.2.6
- opencv-python           4.12.0.88
- pillow                  11.3.0
- import random
- from google.colab.patches import cv2_imshow
- import pandas as pd
- from math import sqrt
- from skimage.metrics import normalized_mutual_information
- from scipy.stats import pearsonr
- import itertools
- from tqdm import tqdm

## Instala las dependencia necesarias con:

```bash
pip install opencv-python
pip install numpy
pip install matplotlib
pip install Pillow
```

## Uso

1. Descargue el proyecto completo.
2. Dirijase al apartado de Requirements.txt y asegurese de tener todas las dependencias instaladas.
3. Dentro de la carpeta notebooks podra encontrar los ejercicios por separado.
4. Ejecute uno por uno los ejercicios.
5. En la carpeta de results podra encontrar todos los resultados de la operaciones.
6. En el archivo Informe.md pordra encontrar el marco teorico y las conclusiones encontradas, asi como diferentes datos que le resultaran utiles.

# Contenido

## Data

- `data/original`: Imagenes originales para ejercicio de registro y mezcla.
- `data/synthetic`: Imagenes sinteticas.

## Notebooks con los Ejercicios

En la carpeta `notebooks/` encontrarás versiones en Jupyter de cada ejercicio, listas para ejecutarse:

- `01_ Validación _con _Imágenes _Sintéticas.ipynb`
- `02_Registro_Imagenes.ipynb`
- `03_Medicion_Objetos.ipynb`

## Resultados

- `results/01_Imagenes_Sinteticas`: Resultados de los ejercicios con imagenes sinteticas.
- `results/02_Registro_de_Imagenes`: Resultados de la combinacion y registro de imagenes.
- `results/03_Calibracion_y_Medicion`: Resultado de los ejercicios de medicion con las imagenes combinadas

## Tests

En esta carpeta encontraras ensayos y pruebas realizados antes de llegar al resultado final.

## Integrantes del Equipo

- Bibiana Andrea Pena Velasquez
- Leidy Marcela Leal Loaiza
- Juan Felipe Arbelaez Uribe

