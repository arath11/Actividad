# Actividad

En esta actividad trabajarás con el archivo computer_prices.csv, basado en un conjunto de datos sobre características técnicas y especificaciones de computadoras portátiles y de escritorio, disponible en KaggleEnlaces a un sitio externo..


Los datos fueron recopilados para analizar el rendimiento y el precio de los dispositivos, e incluyen información sobre hardware, almacenamiento, conectividad y otras especificaciones técnicas. Los indicadores incluidos son:

device_type: Tipo de dispositivo (ej. laptop, desktop)
brand: Marca del dispositivo
model: Modelo del dispositivo
release_year: Año de lanzamiento del dispositivo
os: Sistema operativo instalado
form_factor: Factor de forma o diseño del dispositivo (ej. laptop, ultrabook, desktop tower)
cpu_brand: Marca del procesador
cpu_tier: Nivel o gama del procesador, ordinal del 1 al 6 según desempeño
cpu_cores: Número de núcleos del procesador
cpu_threads: Número de hilos de ejecución del procesador
gpu_brand: Marca de la tarjeta gráfica
gpu_model: Modelo específico de la tarjeta gráfica
gpu_tier: Nivel o gama de la GPU, ordinal del 1 al 6 según desempeño
vram_gb: Memoria de video de la GPU en gigabytes
ram_gb: Memoria RAM del dispositivo en gigabytes
storage_type: Tipo de almacenamiento (ej. HDD, SSD)
storage_gb: Capacidad de almacenamiento en gigabytes
storage_drive_count: Número de unidades de almacenamiento instaladas
display_type: Tipo de pantalla (ej. IPS, TN, OLED)
charger_watts: Potencia del cargador (en watts) para laptops
psu_watts: Potencia de la fuente de poder (en watts) para desktops
wifi: Estándar de conectividad Wi-Fi (ej. Wi-Fi 5, 6, 6E, 7)
bluetooth: Versión de Bluetooth
weight_kg: Peso del dispositivo en kilogramos
warranty_month: Meses de garantía del dispositivo
price: Precio del dispositivo. Es la variable de salida o target, es decir, la que se pretende predecir más adelante al construir el modelo.
 


Utiliza la libreta Actividad6FE.ipynb Descargar Actividad6FE.ipynby sigue estos pasos:


Descarga el archivo: computer_prices.csv Descargar computer_prices.csvy guarda, en un dataframe (compu_df), todos sus registros.
Utiliza el método info() del dataframe, para obtener el resumen de los tipos de datos. ¿Cuántas columnas son numéricas y cuántas de texto?
Determina la cantidad de valores únicos por columna.
Elimina las variables: 
model: Debido a su altísima cardinalidad, lo que dificulta su uso en análisis y modelado.
cpu_model: Además de su elevada cardinalidad, su información ya está representada de manera implícita en otras variables como: cpu_tier, cpu_cores y cpu_threads


Antes de iniciar con el análisis univariado, verifica si hay valores duplicados y/o faltantes.
Obtén las estadísticas descriptivas, separado las numéricas y las categóricas. De estas últimas incluye las tablas de frecuencia.
Genera histogramas para las numéricas y diagramas de barras para las categóricas. Con alta cardinalidad, sólo incluye los 10 valores más frecuentes.


Dibuja un mapa de calor con la matriz de correlación para las variables numéricas del conjunto de datos.
Identifica los pares de variables cuya correlación sea superior a 0.9 e imprímelos.
Reflexiona sobre cuáles variables, de las que se imprimieron, representan de manera general la capacidad del hardware y mantenlas; elimina las demás por aportar información redundante.
Incluye una breve justificación de tus decisiones.



Para comenzar con la ingeniería de características, crea una copia del dataframe y asígnala a un nuevo objeto llamado compu_transf.
Calcula cuántos años han pasado desde el lanzamiento de cada computadora y almacénalo en una nueva columna llamada years_since_release; luego, elimina la columna original.
Utiliza KBinsDiscretizer para reemplazar la columna vram_gb en 4 bins ordinales basados en cuantiles.
Imprime los valores que delimitan cada bin y haz un gráfico de barras para ver la cantidad de observaciones en cada uno, con el fin de entender cómo se agruparon los datos.



Observa los histogramas del ejercicio 2. Notarás que en las variables charger_watts y psu_watts aparece una barra en 0. Analiza por qué ocurre esto y qué significa en relación con el tipo de dispositivo.
Como estas variables son mutuamente excluyentes, combínalas en una nueva columna llamada power_watts que contenga la potencia correspondiente de cada dispositivo y, a continuación, haz un histograma para verificar que la distribución resultante es bimodal.
Por último, borra las columnas originales charger_watts y psu_watts.
Para disminuir el sesgo de la variable price, crea tres transformadores: logaritmo, raíz cuadrada y Box - Cox.
Aplica cada transformador a la variable price, dejando el resultado en variables temporales. El objetivo es comparar los efectos de cada transformación antes de decidir cuál aplicar de manera definitiva sobre las variables continuas del dataframe.
De la variable original y de cada una de las tres transformaciones se debe mostrar:
Histograma: para observar la distribución de los datos.
Boxplot: para identificar posibles valores atípicos.
Q-Q plot: para evaluar la normalidad de la variable.
Skew (sesgo): para cuantificar la asimetría de la distribución.
Cantidad de outliers: para conocer cuántos valores extremos existen.
En función de los resultados obtenidos al comparar las transformaciones, decide cuál logró el mejor efecto sobre la distribución de la variable y aplícala directamente en el dataframe, reemplazando las variables continuas: weight_kg, power_watts y price.



Para que todas las variables numéricas estén en la misma escala, aplica MinMaxScaler de sklearn a todas las columnas numéricas del dataframe, reemplazando las columnas originales. 
Nota: Ambos cambios deben efectuarse sobre la columna original, de manera que quede una única columna wifi con toda la información transformada.


Aunque wifi es una variable categórica, sus categorías tienen un orden natural (Wi-Fi 5 < Wi-Fi 6 < Wi-Fi 6E < Wi-Fi 7). Codifícala usando OrdinalEncoder.
Luego, escala la variable codificada entre 0 y 1 con MinMaxScaler, para que quede en la misma escala que las variables numéricas del dataframe.


La variable gpu_model tiene muchas categorías. Usar One-Hot Encoding aumentaría significativamente la dimensionalidad del dataframe. Por ello, utiliza BinaryEncoder para codificarla.
Guarda el resultado en un dataframe llamado bin_df. Más adelante, lo combinarás con compu_transf para integrar las variables codificadas.


Usa OneHotEncoder para codificar las variables categóricas restantes. Asegúrate de usar drop='first' para evitar la multicolinealidad y guarda el resultado en un dataframe llamado ohe_df.
Combina el dataframe compu_transf con las variables categóricas que fueron codificadas en bin_df y ohe_df. No olvides eliminar las variables originales.
Usa describe() sobre el dataframe resultante para corroborar que todas las columnas estén escaladas entre 0 y 1 y que no queden variables categóricas sin codificar.
