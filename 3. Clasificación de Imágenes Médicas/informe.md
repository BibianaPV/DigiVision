# 1\.Introducción
La clasificación automática de imágenes médicas se ha convertido en un apoyo importante para el diagnóstico clínico, especialmente en situaciones donde es necesario identificar patologías a partir de estudios de imágenes. En el caso de las radiografías de tórax, reconocer de manera temprana condiciones como la neumonía es clave en términos de salúd pública. Tradicionalmente, los procesos computacionales se han basado en descriptores de forma, textura e intensidad diseñados manualmente, combinados con clasificadores clásicos como SVM, k-NN o Random Forest. Sin embargo, el avance de la redes neuronales convolucionales (CNN) en los últimos años ha cambiado este panorama, ya que permiten aprender directamente de los datos y superando en muchos casos el desempeño de los métodos clásicos. 

El actual trabajo tiene como fin desarrollar y comparar el desempeño de ambos enfoques, considerando descriptores handcrafted y modelos basados en deep learning, para lograr diferenciar radiografías de tórax de pacientes con o sin neumonía. El proceso se estructura en tres fases principales: análisis exploratorio y procesamiento del conjunto de datos; diseño y extracción de descriptores clásicos con clasificadores tradicionales, finalmente, entrenamiento de modelos convolucionales .

# 2\. Marco teórico

## 2.1. Análisis Exploratorio y Preprocesamiento
El preprocesamiento de imágenes es una etapa fundamental en los sistemas de visión por computador,  su objetivo es normalizar las imágenes de entrada, reducir la variabilidad no deseada y garantizar que los algoritmos de extracción de características y clasificación trabajen sobre datos coherentes y comparables.

Para el análisis exploratorio, se utiliza la visualización inicial de ejemplos representativos, esto permite identificar variaciones en calidad, contraste y patrones anatómicos entre clases, lo cual es clave para anticipar posibles retos en la clasificación. El análisis de la distribución de clases revela la presencia de desbalance, un factor crítico que puede influir en el desempeño de los modelos de aprendizaje automático. De igual forma, examinar la distribución de los tamaños originales de las imágenes permite evidenciar la heterogeneidad propia de los estudios radiológicos provenientes de diferentes equipos, lo que justifica la necesidad de normalizar las dimensiones antes de extraer características. Así, se puede obtener una visión global del dataset que permite detectar inconsistencias y orienta la selección adecuada de técnicas de preprocesamiento.
1. Normalización de tamaño, se realiza mediante interpolación y busca unificar resoluciones en modelos que requieren entradas fijas, facilitar la comparación entre imágenes, reducir errores asociados a escalas diferentes, mejorar la reproducibilidad del proceso analítico.

2 Normalización de intensidad:convierte las imágenes a un rango estándar ( [0–255] o [0–1]), reduciendo la influencia de diferencias técnicas y permitiendo una comparación más consistente entre imágenes.

Matemáticamente, una normalización min–max típica se formula como:

$$
I_{\text{norm}} = \frac{\,I - I_{\min}\,}{\,I_{\max} - I_{\min}\,}
$$​

3. Mejora de contraste mediante CLAHE: Ecualización adaptativa de histograma limitada por contraste (CLAHE) tiene la capacidad de realzar contrastes locales sin amplificar excesivamente el ruido. Para esto, se divide la imagen en bloques pequeños, ecualiza cada uno y limita el contraste máximo permitiendo: resaltar estructuras anatómicas relevantes, mejorar bordes y detalles finos, evitar sobreecualización que podría introducir artefactos, preservar características radiológicas sutiles esenciales para diagnóstico.
	​
4. Segmentación: permite delimitar regiones anatómicas de interés y separar estructuras relevantes del fondo o de tejidos no deseados. En el caso de radiografías de tórax, la creación de máscaras pulmonares facilita el aislamiento del parénquima y reduce la influencia de elementos externos como costillas, tejidos blandos o marcadores técnicos. Este procedimiento no solo mejora la uniformidad del conjunto de datos, sino que también incrementa la precisión de los descriptores de forma y textura al concentrarse exclusivamente en la región anatómica pertinente. Técnicas modernas de segmentación basadas en deep learning —como modelos U-Net entrenados en bases especializadas— permiten generar máscaras robustas incluso ante variabilidad radiológica significativa. En este sentido, la segmentación actúa como un filtro anatómico que optimiza la extracción de características y fortalece el desempeño de los modelos posteriores de clasificación o análisis cuantitativo.

## 2.2 Extracción de Descriptores Clásicos
### 2.2.1 Descriptores de Forma

#### 2.2.1.1 Descriptores de contorno

#### 2.2.1.2 Fourier Shape Descriptors 


#### 2.2.1.3 Momentos de Hu
Los momentos invariantes de Hu, propuestos por Ming-Kuei Hu en 1962, constituyen uno de los descriptores de forma más utilizados en visión por computador debido a su capacidad para representar la geometría de un objeto de manera invariante a traslación, rotación y cambio de escala (Hu, 1962). Estos descriptores se derivan de combinaciones algebraicas de los momentos centrales normalizados de segundo y tercer orden, lo que permite capturar características globales de la forma sin depender de su orientación o tamaño.

El enfoque se basa en los momentos geométricos, definidos para una imagen 𝑓(𝑥,𝑦) como:

$$
m_{pq} = \sum_x \sum_y \left( x^p \, y^q \, f(x, y) \right)
$$

Los momentos centrales, que introducen invariancia a traslación, se expresan como:

$$
\mu_{pq} =
\sum_x \sum_y
\left(
(x - \bar{x})^{p} \,
(y - \bar{y})^{q} \,
f(x, y)
\right)
$$

donde:
$$
\bar{x} = \frac{m_{10}}{m_{00}}
\qquad
\bar{y} = \frac{m_{01}}{m_{00}}
$$
corresponden al centroide del objeto.

Para lograr invariancia a escala, se introducen los momentos centralizados normalizados:

$$
\eta_{pq} = \frac{\mu_{pq}}{\mu_{00}^{\gamma}}
$$

donde:
$$
\gamma = \frac{p + q}{2} + 1
$$

A partir de estos valores, Hu desarrolló siete combinaciones invariantes, conocidas como Hu Moments, que permiten describir la forma completa del objeto en términos globales,  capturando propiedades tales como:

+Simetría global
+Distribución espacial de masas
+Proporciones y elongación
+Grado de irregularidad
+Relación entre ejes principales

### 2.2.2 Descriptores de Textura

#### 2.2.2.1  Local Binary Patterns (LBP)


#### 2.2.2.2  Gray Level Co-occurrence Matrix (GLCM)
La Matriz de Co-ocurrencia de Niveles de Gris (GLCM) es un método estadístico clásico utilizado para describir la textura en imágenes. En GLCM se calcula la frecuencia con la que pares de píxeles con determinadas intensidades, separados por una distancia específica y en una orientación determinada están presentes en una imagen. Esta información permite capturar patrones de repetición, rugosidad o regularidad de la textura de la imagen.

A partir de la GLCM se pueden extraer varias propiedades que caracterizan la textura de la imagen:

**Contraste:** mide la variación de intensidad entre píxeles vecinos. Valores altos de contraste indican que la imagen tiene transiciones fuertes o bordes marcados, mientras que valores bajos indican regiones más homogéneas.

**Correlación:** evalúa la dependencia lineal entre los niveles de gris de píxeles vecinos. Una correlación alta sugiere que los valores de intensidad están relacionados de manera predecible, mientras que valores bajos reflejan una textura más aleatoria o irregular.

**Energía:** refleja la uniformidad o repetitividad de la textura. Valores altos de energía indican que la imagen contiene patrones repetitivos y homogéneos, mientras que valores bajos indican una textura más heterogénea y variada.

**Homogeneidad:** mide qué tan cercanos están los valores de intensidad a la diagonal principal de la GLCM, lo que se interpreta como el grado de uniformidad local. Una homogeneidad alta indica que los píxeles vecinos tienen intensidades similares, mientras que una homogeneidad baja indica diferencias más pronunciadas entre los niveles de gris de píxeles adyacentes.

Estas propiedades permiten cuantificar y comparar la textura de diferentes regiones o imágenes de manera objetiva, siendo muy útiles en aplicaciones médicas, industriales y de visión por computadora donde la textura es un criterio discriminante importante.

#### 2.2.2.3 Filtros de Gabor
Los filtros de Gabor constituyen una familia de operadores lineales diseñados para analizar simultáneamente la información espacial y de  frecuencia de una imagen. Se basa en la modulación de una función sinusoidal por una envolvente gaussiana, lo que permite lograr localización conjunta en dominio espacial y en dominio de frecuencia. Esta propiedad los convierte en herramientas especialmente adecuadas para la detección de bordes, texturas y estructuras orientadas, características relevantes en imágenes médicas como radiografías de tórax. La expresión general del filtro de Gabor bidimensional es:

$$
g(x, y) =
\exp\!\left(
-\frac{x'^{2} + \gamma^{2} y'^{2}}{2\sigma^{2}}
\right)
\,
\cos\!\left(
2\pi \frac{x'}{\lambda} + \psi
\right)
$$

donde las variables rotadas se definen como:

$$
\begin{aligned}
x' &= x \cos(\theta) + y \sin(\theta), \\
y' &= -x \sin(\theta) + y \cos(\theta)
\end{aligned}
$$

Los parámetros del filtro son:
lambda: longitud de onda de la función sinusoidal (controla la frecuencia espacial).
theta: orientación del filtro.
psi: fase de la onda sinusoidal.
sigma: desviación estándar de la envolvente gaussiana.
gamma: relación de aspecto que ajusta la elipticidad del núcleo.

La combinación de estos parámetros permite definir filtros sensibles a distintas escalas y orientaciones, construyendo así un banco de filtros Gabor multi–escala y multi–orientación.

## 2.3 Clasificación con Descriptores Clásicos

### 2.3.1 SVC: Support Vector Machine
La Máquina de Vectores de Soporte (SVM) es un modelo de aprendizaje supervisado ampliamente utilizado para problemas de clasificación.
Su principio se basa en encontrar un hiperplano óptimo que separe de forma máxima las clases en el espacio de características. Para datos no linealmente separables, SVM utiliza funciones núcleo (kernels) que proyectan los datos a un espacio de mayor dimensión donde la separación es posible. Entre los kernels más comunes se encuentran:

**RBF (Radial Basis Function)**: Transforma los datos en un espacio de alta dimensión usando una función gaussiana, permitiendo separar incluso patrones muy complejos y no lineales.
**Polynomial (Poly)**: Proyecta los datos a un espacio de características mediante un polinomio de grado ddd, capturando relaciones no lineales entre las variables.
**Linear:** No realiza transformación, por lo que es útil cuando los datos ya son linealmente separables; su simplicidad hace que sea eficiente y menos propenso a sobreajuste.

El proceso de estandarización de variables es fundamental para SVM, ya que el modelo depende de distancias en el espacio de características.

### 2.3.2 Random Forest
Método de aprendizaje supervisado basado en la combinación de múltiples árboles de decisión para mejorar la precisión predictiva y la capacidad de generalización del modelo (Leo Breiman, 2001). Random Forest pertenece a la familia de los métodos de ensamble, específicamente al enfoque conocido como bagging (bootstrap aggregating), su objetivo es reducir la varianza del modelo mediante el entrenamiento de múltiples clasificadores sobre subconjuntos aleatorios del conjunto de datos. Cada árbol de decisión dentro del bosque se construye utilizando dos tipos de aleatoriedad:

+Muestreo bootstrap: consiste en seleccionar aleatoriamente, con reemplazo, un subconjunto de las instancias originales para entrenar cada árbol.
+Selección aleatoria de atributos: en cada nodo del árbol, se evalúa únicamente un subconjunto aleatorio de características para determinar la mejor división.

Estas fuentes de aleatoriedad reducen la correlación entre árboles, lo que mejora el desempeño del ensamble

* Construcción de árboles
Dado un conjunto de entrenamiento D con N instancias, Random Forest genera B subconjuntos bootstrap:



Cada subconjunto tiene tamaño N. Sobre cada D_b se entrena un árbol de decisión T_b.

* Divisiones en los nodos (split function)
En cada nodo del árbol, se selecciona aleatoriamente un subconjunto de k características.Se evalúa una función de pureza para encontrar la mejor división.

* Proceso de votación en clasificación
Una vez entrenados los B árboles, la predicción final para una instancia x se obtiene mediante votación por mayoría:

y_pred = mode( T_1(x), T_2(x), ..., T_B(x) )

donde T_b(x) es la predicción del árbol b.

En regresión, se toma el promedio:

y_pred = (1 / B) * sum( T_b(x) )

Algunas ventajas que tiene este modelo son, la reducción de la varianza del modelo sin aumentar significativamente el sesgo , es robusto frente a sobreajuste gracias a la aleatorización, maneja bien datos con ruido y características irrelevantes, puede calcular medidas de importancia de variables, funciona adecuadamente aunque los datos no estén escalados.

# 3. Metodología
## 3.1 Análisis Exploratorio y Preprocesamiento
El procesamiento se estructuró en tres fases principales: (1) análisis exploratorio inicial del dataset, (2) definición del pipeline de preprocesamiento y (3) generación del conjunto de datos normalizado. Todo el procedimiento se llevó a cabo utilizando herramientas de visión por computador, principalmente OpenCV y NumPy, sobre el dataset Chest X-Ray Pneumonia.

### 3.1.1. Análisis exploratorio de datos
El análisis exploratorio tuvo como objetivo comprender la estructura y las características del dataset antes de aplicar cualquier transformación. En primer lugar, se realizó una visualización de muestras representativas de ambas clases (NORMAL y PNEUMONIA), lo que permitió identificar variaciones en el contraste, la iluminación y la presencia de artefactos. Posteriormente, se evaluó la distribución de clases con el fin de reconocer posibles desbalances que pudieran afectar el rendimiento de futuros modelos de clasificación. Finalmente, se analizó la distribución del tamaño original de las imágenes, observándose una amplia variabilidad entre estudios radiográficos, lo cual justificó la necesidad de normalizar las dimensiones como parte del preprocesamiento.

### 3.1.2. Pipeline de preprocesamiento
Con base en los resultados del análisis exploratorio, se definió un pipeline de preprocesamiento orientado a homogenizar y mejorar las imágenes antes de su uso en etapas posteriores.

### 3.1.2.1 Normalización espacial
Todas las imágenes fueron reescaladas a dimensiones fijas de 256×256 píxeles mediante interpolación bilineal. Este paso permitió estandarizar la resolución de entrada, reducir la variabilidad asociada a distintos equipos radiológicos y facilitar la aplicación de descriptores de textura y modelos de clasificación.

### 3.1.2.2 Normalización e intensificación del contraste
Para mejorar la visibilidad de estructuras anatómicas relevantes y compensar diferencias en iluminación, se aplicó la técnica CLAHE .

### 3.1.2.3 Preparación de estructura de salida
Se creó una estructura de carpetas espejo que replica la organización del dataset original en sus particiones: train, test y val. Esto permitió almacenar de manera ordenada las imágenes resultantes y garantizar trazabilidad entre los conjuntos original y procesado.

### 3.1.3. Generación del dataset preprocesado
Una vez definido el pipeline, se procesaron de forma secuencial todas las imágenes del dataset. Finalmente, la imagen transformada se guardó en el directorio correspondiente según su categoría y partición.

### 3.1.2 Segmentación
Para la segmentación se utilizaron dos metodologías.



