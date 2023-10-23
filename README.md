# PROYECTO - Data_cleaning_Sharks

### Descripción del proyecto

Este proyecto se trata de realizar la mayor limpieza posible de un DataFrame basado en ataques de tiburones. Esta limpieza será de utilidad para un posible análisis posterior.

### Restricciones

- Deben quedar al menos 1500 filas y 23 columnas.
  
  -> La dimensión inicial del DataFrame es de (25723, 24) 
  
## 1.	LIMPIEZA DE VALORES NULOS
   
Como punto de inicio de mi limpieza, me dispongo a reemplazar todos los valores nulos del DataFrame. 

-	Observo que todas las columnas tienen valores nulos, incluso alguna columna tiene todos los datos de esta como valores nulos. 
-	Se eliminan las filas en las que todos sus valores son valores nulos, así como aquellas filas en las que todos los valores son nulos excepto uno de ellos, que parece una ha sido un error, o que la información no es útil.

-> Las dimensiones del DataFrame pasan a ser: (6302, 24)

El siguiente paso será hacer una limpieza específica de cada una de las columnas. 
•	Columna Age: Como no nos será posible averiguar la edad del tiburón, en todas las filas en las que su valor sea nulo vamos a sobrescribir “unknown”. 

•	Columna ‘Year’:

  - Valores a número. Todos los que no se puedan, en este caso solamente 2 valor, se elimina la fila. Los demás se pasan a Integer.
  - Antes de continuar, y para menguar los datos que pueden ensuciar el DataFrame, vamos a eliminar aquellas filas que tienen valores muy antiguos. Tendremos en cuenta valores desde el año 1900.

•	Columna 'Date':

  - No tiene valores nulos, ya han sido todos eliminados.

•	Columna 'href formula': 

  - Hay un valor nulo en la columna 'href formula'. Como esta coincide en valores con la columna 'href', se utiliza la función 'map_values' para transformar los valores de la serie 'href' en nuevos valores que se asignarán a la columna 'href formula'.

•	Columnas ‘Country’, ‘Area’ y ‘Location’:
  - Las filas en las que las 3 columnas tienen nulos las elimino.
    
	 • Columnas 'Area', 'Location' nulas
    - Utilizo la funcion df.loc[ ] se utiliza para seleccionar un subconjunto de filas y columnas de un DataFrame.
    - Reemplazar los valores nulos en ambas columnas por "Somewhere in " y el valor correspondiente en la columna 'Country'. 

	 • Solo ‘Country’ nulo
    - Hago un diccionario que pueda utilizar para después utilizar el row de la columna 'Area', y sustituirlo por un index.

	 • Solo ‘Area’ nula
    - Utilizo la función df.loc[ ] para seleccionar un subconjunto de filas y columnas de un DataFrame.
    - Reemplazar los valores nulos de la columna 'Area' por "Area in 'Country'”.

	 • Solo ‘Location’ nula
    - Utilizo la función df.loc[ ] para seleccionar un subconjunto de filas y columnas de un DataFrame.
    - Reemplazar los valores nulos de la columna 'Location' por "Somewhere located in 'Area', 'Country'".

	 • Columnas ‘Country’ y 'Area' nulas
    - Hago un diccionario que pueda utilizar para después utilizar el row de la columna ‘Location’, y sustituirlo por index en cada una de las otras dos columnas. 

	 • Columnas ‘Country’ y ‘Location’ nulas
    - Igual que el anterior, pero con la columna ‘Area’ no nula. 


•	Columna ‘Time’:
  - No conocemos la hora de aquellas que son nulas, por lo que pondremos ‘not available’.

•	Columna ‘Type’:
  - No conocemos el tipo de ataque, por lo que pondremos ‘unknown’ en los 4 valores nulos encontrados.

•	Columnas ‘Injury’ y ‘Fatal (Y/N)’ nulos:
  - Las filas en las que las 2 columnas tienen nulos, las elimino.

	 • Solo ‘Fatal (Y/N)’ nula
    - Observo si en la columna de 'Fatal (Y/N)' están los valores que realmente interesan: ‘Y’ o ‘N’. Me quedo con estos y las demás filas serán eliminadas.


•	Columna ‘Species’ nulos: 
  - Tengo muchas filas que tienen valores nulos porque están vacías. En estas quiero poner un mensaje que pongit ga "unidentified shark".

•	Columna ‘Sex’ y ‘Name’ nulos:

	 • Solo ‘Sex’ nula
    - Reemplazo los valores nulos de la columna 'Sex ' por "unknown gender".
    - Reemplazo los valores 'lli y '.' por 'unknown gender'.
    - Reemplazo valores que parecen estar mal escritos por error: 'M '(con un espacio después) y 'N' por 'M'.
    
	 • Solo ‘Name’ nula
    - Utilizo la función df.loc[ ] para seleccionar un subconjunto de filas y columnas. Se reemplazan los valores nulos en la columna 'Name' por "Name of an unknown' + df['Sex ']".
    
•	Columna ‘Activity’ nulos
  - Mi propósito será unificar aquellos en los que proceda.
  - En cuanto a la limpieza de nulos, reemplazar los que son nulos con: 'unknown activity'.

•	Columna ‘Investigator or Source’ nulos
  - Se cambian nulos por ‘unknown’. No se verifican los valores uno por uno por la gran cantidad de valores únicos.

•	Columnas 'Unnamed: 22' y 'Unnamed: 23' nulos
  - Se eliminan ambas columnas, ya que no se sabe qué representan y todos sus valores son nulos.


## 2.	LIMPIEZA DE VALORES (no nulos)


•	Columna ‘Activity’
  - Mi propósito será unificar aquellos en los que proceda.
  - En la columna de 'Activity' utilizo un np.where como condicional, en el que verifique varias condiciones.
  - En caso de False, rellena con el valor correspondiente de df ['Activity'].
  - Algunas de las que van apareciendo ya no tienen sentido como actividad.

          Como hice con 'Country', 'Area', y 'Location', voy a crear un diccionario con las actividades que he creado. Esto me va a servir para que aquellas tienen una actividad que no he calificado como tal, también los nombraré también como 'unknown activity'.
          Obtendré las filas de la columna 'Activity' calificadas con una actividad.

- Además, se está utilizando la librería Seaborn para crear un gráfico de barras que muestra el número de ataques por cada actividad en la columna 'Activity'. La función countplot() de Seaborn se utiliza para crear el gráfico de barras.

•	Columna ‘Species’
  - Uso de la librería ‘fuzzywuzzy’.
  - Crear una función que utiliza la función process de FuzzyWuzzy para encontrar la cadena de texto más similar a una cadena de texto dada en la lista de cadenas de texto únicas.
  - Se crea una lista de especies únicas y se busca la especie que más se asemeje en cada fila de la columna. Si la especie de tiburón en una fila no coincide con ninguna de las especies de tiburones en la lista, se asigna el valor 'unidentified shark species' a esa fila.

•	Columnas 'Case Number.1' y 'Case Number.2'
  - Reviso si las columnas 'Case Number.1' y 'Case Number.2' son iguales. 
  - Se comparan, entre ellas y con la columna 'Case Number'.
  - La columna 'Case Number.2' es más parecida a la columna 'Case Number'.
  - Limpiar de nulos la columna 'Case Number', reemplazando por el valor de la misma fila de 'Case Number.2'.
  - Se igualan ambas columnas.

•	Columna ‘Time’
  - Se convierten los valores con formato 15h00 a formato 15:00
  - Aquellas que estén puestas con texto, las cambiaré por "Not specified time". Así, los valores en los que hubiera una 'h' no importarían más que en los numéricos.
  - La expresión regular [a-zA-Z] se utiliza en el método str.contains ( ) para seleccionar los valores que contienen letras en la columna 'Time' -> 'Not specified time'
  - Entonces creo un diccionario con los valores restantes que siguen sin el formato adecuado y los sustituyo manualmente.

•	Columna ‘original order’
  -  Se elimina el '.0' que aparece al final de los valores de la columna 'original order'.


## Bonus
Como BONUS he hecho una breve investigación sobre si los ataques se provocan más a hombres o mujeres. Para ello, se puede utilizar la función value_counts() de Pandas.

Para visualizar los resultados de la proporción de ataques a hombres y mujeres de manera más atractiva, he usado la función plot(), para crear un gráfico de pastel, con el parámetro kind='pie' y formato '%1.1f%%' para mostrar los porcentajes con un solo decimal.


## Archivos 

SHARKS-main.ipynb, con el código a ejecutar.

SHARKS-draft.ipynb, con pruebas y trozos de código que he ido haciendo.


## Recursos
https://pypi.org/project/fuzzywuzzy/ 

https://pandas.pydata.org/docs/index.html

