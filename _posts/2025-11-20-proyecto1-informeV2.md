---
layout: post
title: "Proyecto1 Informe V2"
date: 2025-11-20
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

# **Informe Proyecto 1 V2**

# **1\. Introducción**

La visión por computador es una herramienta fundamental que permite procesar y obtener información a partir de imágenes digitales. Para garantizar la precisión geométrica de las mediciones, la calibración de cámara permite estimar parámetros intrínsecos, corregir distorsiones y establecer la relación entre coordenadas reales y de imagen (Zhang, 2000; Duisterhof et al., 2022).

Por otro lado, las transformaciones geométricas permiten alinear y manipular la disposición de los objetos. Adicionalmente, las transformaciones de intensidad como brillo, contraste y corrección gamma mejoran la visibilidad y compensan diferentes condiciones lumínicas (Gonzalez & Woods, 2018). Mientras que obtener la frecuencia de distribución de la intensidad (histograma) y su ecualización permite mejorar el contraste (Patel et al., 2013).

Finalmente, la segmentación por color permite aislar y cuantificar regiones de interés. Estas técnicas representan el conjunto de herramientas aplicadas en este proyecto para mejorar, analizar y procesar imágenes.

# **2\. Marco Teórico**

## 2.1 Calibración de la Cámara

Uno de los primeros procesos en el manejo de imágenes digitales, es la calibración de la lente, Actualmente, existen cientos de modelos de cámaras, con parámetros y características distintos. Conocer de antemano sus parámetros y de qué manera este dispositivo transfiere la información del mundo real **(3D)** como una imagen digital **(2D)**.

Dado que en la mayoría de los casos las características de una cámara digital nos son desconocidas o inaccesibles, recurrimos a métodos prácticos y simulados para tener una estimación. Con la utilización de un patrón específico, en este caso un **tablero de ajedrez** con esquinas rectas y de un largo conocido y definido previamente, buscamos modelar las distorsiones reales que percibe la cámara al tomar una foto. (Sattia Mallyk, Kaustubh Sadekar, 2020\)

Realizando múltiples imágenes sucesivas desde diferentes ángulos y distancias del mismo patrón, es posible modelar artificialmente estas distorsiones que introduce la lente y determinar con elevada precisión como la cámara proyecta puntos en el espacio de la imagen.

Este proceso permite corregir errores de distorsión y establecer un modelo matemático que describe el comportamiento de la cámara, lo cual es crucial para aplicaciones en visión por computadora, reconstrucción 3D, fotogrametría, robótica, y otras áreas donde se requiere precisión geométrica.

Los datos que sacaremos de esta calibración los conoceremos como parámetros intrínsecos y serán: 

* **fₓ, fᵧ**: distancia focal (en píxeles) en los ejes x e y.  
* **cₓ, cᵧ**: coordenadas del punto principal (centro óptico en la imagen).  
* **s**: coeficiente de sesgo (skew), usualmente 0 si los ejes están ortogonales.


$$
K =
\begin{bmatrix}
f_x & s   & c_x \\
0   & f_y & c_y \\
0   & 0   & 1
\end{bmatrix}
$$


## 2.2 Transformaciones de intensidad 

Operaciones puntuales que modifican directamente los valores de intensidad de los píxeles de una imagen, sin alterar su posición ni estructura espacial (Gonzalez & Woods, 2018). Su objetivo es mejorar la apariencia visual, facilitar el análisis o resaltar detalles específicos. Pueden ser:

* **Lineales:** como el ajuste de brillo y contraste. El brillo se modifica sumando una constante β a cada píxel, desplazando la distribución de intensidades: 

$$
I_o(x,y) = I(x,y) + \beta
$$

donde β es una constante de desplazamiento (positiva para aclarar, negativa para oscurecer).

El contraste se modifica mediante un factor multiplicativo α, que expande o comprime la distribución alrededor de un valor medio.

$$
I_o(x,y) = \alpha \cdot (\, I(x,y) - m \,) + m
$$

Donde, α es el factor de contraste y m es un punto medio que permanece constante. 

* **No lineales:** como la corrección gamma, que ajusta la luminancia percibida aplicando una potencia γ sobre cada valor de intensidad normalizado.

$$
I_o(x,y) = c \cdot I(x,y)
$$

Donde c es una constante de escala y γ el exponente de corrección. Si γ\<1, los tonos oscuros se elevan; si γ\>1, los tonos claros se comprimen.

## 2.3 Transformaciones geométricas

Las transformaciones geométricas son operaciones fundamentales en el procesamiento de imágenes,entre las más comunes están la traslación y la rotación, donde, estas transformaciones se representan mediante matrices en coordenadas homogéneas que modifican la relación espacial entre píxeles (Herrera, Guijarro, Guerrero, José. (2016). 

Matemáticamente, estas transformaciones se representan mediante matrices en coordenadas homogéneas: traslación por vector (tx, ty); rotación por ángulo θ, escala por factores (sx, sy). La composición de transformaciones se obtiene mediante multiplicación de matrices y el orden importa: rotar y luego trasladar no es equivalente a trasladar y luego rotar.


## 2.4 Distribución de intensidades

El histograma de intensidades es una representación estadística que muestra la frecuencia con la que aparecen distintos niveles de intensidad en una imagen (Gonzalez & Woods, 2018). Así,

$$
h(r_k) = n_k
$$

donde rk​ es el nivel de intensidad y nk​ el número de píxeles con esa intensidad. Al normalizar se obtiene una función de probabilidad discreta:

$$
p(r_k) = \frac{n_k}{n}
$$

La distribución del histograma permite identificar características como brillo, contraste y rango dinámico. Por ejemplo, un histograma agrupado en intensidades bajas indica una imagen oscura; en altas intensidades indica una imagen clara y uno distribuido uniformemente indica buen contraste (Patel et al., 2013).

La función de distribución acumulada (CDF) se obtiene a partir del histograma y representa la probabilidad acumulada de que un píxel tenga una intensidad menor o igual a un cierto valor. Se define como:

$$
CDF(r_k) = \sum_j p(r_j)
$$


La CDF es fundamental en técnicas de ecualización de histograma, permitiendo construir una transformación de intensidades que redistribuye los niveles originales de manera más uniforme a lo largo del rango dinámico de la imagen (Gonzalez & Woods, 2018). Para esto se busca construir una función de transformación T(r) que mapee los niveles de intensidad originales r en nuevos niveles s, de manera que la distribución de salida se acerque a una distribución uniforme:

$$
s = T(r) = (L - 1)\cdot CDF(r)
$$


Donde L es el número total de niveles de intensidad.

## 2.5 Segmentación de Imagenes por colores

La **segmentación** es una técnica del procesamiento digital de imágenes, en la que buscamos dividir una imagen en regiones o segmentos que comparten características comunes, como color, intensidad o textura (Gonzalez & Woods, 2018). 

Dentro de este contexto la segmentación basada en el color permite identificar y extraer objetos de una imagen digital utilizando la información disponible en los píxeles. Para ello, es común trabajar en espacios de color distintos al RGB.

Aunque el espacio **RGB** es ampliamente utilizado en la visualización de imágenes, para este ejercicio se requerirá de más precisión y lidiar con cambios en la intensidad de la luz. Para esto se utiliza el formato **HSV** (Hue, Saturation, Value), ya que este último separa la tonalidad del color de la intensidad, facilitando así una detección más robusta frente a variaciones de luz (Color Identification in Images using Python \- OpenCV, 2023).

# **3\. Metodología**

## 3.1 Calibración de la Cámara

Nos preparamos para la realización del ejercicio desactivando en nuestra cámara las funciones que puedan obstruir las mediciones (HDR, AutoFocus, Zoom Digital), completado esto, pondremos el **patrón de ajedrez** sobre una superficie plana y realizaremos diferentes fotografías desde distintos ángulos, teniendo el patrón dentro del encuadre, previamente deberemos haber realizado mediciones de uno de los lados de un cuadrado.

Un paso de validación es necesario antes de buscar los parámetros intrínsecos, utilizando  la función *cv2.findChessboardCorners()* de la biblioteca OpenCV, buscamos detectar las esquinas del patrón, si estas esquinas no son detectadas no podremos realizar el siguiente paso por lo que deberemos repetir las imágenes.

Utilizando la medida del cuadrado en **mm** como referente, utilizaremos *cv2.calibrateCamera()* para encontrar los parámetros intrínsecos, esta función nos devolverá los datos que estamos buscando y nos permitirá guardarlo en un archivo aparte para su utilización posterior.

## 3.2 Transformaciones de intensidad 

![image1](/DigiVision/proyecto1/support_files/tranformacionIntensidadesMetodologia.jpg)  
Estas operaciones se realizaron a nivel de píxel y fueron programadas utilizando las librerías de NumPy, Matplotlib y Pillow.

## 3.3 Transformaciones geométricas

1. Prepareción de la immagen: se cargó la imagen y posteriromente se conviritió en RGBA.
2. Definición de la transformaciones: se construyó una lista con 5–8 configuraciones (tx, ty, θ, s). Cada configuración produce un frame.  
3. Ejecución de transformaciones:  
   \- Escala (factor s) aplicada con interpolación bicúbica.  
   \- Rotación sobre el centro (expand=True para evitar recortes).  
   \- Pegado de la imagen rotada en un lienzo del tamaño original y aplicación de la traslación (tx, ty).  
4. Guardar: cada frame se guarda como PNG numerado.  
5. Generación del GIF usando imageio, ajustando \`duration\` para la velocidad.  

## 3.4 Distribución de intensidades

Utilizando las imágenes obtenidas en el punto 3.2 se busca comparar cómo varía el contraste en imágenes bien iluminadas frente a aquellas con baja luminosidad y hacer su respectiva corrección. Para esto, las imágenes RGB fueron convertidas a escala de grises. Luego, utilizando la función np.hist() se calculó el histograma para cada una de las imágenes, lo que permitió comparar su distribución de intensidad inicial.

Posteriormente, se calculó la función de distribución acumulada (CDF) utilizando hist.cumsum() y se normaliza para garantizar un mapeo correcto entre 0 y 255\. Finalmente, cada valor de intensidad original se reemplazó por su nuevo valor transformado mediante indexación directa, obteniendo la imagen ecualizada. 

Las imágenes obtenidas fueron guardadas en la carpeta  *results/.*

## 3.5 Segmentación por Colores

Con el objetivo de identificar colores específicos en una imagen digital, decidimos emplear una combinación de **máscaras de color** y **Canny** para la detección de bordes. La idea principal se basaba en separar los tres colores primarios del espacio RGB (rojo, verde y azul) y, a partir de esta segmentación, resaltar con líneas los objetos que presentaran una mayor intensidad en dichos colores.

Inicialmente, realizamos una prueba con **rangos de valores bajos y altos en RGB** para crear máscaras que filtraran las regiones específicas. No obstante, los resultados fueron que los bordes obtenidos no eran nítidos ni representativos, y se mantenía la presencia de ruido o áreas que no correspondían a los colores buscados.

Tras una revisión de documentación y recursos disponibles, encontramos diversas recomendaciones que sugerían utilizar el espacio de color **HSV en lugar de RGB**, debido a su **mayor precision** y su capacidad para separar la información de tono, saturación e intensidad (Color Identification in Images using Python \- OpenCV, 2023).

Con esta información, definimos **rangos específicos en HSV** para rojo, verde y azul, generamos máscaras para cada uno utilizando la función *cv2.inRange()*. A continuación, aplicamos estas máscaras a la imagen original para segmentar las regiones correspondientes a cada color.

Una vez segmentadas las regiones de interés, desarrollamos un método personalizado para **dibujar, señalar y analizar los objetos detectados**. Este método incluía los siguientes pasos:

* **Aplicación de desenfoque gaussiano** a las máscaras para eliminar el ruido.  
* **Detección de bordes** mediante el algoritmo de Canny.  
* **Detección de contornos** usando *cv2.findcontourns()*.  
* **Cálculo de áreas en píxeles** para cada contorno detectado.  
* **Aplicación de un filtro de tamaño (size filter)** con el fin de eliminar contornos demasiado grandes o irrelevantes que no correspondiera a objetos válidos dentro del análisis.

# 4\. Resultados y Análisis

## 4.1 Calibración de la Cámara

El ejercicio de calibración fue llevado a cabo con 5 imágenes con la cámara se un celular modelo Samsung Galaxy S7:

<p align="center">
  <img src="/DigiVision/proyecto1/img/1.calibracionCamara/1.jpg" width="250" />
  <img src="/DigiVision/proyecto1/img/1.calibracionCamara/2.jpg" width="250" />
  <img src="/DigiVision/proyecto1/img/1.calibracionCamara/3.jpg" width="250" />
  <img src="/DigiVision/proyecto1/img/1.calibracionCamara/4.jpg" width="250" />
  <img src="/DigiVision/proyecto1/img/1.calibracionCamara/5.jpg" width="250" />
</p>
Imagen 2

Realizamos la busqueda de los bordes con la funcion chesscorner antes de comenzar el proceso de calibracion.

<p align="center">
  <img src="/DigiVision/proyecto1/results/1.calibracionCamara/1_detected.jpg" width="250" />
  <img src="/DigiVision/proyecto1/results/1.calibracionCamara/2_detected.jpg" width="250" />
  <img src="/DigiVision/proyecto1/results/1.calibracionCamara/3_detected.jpg" width="250" />
  <img src="/DigiVision/proyecto1/results/1.calibracionCamara/4_detected.jpg" width="250" />
  <img src="/DigiVision/proyecto1/results/1.calibracionCamara/5_detected.jpg" width="250" />
</p>
Imagen 3

Porteriormente, se determino la matriz K:

$$
K =
\begin{bmatrix}
2.77781002\times 10^{3} & 0 & 2.10654944\times 10^{3} \\
0 & 2.83647470\times 10^{3} & 1.57907916\times 10^{3} \\
0 & 0 & 1
\end{bmatrix}
$$


Los resultados mostraron una deformación en forma de cojín al ver como las imágenes se hunde de cierta forma en el centro de cada lado de la imágen dando la impresión de un cojín.

A parte de esta deformación que es el aspecto más visual, vamos a encontrar como ciertos valores presentan ligeras variaciones, uno de estos casos es el del *focal length* en el cual tenemos los siguientes resultados: 

$$
(f_x = 3.16386392\times10^{3},\; f_y = 3.18629093\times10^{3})
$$

Estos valores pueden ser en la mayoría de los casos iguales, estos dos puntos representan las direcciones en **x** y en **y** que podrán diferir dependiendo del tamaño de los píxeles de la cámara, otro valor a tener en cuenta es el de cx y cy los cuales nos dieron como resultado.

$$
(c_x, c_y) = (2.00104091\times 10^{3},\; 1.55433682\times 10^{3})
$$


Tenemos que tanto cx y cy que serían las coordenadas del centro óptico que hemos sacado de acuerdo a la calibración, estos dos puntos están relativamente cerca del centro original aunque notamos una mayor variación en los valores de y, lo cual podría deberse al hecho de que son los lados superior e inferior los que presenta una mayor deformación en las imágenes corregidas.

$$
\begin{aligned}
c_x &= 2.00104091\times 10^{3} = 2001.04091 \\
c_y &= 1.55433682\times 10^{3} = 1554.33682 \\
\\
(c_x, c_y) &= (2001.04091,\; 1554.33682) \\
(i_x, i_y) &= (2016,\; 1512)
\end{aligned}
$$


Tenemos entonces que ix e iy representan el centro según las dimensiones originales de la imagen en este caso estaríamos hablando de que cx está a 14 pixeles a la izquierda del centro de la imagen y que y el cual está más separado, esté 42 pixeles por encima del centro de la imagen.

$$
d = \sqrt{(c_x - i_x)^2 + (c_y - i_y)^2} = 44.87
$$

pixeles de diferencia

Lo más a destacar de los resultados es la variación en y, tanto en la **distancia focal** como en la distancia del centro de la cámara al centro de la imagen que presenta una variación mayor, esto nos hace intuir que los píxeles de la cámara presenta un tamaño mayor en su altura dando lugar a estas variaciones.

<p align="center">
  <img src="/DigiVision/proyecto1/img/1.calibracionCamara/1.jpg" width="40%">
  <img src="/DigiVision/proyecto1/results/1.calibracionCamara/20251010_160324_undistorted.jpg" width="40%">
</p>
Imagen 4: Comparación imagen original vs imagen corregida


Finalmente, se determino el error RMS:

$$
\text{Error RMS de reproyección} = 0.9848851012377567
$$


## 4.2 Transformaciones de intensidad

**Brillo**  

![image5](/DigiVision/proyecto1/results/2.transformacionesIntensidad/comparacionBrillo.png) 

Se puede observar en la imagen de la fachada AM al utilizar un beta negativo la imagen se oscurece de manera global, desplazando la distribución de intensidades hacia valores más bajos. Mientras que en la imagen de la fachada PM con un beta positivo se desplazan las intensidades hacia valores más altos, la imagen se aclara, es decir, se produce un aumento del brillo general. 

**Contraste** 

![image6](/DigiVision/proyecto1/results/2.transformacionesIntensidad/compracionContraste.png)  

En la fachada AM al aplicar un α=2 y un punto medio de 0.5, se produce una mayor diferencia entre las intensidades, las regiones claras (como el cielo y las fachadas) se vuelven más brillantes, mientras que las zonas oscuras (árboles) se oscurecen aún más. Esto incrementa el contraste global, pero también puede generar pérdida de detalle en zonas oscuras debido a la saturación.

En la fachada PM al aplicar un α=-1 y un punto medio de 0.5, la relación se invierte respecto al punto medio generando un efecto negativo, las zonas oscuras se vuelven claras y viceversa.

**Corrección Gama**  

![image7](/DigiVision/proyecto1/results/2.transformacionesIntensidad/comparacionGamma.png) 

Para la imagen de la fachada AM, al aplicar un factor gamma γ=2 con una constante c=1 se genera un oscurecimiento. Esto se debe a que los valores de intensidad inferiores a 1 se elevan a una potencia mayor lo que desplaza parte del rango dinámico hacia valores más bajos. Esto produce una pérdida de detalle en las zonas oscuras y del primer plano.

Para la imagen de la fachada PM, al aplicar un factor gamma γ=2 con una constante c=1 se produce un efecto de aclarado no lineal, ya que los valores bajos se amplifican al elevarlos a un exponente menor que 1\. Esto mejora la visibilidad de detalles en regiones oscuras, como en los árboles y las fachadas inferiores, sin sobreexponer las zonas ya iluminadas como las ventanas.

**Operaciones aritméticas**

![image8](/DigiVision/proyecto1/results/2.transformacionesIntensidad/operacionesAritmeticas.png) 

La suma de ambas imágenes produce un incremento en la intensidad al combinar los valores de brillo de las dos imágenes. Así, la imagen se ve más clara y brillante, con detalles visibles en los árboles y  la fachada. También, se observa saturación en las regiones más iluminadas como el cielo y reflejos.

La resta realza las diferencias entre ambas imágenes evidenciando zonas en las que la iluminación cambia significativamente entre el día y la noche. Se observan cambios en el cielo y zonas oscuras en la fachada donde había más iluminación en la noche.

La multiplicación pixel a pixel genera una imagen más oscura ya que intensidades menores a 1 reducen aún más el resultado. Se observa una pérdida de detalle en toda la imagen especialmente en las zonas más oscuras y alcanza a evidenciar levente un realce en la zona donde hay iluminación alta en ambas imágenes.

La división amplifica intensidades cuando los valores en la imagen PM son pequeños y disminuye las intensidades cuando los valores de la imagen PM son grandes. En este caso, la imagen de la fachada PM tiene valores bajos de intensidad lo que amplifica las intensidades dando como resultado una imagen fuertemente aclarada y contrastada, con zonas de saturación extrema en árboles y fachada. 

## 4.3 Transformaciones geométricas

Este ejercicio permitió observar cómo las transformaciones geométricas influyen en la apariencia y trayectoria de una imagen aplicadas de forma sucesiva. El orden de las operaciones es muy importante, ya que, modificar la secuencia entre escala, rotación y traslación cambia bastante la posición final de los píxeles.

La función **transform** es la que permite ajustar el tamaño mediante escalado, girarla según el ángulo de rotación y desplazarla en el plano mediante traslación. Para evitar que la imagen recortada pierda información, se crea un lienzo del tamaño original donde se pega la versión transformada de la imagen, garantizando que  la imagen permanezca dentro de los límites visibles, siendo esto una herramienta esencial en visión por computadora que permite modelar cómo un sistema percibe los objetos independientemente de su posición, tamaño u orientación.

![image9](/DigiVision/proyecto1/results/3.transformacionesRotaciónTraslación/frames/frame_01.png)
![image10](/DigiVision/proyecto1/results/3.transformacionesRotaciónTraslación/frames/frame_04.png)
![image11](/DigiVision/proyecto1/results/3.transformacionesRotaciónTraslación/frames/frame_06.png)

La función **generate_transform_sequence** se encarga de aplicar varias transformaciones a una imagen y crear un GIF animado con todo el proceso. Con la imagen principal (base), le va aplica distintos cambios como moverla, girarla y escalarla, guardando cada paso como una imagen separada (frames). Luego, con la librería imageio, se juntan todos los frames como una animación, mostrando de forma visual cómo la imagen se va transformando poco a poco.


![image12](/DigiVision/proyecto1/results/3.transformacionesRotaciónTraslación/frames/transformations.gif)


## 4.4 Distribución de intensidades

![image13](/DigiVision/proyecto1/results/4.distribuciónIntensidad/histogramas.png) 

Para la fachada AM el histograma tiene una distribución a lo largo de todo el rango dinámico con un aumento en valores bajos. Aunque la imagen tiene zonas oscuras como los árboles cuenta con una iluminación más equilibrada que incluye intensidades medias y altas. Por el contrario, en la imagen de la fachada PM el histograma se concentra principalmente en intensidades bajas, lo que refleja una escena predominantemente oscura.

![image14](/DigiVision/proyecto1/results/4.distribuciónIntensidad/CDF%20Normalizada.png)

la CDF en el caso  AM el aumento fue más constante con una pequeña meseta \~ 100 y 200 y un aumento casi lineal al final, mientras que en el caso PM se evidenció un cambio más pronunciado al inicio y una meseta \~ desde 100, lo que evidencia un aumento de intensidades en las zonas más oscuras de la imagen.

![image15](/DigiVision/proyecto1/results/4.distribuciónIntensidad/ImágenesEcualizadas.png)

En la imagen AM, la distribución de intensidades ya era amplia y la transformación de ecualización fue más suave, con cambios leves en los niveles de intensidad y un incremento moderado del contraste.

Por otro lado, en la imagen PM, el histograma estaba concentrado en intensidades bajas, la CDF extendió significativamente estos valores hacia todo el rango dinámico, generando un aumento fuerte del contraste.

## 4.5 Segmentación por Colores

El resultado obtenido demuestra que el sistema de segmentación implementado es capaz de **delimitar los contornos** de los objetos presentes en la imagen, independientemente de su ubicación relativa (cercana o lejana) dentro del campo visual.

Además, el algoritmo desarrollado no solo segmenta los colores objetivo (rojo, verde y azul), sino que también logra **resaltar cada región con líneas de contorno claramente visibles**, aplicando un grosor que mejora la percepción visual de los límites detectados.

Este comportamiento es posible gracias a la combinación de varias técnicas implementadas:

* La utilización del **espacio de color HSV**, que permite una detección más robusta de los colores frente a variaciones de iluminación.  
* La aplicación de **desenfoque gaussiano** que elimina el ruido en las máscaras.  
* El uso del algoritmo de **detección de bordes de Canny** y *cv2.findContours()* para obtener contornos nítidos y precisos.  
* La visualización final con *cv2.drawContours()* o funciones similares, que permiten resaltar cada objeto con trazos diferenciados y adaptativos.

En conjunto, estos resultados validan que el sistema desarrollado no solo es funcional, sino que es capaz de realizar una **detección y visualización efectiva** de los colores de interés, así como el cálculo de su área y conteo de los bordes detectados.

![image16](/DigiVision/proyecto1/results/5.segmentacion/resultado_segmentacion.jpg)

# 5\. Referencias Bibliográficas

Zhang, Z. (2000). *A flexible new technique for camera calibration*. IEEE TPAMI.

Gonzalez, R. C., & Woods, R. E. (2018). *Digital Image Processing*. Pearson.

Herrera, P. J., Guijarro, M., & Guerrero, J. M. (2016). Operaciones de transformación de imágenes. En Conceptos y métodos en visión por computador (cap. 4). Madrid, España: Universidad Francisco de Vitoria.

Patel, O. et al. (2013). *SIPIJ*, 4(5).

Vinayak Ray (03 de Enero de 2023). Color Identification in Images using Python \- OpenCV. GeeksforGeeks. [https://www.geeksforgeeks.org/python/color-identification-in-images-using-python-opencv/](https://www.geeksforgeeks.org/python/color-identification-in-images-using-python-opencv/) 

Gururaj (26 de septiembre de 2024). Choosing the correct upper and lower HSV boundaries for color detection with cv::inRange (OpenCV).  
[https://www.geeksforgeeks.org/computer-vision/choosing-the-correct-upper-and-lower-hsv-boundaries-for-color-detection-with-cv-inrange-opencv/](https://www.geeksforgeeks.org/computer-vision/choosing-the-correct-upper-and-lower-hsv-boundaries-for-color-detection-with-cv-inrange-opencv/)

priyarajtt (15 de Julio de 2025). Camera Calibration using Python \- OpenCV. GeeksforGeeks. [https://www.geeksforgeeks.org/python/camera-calibration-with-python-opencv/](https://www.geeksforgeeks.org/python/camera-calibration-with-python-opencv/)

Kaustubh Sadekar, Satya Mallick (25 de Febrero del 2020\) Camera Calibration using OpenCV. [https://learnopencv.com/camera-calibration-using-opencv/](https://learnopencv.com/camera-calibration-using-opencv/) 

# 6\. Reporte de Contribución Individual

| Estudiante | Aporte Personal |
| :---- | :---- |
| Leidy Marcela Leal Loaiza | Ejercicio de Transformaciones geométricas|
| Juan Felipe Arbelaez | Ejercicio de Calibración de Cámara y Segmentación por colores. |
| Bibiana Andrea Peña V | transformación de intensidades y la ecualización del histograma para mejorar el contraste de las imágenes, incluyendo la implementación y validación del procedimiento. Además, participé en la redacción del informe final y en la organización de los archivos comunes del proyecto, contribuyendo a mantener una estructura clara y coherente para el trabajo en equipo. |


<p style="text-align:center;">
  <a href="{{ "/" | relative_url }}">⬅ Volver al inicio</a>
  <a href="https://github.com/bibianapv/DigiVision" target="_blank">🌐 Ver en GitHub</a>
</p>

</div>
</div>
</div>
