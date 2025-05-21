# Renta de inmuebles en la Ciudad de México
![Static Badge](https://img.shields.io/badge/Oracle_Next_Education-blue?style=plastic&logo=python&logoColor=green&logoSize=auto&label=Alura%20Latam)
![Static Badge](https://img.shields.io/badge/Proyecto-blue?style=plastic&logo=pandas&logoColor=green&logoSize=auto&label=Pandas%20ETL)

## Descripción de la actividad:
La finalidad de la actividad es dar los primeros pasos en el análisis y exploración de datos mediante la librería Pandas.

### Objetivo general del proyecto: Brindar soporte a las demandas del equipo del equipo de Aprendizaje Automático y del equipo de Desarrollo de la empresa.

### Objetivos particulares:

#### 🤖Demandas del equipo de Machine Learning
- 🎯Importar la base de datos 
- 🎯Análisis exploratorio de los datos
- 🎯Tratamiento de valores nulos
- 🎯Remover registros inconsistentes 
- 🎯Aplicar filtros 
- 🎯Guardar los datos

#### 👨‍💻Demandas del equipo de Desarrollo
 - 🎯Crear columnas numéricas
 - 🎯Crear columnas categóricas

## Información relevante

### Exploración inicial
☑️ Características **Data set**: 
- Filas = 25,121
- Columnas = 9 

☑️Conociendo las primeras cinco filas:
|index|Tipo|Colonia|Habitaciones|Garages|Suites|Area|Valor|Condominio|Impuesto|Valor\_mensual|Valor\_anual|Descripcion|Tiene\_suite|
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|0|Cocineta|Condesa|1|0|0|40|5950\.0|1750\.0|210\.0|7700\.0|92610\.0|Cocineta en la colonia Condesa con 1 cuarto\(s\) y 0 plazas de estacionamiento\.|No|
|1|Casa|Polanco|2|0|1|100|24500\.0|NaN|NaN|NaN|NaN|Casa en la colonia Polanco con 2 cuarto\(s\) y 0 plazas de estacionamiento\.|Sí|
|2|Conjunto Comercial/Sala|Santa Fe|0|4|0|150|18200\.0|14070\.0|3888\.5|32270\.0|391128\.5|Conjunto Comercial/Sala en la colonia Santa Fe con 0 cuarto\(s\) y 4 plazas de estacionamiento\.|No|
|3|Departamento|Centro Histórico|1|0|0|15|2800\.0|1365\.0|70\.0|4165\.0|50050\.0|Departamento en la colonia Centro Histórico con 1 cuarto\(s\) y 0 plazas de estacionamiento\.|No|
|4|Departamento|Del Valle|1|0|0|48|2800\.0|805\.0|NaN|3605\.0|NaN|Departamento en la colonia Del Valle con 1 cuarto\(s\) y 0 plazas de estacionamiento\.|No|

### Análisis Exploratorio de los Datos
- 📊 Valores promedio del alquiler por tipo de inmueble
![Image](https://github.com/user-attachments/assets/50faaec5-63db-4b5c-ab80-d5737bb41a5a)

Dado que los datos de nuestro interés tienen que ver con el valor promedio del tipo vivienda, retiramos los inmuebles comerciales. Al llevar a cabo esta limpieza de datos obtuvimos el siguiente gráfico.

![Image](https://github.com/user-attachments/assets/755cc011-7e08-4197-9655-671cefeb0ac5)

- 📊 Porcentaje de participación por inmueble del tipo vivienda

Continuando con la exploración de datos, se observan 10 tipos distintos de viviendas en la Ciudad de México; los cuales se encuentran distribuidos con la siguiente ponderación:
|Tipo|proportion|
|---|---|
|Departamento|0\.85|
|Cocineta|0\.04|
|Casa de Condominio|0\.04|
|Casa|0\.03|
|Departamento en Hotel|0\.03|
|Casa de Vecindad|0\.01|
|Loft|0\.0|
|Rancho|0\.0|
|Estudio|0\.0|
|Posada/Chalé|0\.0|

El siguiente gráfico, es la visualización de la ponderación expresada en la tabla anterior.

![Image](https://github.com/user-attachments/assets/b8d177c3-f74b-45f5-ac53-4ab0f2068da3)
De acuerdo a lo obtenido, el equipo ML recomendó que en el ejercicio solamente se ocuparan los campos que fueran departamentos, ya que, la participación de este tipo de vivienda es la más representativa de la muestra.

Para ello implementamos la siguiente consulta

    df_deptos = df_tipo_viviendas.query('Tipo == "Departamento"')

### Tratamiento de valores nulos
Para conocer el total de valores nulos que se encuentran en nuestro dataframe se aplicó la siguiente operación:

    df_deptos.isnull().sum().to_frame()
Lo anterior nos dio como resultado:
|index|0|
|---|---|
|Tipo|0|
|Colonia|0|
|Habitaciones|0|
|Garages|0|
|Suites|0|
|Area|0|
|Valor|7|
|Condominio|493|
|Impuesto|3797|

Es decir que se retiraron 4,297 datos nulos del dataframe.

### Tratamiento de registros inconsistentes
Durante el análisis de datos se observó la inconsistencia en algunos valores. En el caso del "Valor" de los departamentos, se detectó que había valores iguales a cero. Por otra parte, se dio el mismo caso en los registros de la columna "Condominios".

Por lo anterior, se retiraron dichas inconsistencias mediante los siguientes scripts:

    df_deptos_snull.query('Valor == 0 | Condominio == 0').index
    
    df_remover = df_deptos_snull.query('Valor == 0 | Condominio == 0').index
    
    df_deptos_snull.drop(df_remover,axis=0,inplace=True, errors='ignore')


### Demanda del equipo de Machine Learning: Filtrado de datos
☑️Apartamentos que tienen al menos 2 dormitorios, un alquiler menor a MXN 10500 y una superficie mayor a 70 m².
    filtro2 = (df_deptos_snull['Habitaciones'] >= 2) & (df_deptos_snull['Valor'] < 10500) & (df_deptos_snull['Area'] > 70)
|index|Colonia|Habitaciones|Garages|Suites|Area|Valor|Condominio|Impuesto|
|---|---|---|---|---|---|---|---|---|
|14|Narvarte|2|1|0|110|6650\.0|2450\.0|483\.0|
|16|Narvarte|2|1|0|78|7000\.0|2450\.0|0\.0|
|21|Roma|2|1|0|76|8750\.0|2590\.0|0\.0|
|33|Santa Fe|3|1|1|72|8225\.0|2100\.0|245\.0|
|58|Santa Fe|3|2|1|104|9100\.0|4774\.0|1421\.0|

### Demanda del equipo DEV
☑️Crear dos nuevas columnas:
- Descripción -> Esta columna debe contener un resumen de las características de los departamentos a partir de la información expresada en las columnas del data frame.
- Tiene_Suite -> Esta nueva columna debe identificar mediante una función condicional si el departamento tiene o no tiene suite (cuarto con baño).

|index|Descripcion|Tiene\_suite|
|---|---|---|
|0|Cocineta en la colonia Condesa con 1 cuarto\(s\) y 0 plazas de estacionamiento\.|No|
|1|Casa en la colonia Polanco con 2 cuarto\(s\) y 0 plazas de estacionamiento\.|Sí|
|2|Conjunto Comercial/Sala en la colonia Santa Fe con 0 cuarto\(s\) y 4 plazas de estacionamiento\.|No|
|3|Departamento en la colonia Centro Histórico con 1 cuarto\(s\) y 0 plazas de estacionamiento\.|No|
|4|Departamento en la colonia Del Valle con 1 cuarto\(s\) y 0 plazas de estacionamiento\.|No|



