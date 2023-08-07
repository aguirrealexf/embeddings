# Q&A con Embeddings: pruebas de concepto y funcionamiento
En este proyecto se pretende probar y el funcionamiento de Embeddings.
Para eso se propone crear un "asistente" al cual podamos realizarle preguntas, pueda encontrar los artículos que mejor se ajusten como respuesta, y procesar la respuesta con el modelo de ChatGPT.

De esta manera, el flujo de trabajo lo podemos pensar en 3 partes:

### Procesamiento de fuente de datos:
Se considera como fuente de datos un documento de Word, en donde se lo va a procesar y separar su contenido según el tipo del Título.
En este trabajo, el documento debe tener un esquema de 3 niveles:
- el primer nivel, definido con el formato "Titulo 1"
- el segundo nivel, defino con formato "Título 2"
- el tercer nivel, con formato de "Título 3"

Cada nivel puede contar como contenido texto con un formato "Normal".
Para el procesamiento se va a leer párrafo a párrafo todo el contenido, y se crearan artículos para cada tipo de Título, incluyendo el texto en formato "Normal" que cada uno puede contener.
Esta información se almacenara en un DataFrame, donde iremos guardando todos los artículos generados.
Adicionalmente, es encuentra la alternativa de poder guardar cada artículo como un documento Word particular (es una línea, la cual esta comentada en el código).
(1)


### Análisis de Tokens y generación de vectores de Embeddings:
Con los artículos generados, pasamos controlar la cantidad de "tokens" de cada artículo y luego generar su vector de Embedding correspondiente.
Se debe controlar la cantidad de "tokens" antes de realizar el proceso de Embeddings, ya que cada modelo tiene una cantidad máxima de "tokens" como input. En este caso se están utilizando los modelos de OpneAI, pero usando el framework de Microsoft Azure.
Para el proyecto, el control de "tokens" no presento ningún inconveniente, ya que la longitud del contenido de cada artículo estaba muy por debajo de los 4k que tiene como límite el modelo "text-embedding-ada-002".
Una vez asegurado la cantidad de "tokens" de entrada, se recorre el contenido del DataFrame, se generan los vectores de Embeddings correspondientes, y se guarda la info también en el DataFrame.
(2)

### Generación de mensaje a entregarle a ChatGPT:
Para la creación del mensaje que luego se entrega a ChatGPT, se tiene en cuenta:
- la consulta ingresada: primeramente, se genera el vector de Embedding de la misma, y luego con la ayuda de la métrica de "similitud de coseno", seleccionamos los artículos que más se asemejan.
- armado de mensaje: con esta colección de articulos, más la consulta ingresada, se arma un único string de datos, que sera entregado a ChatGPT.
- consulta a ChatGPT: se define la estructura de mensaje necesaria para definir el rol que debe tomar ChatGPT, y el mensaje generado en el item anterior (pregunta + artículos)
Para este proyecto, también se utilizan los modelos de OpneAI, pero usando el framework de Microsoft Azure. En particular estamos usando el modelo "gpt-35-turbo".
(3)


## Fuentes de soporte y códigos base:
Para el desarrollo de esta parte del proceso, se utilizó como base los script de la siguientes referencias:
- https://github.com/python-openxml/python-docx/issues/611 (1)
- https://github.com/openai/openai-cookbook/blob/3115683f14b3ed9570df01d721a2b01be6b0b066/examples/Embedding_Wikipedia_articles_for_search.ipynb (2)
- https://github.com/openai/openai-cookbook/blob/main/examples/Question_answering_using_embeddings.ipynb (2) y (3)
- https://stackoverflow.com/questions/74000154/openai-gpt-3-api-how-do-i-make-sure-answers-are-from-a-customized-fine-tuning/75192794#75192794 (idea general)
