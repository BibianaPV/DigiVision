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
I_{\text{norm}} =
\frac{I - I_{\min}}{I_{\max} - I_{\min}}
$$

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
m_{pq} = \sum_x \sum_y ( x^p * y^q * f(x, y) )
$$


Los momentos centrales, que introducen invariancia a traslación, se expresan como:

$$
\mu_{pq} = \sum_x \sum_y \big( (x - \bar{x})^p * (y - \bar{y})^q * f(x, y) \big)
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
\exp\left(
-\frac{x'^{2} + \gamma^{2} y'^{2}}{2\sigma^{2}}
\right)
\*
\cos\left(
2\pi \frac{x'}{\lambda} + \psi
\right)
$$


donde las variables rotadas se definen como:

$$
\begin{aligned}
x' &= x \cos(\theta) + y \sin(\theta) \\
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

$$
D_b = \{\, (x_i, y_i) \ \text{seleccionados con reemplazo} \,\},
\qquad
b = 1, 2, \ldots, B
$$


Cada subconjunto tiene tamaño N. Sobre cada D_b se entrena un árbol de decisión T_b.

* Divisiones en los nodos (split function)
En cada nodo del árbol, se selecciona aleatoriamente un subconjunto de k características.Se evalúa una función de pureza para encontrar la mejor división.

* Proceso de votación en clasificación
Una vez entrenados los B árboles, la predicción final para una instancia x se obtiene mediante votación por mayoría:

$$
\hat{y} = \mathrm{mode}( T_1(x), T_2(x), \ldots, T_B(x) )
$$


donde \; T_b(x) \; \text{es la predicción del árbol } b.


En regresión, se toma el promedio:

$$
\hat{y} = \frac{1}{B} \sum_{b=1}^{B} T_b(x)
$$


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

#### 3.1.2.1 Juan


#### 3.1.2.2 Segmentación automática de los pulmones mediante un modelo PSPNet
Para segmentar los pulmones, se utilizó un modelo PSPNet preentrenado en el conjunto ChestX-Det, disponible en la librería TorchXRayVision. Este modelo se seleccionó debido a su capacidad para detectar estructuras anatómicas en radiografías de tórax con alta precisión. El modelo produce mapas de probabilidad para diversas estructuras, entre ellas Left Lung y Right Lung, que se integran para obtener una máscara pulmonar final.

Cada imagen radiográfica se sometió a los siguientes pasos:

Lectura de la imagen (RGB o escala de grises).
Normalización del rango dinámico a [-1024, 1024], siguiendo el estándar de TorchXRayVision.
Conversión a un solo canal en caso de imágenes RGB, mediante promediado.

Aplicación de transformaciones estándar:
XRayCenterCrop: centrado anatómico,
XRayResizer: ajuste a 512×512 píxeles.

Se seleccionaron los canales correspondientes a Left Lung y Right Lung, y se aplicó una función sigmoide para obtener probabilidades por píxel. Finalmente, la máscara pulmonar binaria se generó mediante umbralización (thresh = 0.5). El proceso se automatizó para todo el dataset, creando una estructura de carpetas espejo para almacenar las máscaras generadas según conjunto (train, val, test) y clase (NORMAL, PNEUMONIA). Cada imagen segmentada se almacenó como un archivo independiente en formato JPEG.

Una vez generadas las máscaras, se procedió a recortar las imágenes originales para conservar únicamente la región pulmonar, lo cual mejora el desempeño de descriptores como textura, forma o frecuencia.

Aplicación de la máscara mediante operación bitwise AND:

lung_only = cv2.bitwise_and(img_resized, img_resized, mask=mask)

Esta operación conserva exclusivamente los píxeles correspondientes a los pulmones y elimina el resto de la estructura torácica (costillas, diafragma, tejidos blandos no relevantes).Cada imagen recortada se almacenó en una nueva estructura de carpetas espejo, destinada a contener el dataset final segmentado.

## 3.2 Extracción de Descriptores Clásicos
### 3.2.1 Descriptores de Forma

#### 3.2.1.1 Descriptores de contorno



#### 3.2.1.2 Fourier Shape Descriptors 


#### 3.2.1.3  Momentos de Hu
La extracción de Momentos de Hu se realizó a partir de las máscaras pulmonares segmentadas previamente. Cada máscara fue leída en escala de grises y convertida a formato binario mediante un umbral fijo, garantizando una delimitación consistente de la región pulmonar. A partir de esta máscara binaria, se calcularon los momentos espaciales y centrales utilizando cv2.moments(), y posteriormente se obtuvieron los siete Momentos de Hu mediante cv2.HuMoments(). Para mejorar su estabilidad numérica y reducir su rango dinámico, se aplicó la transformación logarítmica estándar a cada descriptor. Finalmente, los vectores de características generados para cada imagen se organizaron por partición del dataset (train, val, test) y se almacenaron en archivos .npz, junto con sus etiquetas y nombres de archivo, permitiendo su uso directo en etapas posteriores de clasificación o análisis morfológico.

### 3.2.2 Descriptores de Textura

#### 3.2.2.1 Local Binary Patterns (LBP)
Para la caracterización de la textura presente en las radiografías de tórax, uno de los descriptores a probar fueron los Local Binary Patterns (LBP), usados ampliamente en análisis de imágenes médicas por su eficiencia en terminos de iluminación y capacidad para representar texturas relevantes. 

#### 3.2.2.2 Gray Level Co-ocurrence Matrix (GLCM)
El procedimiento implementado realiza la extracción de características de textura a partir de imágenes médicas utilizando la Matriz de Co-ocurrencia de Niveles de Gris (GLCM), para luego generar un dataset listo para clasificación. Primero, cada imagen se convierte a escala de grises y se redimensiona a 256×256 píxeles. Los niveles de gris se reducen a un número fijo L = 16. Esto se hace dividiendo cada valor de píxel por 16 y tomando la parte entera:

$$
I_{\text{norm}} = \left\lfloor \frac{I}{256 / L} \right\rfloor
$$

Esto normaliza la intensidad de la imagen y reduce el rango de valores para facilitar el cálculo de la GLCM.
Para cada distancia d ∈ {1, 2, 4} y cada ángulo θ ∈ {0, π/2, π/4, 3π/4}, se calcula la matriz de co-ocurrencia de niveles de gris, normalizada y simétrica. Cada elemento de la matriz representa la probabilidad de que un píxel con un nivel de gris específico aparezca junto a otro píxel a esa distancia y ángulo dados:

$$
\text{GLCM}_{d,\theta}[i,j] =
P\left( I_{\text{norm}}(x, y) = i, I_{\text{norm}}(x', y') = j \right)
$$

Donde (x', y') es el píxel vecino a distancia d en la dirección θ.

A partir de cada GLCM se calculan cuatro propiedades de textura:

Contraste: 

$$
\text{Contraste} = 
\sum_{i}\sum_{j} (i - j)^{2}\, \text{GLCM}[i,j]
$$


Correlación: 

$$
\text{Correlacion} =
\frac{
\sum_{i}\sum_{j} (i - \mu_i)(j - \mu_j)\,\text{GLCM}[i,j]
}{
\sigma_i \,\sigma_j
}
$$

Energía:

 

Homogeneidad:

 Homogeneidad = Σ{i,j} GLCM[i,j] / (1 + |i - j|)

Estas características se calculan para cada combinación de distancia y ángulo, generando un vector de características que representa la textura de cada imagen.

#### 3.2.2.3 Filtros de Gabor
La extracción de descriptores de textura mediante filtros de Gabor se realizó utilizando un banco de filtros multi–escala y multi–orientación aplicado sobre las imágenes pulmonares previamente recortadas. En primer lugar, se definió un conjunto de kernels Gabor utilizando tres longitudes de onda (λ = 4, 8 y 16 píxeles) y cuatro orientaciones uniformemente distribuidas entre 0 y π radianes. Cada kernel se generó mediante la función cv2.getGaborKernel(), empleando valores típicos para los parámetros de la envolvente gaussiana, como sigma = 0.56·λ, gamma = 0.5 y fase psi = 0. Esto permitió crear un banco total de 12 filtros sensibles a distintas frecuencias espaciales y direcciones predominantes de textura.

Para cada imagen en escala de grises, se aplicó cada filtro mediante convolución bidimensional (cv2.filter2D()), obteniendo una respuesta filtrada por combinación de escala y orientación. Con el fin de construir un descriptor compacto y robusto, se extrajeron dos estadísticas por cada respuesta: el valor medio y la desviación estándar. Estos valores se concatenaron para formar un vector final de 48 características por imagen. Posteriormente, dichos vectores se almacenaron en archivos .npz separados para cada partición del dataset (train, val y test), junto con sus respectivas etiquetas y rutas de archivo

## 3.3 Clasificación con Descriptores Clásicos
### 3.3.1 SVC: Support Vector Machine


### 3.3.2 Random Forest
El modelo fue construido a partir de las matrices de características generadas previamente (Hu, Gabor, Fourier, GLCM, LBP y contornos). En primer lugar, se cargaron los conjuntos train, val y test, combinando los descriptores seleccionados según cada experimento. Para mejorar la comparabilidad entre características heterogéneas, todos los vectores fueron normalizados mediante MinMaxScaler, garantizando que cada variable se encontrara entre 0 y 1.

Posteriormente, se aplicó una reducción de dimensionalidad mediante SelectKBest con el estadístico ANOVA (f-classif), permitiendo conservar únicamente las características más relevantes para la discriminación entre clases. Con el conjunto reducido, se unieron los subconjuntos de entrenamiento y validación para conformar un bloque de entrenamiento robusto. Sobre este conjunto se entrenó un Balanced Random Forest, que incorpora balanceo interno para mitigar el desbalance entre clases, utilizando 400 árboles, profundidad máxima de 18 y criterios de división ajustados para evitar sobreajuste.

El desempeño del modelo se evaluó mediante validación cruzada estratificada de 10 particiones, registrando métricas estándar como accuracy, precisión, sensibilidad, F1-score y AUC. Luego, el modelo final se entrenó con todos los datos de entrenamiento y validación combinados, y se evaluó sobre el conjunto test independiente. Finalmente, se generaron la matriz de confusión, curvas ROC y el análisis de importancia de características, permitiendo interpretar el aporte relativo de cada descriptor en la clasificación.

# 4. Resultados y Análisis
## 4.1 Análisis Exploratorio y Preprocesamiento
## 4.2 Extracción de Descriptores Clásicos
### 4.2.1 Descriptores de Forma

#### 4.2.1.1 Descriptores de contorno


#### 4.2.1.2 Fourier Shape Descriptors 

#### 4.2.1.3 Momentos Hu



### 4.2.2 Descriptores de Textura

#### 4.2.2.1 Local Binary Patterns (LBP)
 
#### 4.2.2.2 Gray Level Co-ocurrence Matrix (GLCM)

#### 4.2.2.3 Filtros de Gabor



## 4.3 Clasificación con Descriptores Clásicos
 
 
 
 
 


# 5. Referencias Bibliográficas
Hu, M.-K. (1962). “Visual pattern recognition by moment invariants.” IRE Transactions on Information Theory, 8(2), 179–187. https://doi.org/10.1109/TIT.1962.1057692

Gonzalez, R. C., & Woods, R. E. (2018). Digital Image Processing (4th ed.). Pearson.

Breiman, L. (2001). Random Forests. Machine Learning, 45(1), 5–32. https://doi.org/10.1023/A:1010933404324



# 6. Reporte de Contribución Individual








