![IMAGEN PORTADA F](https://github.com/maferhel/PI2H/blob/main/DATA/IMAGENES/IMAGEN%20PORTADA%20F.png)
<h1 align="center">SINIESTROS VIALES EN LA CIUDAD DE BUENOS AIRES</h1>

### DESCRIPCIÓN DEL PROYECTO.<br />
Con el fin de cumplir con la función para la que fue creado el Observatorio de Movilidad y Seguridad Vial (generar información fehaciente que le permita a la Secretaría de Transporte del Gobierno de la Ciudad Autónoma de Buenos Airesnos tomar decisiones de manera conjunta con vecinos, vecinas y la sociedad civil para mejorar la seguridad vial y la forma de la circulación que se desarrolla todos los días) encomendó la elaboración de un proyecto de análisis de datos, con el objetivo de obtener información relevante que le permita a las autoridades locales tomar medidas para disminuir la cantidad de víctimas fatales de los siniestros viales. Para ello, se disponibilizó un dataset sobre homicidios en siniestros viales acaecidos en la Ciudad de Buenos Aires durante el periodo 2016-2021, el que contiene dos hojas llamadas: hechos y víctimas, junto con otras dos hojas adicionales de diccionarios de datos, que fueron útiles para un mayor entendimiento de la data compartida.<br />

También teníamos a disposición un data set denominado lesiones, en el que se encontraban registrados los hechos que no poseían victimas fatales, el que por su gran contenido de datos nulos no fue tenido en cuenta para el análisis, además de que la información inserta en él, a mi juicio no apartaba información de relevancia en vista del propósito de la investigaciónn que es "disminuir la cantidad de víctimas fatales de los siniestros viales".<br />

A su vez, para ésta elaboración también apelé a la bases de datos, de acceso público, que tiene a disposición el [Gobierno de la Ciudad de Buenos Aires](https://data.buenosaires.gob.ar/dataset/semaforos) sobre las intersecciones semaforizadas, con el fin de localizar aquellos siniestros que se han dado en estos lugares a ver si ello nos aporta algún dato relevante. <br />

### CONTEXTO.<br />
Antes de continuar, según el [informe anual de víctimas fatales](https://buenosaires.gob.ar/infraestructura/movilidad/informes-estadisticos-de-seguridad-vial) confeccionado por ese organismo estatal, durante el 2022 fallecieron 111 personas como consecuencia de un siniestro vial. Dando por rasultado una tasa de mortalidad de la Ciudad (4,1 decesos cada 100.000 habitantes), posicionándola por debajo de las registradas a nivel nacional, con una evolución similar en ambos distritos entre los años 2019 y 2022.<br />

Entonces, entendiendo que los siniestros viales son una preocupación importante debido al alto volumen de tráfico y la densidad poblacional que reviste ésta Ciudad, así como que las tasas de mortalidad relacionadas con siniestros viales suelen ser un indicador crítico de la seguridad vial en una región, por lo que reducir estas tasas es un objetivo clave para mejorar la seguridad vial y proteger la vida de las personas en la ciudad.<br />

Es por ello que a partir de la premisa expuesta y de los resultado conseguidos en los últimos años es que se encomendó profundizar la investigaciión de los datos registrados hasta el momento, para explotar la información que existe en la base de datos, a los fines de desarrollar políticas públicas para la educación vial, el cumplimiento de las normas de tráfico, la infraestructura segura de carreteras y calles, así como la promoción de vehículos más seguros.<br />

## INFORME EDA. <br />
![FUJOGRAMA](https://github.com/maferhel/PI2H/blob/main/DATA/IMAGENES/FLUJOGRAMA.png)  
<br />

**1. INGESTA DE DATOS.** <br />

En este primer paso disponibilice los datasets 'homicidios',  de acuerdo a las dos hojas de excel que componían el archivo, desglosándolo en: 'df_Hhechos' y en el 'df_Hvictima', además de manipular el archivo 'semáforos', con el que cree una columna de igual nombre en el primero de los dataframe mencionados y por medioo de los comandos display() e .info() accidí a su composición e información general para ver como cada data set estaba estructurado.<br />
Documentación original que se encuentra en la carpeta "DATA", a la que se puede acceder presionando **[aquí](https://github.com/maferhel/PI2H/tree/main/DATA).**

**2. E T L (EXPLORACIÓN PRELIMINAR).** <br />

La finalidad de esta etapa primero es entender cual es la relación entre las tablas, hallando que ellas se vinculan a partir de las columnas 'df_Hhechos[ID]' y de 'df_Hvictima[ID_HECHO]' de la que surgieron las siguientes conclusiones preliminares:<br />

En el 'df_Hhechos':<br />

- La impresión de las columnas que contienen datos "SD", me lleva a inferir por ejemplo que, los existentes en la columna "ACUSADO" se podría deber a que quien provocó el homicidio se dió a la fuga o no pudo ser indentificado por las autoridades que intervinieron una vez ocurrido el hecho.<br />
- Ahora, si observamos la columna "VICTIMA", arribar a una conclusión es un poco más complejo. Una deducción fácil podría ser que las victimas fueron llevadas a un nosocomio por alguien que no haya sido parte de las autoridades que normalmente intervienen en los siniestros viales y el relevamiento del dato se haya efectuado en el hospital, produciendose una pérdida de la trazabilidad de la información (situación que también, me da a sospechar, podría estar encubriendo que la causa de esa muerte pudo haberse debido a una circunstancia que no sea un siniestro vial).<br />
- Esa primera inferencia sería procedente, en tanto y en cuanto en la columna "ACUSADO" también se haya cargado el mismo dato "SD", situación que no se replica en las filas 88, 164, 214 y 269, porque la existencia de información del tipo de vehículo involucrado en el siniestro y que sería el agente causal de la muerte, me lleva a entender que los datos se relevaron en la escena del accidente, por lo que la ausencia de datos en la columna "VICTIMA" pudo deberse a: una omisión al momento de confeccionarse la documentación ó que se de por sentado que la carga no es necesaria cuando quien fallece sea un peaton.<br />

En el 'df_Hvictima':<br />

- De ésta visualización se puede decir, por ejemplo, que en primer lugar, debe efectuarse su contrastación con el df_Hhechos, para ver si en la columnas "VICTIMAS" de ambos data sets coinciden los campos ingresados como "SD", para lo cual nos podemos valer de las columnas "ID" y "ID_hechos". <br />
- Consultados los primeros 50 resultados de la cabeza de data frame se observa que los valores consignados como "SD", en la columna "VICTIMAS" del df_Hvictimas, para las filas 39 y 85 (ID_hechos: 2016-0052 y 2016-0085, respectivamente) se encuentran cargados con un valor distinto a "SD", en la columna "VICTIMAS" del df_Hhechos, por lo que una buena practica sería entrelazar ambos datas set y completar los datos faltantes de una columna con los existentes en la otra.<br />

**3. E D A.** <br />

- En esta etapa exploré valores únicos/repetidos, así como por medio de la utilización de herramientas incorporadas a la librería missingno, como ser: MATRIZ DE NULOS, MAPA DE CALOR DE CORRELACIÓN DE NULOS, GRÁFICO DE BARRAS DE VALORES FALTANTES y GRÁFICO DE DENDROGRAMA DE NULOS, obtuve una idea general de la distribución de valores faltantes en cada conjunto de datos, visualice la correlación entre las variables que tienen valores faltantes, determine la cantidad de valores faltantes en cada columna y agrupe las variables que tienen patrones de valores faltantes similares.<br />
- Por otro lado investigue la existencia de valores atípicos, extremos y outliers, para lo que tuve que clasificar cada una de las variables que componen cada dataframe con el fin de aplicar el método adecuado para su detección, utilizanzo para las COLUMNAS CUANTITATIVAS el método estadístico descriptivo y la visualización de los datos por medio de Boxplot, y para las COLUMNAS CUALITATIVAS un análisis de frecuencia de palabras.<br />
- Ésta etapa también involucró la normalización de datos, recayendo dicha actividad sobre las columnas del 'df_Hhechos':"Dirección Normalizada", "XY (CABA)", "pos x", "pos y", "TIPO_DE_CALLE", e incorporación de la columna "SEMAFORO" (utilizanzo la tabla de datos semáforo que mencioné al inicio), tarea de normalización para la cual además me valí de la API de GOOGLE MAPS para completar los valores faltantes en las columnas "XY (CABA)", "pos x" y "pos y". Respecto del 'df_Hvictimas' la labor recayó sobre la columna "VICTIMA", datos faltantes que intenté completar a partir de la columna "VICTIMA" del 'df_Hhechos', pero de su ejecución resultó que los datos faltantes en la columna "VICTIMA" del 'df_Hvictimas' también faltan en la columna "VICTIMA" del 'df_Hhechos'. <br />

**4. ELABORACIÓN DE INSIGHTS.** <br />

Teniendo presente el objetivo principal de este proyecto, que recuerdo es: "disminuir la cantidad de víctimas fatales de los siniestros viales", inmediatamente me surgió el interrogante de cuál sería el número aceptable de fallecimientos para una sociedad, y la respuesta la hallé en el siguiente video, el que te invito a ver:<br />

<p align="center">
  <a href="https://youtu.be/mWOzoMeNqDc?si=47PFbg1trBbGXA7H">
    <img src="https://github.com/maferhel/PI2H/blob/main/DATA/IMAGENES/VIDEO.png" alt="Video" width="560" height="315">
  </a>
</p>
<p align="center">
  Enlace directo al video: <a href="https://youtu.be/mWOzoMeNqDc?si=47PFbg1trBbGXA7H">Ver video en YouTube</a>
</p>

Con ésta premisa en mente agrupé los análisis de acuerodo al dato sobre el cuál quería obtener información, es decir, los hallazgos se generaron a partir de indagar el tiempo, los sujetos involucrados y lugar de ocurrencia de los hechos, resultando los siguientes insights:<br />

- Con los ANÁLISIS TEMPORALES busque lo siguiente:<br />
  + Tendencia por año, mes y día.<br />
  + Hora más frecuente por año.<br />
  + Cantidad de siniestros por momento del día (mañana, tarde o noche).<br />
  + Siniestros por temporada.<br />
  + Siniestros por temporada, por años apilados.<br />
  + Tendencia de los siniestros, por temporada a lo largo de los años.<br />
  + Cantidad de hechos por día del mes de diciembre, apilados por año.<br />
  + Cantidad de víctimas, por tipo, involucrada en la totalidad de la base de datos, como en los hechos ocurridos en el mes de diciembre.<br />

- Del ANÁLISIS DE SUJETOS INVOLUCRADOS, rastree:<br />
  + Tipo de víctima involucrada por año.<br />
  + Rol de la víctima en los hechos registrados en todo el data set y en el mes de diciembre de cada año.<br />
  + Cantidad de acusados, por tipo, involucrada en la totalidad de la base de datos, como en los hechos ocurridos en el mes de diciembre.<br />
  + Tipo de acusado involucrado por año.<br />
  + Relación víctima-acusado en todo el data set y en el mes de diciembre.<br />

- Desde un ANÁLISIS EN BASE A LOCALIZACIÓN DEL HECHO Y SUS CIRCUNSTANCIAS GEOGRÁFICAS DE ACAECIMIENTO, busque:<br />
  + Localización del hecho de acuerdo a la comuna.<br />
  + Localización del hecho de acuerdo a la vía de circulación.<br />
  + Localización del hecho de acuerdo a su posición en la vía de circulación (en una intersección con otra vía o no).<br />
  + Semaforización de la intersección.<br />
  + Rol de la víctima que muere a mitad de calle.<br />

En ésta etapa también debí elaborar y graficar los siguientes KPIs:<br />
  + Reducir en un 10% la tasa de homicidios en siniestros viales de los últimos seis meses, en CABA, en comparación con la tasa de homicidios en siniestros viales del semestre anterior. Defiendo a la tasa de homicidios en siniestros viales como el número de víctimas fatales en accidentes de tránsito por cada 100,000 habitantes en un área geográfica durante un período de tiempo específico. Su fórmula es: (Número de homicidios en siniestros viales / Población total) * 100,000.<br />
  + Reducir en un 7% la cantidad de accidentes mortales de motociclistas en el último año, en CABA, respecto al año anterior, cuadno se define a la cantidad de accidentes mortales de motociclistas en siniestros viales como el número absoluto de accidentes fatales en los que estuvieron involucradas víctimas que viajaban en moto en un determinado periodo temporal. Su fórmula para medir la evolución de los accidentes mortales con víctimas en moto es: (Número de accidentes mortales con víctimas en moto en el año anterior - Número de accidentes mortales con víctimas en moto en el año actual) / (Número de accidentes mortales con víctimas en moto en el año anterior) * 100.<br />
  + Reducir en un 8% la cantidad de accidentes que involucren vehículos de transporte de pasajeros y peatones como víctimas en el último año, en CABA, respecto al año anterior, para lo que se necesitó contar el número absoluto de accidentes mortales en los que estuvieron involucrados vehículos de transporte de pasajeros y peatones como víctimas en un año específico y luego se comparó esa cantidad con la del año anterior para calcular la reducción porcentual.<br />

Para no hacer tan extenso éste readme, se invita al lector a consultar el siguiente **[script](https://github.com/maferhel/PI2H/blob/main/PI2H.ipynb)** para conocer cuáles son las conclusiones a las que arribé a lo largo de todo el análisis exploratorio de datos.<br />
  
**5. DISPONIBILIZACIÓN DE DASHBOARD.** <br />
Utilizando la herramienta de Power BI la información obtenida del paso anterior fue plasmada en un lienzo, de un modo tal que se pueda narrar la historia que nos muestran esos datos, **[dashboard](https://github.com/maferhel/PI2H/blob/main/......)** que también los invito a explorar junto al siguiente **[video](https://github.com/maferhel/PI2H/blob/main/......)** que grabé con el relato sobre la información hallada.<br />



## AUTORA.<br />
#### María Fernanda Helguero. <br />
Para cualquier duda/sugerencia/recomendación/mejora respecto al proyecto agradeceré que me la hagas saber, para ello contactame por [LinkedIn](https://www.linkedin.com/in/maria-fernanda-helguero-284087181/)<br />

<p align="center">
  <img src="https://github.com/maferhel/PI2H/raw/main/DATA/IMAGENES/VAMOS%20BS%20AS.gif">
</p>


