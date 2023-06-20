# Marketing-Mix-Modeling-con-Robyn
 #### Autor: Antonia Chica Díaz
 
La pretensión de este proyecto es presentar y utilizar una herramienta analítica llamada Marketing Mix Modeling (MMM), un modelo econométrico diseñado para medir el impacto real de todos los factores que influyen en las ventas, preparar los datos históricos de series temporales de la actividad de marketing y crear un modelo estadístico capaz de ajustar la inversión para mejorar los resultados. Además, trataremos de analizar y demostrar los pasos del MMM  para brindar soluciones a preguntas como: ¿Cuántas ventas generó cada canal de medios?, ¿Cuál fue el ROI de cada canal de marketing?, si tuviera que recortar mi presupuesto de marketing ¿en qué canal debería recortar? 

# 1. Introducción
La industria de la tecnología ha crecido exponencialmente a lo largo de los años, revolucionando así, entre otros, el mundo de la publicidad, convirtiéndolo en un entorno híbrido, complejo y diverso. Así pues, cada avance tecnológico trae consigo un aumento exponencial de la dificultad del seguimiento de las campañas publicitarias, a lo que le debemos agregar las sucesivas protecciones de privacidad europeas[1].

Los especialistas en marketing, se esfuerzan por demostrar cómo las campañas de marketing impulsan los resultados comerciales, pero a pesar de los numerosos avances tecnológicos, la medición del marketing se vuelve cada vez más compleja, puesto que además de los medios publicitarios tradicionales como el periódico, la radio, la televisión, etc. también se han sumado los medios online como las redes sociales, los buscadores, las páginas web, etc.  La ventaja de estos medios digitales es que son fáciles de rastrear y analizar, pero cuando hablamos de medios tradicionales u offline, el seguimiento de la actividad publicitaria es mucho más difícil debido a su modelo de comunicación masiva y unidireccional, imposibilitando así el seguimiento de los usuarios impactados.
Este trasfondo ha llevado al uso de técnicas como el Marketing Mix Modeling (MMM) basadas en modelos estadísticos avanzados.

# 2. Marketing Mix Modeling
Para llevar a cabo el modelado de Marketing Mix Modeling (MMM), utilizaremos una herramienta muy innovadora: “Project Robyn”. Esta herramienta fue creada por el equipo de Meta Marketing Science, y es un paquete de código abierto para desarrollar modelos de Marketing Mix.[2]

A continuación, implementaremos un modelo de Marketing Mix Modeling utilizando datos de una marca de fabricación de televisores ubicada con el fin de calcular la efectividad de su la campaña de marketing. Para implementar este modelo, utilizaremos la herramienta “Project Robyn” con el programa estadístico R, para dar solución a las siguientes preguntas comerciales que se plantea la marca:

-¿Qué canales de medios (online y offline) generan más ingresos?

-¿Cuál fue el ROI de cada canal de marketing?

-Si tuviera que recortar mi presupuesto de marketing, ¿en qué canal debería recortar?

-¿Cuánto impulsan los ingresos incrementales de las actividades comerciales?

## 2.1. Configuración del proyecto
Antes de comenzar a construir el modelo de MMM, debemos instalar y configurar Robyn en R así como descargar la biblioteca de Python requerida por Robyn: Nevergrad. Para hacer esto, debemos ejecutar el siguiente comando en la consola de R[3]:
### Instala,carga y comprueba la (última) versión de Robyn, usando una de estas 2 fuentes:
1) **Instalar la última versión** estable desde CRAN:
   
    ```install.packages("Robyn")```
 
3) Instalar la última versión dev desde GitHub:
    ```
    remotes::install_github("facebookexperimental/Robyn/R")
    install.packages("remotes")
    ```
   
Una vez instalado Robyn en R, **cargamos el paquete**:

 ```library(Robyn)```
 
Robyn, requiere de la biblioteca de Python  ```Nevergrad```  por lo que debemos instalarlo para que el código funcione correctamente. Para su instalación seguiremos dos pasos:

1) **Instalamos el paquete de Python ```reticulate```**:
   ```
   install.packages("reticulate")
   library(reticulate)
   ```
2) **Instalamos  ```Nevergrad``` la biblioteca de Python**. Existen 2 opciones de instalación:

   **Opción 1** *usando PIP*
   
     ```
     virtualenv_create("r-reticulate")
     py_install("nevergrad", pip = TRUE)
     use_virtualenv("r-reticulate", required = TRUE)
     ```

   **Opción 2** *usando Conda*
     ```
     conda_create("r-reticulate")
     conda_install("r-reticulate", "nevergrad", pip = TRUE)
     use_condaenv("r-reticulate")
     ````
En caso de que Nevergrad aún no se pueda importar después de la instalación, ubique su archivo python y ejecute esta línea usando su ruta:

```
use_python("~/Library/r-miniconda/envs/r-reticulate/bin/python")
```
Una vez que tengamos las bibliotecas de Robyn y Nevergrad instaladas en nuestro dispositivo, podemos comenzar nuestro estudio de Marketing Mix Modeling. Si, por el contrario, no se desea instalar estas bibliotecas en el dispositivo, se puede usar la versión de R-cloud registrándose en esta web gratuita: https://rstudio.cloud/

## 2.2. Data

Para realizar el modelo de Marketing Mix Modeling hemos usado el conjunto de datos abierto disponible en la página web de GitHub. El conjunto de datos es un dataframe de 2613 observaciones (filas) y 19 variables, todas de ellas numéricas. En él, constan datos desde el 1 de enero de 2010 al 25 de febrero de 2017.

•	**5 canales de inversión en medios**: Advertising expenses SMS, advertising expenses Newspaper ads, advertising expenses radio, advertising expenses TV y advertising expenses Internet.

•	**5 canales de medios** que también tienen la información de exposición (Impresión, Clics): GRP SMS, GRP Newspaper ads, GRP Radio, GRP TV, GRP Internet.

•	**4 variables de contexto**: Consumer Price Index (CPI), Producer Price Index (PPI), Unit Price ($) y Consumer Confidence Index (CCI).

•	**Variables de control**: tendencias, eventos y vacaciones.

## 2.3. Definición de preguntas comerciales

Para comenzar a realizar el proyecto de Marketing Mix Modeling es necesario identificar el objetivo de la empresa así como las preguntas comerciales clave que desea que el modelo de MMM le responda. En este caso, el objetivo de la empresa de fabricación de televisores es predecir la demanda así como determinar el impacto de los medios de campañas publicitarias sobre la demanda. Para ello, la empresa se plantea las siguientes cuestiones:
1. ¿Qué canales de medios (online y offline) generan más ingresos?
2. ¿Cuál es el ROI de cada canal de marketing?
3. ¿En qué canales debo centrar más mi presupuesto en marketing para maximizar los ingresos?
4. ¿Cuál es el nivel óptimo de gasto para cada uno de los principales canales de marketing?
5. ¿Cuánto impulsan los ingresos incrementales de las actividades comerciales?

Una vez identificados los objetivos y las preguntas comerciales de la empresa, pasamos a la siguiente fase, la fase de pre-modelado. En esta fase tratamos de recopilar y revisar los datos a analizar.

## FASE DE PRE MODELADO

## 2.4. Recopilación de los datos
Una vez definidos los objetivos y las preguntas comerciales, comenzamos con la recopilación de datos y con ello, empieza el proceso de pre-modelado. La complicación de este paso es recopilar datos en el mismo formato a través de múltiples fuentes diferentes, lo que lo convierte en el paso que consume más tiempo por lo que tendríamos que asignar a este paso entre 4 y 6 semanas.
Para la recopilación de datos debemos tener en cuenta los siguientes factores:

### 2.4.1.	Datos reales Vs Datos planificados:
Necesitamos recopilar datos reales que muestren eventos que realmente ocurrieron en lugar de datos planificados. En este cao, recopilaremos datos sobre los gastos de anuncios en periódico, radio, televisión  e Internet, así como las impresiones en estos medios. Además incluiremos datos de contexto y de control. Los datos planificados no siempre reflejan lo que realmente ha sucedido por lo que incluir este tipo de datos en el modelo puede generar salidas de datos inexactas.

### 2.4.2.	Variables dependientes frente a independientes
En cuanto al conjunto de datos que necesitamos recopilar, es crucial distinguir qué variables independientes y dependientes deben medirse en el modelado de Marketing Mix Modeling.

•	La **variable dependiente** será la métrica más importante o KPI con la que midamos el MMM. Para seleccionar la variable dependiente de entre todos los conjuntos de datos que estudiamos, nos haremos la siguiente pregunta: ¿Cuál es la métrica o KPI clave para la empresa de fabricación de televisores? Para responder a esta pregunta, nos fijamos en el objetivo de la empresa, y deducimos que la métrica más importante para esta empresa es pronosticar la demanda, por lo que elegimos este conjunto de datos como la variable dependiente.

•	Las **variables independientes** serán las variables en el modelo de Marketing Mix Modeling que afectarán a la variable dependiente. Para seleccionar las variables independientes del proyecto debemos de realizar las siguientes preguntas: ¿Cuáles son los diferentes factores que afectan a la variable dependiente? 

### 2.4.3.	Métricas que se deben recopilar
Como hemos visto en el punto anterior debemos seleccionar una variable dependiente que deberá ser el KPI más importante para la empresa, en este caso es la demanda. Para recopilar las variables independientes debemos fijarnos en la actividad de la empresa. 

El MMM utiliza medios tanto online como offline por lo que debemos recopilar los datos de las actividades de marketing en los distintos medios. En el caso de los medios online como internet y SMS utilizaremos las métricas de las impresiones en estos medios. En el caso de la televisión, la radio y el periódico, utilizaremos los GRP (Puntos de rating brutos) como métricas.

Asimismo, el Marketing Mix Modeling se caracteriza por medir el tracking de campañas utilizando medios pagados y orgánicos, por lo que además deberemos de recopilar los datos de medios orgánicos así como las actividades de marketing que no están relacionadas con los medios, como el precio unitario. Además, la estacionalidad y las vacaciones, tienen un gran impacto en la demanda, por lo que ambos factores deberán ser incluidos en el modelo. Para incluirlos en el modelo utilizaremos la herramienta que veremos en la fase de modelado que nos proporciona Robyn: Prophet. A su vez, recopilaremos datos de factores macroeconómicos como el índice de precios al consumidor (CPI), índice de confianza del consumidor (CCI) y el índice de precios al productor (PPI) para ver la influencia de estos en el negocio.

### 2.4.4.	Datos históricos necesarios
Para conseguir datos sólidos para el modelo de Marketing Mix Modeling necesitaremos datos de mínimo dos años. La empresa facilita datos de 7 años desde 2010 a 2017. Esto nos ayudará a medir mejor el impacto de la	estacionalidad en los resultados comerciales de la empresa.

## 2.5. Revisión de los datos
Una vez que se hayan recopilado todos los datos, se debe realizar una revisión de datos para verificar su precisión y si se alinea con el plan de medios, las expectativas y si puede responder las preguntas comerciales deseadas.

Junto con la recopilación de datos, la revisión de datos es una de las etapas más críticas en un estudio de MMM. Si los datos recopilados no se revisan, terminan siendo inexactos y se utilizan en el MMM, entonces los resultados de los datos también serán inexactos y no confiables.

## FASE DE MODELADO
Para llevar a cabo la segunda fase del modelado de Marketing Mix Modeling debemos considerar los siguientes aspectos.

## 2.6. Modelado

### 2.6.1.Ingeniería de características
Las características de nuestros datos influyen directamente en el modelo de predicción y en los resultados obtenidos. Por lo tanto, cuanto mejores sean las características de los datos, mayor será la precisión del modelo (Brownlee, J., 2020). En este apartado procesamos los datos que no han sido incluidos anteriormente en el modelo pero que son influyentes en el modelo predictivo.

**Prophet descomposición estacional**

Prophet es un código fuente abierto de Meta incluido en el código de Robyn para descomponer los datos de series temporales en estacionalidad, tendencias y días festivos con el fin de mejorar las capacidades del ajuste y de pronóstico del modelo. En nuestro modelo utilizaremos Prophet con la finalidad de medir el impacto de la estacionalidad y las festividades de China en la demanda (variable dependiente). 

Por lo tanto, al modelo que creamos anteriormente (Figura 1) le añadimos las siguientes líneas de base de series temporales de Prophet en China para mejorar así los resultados de la predicción. 

•	*Estacionalidad*

•	*Tendencias*

•	*Días de la semana*

•	*Días festivos*


**Ventana del modelo**

Para realizar un buen modelo de Marketing Mix Modeling debemos tener en cuenta el período de tiempo adecuado a partir de los datos recopilados. En este caso vamos a utilizar datos recopilados desde el 1 de julio de 2010 hasta el 25 de febrero de 2017. En este periodo de 7 años nos permitirá estudiar con mayor precisión la tendencia, la estacionalidad y los efectos de las vacaciones y días de la semana.

**Técnicas de transformación de datos**

El MMM se caracteriza por dos hipótesis, una de ellas que la inversión en anuncios tiene un efecto retardado y otra, que la inversión tiene rendimientos decrecientes. Por ello, para advertir esto Robyn utiliza dos transformaciones, que llevaremos a cabo en nuestro modelo:


**1. Adstock o transformación de acciones publicitarias**

Esta transformación hace referencia a la teoría de que la publicidad se retrasa y decae después de la exposición inicial, y se relaciona con el recuerdo del anuncio por parte del consumidor. En otras palabras, esta memoria de los consumidores decae a medida que pasa el tiempo. En Robyn, existen tres diferentes tipos de transformaciones de acciones publicitarias: geométrica, PDF Weibull y CDF Weibull. En este estudio, hemos seleccionado la transformación geométrica de Adstock, ya que es más rápida de ejecutar que la Weibull. La implementación de este tipo de transformación se define mediante la siguiente función:

![image](https://github.com/Antchica/Marketing-Mix-Modeling-con-Robyn/assets/136733800/bfe6c781-3cb0-42ef-ac10-c5dfb9f0c687) 

Asimismo, la propiedad de decaimiento de la transformación geométrica de Adstock es que el límite de la suma infinita es: 1/(1-theta) y se representa de la siguiente manera:

**Figura 1**:

 *Gráfica transformación geométrica de Adstock*
 
![image](https://github.com/Antchica/Marketing-Mix-Modeling-con-Robyn/assets/136733800/2ca5d917-3ca8-471f-a5fb-d0e4b1488b11)

Para implementarla en R debemos añadir al modelo los siguientes comandos:

```
adstock = "geometric"
```
Para implementar los hiperparametros de las variables de medios pagados y orgánicos: 

```
hyper_names(adstock = InputCollect$adstock, all_media = InputCollect$all_media)
```

**2.La transformación de saturación**

Esta transformación sustenta que cada unidad adicional de publicidad aumenta la respuesta, pero a un ritmo decreciente (Figura 2). 

**Figura 2:**

*Gráfica transformación de Saturación*

![image](https://github.com/Antchica/Marketing-Mix-Modeling-con-Robyn/assets/136733800/1b082f8d-023b-4fea-bc23-2c5c788334ac)

Como vemos nos encontramos ante una respuesta no lineal por lo que usamos la transformación de curva S flexible (Hill) para modelarla. Esta función viene definida de la siguiente manera:

![image](https://github.com/Antchica/Marketing-Mix-Modeling-con-Robyn/assets/136733800/9273dec8-d301-4fb2-ac2b-17e4a7182e7e)

La función Hill es una función de dos parámetros alfa y gamma, en la que alfa controla la forma de la curva (exponencial o forma de S) de manera que cuanto mayor sea alfa más forma de S tendrá la función y, cuanto más pequeña, tendrá forma de C. Por otro lado, gamma controla el punto de inflexión de tal forma que cuanto mayor sea gamma, más tarde será el punto de inflexión en la curva de respuesta. En la Figura 3 vemos representada la función Hill, donde el eje X  representa los gastos y el eje Y representa las respuestas. De manera que, a medida que aumenta el gasto, la respuesta cambia y, a partir de la curva, entendemos cuál es la respuesta marginal.

**Figura 3:**

*Gráfica función Hill con gamma=0.5*

 ![image](https://github.com/Antchica/Marketing-Mix-Modeling-con-Robyn/assets/136733800/5a03e544-dfec-4256-b9cc-ead050bd0285)

**Figura 4:**

 *Gráfica función Hill con alpha=2*
 
![image](https://github.com/Antchica/Marketing-Mix-Modeling-con-Robyn/assets/136733800/dd3a7df4-41f2-4995-97a1-cac74cec7601)


Para **implementar esta transformación en R** debemos:

Añadir un hiperparámetro alfa y gamma para cada variable de medios pagados ```hyper_names()``` con un rango especificado. Para ello verificamos los límites superior e inferior máximos por rango con la función  ```hyper_limits()```

### 2.6.2. Técnicas de modelado

Una vez completada la configuración de la ingeniería de características, pasamos a la ejecución del modelo. Aunque Robyn automatiza este proceso y nos genera los resultados automáticamente, explicaremos como lo hace:

**Regresión de Ridge**

El Marketing Mix Modeling utiliza este modelo de regresión que tiene como objetivo utilizar la regularización para reducir la varianza mediante la introducción de un sesgo. El propósito de la regresión de Ridge es obtener una ecuación que pueda describir la variable dependiente, resolver el problema de la multicolinealidad y evitar el sobreajuste. La notación matemática para la regresión de Ridge es la siguiente:

![image](https://github.com/Antchica/Marketing-Mix-Modeling-con-Robyn/assets/136733800/9119f35f-6f01-4445-bc93-52ce9cf3a7e3)

Asimismo, esta regresión trata de asignar un coeficiente a cada variable independiente para mejorar el rendimiento predictivo del Marketing Mix Modeling. Esto se puede ver en la siguiente función, donde podemos ver cómo el KPI se ve afectado por cambios en todos los factores para los existen datos:

![image](https://github.com/Antchica/Marketing-Mix-Modeling-con-Robyn/assets/136733800/d1c4a541-c127-4cfb-a539-df4e94c81d07)

Donde:
**〖KPI〗_t:** El KPI en el momento (t) que desea modelar.
**β_0:** El rendimiento base o rendimiento si todos los demás factores fuesen 0.
**β:** Los coeficientes, o lo que significa un cambio en la variable (x) para el KPI.

**Selección del modelo**

Robyn proporciona un proceso de selección de modelos semiautomático, que devuelve automáticamente un conjunto óptimo de resultados. Para ello, utiliza Nevergrad la optimización multiobjetivo de Meta Platforms. Nevergrad tiene dos objetivos requeridos por Robyn:

•**Ajuste del modelo:** El objetivo es minimizar el error de predicción del modelo (NRMSE o error cuadrático medio normalizado)

•**Adaptación empresarial:** El objetivo es minimizar la distancia de caída (DECOMP.RSSD, raíz cuadrada de la distancia de descomposición). Esta distancia representa la diferencia entre la participación del gasto y la participación del efecto para las variables de medios pagados. Si la distancia es demasiado grande los resultados podrían ser poco realistas.

Para llevar a cabo estos objetivos, Nevergrad utiliza la técnica de los frentes de Pareto, la cual nos ofrece un gráfico de puntos en los que cada uno de ellos representa una solución de modelo explorada. Además, nos ofrece unas líneas en su esquina inferior izquierda llamadas frentes de Pareto 1-3 que contienen los mejores resultados posibles del modelo de todas las iteraciones. Este gráfico posee dos ejes (NRMSE en el eje X y DECOMP.RSSD en el eje Y) que son las dos funciones objetivo a minimizar. Sin embargo, en lugar de explorar todas las soluciones óptimas de Pareto (puntos), Robyn agrupa en clúster las soluciones con el ROI más similares entre sí. En cambio, esto no quiere decir que estos sean necesariamente los mejores modelos o los más "correctos”. Para llevar a cabo la técnica de Pareto en R, en primer lugar, debemos de crear los modelos de salida, donde se ejecutaran las distintas pruebas e iteraciones y, en segundo lugar, calculamos los frentes de Pareto. Para ello, le indicamos los siguientes comandos al programa estadístico:

**1. Para ejecutar todas las pruebas e iteraciones**

```
OutputModels <- robyn_run(
InputCollect = InputCollect,
cores = NULL, 
iterations = 2000,
trials = 5,
ts_validation = FALSE, 
add_penalty_factor = FALSE)
```
```
print(OutputModels)
```
Donde:
**Iterations:** es el número de las iteraciones recomendadas por Robyn que se 	harán.
**Trials:** es el número de ensayos recomendados por Robyn.

**2.Para calcular los frentes de Pareto**

```
OutputCollect <- robyn_outputs(
InputCollect, OutputModels,
pareto_fronts = "auto",
csv_out = "pareto",
clusters = TRUE,
plot_pareto = TRUE, 
plot_folder = robyn_object, 
export = TRUE)
```
```
print(OutputCollect)
```
Una vez calculados las 2000 iteraciones y los 5 ensayos por parte de R, nos ofrece los modelos que pueden dar solución a las preguntas planteadas por parte de la empresa.

Pero, ¿Qué técnica debemos utilizar para seleccionar el modelo final entre todos los resultados óptimos de Pareto? Robyn ofrece las siguientes técnicas para seleccionar el modelo final:

- *Parámetros de información empresarial*
- *Calibración experimental*
- *Parámetros estadísticos*
- *Gráfico de convergencia de ROAS sobre iteraciones*

En este caso, para la elección del modelo final de entre todos los candidatos, utilizaremos la técnica de los parámetros estadísticos. En la cual deberemos de estudiar los siguientes parámetros: R-cuadrado (R2), que nos indicará la precisión de los datos del modelo (mientras más alto sea el R-cuadrado mayor precisión tendrá el modelo), el NRMSE (error de predicción del modelo) y DECOMP.RSSD (distancia de la relación entre el porcentaje de gastos y el porcentaje de descomposición del coeficiente de un canal o error comercial). 

Tras seleccionar el modelo final pasamos a la siguiente y última fase de Marketing Mix Modeling, en la que interpretaremos los resultados y gráficos del modelo proporcionados por Robyn para la toma de decisiones.


## Fase de Salidas y Diagnósticos

El Marketing Mix Modeling genera automáticamente diversos gráficos en el directorio donde guardemos el modelo seleccionado anteriormente. Cada uno de estos gráficos representa una de las soluciones óptimas del modelo, que es el resultado del proceso de optimización de Pareto. 

### 2.6.3. Aplicaciones del modelo

Para poder brindar las soluciones a las preguntas comerciales propuestas por la empresa y tomar decisiones correctas, debemos analizar e interpretar previamente los gráficos proporcionados por el modelo seleccionado. Concretamente Robyn ofrece 6 gráficos diferentes que son los que veremos a continuación.

**1. Ajuste del modelo**

Según Robyn, para tener un modelo de alta precisión requerirá que el ajuste de los datos del modelo sea preciso en relación con los datos reales. De manera que, si añadimos demasiadas o pocas variables al modelo puede inducir a errores de lectura en algunos medios.

Este gráfico muestra los datos reales en comparación con los datos modelados de tal forma que, en el eje X, aparecen las ventas año tras año de los datos estudiados, es decir, desde el año 2010 al año 2017 y, en el eje Y, aparece la respuesta o demanda. Asimismo, en el gráfico aparecen dos tipos de líneas sobrepuestas, una de ellas es la línea amarilla, esta muestra las ventas reales, y otra línea azul que representa las ventas pronosticadas del modelo. De manera que, si las dos líneas prácticamente coinciden podremos afirmar que el modelo se ajusta bien a los datos reales, por lo que obtendremos resultados fiables.

**2. Cascada de descomposición de respuesta por predictor**

Además de examinar el ajuste del modelo con los datos reales, también examinaremos la contribución porcentual de cada medio publicitario en la demanda. A través de este gráfico, podremos descubrir qué medios son los que generan más demanda o, por el contrario, cuáles generan menos demanda, con el objetivo de organizar mejor el presupuesto de marketing para cada uno de los canales.

**3. Porcentaje del gasto frente al efecto con el ROI total**

Usando este gráfico, estudiaremos el impacto detallado de la inversión en los distintos medios comparando y comprobando varias métricas diferentes:
- *Porcentaje de inversión por canal (barras de color azul oscuro).*
- *La participación de cada canal en la crecida de las ventas (barras de color azul claro).*
- *ROI (retorno de la inversión), que refleja la efectividad de cada canal (puntos).*
  
**4.Curvas de respuesta**

Las curvas de respuesta o curvas de saturación nos muestran si el gasto en un medio publicitario se encuentra a un nivel óptimo o, por el contrario, se acerca a la saturación por lo que habría que redistribuir el presupuesto en los canales que presenten una alta saturación. 
Para ver si un canal de medios presenta una alta saturación debemos fijarnos en sus curvas de respuesta, de manera que si alcanzan rápidamente una pendiente horizontal y un punto de inflexión (el punto en el que la inversión no genera más ventas), indican que ese canal de medios se saturará rápidamente con cada $ adicional gastado en él. Usando este gráfico, realizaremos un análisis para informar sobre las mejores oportunidades para reasignar el gasto en canales más saturados a canales menos saturados para mejorar los resultados.

**5. Tasa de caída de Adstock**

El gráfico representa la tasa de decaimiento de medios, de manera que cuanto mayor sea la tasa de decaimiento, mayor impacto tendrá un canal de medios específico después de la exposición inicial. Usando este gráfico, podemos entender qué canales tienen un impacto inmediato y qué canales tienen un impacto de marca a medio o largo plazo.

**6. Ajustados Vs Residuos**

Este gráfico representa la relación entre los valores ajustados y los valores residuales (distancia a la línea de regresión). Por lo tanto, si los puntos en el gráfico de residuos se distribuyen aleatoriamente entorno al eje horizontal, el modelo se ajusta correctamente al modelo de regresión; de lo contrario, el modelo no sería apropiado.

## Resultados

En este apartado presentamos los resultados del análisis de los datos obtenidos en el estudio de Marketing Mix Modeling sobre la campaña publicitaria de la empresa de televisores. La empresa, planteaba algunas preguntas comerciales que no pudieron resolver, las cuales podemos responder tras realizar el análisis de Marketing Mix Modeling.

Como mencionamos, examinamos los datos y los dividimos en dos variables: dependiente e independientes. Por otro lado, dividimos las variables independientes en medios online, medios offline y otros factores económicos. Una vez clasificadas las variables y agregadas las líneas de base de series temporales de Prophet (estacionalidad, tendencias, días de la semana, días festivos), aplicamos las transformaciones de los datos. Una vez hecho esto, comenzamos con las técnicas de modelado con el objetivo de obtener un modelo de MMM que dé respuestas a las preguntas planteadas.

Para obtener los modelos candidatos a modelo final, aplicamos la técnica de los frentes de Pareto, que nos da el siguiente gráfico (Figura 5), donde se puede ver cómo Nevergrad elimina la mayoría de los modelos con los mayores errores de predicción y/o efectos mediáticos poco realistas. Cada punto del gráfico representa una solución del modelo explorado, mientras que las líneas de la esquina inferior izquierda son los frentes de Pareto 1-3, que contienen los mejores resultados posibles del modelo de todas las iteraciones. 

**Figura 5:**

*Gráfica selección de modelo líneas de Pareto*

![image](https://github.com/Antchica/Marketing-Mix-Modeling-con-Robyn/assets/136733800/fc9d0c6e-1f11-4e7d-96ed-fae6f4382b5e)

Sin embargo, en lugar de explorar todas las soluciones óptimas de Pareto (puntos), Robyn agrupa en clúster las soluciones con el ROI más similares entre sí y los selecciona como modelos candidatos a modelo final. En cambio, esto no significa que estos sean necesariamente los mejores modelos o los modelos más "correctos”. En la Tabla 1 podemos ver los modelos candidatos propuestos por Robyn.

**Tabla 1**

*Tabla de los clústers o modelos óptimos candidatos a modelo final*

![image](https://github.com/Antchica/Marketing-Mix-Modeling-con-Robyn/assets/136733800/552ce288-c118-4326-9b71-0b40bce8c41f)

Para escoger uno de los modelos candidatos como modelo final, elegimos la técnica de los parámetros estadísticos de manera que en la tabla (Tabla 2), podemos ver los parámetros para cada uno de los modelos proporcionados por Robyn.

**Tabla 2**

*Tabla resumen de parámetros estadísticos para cada modelo*

![image](https://github.com/Antchica/Marketing-Mix-Modeling-con-Robyn/assets/136733800/81e80e14-fd5a-421e-b0e6-781162376289)

Entre todos estos modelos (Tabla 2), observamos que los modelos con mejor ajuste (mayor R2 y menor NRMSE) fueron: **4_201_8** y **4_199_5**. Sin embargo, vemos que el **modelo 4_201_8** presenta el R2 más alto (0.7784) y el error de predicción del modelo más bajo (0.0727) de los dos modelos seleccionados, aunque presenta el DECOMP.RSSD más alto (0.4933), este modelo será el candidato más idóneo de entre todos los obtenidos. 

Después de elegir el **modelo 4_201_8** como modelo final, continuamos interpretando los resultados y gráficos de este modelo proporcionados por Robyn para brindar las soluciones a las preguntas planteadas por la empresa y tomar así decisiones correctas. A partir del modelo final, obtenemos los siguientes gráficos:

**1. Ajuste del modelo**

La Figura 6 muestra un modelo más o menos ajustado, ya que la línea amarilla, que corresponde a los datos actuales, coincide en la mayor parte de su dominio con las predicciones (línea azul) que obtuvimos en el modelo de Marketing Mix Modeling, por lo que podríamos decir que es un modelo confiable. Sin embargo, podemos mejorar el modelo agregando más variables independientes que expliquen mejor la variable dependiente (demanda).

**Figura 6**

*Gráfica datos reales Vs datos predictivos*

![image](https://github.com/Antchica/Marketing-Mix-Modeling-con-Robyn/assets/136733800/37ddbcf7-08c1-4bca-a3c6-3611855b9c03)

**2.	Cascada de descomposición de respuesta por predictor**

Lo más destacable que podemos extraer del gráfico de cascada de descomposición de respuesta por predictor (Figura 7), es que la tendencia aumentó las ventas de la empresa en un 101,7% aportando 12M a los ingresos. Asimismo, el Intercept representó el 70% de las ventas (8,24M), lo que demuestra que la empresa está bien establecida en el mercado por lo que cuenta con una amplia base de ventas sin gastos en medios pagados.

Del mismo modo, se pueden apreciar las contribuciones de otros canales a los ingresos, por ejemplo, el impacto del índice de precios del consumidor (CPI) influye en la demanda en un 66,6% aportando 7,84M a las ventas totales. Otros canales importantes que impulsan las ventas del negocio son los gastos en anuncios en periódicos, en Internet y en la radio. Estos tres canales, representaron el 25,1%, el 19.9% y el 17.9% de los ingresos respectivamente, aportando un total de 7,39M.

Asimismo, los canales que también contribuyen a las ventas de la compañía, pero en menor medida que los anteriores, son: las impresiones en televisión, aportando un 5,3% de los ingresos (624K), las impresiones en el periódico, que aportan a las ventas un 3,5%, concretamente 406K y, las impresiones en SMS aportando un 1,8%, es decir, 216k. En última estancia, encontramos que las vacaciones y la estación del año que aportan a las ventas un 0,2% y un 0,1% respectivamente.

Por otro lado, cuando analizamos la tendencia del gráfico vemos que hay algunos canales que no contribuyen en absoluto a las ventas totales, como las impresiones en Internet y otras variables con una atribución negativa correspondientes al precio unitario y al índice de precios del productor (PPI). Estas tendencias negativas les hicieron perder ingresos, siendo el índice de precios del productor el que más hace que disminuyan los ingresos, en concreto 22,8M.

**Figura 7**

*Gráfica cascada de descomposición de respuesta por predictor*

![image](https://github.com/Antchica/Marketing-Mix-Modeling-con-Robyn/assets/136733800/a96fcc48-df59-4dac-a210-59974c11dfe0)

**3.	Porcentaje del gasto frente al efecto con el ROI total**

Por un lado, encontramos que el 0,2% se invirtió en anuncios en periódicos (Advertising Expenses Newspaper ads), siendo la inversión más baja entre todos los canales. Sin embargo, gracias a esta inversión, las ventas aumentaron en un 33,2%, convirtiéndose en uno de los canales que más contribuyó al incremento de las ventas. Además, su ROI es muy superior con respecto al resto (96,12).

Otro medio a destacar son los anuncios en la radio (Advertising Expenses Radio), ya que una inversión del 1,3% en este canal incrementó las ventas en un 23,6%, y logró el segundo ROI más alto (9,84). Asimismo, descubrimos que el 0,5 % invertido en impresiones de SMS resultó en un aumento del 2,4 % en las ventas, obteniendo así el tercer ROI más alto (2,89).

Por otro lado, nos encontramos canales en los que el porcentaje de ingresos no superó el gasto en ese canal, aunque impulsaron las ventas. Este es el caso de los siguientes canales: 

•	*Anuncios en Internet*, invirtió un 46,6% aumentando las ventas un 26,4% y un ROI de 0,31.
•	*Anuncios en televisión*, en el cual se invirtió un 19,9% y generó un crecimiento de los ingresos del 2,8% y un ROI de 0,08.
•	*Impresiones (GRP) en TV*, donde se realizó una inversión del 17,3% resultando un aumento de las ventas del 7% y un ROI igual a 0,22.
•	Las *impresiones en el periódico*, el que se invirtió un 7,7% y ha impulsado las ventas un 4,6% y un ROI igual a 0,33.

Por otro lado, existen otros canales en los que se ha invertido dinero y, sin embargo, no han impulsado nada las ventas, estos son:
•	Las *impresiones en radio* (2,1% de inversión, ROI igual a 0)
•	Las *impresiones en internet* (4,3% de inversión, ROI igual a 0)

**Figura 8**

*Gráfica porcentaje de gasto frente al efecto con el ROI total*

![image](https://github.com/Antchica/Marketing-Mix-Modeling-con-Robyn/assets/136733800/0c3a78ad-9505-4132-af4b-bc40ee55e09b)

**4.	Curvas de respuesta**
   
En este caso, vemos que el canal menos saturado es la publicidad en periódicos, sin embargo, el volumen de inversión no retorna en más ventas a partir de los 12,6 ($). En la Figura 9, podemos ver que hay otros canales que también tendrán un gran crecimiento significativo pero sin una rápida saturación, este es el caso de: la publicidad en la radio y la publicidad en Internet. Por otro lado, encontramos que algunos  canales tienen una alta saturación, como en el caso de: los anuncios en la televisión, impresiones en Internet, en la radio, en SMS y en la televisión por lo que no convendría invertir demasiado dinero en estos canales.

**Figura 9**

*Gráfica curvas de respuesta*

![image](https://github.com/Antchica/Marketing-Mix-Modeling-con-Robyn/assets/136733800/226f51f0-7911-48ce-8c3f-d1520500845a)

**5.	Tasa de caída de Adstock**

De este gráfico (Figura 10), podemos destacar que los canales con una mayor tasa de decaimiento y, que por tanto tienen mayor impacto después de la exposición inicial, son:

1.	Anuncios en la radio
2.	Impresiones en SMS 
3.	Impresiones en Internet
4.	Anuncios en Internet
5.	Anuncios en televisión
6.	Impresiones en televisión
7.	Impresiones en periódicos
8.	Anuncios en periódicos
9.	Impresiones en radio

**Figura 10**

*Gráfica tasa de caída de Adstock*

![image](https://github.com/Antchica/Marketing-Mix-Modeling-con-Robyn/assets/136733800/f9686a15-f3fa-46d9-af10-8ec0b51c490d)

**10.	Ajustados Vs Residuos**
    
En este caso analizamos el gráfico de la Figura 11, donde podemos ver cómo los datos siguen la función en entorno a la línea azul (modelo de regresión óptimo). Aunque la mayoría de los datos están agrupados al principio de la función, se puede decir que los valores ajustados y residuales se ajustan bien al modelo de regresión.
Por el contrario, la línea de regresión (línea azul) predice sistemáticamente datos que son demasiado altos o bajos en diferentes puntos de la curva, lo que se denomina sesgo. Estos sesgos se producen por la falta de predictores o términos de interacción importantes en el modelo, por lo que se podrían añadir y mejorar el ajuste.

**Figura 11**

*Gráfico Fitted vs. Residual*

![image](https://github.com/Antchica/Marketing-Mix-Modeling-con-Robyn/assets/136733800/60aa1308-4969-4f8e-a039-852303f7d4a9)

## Conclusiones

Lo expuesto a lo largo del estudio de Marketing Mix Modeling, nos permite responder a la problemática que nos planteaba la empresa de televisores, la cual nos pedía respuestas a las siguientes preguntas:

1.	¿Qué canales de medios (online y offline) generan más ingresos?
2.	¿Cuál es el ROI de cada canal de marketing?
3.	¿En qué canales debo centrar más mi presupuesto en marketing para maximizar los ingresos?
4.	¿Cuál es el nivel óptimo de gasto para cada uno de los principales canales de marketing?
5.	¿Cuánto impulsan los ingresos incrementales de las actividades comerciales?
   
**Según los resultados obtenidos** en el apartado anterior podemos concluir lo siguiente:

1.	En cuanto a los canales de medios tanto online como offline, concluimos que el canal que más ingresos genera es: la publicidad en el periódico, por lo que se podría realizar un aumento potencial en el gasto de este medio, dado que genera muy buenos retornos (33,2% de los ingresos) y no se saturará rápidamente debido a los bajos gastos (0,2%). Otro canal que aumentan los ingresos, pero en menor medida que la publicidad en el periódico, son los anuncios en la radio. Este canal representa el 23,6% de los ingresos y por sus bajos costos (1,3%) tiene baja saturación, por lo que sería conveniente invertir más en este canal. En cuanto a las impresiones en SMS, podemos decir que tiene el tercer ROI más alto (2,89) y el costo más bajo de todos los canales, por lo que sería conveniente seguir invirtiendo en él. Por otro lado, existen otros canales que aumentan los ingresos, pero con unos gastos más elevados por lo que no se recomienda invertir demasiado, por su alta saturación, en: anuncios en TV, ya que generan el 26,4% de las ventas pero tienen unos gastos muy elevados del 46,6%, por lo que se saturará rápidamente; en anuncios en Internet ya que presenta gastos muy elevados (19,9%) y aumentan los ingresos solo un 2,8%; y en impresiones de periódicos, que aunque aumente los ingresos en un 4,6% sus gastos son más elevados (7,7%). Finalmente, los canales que contribuyen levemente a los ingresos son las impresiones en Internet, la radio y la televisión.

2.	En cuanto al ROI de cada canal concluimos que: el canal que posee el mayor retorno de inversión, con respecto al resto de canales, son los anuncios en el periódico, con un 96,12. Seguidamente, nos encontramos con dos canales que representan los dos ROI más altos, estos son los anuncios en la radio y las impresiones en SMS con un 9,84 y un 2,89 respectivamente. Por otro lado nos encontramos con los canales con el ROI más bajos: impresiones en periódicos (0,33), anuncios en Internet (0,3), impresiones en televisión (0,22), anuncios en televisión (0,08), impresiones en radio (0) e impresiones en Internet (0).

3.	En cuanto a los canales donde debe centrar su presupuesto por canal para maximizar sus ingresos, debe invertir más dinero en anuncios en periódicos, anuncios en la radio e impresiones en SMS ya que son los tres canales que más retorno de inversión generan.

4.	En cuanto al nivel óptimo de gasto para cada uno de los principales canales de marketing recomendamos no invertir más de las siguientes cantidades para cada canal, ya que el volumen de inversión no retorna en más ventas a partir de dicho punto. Estas cantidades son:
   
•	Anuncios en la radio 87,9

•	Anuncios en Internet 3,18K

•	Anuncios en televisión 1,32K

•	Anuncios en periódicos 12,6

6.	En cuanto al impulso de los ingresos incrementales de las actividades comerciales, concluimos que los anuncios en periódicos impulsan un 33.2% los ingresos, seguido de los anuncios en Internet con un 26,4%, los anuncios en radio generan un 23,6% de ingresos y, por último, los anuncios en televisión que generan solamente un 2.4%.

A parte de las preguntas comerciales, gracias al modelo de Marketing Mix Modeling deducimos que su empresa está bien establecida en el mercado aunque tiene una gran base de ventas sin gastos en medios publicitarios. Además, hemos visto que la tendencia juega un papel muy importante en el incremento de sus ventas y que las vacaciones y estaciones del año en China no afectan en gran medida a las ventas. Del mismo modo, podemos afirmar que la influencia del índice de precios influye en la demanda de sus productos.



## Referencias

[1]https://www2.deloitte.com/es/es/pages/strategy-operations/articles/marketing-mix-modeling-herramienta-analytics-eficacia-publicitaria.html

[2]https://towardsdatascience.com/automated-marketing-mix-modeling-with-facebooks-robyn-fd79e60b489d

[3]https://facebookexperimental.github.io/Robyn/docs/features/

https://www.ambit-bst.com/blog/conoces-todos-los-sistemas-de-almacenamiento-de-datos

https://www.brandmanic.com/marketing-en-redes-sociales-evolucion/

https://machinelearningmastery.com/discover-feature-engineering-how-to-engineer-features-and-how-to-get-good-at-it/

https://www.dynamicgc.es/historia-del-big-data/

https://www.enae.es/blog/ciencia-de-datos-y-marketing-dos-disciplinas-irremediablemente-unidas?_adin=02021864894#gref

https://doc.arcgis.com/es/insights/latest/analyze/regression-analysis.htm

https://www.marcialpons.es/libros/big-data-privacidad-y-proteccion-de-datos/9788434023093/

https://www.master-data-scientist.com/aplicaciones-data-science-marketing/
