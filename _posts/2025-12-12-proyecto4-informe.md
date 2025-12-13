---
layout: post
title: "Proyecto4 Informe"
date: 2025-12-12
---

<!-- Activar MathJax en Minima -->
<script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
<script id="MathJax-script" async
        src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js">
</script>

<style>
.post-title { display: none; }
.page-content {
  max-width: 100% !important;
  width: 100% !important;
  padding: 0 !important;
  margin: 0 !important;
}

.wrapper {
  max-width: 100% !important;
  width: 100% !important;
  padding: 0 !important;
  margin: 0 !important;
}
.full-width-section {
  width: 90%;
  max-width: 100%;
  margin: 0 auto;
  padding: 0;
  box-sizing: border-box;
}
/* Contenedor principal que aloja índice y contenido */
.main-container {
  display: flex;
  gap: 25px;              /* espacio entre índice y contenido */
  align-items: flex-start; /* alinea arriba ambos */
}

/* Índice lateral */
.toc-container {
  flex: 0 0 320px;         /* ancho fijo de 220px */
  position: sticky;
  top: 20px;
  max-height: 90vh;
  overflow-y: auto;
  background: #f8f9fa;
  padding: 10px;
  border-right: 2px solid #ddd;
}

/* Contenido principal */
.post-content {
  flex: 1;                /* ocupa todo el espacio restante */
  text-align: justify;
  min-width: 0;           /* 👈 evita que el contenido se desborde */
}

/* Estilo índice */
.toc-container ul {
  list-style: none;
  padding-left: 0;
}

.toc-container li a {
  text-decoration: none;
  color: #004aad;
}

.toc-container li a:hover {
  text-decoration: underline;
}

/* Diseño responsive para pantallas pequeñas */
@media (max-width: 1200px) {
  .main-container {
    flex-direction: column;
  }
  .toc-container {
    width: 100%;
    border-right: none;
    border-bottom: 2px solid #ddd;
    max-height: none;
    overflow: visible;
  }
}
</style>

<div class="full-width-section">
<div markdown="1" class="main-container">
<div markdown="1" class="toc-container">
## Contenido
* TOC
{:toc}
</div>

<!-- Contenido principal -->
<div markdown="1" class="post-content">

# Informe Proyecto 4 

# **1\. Introducción**

El análisis automático del fútbol mediante técnicas de visión por computador ha adquirido un papel fundamental en áreas como el análisis táctico, la evaluación del rendimiento deportivo y el desarrollo de sistemas inteligentes de apoyo al arbitraje y a la transmisión deportiva. En este contexto, la detección y el seguimiento de jugadores y del balón constituyen tareas clave, ya que permiten describir la dinámica del juego a partir de información espacial y temporal extraída directamente de secuencias de vídeo.

Este proyecto aborda el problema del seguimiento de objetos en fútbol mediante un enfoque de tracking by detection, combinando un modelo de detección de objetos basado en aprendizaje profundo (YOLO) con técnicas de asociación temporal para mantener la identidad de los objetos a lo largo del tiempo. Para ello, se utiliza el dataset SoccerNet MOT, el cual proporciona secuencias anotadas de partidos reales con información detallada de jugadores, árbitros y balón.

El desarrollo del proyecto incluye la preparación del dataset en formato YOLO, el entrenamiento de un detector para las clases player y ball, la evaluación del desempeño mediante métricas estándar de detección, y la visualización de resultados. Posteriormente, las detecciones obtenidas sirven como base para implementar estrategias de seguimiento multi-objeto, permitiendo analizar trayectorias y movimientos en escenarios deportivos complejos.

# **2\. Marco Teórico**

## 2.1 Aplicación Práctica: Seguimiento deportivo  
El seguimiento automático (player & ball tracking) es un componente fundamental en los sistemas modernos de análisis de rendimiento deportivo, arbitraje asistido por tecnología y generación de estadísticas avanzadas en fútbol. Este tipo de seguimiento permite identificar, en cada fotograma, la posición, trayectoria e interacción espacio-temporal de jugadores y del balón, lo cual es crucial para comprender la dinámica del juego, evaluar estrategias y medir el comportamiento táctico de equipos y deportistas (Gudmundsson & Horton, 2017).

El seguimiento del balón es especialmente desafiante debido a su tamaño pequeño, alta velocidad, oclusiones frecuentes y cambios bruscos de dirección, por lo que los sistemas basados en aprendizaje profundo y detección multi-objeto (como YOLO) han mostrado mejoras significativas frente a métodos tradicionales basados en filtros o heurísticas (Cioppa et al., 2019; Ren et al., 2015). Asimismo, el seguimiento de jugadores permite obtener métricas clave como velocidad, distancia recorrida, ocupación del espacio, presión defensiva y patrones tácticos, lo cual es ampliamente utilizado en la analítica profesional de clubes y federaciones (Mackenzie & Cushion, 2013).

Bases de datos como SoccerNet MOT han impulsado el desarrollo de algoritmos estandarizados, proporcionando secuencias con anotaciones consistentes para detección, seguimiento e identificación. La combinación de detección con redes neuronales, YOLOv8, y métodos de asociación temporal constituye una metodología para alcanzar un seguimiento robusto en escenarios deportivos complejos (Deliège et al., 2021).

## 2.2 Detección de Objetos: YOLO  
La detección de objetos es una tarea fundamental de la visión por computador que consiste en identificar y localizar instancias de objetos de interés dentro de una imagen o secuencia de video, generalmente mediante cajas delimitadoras (bounding boxes) y una etiqueta de clase asociada. A diferencia de la clasificación de imágenes, la detección requiere no sólo reconocer la presencia de un objeto, sino también estimar su posición espacial.

### 2.2.1 YOLO (You Only Look Once)  
YOLO (You Only Look Once) es una familia de modelos de detección de objetos basados en aprendizaje profundo que plantean la detección como un problema de regresión directa, resolviendo en una sola pasada de la red neuronal la localización y clasificación de los objetos presentes en la imagen (Redmon et al., 2016). Este enfoque contrasta con métodos de dos etapas como Faster R-CNN, donde primero se generan regiones candidatas y luego se clasifican.

En YOLO, la imagen de entrada se divide en una cuadrícula y, para cada celda, la red predice un conjunto de cajas delimitadoras, cada una caracterizada por sus coordenadas normalizadas   
(𝑥,𝑦,𝑤,ℎ), una confianza y una probabilidad de clase. De esta forma, el modelo aprende simultáneamente información espacial y semántica, lo que permite una detección eficiente y en tiempo real.

2.2.2 Función de pérdida y entrenamiento  
El entrenamiento de YOLO se basa en una función de pérdida compuesta que penaliza los errores en: La localización de las cajas (regresión de coordenadas), la confianza de detección, la clasificación del objeto. En general, la pérdida puede expresarse como:

$$
L = L_{loc} + L_{conf} + L_{cls}
$$


donde 

$$
L_{loc}: \text{error de localización de las cajas}
$$
$$
L_{conf}: \text{confianza en la presencia del objeto}
$$
$$
L_{cls}: \text{error de clasificación}
$$
 

Versiones más recientes, como YOLOv8, incorporan mejoras en la función de pérdida, arquitecturas más profundas y mecanismos de anchor-free detection para mejorar la precisión y estabilidad del entrenamiento.

De esta manera, YOLO destaca por su alta velocidad de inferencia, su capacidad para operar en tiempo real y su facilidad de adaptación mediante fine-tuning a nuevos dominios con conjuntos de datos específicos. Estas características lo hacen especialmente adecuado para aplicaciones dinámicas como el análisis deportivo, la conducción autónoma y la videovigilancia, donde es necesario detectar múltiples objetos en secuencias de vídeo de forma eficiente.

En el contexto del fútbol, YOLO se ha utilizado ampliamente para la detección de jugadores y balón, sirviendo como etapa inicial en sistemas de seguimiento multi-objeto (tracking by detection), donde las detecciones frame a frame son posteriormente asociadas a lo largo del tiempo para mantener la identidad de cada objeto.

## 2.3 Seguimiento de Objetos  
En escenarios dinámicos como el fútbol, el seguimiento de jugadores y del balón es especialmente desafiante debido a factores como movimientos rápidos, oclusiones frecuentes, cambios de escala, variaciones de iluminación y similitud visual entre objetos. Por esta razón, los sistemas modernos de seguimiento suelen combinar técnicas de detección robustas con modelos de asociación temporal.

### 2.3.1 Seguimiento basado en detección (Tracking by Detection)  
El enfoque predominante en aplicaciones reales es el denominado tracking by detection, en el cual un detector de objetos , YOLO, se aplica de manera independiente a cada fotograma para obtener las posiciones de los objetos, y posteriormente un algoritmo de seguimiento se encarga de asociar las detecciones entre fotogramas consecutivos, manteniendo un identificador único (ID) para cada objeto (Bewley et al., 2016).

Este enfoque separa el problema en dos etapas:

* Detección: localización de los objetos en cada fotograma.  
* Asociación temporal: correspondencia de las detecciones a lo largo del tiempo.

Su principal ventaja es la modularidad, ya que permite mejorar la detección o el seguimiento de manera independiente.

### 2.3.2 Métodos de asociación temporal  
La asociación temporal puede realizarse mediante distintos métodos, entre los que destacan:

**\* Flujo óptico (Optical Flow):** estima el desplazamiento de píxeles o puntos característicos entre fotogramas consecutivos, permitiendo predecir la nueva posición de un objeto a partir de su movimiento previo. Métodos como Lucas–Kanade o Farnebäck han sido ampliamente utilizados para seguimiento a corto plazo.

**\* Modelos cinemáticos:** el uso de filtros de Kalman permite modelar el estado dinámico de un objeto (posición, velocidad) y predecir su ubicación futura, facilitando la asociación incluso ante detecciones perdidas u oclusiones parciales.

**\* Métricas espaciales:** la superposición entre cajas delimitadoras, comúnmente medida mediante la Intersection over Union (IoU), es utilizada para asociar detecciones cercanas en el espacio.

Algoritmos como SORT combinan filtros de Kalman con IoU para lograr un seguimiento eficiente en tiempo real, mientras que extensiones como DeepSORT incorporan descriptores de apariencia para mejorar la robustez ante oclusiones prolongadas (Wojke et al., 2017).

En el análisis de fútbol, el seguimiento de jugadores y del balón permite extraer información espacio-temporal clave, como trayectorias, velocidades, distancias recorridas, zonas de influencia e interacciones entre jugadores. Estos datos son fundamentales para el análisis táctico, la evaluación del rendimiento y el desarrollo de sistemas de apoyo al arbitraje y a la transmisión deportiva (Gudmundsson & Horton, 2017).

# **3\. Metodología**

## 3.1 Análisis Exploratorio del Dataset

En primer lugar, se realizó una inspección visual de las imágenes correspondientes a las distintas clases del dataset, permitiendo verificar la calidad de los fotogramas, la variabilidad de escenas y la correcta correspondencia entre imágenes y anotaciones. Esta visualización inicial facilita la detección de posibles problemas como imágenes corruptas, variaciones de iluminación o diferencias significativas en la escala de los objetos de interés.

Posteriormente, se analizó la distribución de clases, cuantificando el número de instancias e imágenes asociadas a cada clase (jugadores y balón). Este análisis permitió identificar posibles desbalances entre clases, un aspecto relevante dado que el balón suele aparecer con menor frecuencia y tamaño en comparación con los jugadores, lo cual puede afectar el entrenamiento del modelo de detección.

Finalmente, se verificó la estructura temporal del dataset, confirmando que las imágenes se encuentran organizadas como secuencias ordenadas de fotogramas. Esta característica resulta esencial para las etapas posteriores de seguimiento de objetos, ya que permite explotar la continuidad temporal y analizar el movimiento de jugadores y balón a lo largo del tiempo.

## 3.1.2  Creación Dataset formato YOLO

La creación del dataset en formato YOLO se realizó con el objetivo de adaptar las anotaciones originales del dataset SoccerNet MOT a un formato compatible con modelos de detección de objetos basados en aprendizaje profundo. Este proceso permite utilizar directamente las secuencias de video anotadas para entrenar y evaluar un detector de objetos que identifique jugadores y balón en cada fotograma.

En primer lugar, se analizó la estructura original del dataset, el cual se encuentra organizado en secuencias independientes que contienen fotogramas ordenados temporalmente (img1), archivos de anotación (gt.txt) y archivos de configuración (seqinfo.ini y gameinfo.ini). A partir de estos archivos, se identificaron tanto las coordenadas espaciales de los objetos como la información semántica asociada a cada tracklet, incluyendo el tipo de objeto (jugador, portero, árbitro o balón).

Posteriormente, se realizó la conversión de las anotaciones al formato YOLO, en el cual cada objeto es representado mediante una línea que contiene el identificador de clase y las coordenadas normalizadas de la caja delimitadora (𝑥𝑐,𝑦𝑐,𝑤,ℎ). Para ello, las coordenadas originales en píxeles fueron transformadas a valores relativos al ancho y alto de la imagen, utilizando la información proporcionada en seqinfo.ini. En esta etapa se definió un mapeo de clases, asignando la clase player a jugadores y porteros, y la clase ball al balón, mientras que los árbitros fueron excluidos del proceso.

A continuación, las imágenes y sus respectivas etiquetas fueron reorganizadas en una estructura de carpetas compatible con YOLO, separando los datos en conjuntos de entrenamiento y validación. Esta división se realizó respetando el orden temporal de los fotogramas dentro de cada secuencia, con el fin de evitar fugas de información entre ambos conjuntos y garantizar una evaluación más realista del desempeño del modelo.

Finalmente, se generó un archivo de configuración (.yaml) que especifica las rutas de acceso al dataset, el número de clases y los nombres correspondientes a cada clase. Este archivo permite a YOLO acceder correctamente a las imágenes y anotaciones durante las fases de entrenamiento y validación. Como paso de verificación, se realizó una visualización aleatoria de imágenes con sus cajas delimitadoras para confirmar la correcta conversión de las anotaciones y la coherencia entre imágenes y etiquetas.

## 3.2 Detección y Seguimiento con YOLOv8n

Se utilizó un modelo YOLOv8n preentrenado sobre el conjunto de datos COCO como punto de partida, aplicando una estrategia de transfer learning. Esta aproximación permite aprovechar representaciones previamente aprendidas para acelerar la convergencia del modelo y mejorar el desempeño, especialmente cuando el conjunto de datos específico es limitado. El modelo fue ajustado (fine-tuning) utilizando únicamente las clases de interés del problema.

El entrenamiento se realizó durante un número determinado de épocas, empleando un subconjunto controlado de imágenes con el fin de reducir el tiempo computacional y facilitar la experimentación. Durante este proceso, se monitorizaron métricas estándar de detección, tales como la pérdida del modelo, la precisión, el recall y el mean Average Precision (mAP), evaluadas sobre el conjunto de validación.

Una vez finalizado el entrenamiento, se seleccionó el modelo con mejor desempeño según la métrica mAP en validación. Finalmente, se realizó una evaluación cualitativa mediante la visualización de detecciones sobre imágenes de validación, verificando la correcta localización de jugadores y balón. Las detecciones obtenidas en esta etapa constituyen la entrada para la fase posterior de seguimiento multi-objeto, donde se analiza la evolución temporal de los objetos detectados.

## 3.3 Detección y Seguimiento con YOLOv8l

Para la detección de jugadores utilizando YOLOv8l se utilizó al igual que en las anteriores el dataset de SoccerNet, para este ejercicio se tomó un partido en específico descargado desde el dataset y guardado en el content local del collab..

El procedimiento se llevó a cabo dividiendo el video completo en múltiples segmentos para reducir la carga computacional, cada segmento se procesa de manera independiente a petición del usuario, generando una salida donde se almacenará el clip resultante con las anotaciones visuales.

En cada fotograma del segmento, la imagen se redimensiona a una resolución menor para acelerar la inferencia del modelo **YOLOv 8l.pt**. 

Una vez procesada la imagen, el modelo produce predicciones de cajas delimitadoras para tanto el balón como el jugador, las detecciones pasan por filtros adicionales basados en restricciones de área, proporciones y coherencia geométrica, asegurando que solo se conservan objetos válidos, como los anteriormente mencionados.

Una vez filtradas, las detecciones se envían al rastreador **DeepSORT**, que mantiene la identidad de los objetos a lo largo de los fotogramas. El rastreador realiza predicciones de movimiento utilizando un filtro de Kalman, que anticipa la ubicación del objeto antes de recibir una nueva detección. La predicción sigue la forma:

$$
x_k = F\, x_{k-1} + w_k
$$


mientras que la corrección, una vez llegada la medición real, se calcula como

$$
x_k = x_k + K ( z_k - H x_k )
$$

El filtro de Kalman predice la posición futura de un objeto mediante la ecuación…

$$
x_k = F x_{k-1} + w_k
$$

***`xk` \= estado predicho***

***`F` \= matriz de transición***

***`wk` \= ruido del proceso***

 y corrige esa predicción con información nueva a través de…

$$
x_k = x_k + K ( z_k - H x_k )
$$

***`zk` \= medición observada***

***`H` \= matriz de observación***

***`K` \= ganancia de Kalman***

Estas ecuaciones permiten suavizar variaciones abruptas y mantener trayectorias coherentes. Además del modelo de movimiento, **DeepSORT** utiliza una medida de similitud de apariencia basada en distancia del coseno para asociar correctamente las detecciones con trayectorias anteriores.

En el caso del balón, se incorpora un mecanismo adicional para manejar situaciones en las que no se detecta durante varios fotogramas consecutivos. Si el balón ha sido visible recientemente, se estima una posición predicha mediante extrapolación lineal basada en el desplazamiento previo. Esta estimación puede expresarse como ***Xpredicted \= Xprev \+ α(Xprev − Xant)*** & ***Ypredicted \= Yprev \+ α(Yprev − Yant)***, donde α controla la suavidad de la predicción. Este mecanismo evita pérdidas bruscas del balón durante oclusiones momentáneas y mantiene la continuidad de la trayectoria.

Cada fotograma del segmento se anota con cajas, etiquetas y marcas de seguimiento, tanto para jugadores como para el balón o su posición predicha. 

Adicionalmente, cada posición del balón detectado se almacena para formar una trayectoria a lo largo del video completo. Para reducir irregularidades en este registro, puede emplearse un suavizado mediante ventana móvil, donde cada nuevo punto se calcula como el promedio de los últimos valores registrados:

***x\_suave(i) \= promedio\[x(i−w+1) … x(i)\]***  
***y\_suave(i) \= promedio\[y(i−w+1) … y(i)\]***

Finalmente, los fotogramas anotados se escriben en el archivo de salida del segmento correspondiente. Este procedimiento puede repetirse para todos los segmentos hasta reconstruir el análisis completo del video. 

El resultado final consiste en clips procesados con detección y seguimiento integrados.

# 4\. Resultados y Análisis

## 4.1 Análisis Exploratorio 

Primero se realizó la inspección visual del dataset. 

<p align="center">
  <img src="/DigiVision/sistema-de-deteccion/results/Ejemplo IMG.png" alt="image1" />
</p> 

Se realizó un muestreo de la distribución de cada una de las secuencias:

| seq | fps | W | H | seqLength |  |
| ----- | ----: | ----: | ----: | ----: | ----- |
| **0** | SNMOT-060 | 25 | 1920 | 1080 | 750 |
| **1** | SNMOT-061 | 25 | 1920 | 1080 | 750 |
| **2** | SNMOT-062 | 25 | 1920 | 1080 | 750 |
| **3** | SNMOT-063 | 25 | 1920 | 1080 | 750 |
| **4** | SNMOT-064 | 25 | 1920 | 1080 | 750 |
| **5** | SNMOT-065 | 25 | 1920 | 1080 | 750 |
| **6** | SNMOT-066 | 25 | 1920 | 1080 | 750 |
| **7** | SNMOT-067 | 25 | 1920 | 1080 | 750 |
| **8** | SNMOT-068 | 25 | 1920 | 1080 | 750 |
| **9** | SNMOT-069 | 25 | 1920 | 1080 | 750 |

Todas las secuencias tienen la misma distribución.

Se realiza la distribución de clase. Puede notarse un gran desbalance entre la clase jugadores y la clase balón. Esto dificulta el buen desempeño del modelo al realizar la detección de las clases en cada uno de los frames.

<p align="center">
  <img src="/DigiVision/sistema-de-deteccion/results/Clases.png" alt="image1" />
</p>

player: 56203 | ball: 3564 | ratio (player/ball): 15.769640852974186

Finalmente, se reconstruye una de las secuencias como ejemplo.

<p align="center">
<video controls width="720" preload="metadata">
  <source src="https://raw.githubusercontent.com/BibianaPV/DigiVision/main/sistema-de-deteccion/results/SNMOT064_preview.mp4" type="video/mp4">
</video>
</p>

### 4.1.2 Creación Dataset formato YOLO

se realizó la conversión de las anotaciones al formato YOLO, en el cual cada objeto es representado mediante una línea que contiene el identificador de clase y las coordenadas normalizadas de la caja delimitadora (𝑥𝑐,𝑦𝑐,𝑤,ℎ). Se guardó en la carpeta *[/data](https://drive.google.com/drive/folders/1fHlZdzffsMKoR8RoTJ6lrVCPH4ihi5EG?usp=drive_link)* para ser utilizado posteriormente.

Se realiza la visualización de ejemplos tanto de train como de val para verificar el funcionamiento de las nuevas anotaciones.

<p align="center">
  <img src="/DigiVision/sistema-de-deteccion/results/ejmTrain.png" width="500" />
  <img src="/DigiVision/sistema-de-deteccion/results/ejmVal.png" width="500" />
</p> 

## 4.2 Detección y seguimiento de Objetos: YOLOv8n
Primero se realizó la detección con YOLOv8n en 5 épocas y se tomó el mejor modelo, /results. Se puede observar que con el subset tan bajo y el poco entrenamiento no se alcanza a detectar el balón.

Class     Images  Instances      Box(P          R      mAP50  mAP50-95): 100% ━━━━━━━━━━━━ 32/32 1.3it/s 24.1s
all        500       7504      0.444      0.405      0.432      0.201
player        500       7056      0.887       0.81      0.865      0.403
ball        438        448          0          0          0          0

Esto puede observarse en la matriz de confusión donde el balón se confunde con el fondo o no es detectado. 

<p align="center">
  <img src="/DigiVision/sistema-de-deteccion/results/YOLOv8n/runs/detect/val2/confusion_matrix.png" width="500" />
  <img src="/DigiVision/sistema-de-deteccion/results/YOLOv8n/runs/detect/val2/BoxR_curve.png" width="500" />
</p> 

Los resultados obtenidos muestran que el modelo YOLO presenta un alto desempeño en la detección de jugadores, alcanzando valores elevados de precisión, recall y mAP, lo que confirma su capacidad para identificar objetos de gran tamaño y alta frecuencia en escenas de fútbol. Sin embargo, el desempeño global del modelo se ve afectado por la detección nula del balón, una clase minoritaria y visualmente más compleja debido a su reducido tamaño, oclusiones frecuentes y movimiento rápido.
Este comportamiento evidencia una limitación común en modelos de detección cuando se enfrentan a fuertes desbalances de clase y objetos pequeños, incluso al emplear técnicas de transferencia de aprendizaje.

<p align="center">
  <img src="/DigiVision/sistema-de-deteccion/results/YOLOv8n/runs/detect/val2/val_batch0_labels.jpg" width="700" />
</p> 

Posteriormente, se realizó un seguimiento , el seguimiento se realizó utilizando una estrategia basada en asociación espacial mediante Intersection over Union (IoU) entre las cajas detectadas en fotogramas consecutivos. Para cada detección actual, se compara su superposición con las detecciones del fotograma anterior y se asigna el mismo identificador (ID) al objeto cuya IoU supera un umbral predefinido. En caso contrario, se crea un nuevo ID.

Secuencia 62 y 64 :
<iframe
  src="https://drive.google.com/file/d/1R4G23XqFXWk1gO19bH4I8PLFllgDA5dt/preview"
  width="720"
  height="405"
  allow="autoplay">
</iframe>

<iframe
  src="https://drive.google.com/file/d/1A3PnXaEeTomFPfBYTevv1mSOHXHlkdHY/preview"
  width="720"
  height="405"
  allow="autoplay">
</iframe>

Puede observarse que existen cambios en los Ids y se hace dificil el seguimiento. Por tal motivo, para cada track se utilizó predicción Kalman (centro y velocidad), desplazamiento por flujo óptico LK (mueve la caja según píxeles), combinación de ambas predicciones, asociación con Hungarian (como SORT) usando costo 1 - IoU y Umbrales distintos por clase (player vs ball). Al final, se obtuvo  video con IDs.

Secuencia 62 
<iframe
  src="https://drive.google.com/file/d/1u1ek9N67C-r_ToLKnjkz6Lb6cVisWoDN/preview"
  width="720"
  height="405"
  allow="autoplay">
</iframe>

=== PLAYERS (IoU ≥ 0.5) ===
Secuencia	MOTA	IDF1	Num. switches	Precisión	Recall	Falsos positivos	Misses
SNMOT-062	0.405443	0.364119	224	0.644354	0.948078	6057	601

=== BALL (IoU ≥ 0.3) ===
Secuencia	MOTA	IDF1	Num. switches	Precisión	Recall	Falsos positivos	Misses
SNMOT-062	0.000000	0.000000	0	NaN	0.000000	0	750

<iframe
  src="https://drive.google.com/file/d/1MVhxkK1w_AxfrQtPZcQTgfhwKlL-J-yc/preview"
  width="720"
  height="405"
  allow="autoplay">
</iframe>

=== PLAYERS (IoU ≥ 0.5) ===
Secuencia	MOTA	IDF1	Num. switches	Precisión	Recall	Falsos positivos	Misses
SNMOT-062	-0.042748	0.217297	708	0.502567	0.811563	11143	2614

=== BALL (IoU ≥ 0.3) ===
Secuencia	MOTA	IDF1	Num. switches	Precisión	Recall	Falsos positivos	Misses
SNMOT-062	0.000000	0.000000	0	NaN	0.000000	0	590

Aunque pueden seguirse los jugadores, no es posible con este entrenamiento de detección seguir el balón.

## 4.3 Detección y seguimiento de Objetos: YOLOv8l

En base a la metodología aplicada se obtuvieron tres clips de 40 segundos cada uno. En estos se aprecia claramente la detección de los distintos jugadores durante el partido con un recuadro verde, así como una adecuada detección del balón, el cual es marcado con un recuadro amarillo.

Si nos dirigimos a la siguiente tabla veremos que por cada frame analizado estamos obteniendo valores muy similares y resultados estables.

| Nº | Resolución | Conteo de objetos | Tiempo total (ms) | Preprocess (ms) | Inference (ms) | Postprocess (ms) | Shape |
| ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- |
| 1 | 384x640 | 3 persons, 1 sports ball | 40.5 | 3.8 | 40.5 | 2.0 | (1, 3, 384, 640\) |
| 2 | 384x640 | 4 persons, 1 sports ball | 40.0 | 5.5 | 40.0 | 1.9 | (1, 3, 384, 640\) |
| 3 | 384x640 | 3 persons, 1 sports ball | 38.4 | 2.3 | 38.4 | 2.1 | (1, 3, 384, 640\) |
| 4 | 384x640 | 2 persons, 1 sports ball | 38.4 | 2.9 | 38.4 | 2.0 | (1, 3, 384, 640\) |
| 5 | 384x640 | 2 persons, 1 sports ball | 38.3 | 4.5 | 38.3 | 2.0 | (1, 3, 384, 640\) |

<p align="center">
  <img src="/DigiVision/sistema-de-deteccion/results/YOLOv8l/detecciones_clip_25.png" alt="image1" />
</p>

Los datos muestran consistencia en la velocidad del modelo, especialmente en la etapa de **inferencia**, que oscila entre 38.3 y 40.5 ms sin depender demasiado del número de objetos detectados.

<p align="center">
  <img src="/DigiVision/sistema-de-deteccion/results/YOLOv8l/ClipScreenYolo8vl.png" alt="image1" />
</p>

La variación más grande se observa en los tiempos de **preprocesamiento**, lo cual indica que esta etapa está más sujeta a fluctuaciones posiblemente por carga del sistema. 

Aun así, todos los tiempos se mantienen dentro de un rango estrecho que demuestra un rendimiento estable, confirmando que el modelo funciona de forma eficiente incluso con cambios en la cantidad y tipos de objetos presentes en cada cuadro.

No obstante, también fue posible identificar algunas limitaciones. En el caso de los jugadores, cuando varios de ellos se cruzan a muy poca distancia, los recuadros tienden a volverse erráticos y, en diversas ocasiones, algunos se pierden temporalmente o se agrandan de manera desmedida.

En lo referente al balón, cuando éste acelera de forma brusca tiende a perderse la localización, recuperándola únicamente cuando disminuye su velocidad o cuando un jugador realiza una parada.

<p align="center">
  <img src="/DigiVision/sistema-de-deteccion/results/YOLOv8l/ErrorScreenYolo8vl.png" alt="image1" />
</p>

Es probable que, con una mayor capacidad de cómputo o con videos grabados desde ángulos más cercanos o con mayor definición, estas problemáticas pueden mitigarse. Sin embargo, dado que el dataset SoccerNet solo ofrece videos en resoluciones de 244p o 720p, y que un escalado a resoluciones superiores incrementa significativamente los tiempos de procesamiento, los resultados obtenidos corresponden a las condiciones reales de entrada y se reflejan en los clips generados.

Video 1:
<iframe
  src="https://drive.google.com/file/d/1ogpWJWDzcWrc2DRT5V6fDRSqtK7v8scA/preview"
  width="720"
  height="405"
  allow="autoplay">
</iframe>

Video 2:
<iframe
  src="https://drive.google.com/file/d/1b7F-HP43qiCf7eKUj6ZLbdO64ALQ3F7d/preview"
  width="720"
  height="405"
  allow="autoplay">
</iframe>

Video 3:
<iframe
  src="https://drive.google.com/file/d/1awXv5OFX08el6dbBZDFmD8HHYr2m7K_o/preview"
  width="720"
  height="405"
  allow="autoplay">
</iframe>


# 5\. Referencias Bibliográficas

Cioppa, A., Giancola, S., & Ghanem, B. (2019). ARTHuS: Adaptive real-time human segmentation in sports videos. En Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition Workshops (CVPRW) (pp. 1–10). IEEE.  
[https://doi.org/10.1109/CVPRW.2019.00010](https://doi.org/10.1109/CVPRW.2019.00010)

Deliège, A., Cioppa, A., Giancola, S., Seikavandi, M. J., Dueholm, J. V., Nasrollahi, K., Moeslund, T. B., & Ghanem, B. (2021). SoccerNet-v2: A dataset and benchmarks for holistic understanding of broadcast soccer videos. IEEE Transactions on Pattern Analysis and Machine Intelligence, 44(12), 8928–8948. [https://doi.org/10.1109/TPAMI.2021.3119082](https://doi.org/10.1109/TPAMI.2021.3119082)

Gudmundsson, J., & Horton, M. (2017). Spatio-temporal analysis of team sports. ACM Computing Surveys, 50(2), Article 22\. [https://doi.org/10.1145/3054132](https://doi.org/10.1145/3054132)

Mackenzie, R., & Cushion, C. (2013). Performance analysis in football: A critical review and implications for future research. Journal of Sports Sciences, 31(6), 639–676. [https://doi.org/10.1080/02640414.2012.746720](https://doi.org/10.1080/02640414.2012.746720)

Ren, S., He, K., Girshick, R., & Sun, J. (2015). Faster R-CNN: Towards real-time object detection with region proposal networks. Advances in Neural Information Processing Systems, 28, 91–99

Redmon, J., Divvala, S., Girshick, R., & Farhadi, A. (2016). *You Only Look Once: Unified, real-time object detection*. En Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition (CVPR) (pp. 779–788). [https://doi.org/10.1109/CVPR.2016.91](https://doi.org/10.1109/CVPR.2016.91)

Redmon, J., & Farhadi, A. (2018). *YOLOv3: An incremental improvement*. arXiv preprint arXiv:1804.02767.

Jocher, G., Chaurasia, A., & Qiu, J. (2023). *YOLOv8*. Ultralytics. [https://github.com/ultralytics/ultralytics](https://github.com/ultralytics/ultralytics)

Ren, S., He, K., Girshick, R., & Sun, J. (2015). *Faster R-CNN: Towards real-time object detection with region proposal networks*. Advances in Neural Information Processing Systems, 28, 91–99.

Bewley, A., Ge, Z., Ott, L., Ramos, F., & Upcroft, B. (2016). *Simple online and realtime tracking*. En Proceedings of the IEEE International Conference on Image Processing (ICIP) (pp. 3464–3468). [https://doi.org/10.1109/ICIP.2016.7533003](https://doi.org/10.1109/ICIP.2016.7533003)

Gudmundsson, J., & Horton, M. (2017). *Spatio-temporal analysis of team sports*. ACM Computing Surveys, 50(2), Article 22\. [https://doi.org/10.1145/3054132](https://doi.org/10.1145/3054132)

Lucas, B. D., & Kanade, T. (1981). *An iterative image registration technique with an application to stereo vision*. En Proceedings of the 7th International Joint Conference on Artificial Intelligence (IJCAI) (pp. 674–679).

Wojke, N., Bewley, A., & Paulus, D. (2017). *Simple online and realtime tracking with a deep association metric*. En Proceedings of the IEEE International Conference on Image Processing (ICIP) (pp. 3645–3649). [https://doi.org/10.1109/ICIP.2017.8296962](https://doi.org/10.1109/ICIP.2017.8296962)

# 6\. Reporte de Contribución Individual

| Estudiante | Aporte Personal |
| :---- | :---- |
| Bibiana Andrea Peña V. |  |
| Juan Felipe Arbeláez. |  |
| Leidy Marcela Leal L. |  |






<p style="text-align:center;">
  <a href="{{ "/" | relative_url }}">⬅ Volver al inicio</a>
  <a href="https://github.com/bibianapv/DigiVision" target="_blank">🌐 Ver en GitHub</a>
</p>
