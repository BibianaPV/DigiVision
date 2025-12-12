# Trabajo 4: Sistemas de Deteccion

Este repositorio contiene el cuarto trabajo de digivision para vision por computador
en este realizamos un proceso de deteccion de imagenes usando datasets de video,
especificamente el de SoccerNet, con el cual creamos un detector de jugadores y
pelota mediante distintas versiones de YOLO.

## Requisitos

- Un Editor de codigo que tenga soporte de python y archivos tipo ipynb
- SoccerNet libreria
- YOLOv8n
- YOLOv8l
- Python 3.11
- jupyter_client          8.6.3
- jupyter_core            5.8.1
- jupyterlab_widgets      3.0.16
- matplotlib              3.10.6
- matplotlib-inline       0.1.7
- numpy                   2.0.2
- opencv-python           4.12.0.88
- pillow                  11.3.0
- os
- cv2
- pandas
- import os
- configparser
- shutil
- glob
- random
- deepsort
- ultralytics
- IPython.display

## Instala las dependencia necesarias con:

```bash
pip install opencv-python
pip install numpy
pip install matplotlib
pip install Pillow
pip install -U numpy scikit-image
pip install SoccerNet --upgrade
pip install ultralytics opencv-python lap matplotlib
pip install deep_sort_realtime
```

## Uso

1. Descargue el proyecto completo.
2. Asegurese de que tiene todas las dependencias instaladas que fueron enunciadas en este readme.
3. Dentro de la carpeta notebooks podra encontrar los ejercicios por separado.
4. Ejecute los ejercicios en orden ya que es requerido para el buen funcionamiento del proyecto, para el ejercicio 2_datasetYOLO necesitara primero ejecutar 1.dataset.ipynb
5. En la carpeta de results podra encontrar todos los resultados de la operaciones, tanto los clips resultantes como las graficas.
6. En el archivo Informe.md pordra encontrar el marco teorico y las conclusiones encontradas, asi como diferentes datos que le resultaran utiles y resultaron de interes para el equipo.

# Contenido

## Data

## Notebooks con los Ejercicios

En la carpeta `notebooks/` encontrarás versiones en Jupyter de cada ejercicio, listas para ejecutarse:

- `1.dataset.ipynb` Obtencion y procesamiento del dataset elegido
- `2_datasetYOLO.ipynb` Utilizacion de YOLOv8n para deteccion y seguimiento
- `3b.Yolov8l.ipynb` Utilizacion de YOLOv8l para deteccion y seguimiento

## Resultados

- `results/YOLOv8l` Clips y resultados de YOLOv8l
- `results/YOLOv8n` Clips y resultados de YOLOv8n

## Integrantes del Equipo

- Bibiana Andrea Pena Velasquez
- Leidy Marcela Leal Loaiza 
- Juan Felipe Arbelaez Uribe
