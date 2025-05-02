## 1. Introducción

Este proyecto tiene como objetivo analizar datos de viajes en taxi en Chicago para identificar patrones clave en las preferencias de los pasajeros y evaluar el impacto de factores externos —como las condiciones meteorológicas— en la frecuencia y duración de los trayectos. El propósito final es facilitar la entrada estratégica de **Zuber**, una nueva empresa de viajes compartidos, al mercado local.


### Objetivos específicos

- Identificar las empresas de taxis más activas en noviembre de 2017.
- Determinar los barrios con mayor número de viajes finalizados.
- Evaluar si el clima afecta la duración de los viajes desde el Loop hasta el Aeropuerto Internacional O’Hare durante los sábados lluviosos.

### Descripción de los datos

El análisis se basa en una base de datos y archivos CSV con información sobre viajes, empresas de taxi, clima y ubicación:

**Tablas principales de la base de datos:**

- `neighborhoods`: Información sobre los barrios de Chicago.
    - `neighborhood_id`, `name`

- `cabs`: Detalles sobre los taxis
    - `cab_id`, `vehicle_id`, `company_name`

- `trips`: Información sobre los viajes realizados
    - `trip_id`, `cab_id`, `start_ts`, `end_ts`, `duration_seconds`, `distance_miles`, `pickup_location_id`, `dropoff_location_id`

- `weather_records`: Registros meteorológicos por hora
    - `record_id`, `ts`, `temperature`, `description`


**Archivos CSV proporcionados:**

- `project_sql_result_01.csv`: Número de viajes por empresa (15-16 de noviembre)
- `project_sql_result_04.csv`: Promedio de viajes por barrio en noviembre
- `project_sql_result_07.csv`: Viajes sabatinos del Loop a O’Hare, con duración y condiciones meteorológicas


## 2. Metodología

El proyecto se dividió en cinco etapas principales:

### 2.1 Recopilación de datos meteorológicos

- Se extrajeron datos sobre las condiciones climáticas en Chicago durante noviembre de 2017 desde una página web proporcionada por el Bootcamp.

- Se utilizó `requests` y `BeautifulSoup` para obtener los datos HTML, que fueron luego transformados en un DataFrame utilizando `pandas`.

- Se llevó a cabo una limpieza del conjunto de datos para asegurar la correcta tipificación, eliminación de duplicados e interpretación de unidades (temperatura en grados Kelvin).


### 2.2 Análisis exploratorio mediante consultas SQL

- Se realizaron múltiples consultas SQL para:

    - Determinar el número de viajes por empresa en fechas específicas.

    - Identificar el volumen de viajes asociados a empresas con nombres que contienen "Yellow" o "Blue".

    - Clasificar las empresas más populares y agrupar al resto bajo la categoría “Other”.

    - Identificar los códigos de barrio del Loop y del Aeropuerto O'Hare.

    - Clasificar las condiciones climáticas en "Good" o "Bad" usando el operador `CASE`.

    - Recuperar los viajes que iniciaron en el Loop y finalizaron en O’Hare los días sábado.


### 2.3 Análisis de datos en Python

- Se importaron tres archivos CSV resultantes de las consultas SQL.

- Se evaluaron duplicados, valores nulos, tipos de datos y estadísticas descriptivas para validar la calidad de los datos.

- Se realizaron gráficos con `plotly.express` para visualizar:

    - El número de viajes por empresa (15-16 de noviembre).

    - Los 10 barrios más comunes como destino de viaje en noviembre.


### 2.4 Prueba de hipótesis

- Se evaluó si la duración promedio de los viajes desde el Loop hasta O’Hare cambiaba bajo condiciones meteorológicas adversas los días sábado.

- Se aplicó la prueba de Levene para verificar la homogeneidad de varianzas.

- Según el resultado, se utilizó la prueba de Student (`ttest_ind`) para comparar las medias entre grupos de clima “Good” y “Bad”.


### 2.5 Interpretación y conclusiones

- A partir de los resultados estadísticos y exploratorios, se identificaron patrones de comportamiento del mercado de taxis en Chicago.

- Se derivaron conclusiones útiles para la futura estrategia de entrada de Zuber en el mercado.


## 3. Resultados

### 3.1 Análisis Exploratorio con Python

- El análisis de `project_sql_result_01.csv` mostró una fuerte concentración de viajes en pocas empresas, con una amplia dispersión y presencia de valores atípicos.

- En el archivo `project_sql_result_04.csv`, el barrio Loop sobresalió como el destino más frecuente. Otros barrios céntricos y comerciales también mostraron alta actividad de viajes, mientras que barrios residenciales o periféricos presentaron volúmenes menores.


### 3.2 Prueba de Hipótesis

Se planteó la siguiente hipótesis:

- **Hipótesis nula (H₀)**: La duración promedio de los viajes desde el Loop hasta el aeropuerto de O’Hare es la misma en sábados lluviosos y no lluviosos.

- **Hipótesis alternativa (H₁)**: La duración promedio de los viajes sí cambia en función del clima.

Se utilizó la prueba de Levene para evaluar la igualdad de varianzas y se optó por la prueba t de Student debido a la igualdad encontrada. El valor p obtenido fue significativamente menor al nivel de significancia (α = 0.05), lo que permite rechazar la hipótesis nula y sugiere que el clima podría influir en la duración de los viajes.


## 4. Conclusiones

El análisis de los datos sugiere que el mercado de transporte en Chicago está altamente concentrado, con unas pocas empresas (como Flash Cab) dominando la mayoría de los viajes realizados durante los días analizados.

En cuanto a la distribución geográfica, los barrios con mayor actividad de viajes tienden a ser zonas céntricas, comerciales y de alto tráfico como Loop, River North y Streeterville. Esto puede deberse tanto a la alta concentración de oficinas, hoteles y atracciones turísticas, o una disponibilidad limitada de estacionamientos privados.

La prueba de hipótesis encontró evidencia estadísticamente significativa de que la lluvia afecta la duración de los viajes desde el Loop al Aeropuerto Internacional O’Hare. 

Estos hallazgos pueden ayudar a Zuber a definir una estrategia de entrada al mercado enfocada en zonas de alta demanda como el Loop, y a establecer alianzas con taxistas o flotas en zonas con baja participación de competidores fuertes. Además, podrían priorizar el análisis de otras variables —como la frecuencia de viajes, tiempo de espera, o satisfacción del cliente— que podrían verse más influenciadas por el clima.

### Nota

Limitaciones: Este análisis se basa exclusivamente en datos de noviembre de 2017, por lo que no se pueden generalizar los resultados a todo el año sin un análisis adicional.


## 5. Archivos del proyecto

- `Sprint8_Zuber_Chicago_Project.ipynb`: Cuaderno principal con todo el análisis, gráficos, y prueba de hipótesis.
- `project_sql_result_01.csv`: Datos de viajes por empresa del 15-16 de noviembre.
- `project_sql_result_04.csv`: Promedio de viajes por barrio en noviembre.
- `project_sql_result_07.csv`: Viajes del Loop a O'Hare con duración y clima.
- `README.md`: Documento explicativo del proyecto.
- `requirements.txt`: Lista de librerías necesarias para ejecutar el análisis.
