# Trabajo 3: Clasificacion de Imagenes medicas

Este repositorio contiene el tercer trabajo de digivision para vision por computador
en este realizamos el analisis del dataset de kaggle para imagenes medicas de pulmones
normales y con neumonia, para este trabajo realizaremos pre-procesado de imagenes asi
como su correspondiente segmentacion y normalizacion, realizaremos analisis usando
descriptores de forma y de textura y entrenaremos modelos de clasificacion para la
deteccion de las imagenes

## Requisitos

- Un Editor de codigo que tenga soporte de python y archivos tipo ipynb
- Python 3.11
- jupyter_client          8.6.3
- jupyter_core            5.8.1
- jupyterlab_widgets      3.0.16
- matplotlib              3.10.6
- matplotlib-inline       0.1.7
- numpy                   2.0.2
- opencv-python           4.12.0.88
- pillow                  11.3.0
- import random
- import pandas as pd 2.2.2
- from math import sqrt
- from skimage.metrics import normalized_mutual_information
- from scipy.stats import pearsonr
- import itertools
- from tqdm import tqdm
- import seaborn as sns
- tensorflow: 2.19.0
- import sklearn
- import joblib

## Instala las dependencia necesarias con:

```bash
pip install opencv-python
pip install numpy
pip install matplotlib
pip install Pillow
pip install -U numpy scikit-image
```

## Uso

1. Descargue el proyecto completo.
2. Asegurese de que tiene todas las dependencias instaladas que fueron enunciadas en este readme.
3. Dentro de la carpeta notebooks podra encontrar los ejercicios por separado, asi como pruebas realizadas con los mismos
4. Ejecute los ejercicios en orden ya que es requerido para el buen funcionamiento del proyecto
5. En la carpeta de results podra encontrar todos los resultados de la operaciones, las imagenes de segmentacion asi como los npz.
6. En el archivo Informe.md pordra encontrar el marco teorico y las conclusiones encontradas, asi como diferentes datos que le resultaran utiles y resultaron de interes para el equipo.

# Contenido

## Data

- `data/test/NORMAL`: Imagenes de pulmones normales para testeo
- `data/test/PNEUMONIA`: Imagenes de pulmones con neumonia para testeo
- `data/train/NORMAL`: Imagenes de pulmones normales para entrenar un modelo de clasificacion
- `data/train/PNEUMONIA`: Imagenes de pulmones con neumonia para entrenar un modelo de clasificacion
- `data/val/NORMAL`: Imagenes de pulmones normales para validar la eficacia del modelo de clasificacion
- `data/val/PNEUMONIA`: Imagenes de pulmones con neumonia para validar la eficacia del modelo modelo de clasificacion

## Notebooks con los Ejercicios

En la carpeta `notebooks/` encontrarás versiones en Jupyter de cada ejercicio, listas para ejecutarse:

- `1B_SegmentacionGaussiana_JuanFelipeArbelaez.ipynb` Ejercicio de segmentacion por umbralizacion
- `1B_segmteacion_PSPNet.ipynb` Segmentacion de recorte utilizada
- `1C_recorte_dataset_mask.ipynb` Recorte de la mascara del dataset
- `1_exploracion_preprocesamiento.ipynb` 
- `2A_descriptores_Forma.ipynb` Aqui estan presente todos lod descriptores de forma HOG, Hu, Contorno y Fourier
- `2B_Descriptores_De_Textura.ipynb` Aqui estan presentes los descriptores de textura LBP, GLCM y Filtro de Gabor
- `3A_Clasificacion_SVC.ipynb` Clasificacion con SVC y tres kernels (RBF, Poly y Linear)
- `3B_Clasificacion_RandomForest.ipynb` Clasificacion usando Random Forest 
- `3D_clasificacion_CNN.ipynb` Clasificacion de Redes convolucionales


## Resultados

- `results/chest_xray_preprocessed` Imagenes procesadas a partir de las originales para mostrar punto de interes
- `results/features/` Archivos .hpz extraidos de las operaciones con los descriptores
- `results/imagenes/` Imagenes varias de los resultados obtenidos
- `results/lungcropped/` Imagenes recortadas de los pulmones
- `results/masks/` Imagenes de las mascaras de los pulmones
- `results/models/` Modelos Entrenados


## Tests

En esta carpeta encontraras ensayos y pruebas realizados antes de llegar al resultado final.

## Integrantes del Equipo

- Bibiana Andrea Pena Velasquez: 
- Leidy Marcela Leal Loaiza: 
- Juan Felipe Arbelaez Uribe: Segmentacion con Umbralizacion, descriptores de forma (Fourier), descriptores de textura GLMC, Clasificacion SVC y aporte al informe.md
