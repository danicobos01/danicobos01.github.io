## Descripción del proyecto

Ánalisis relacional y estudio de mercado de alrededor de 21 millones reseñas de 300 diferentes videojuegos utilizando técnicas de big data y herramientas de cloud computing. El proyecto consiste en la realización de un análisis de las reviews que se han publicado en Steam en los útlimos años, con el fin de entender mejor las motivaciones de los usuarios a la hora de realizar una review o con el fin de extraer posibles correlaciones entre distintas variables que podrían explicar porque se producen ciertas reseñas. También hemos analizado los idiomas de las reseñas para ver diferencias a la hora de valorar un juego entre distintos países, y la cantidad de reseñas que tienen los juegos más populares o mejor valorados. 

### Necesidad de Big Data

Steam es una plataforma que cuenta con 69 millones de usuarios en activo, y aún muchísimos más que no usan la plataforma a diario pero que son usuarios de Steam. Al ser una plataforma tan popular y mundialmente conocida y usada, no se puede realizar un estudio de las reseñas si no contamos con una gran cantidad de datos. En este contexto donde el Big Data juega un gran valor. Algunos juegos tienen más de un millón de reseñas ellos solos por lo que es imposible no recurrir al Big Data para tratar las reseñas de más de 300 juegos como hemos hecho nosotros. 
Junto al Big Data hemos usado computación en la nube para realizar el estudio, gracias a su gran capacidad de procesamiento de datos y almacenamiento de archivos 

### Dataset de reviews

Fran


### Dataset de Juegos

Complementado el dataset anterior, también hemos utilizado un conjunto de datos algo más pequeño que incluye información de 40000 juegos en Steam. Al igual que con el dataset anterior lo hemos obtenido a través de Kaggle. Su tamaño es de 80Mb y la estructura que presenta es la siguiente: 
"url", Url of a game, "types", type of package - app, sub or bundle, "name", Name of a game, "desc_snippet", short description of a game, "recent_reviews", recent reviews, "all_reviews", all reviews, "release_date", release date, "developer", developer of a game, "publisher", publisher or publishers of a game.

De todos estos datos hemos utilizado sobre todo el género del juego, para poder estudiar cuáles son los generos más populares o los que generan más cantidad de reviews, complementándose con el otro dataset. 

Link del dataset: [Dataset](https://www.kaggle.com/datasets/trolukovich/steam-games-complete-dataset)


### ¿Cómo se puede ejecutar el código?
Vamos a ver las distintas formas en las que se puede ejecutar nuestro trabajo

* Usar Spark en local:

    Para poder ejecutar el código que realiza un procesamiento de los datos, es necesario instalar Java en tu dispositivo Linux. Se puede instalar con el siguiente comando:

      *sudo apt install default-jre*

    Una vez instalado, comprobamos que verdaderamente se ha instalado con el siguiente comando:

      *java -version*

    También se necesita Python, pero no es necesario instalarlo porque suele suele venir instalado por defecto en Linux.

    Habiendo instalado Java, es momento de instalar Spark:

      *curl -O https://archive.apache.org/dist/spark/spark-3.3.1/spark-3.3.1-bin-hadoop3.tgz*

      *tar xvf spark-3.3.1-bin-hadoop3.tgz*

      *sudo mv spark-3.3.1-bin-hadoop3 /usr/local/spark*

      *echo 'PATH="$PATH:/usr/local/spark/bin"' >> ~/.profile*

      *source ~/.profile*

    Se cargan los archivos necesarios.

    Ahora que está instalado Spark y los archivos necesarios están cargados, se puede realizar el procesamiento de datos mediante el envío de trabajos a Spark con el siguiente comando:

      *spark-submit yourFile.py inputFile(s).extension outputFile(s).extension (opcional)*
  
* Usar un Cluster Hadoop en Google Cloud:

    Para poder ejecutar el código que procesa los datos, es necesario crear un cluster Hadoop. Se puede utilizar una región cualquiera, en nuestro caso hemos utilizado europe-west6. Para crearlo, se puede utilizar el siguiente comando:

      *gcloud dataproc clusters create nombre-cluster --region europe-west6 --enable-component-gateway --master-boot-disk-size 50GB --worker-boot-disk-size 50GB*

    Una vez creado el cluster, es necesario crear un bucket donde se almacenarán, tanto los archivos con los datos, como los archivos de entrada y salida.

    Para crear un cluster, es necesario especificar una región, que puede ser cualquiera de las disponibles.

    Ahora es momento de subir al cluster los archivos que contienen los datos, y los archivos de contienen el código para procesar los datos.

    Habiendo subido todos los archivos necesarios al bucket, para poder realizar el procesamiento de datos mediante el envío de trabajos a Spark, es necesario especificar el bucket, que se puede hacer con el siguiente comando:

      *BUCKET=gs://nombre-bucket*

    Procedemos a enviar el trabajo a Spark:

      *gcloud dataproc jobs submit pyspark --cluster nombre-cluster --region=europe-west6 $BUCKET/archivoParaProcesar.py -- $BUCKET/archivo(s)DeDatos $BUCKET/output(s)*

    También se puede realizar una ejecución paralela con distintos nodos mediante las siguientes opciones, pero ejecutándolo desde el nodo master del cluster con el comando:

      *spark submit --num-executors numExecutors --executor-cores numCores <script>*
     
### Desarrollo 

En los siguientes enlaces podéis obtener toda la información relevante del trabajo.
En la carpeta de [Código](link) podéis acceder a todo el código en lenguaje Python que hemos utilizado.
* En "Steam.py" tenemos las distintas funciones que hemos utilizado para generar nuevos csvs a partir del conjunto de datos inicial. Con estos csvs más adelante hemos elaborado un cuaderno de jupyter notebook que contiene todo el análisis de mercado
* En "reducirDatasetID.py" tenemos el proceso llevado a cabo para unir los dos conjuntos de datos que hemos usado, para poder acceder a información relevante de ambos conjuntos. 
* En "wordCountREVIEW.py" tenemos el código que hemos desarrollado para poder generar wordClouds a través de las reviews del conjunto de datos. Dichas nubes de palabras se analizarán en el notebook donde hemos hecho el análisis. 

En la carpeta de [CSVs generados](link) podeis acceder a todos los csvs que se han generado a partir de las funciones
Estos csvs se pueden ver con mayor detalle en el notebook dónde hemos realizado el análisis. 

En el siguiente [Enlace](link) podeis encontrar el notebook en el que hemos realizado el ánalisis de las reseñas. El notebook cuenta con distintos apartados en los que analizamos distintos aspectos del conjunto de datos, como el numero medio de palabras utilizadas o los idiomas de las mismas. También se han estudiado distintas correlaciones para encontrar patrones a la hora de hacer reseñas. 
El cuaderno está guiado para entender el análisis que se ha llevado a cabo sin necesidad de más explicaciones. 

### Rendimiento
