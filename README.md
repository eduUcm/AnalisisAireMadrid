# Análisis Aire Madrid

## Descripción

En el marco de trabajo de la asignatura de [Cloud Computing and Big Data de la UCM](http://www.fdi.ucm.es/Pub/ImpresoFichaDocente.aspx?Id=1312) se ha realizado un análisis detallado del NO<sub>2</sub> del aire de la ciudad de Madrid, compuesto que utiliza el Ayuntamiento para poner en marcha los periodos de restricciones por alta contaminación, comprobando el resultado de dichos periodos a lo largo del año.

Se han utilizado los datos proporcionados por el Ayuntamiento atraves de su [portal de datos abiertos](http://datos.madrid.es/portal/site/egob/) de los contaminantes recogidos por las distintas estaciones repartidas por el municipio. Se ha utilizado Apache Spark para el análisis de datos, utilizando una máquina virtual en Amazon AWS con el servicio EC2 para su procesado y un Bucket S3 para su almacenamiento.

El proyecto tiene una [pequeña web](https://hunzagit.github.io/AnalisisAireMadrid/) para explicar el proceso y los resultados y un [mapa interactivo](https://hunzagit.github.io/AnalisisAireMadrid/#MapaInteractivo) para mostrar los niveles de NO<sub>2</sub> de las estaciones en tiempo real.

## Entorno

 - **Python** 2.7.12
 - **Apache Spark** 2.2.0
 - **AWS EC2** t2.nano
 - **Ubuntu** 16.04 

## Tecnologías

<img height="500" align="center" src="public/resources/images/Herramientas/Infografia.png">

## Scripts

### Script 1 - [Media Anual por Dias](https://github.com/hunzaGit/AnalisisAireMadrid/tree/master/Scripts/1%20-%20Media%20anual%20por%20dias)

Este script coge los datos diarios del año 2017 de todas las estaciones, y quedándose solo con aquellas filas que indican los valores de NO<sub>2</sub>, calcula los valores medios de NO<sub>2</sub> del Ayuntamiento de Madrid por días.

Genera un archivo con el valor medio de NO<sub>2</sub> por cada día, formado por tantas filas como días con el siguiente formato:
   
    "fecha; valormedio".

### Script 2 - [Media Anual por Zonas](https://github.com/hunzaGit/AnalisisAireMadrid/tree/master/Scripts/2%20-%20Media%20anual%20por%20zonas)

Estre script coge los datos diarios del año 2017 de todas las estaciones, quedandose solo con aquellas filas que indican los valores de NO2. Se encarga de calcular la media diaria de NO2 de cada zona de las que define la Comunidad de Madrid.

Genera un archivo con el valor medio de NO<sub>2</sub> por cada zona y día, formado por tantas filas cono días con el siguiente formato: 

    "fecha;valorZona1;valorZona2;valorZona3;valorZona4;valorZona5".

### Script 3 - [Cantidad diaria por estacion](https://github.com/hunzaGit/AnalisisAireMadrid/tree/master/Scripts/3%20-%20Cantidad%20diaria%20por%20estacion)

Este script coge los datos diarios del año 2017 de todas las estaciones, quedandose solo con aquellas filas que indican los valores de NO<sub>2</sub>. 

Genera un archivo con el valor medio de NO<sub>2</sub> por cada estación y día, formado por tantas filas cono días con el siguiente formato: 

    "fecha;estaciones(004;008;011;016;017;018;024;027;035;036;038;039;040;047;048;049;050;054;055;056;057;058;059;060)"

### Script 4 - [Cantidad diaria por zonas](https://github.com/hunzaGit/AnalisisAireMadrid/tree/master/Scripts/4%20-%20Cantidad%20diaria%20por%20zonas)

Este script coge los datos diarios del año 2017 de todas las estaciones, quedandose solo con aquellas filas que indican los valores de NO<sub>2</sub>. 

Genera 5 archivos con los distintos valores de NO2 de cada estación por cada zona que delimita el Ayuntamiento de Madrid. Teniendo cada archivo el siguiente formato:

    - Zona 1: "fecha;estaciones(004;008;011;035;038;039;047;048;049;050)"
    - Zona 2: "fecha;estaciones(036;040;054)"
    - Zona 3: "fecha;estaciones(016;027;055;057;059;060)"
    - Zona 4: "fecha;estaciones(024;058)"
    - Zona 5: "fecha;estaciones(017;018;056)"

### Script 5 - [Estaciones por zonas propuestas](https://github.com/hunzaGit/AnalisisAireMadrid/tree/master/Scripts/5%20-%20Estaciones%20por%20zonas%20propuestas)

Este script coge los datos diarios del año 2017 de todas las estaciones, quedandose solo con aquellas filas que indican los valores de NO<sub>2</sub>. 

Genera 6 archivos con los distintos valores de NO<sub>2</sub> de cada estación por cada zona nueva que nosotros hemos delimitado. Teniendo cada archivo el siguiente formato:
    
    - Zona 1: "fecha;estaciones(004;035;038;048)"
    - Zona 2: "fecha;estaciones(011;039;050)"
    - Zona 3: "fecha;estaciones(008;047;049;056)"
    - Zona 4: "fecha;estaciones(016;027;055;057;059)"
    - Zona 5: "fecha;estaciones(017;036;040;054)"
    - Zona 6: "fecha;estaciones(018;024;058;060)"

### Script 6 - [Media Anual por Zonas propuestas](https://github.com/hunzaGit/AnalisisAireMadrid/tree/master/Scripts/6%20-%20Media%20anual%20por%20zonas%20propuestas)

Este script coge los datos diarios del año 2017 de todas las estaciones, quedandose solo con aquellas filas que indican los valores de NO<sub>2</sub>. Se encarga de calcular la media diaria de NO<sub>2</sub> de cada una de las nuevas zonas que hemos delimitado.

Genera un archivo con el valor medio de NO<sub>2</sub> por cada zona y día, formado por tantas filas como días con el siguiente formato: "fecha;zona1;zona2;zona3;zona4;zona5;zona6".



### Script 7 - [Contaminacion en tiempo real](https://github.com/hunzaGit/AnalisisAireMadrid/tree/master/Scripts/7%20-%20Contaminacion%20en%20tiempo%20real)

En este script se utiliza el fichero de datos con los datos de contaminación del día actual (que se actualiza cada hora entre los minutos 20 y 30).

Lo que hace el script es coger el último valor válido (el de la hora actual indicado con una 'V') del NO<sub>2</sub> y crear un JSON con el id de la estación, el valor tomado y la hora de actualización:

```javascript
    {
        "28079004": {
            "valor": "00049", 
            "hora": "15"
        },
        "28079008": {
            "valor": "00056", 
            "hora": "15"
        },
        ...
    }
```

Además se complementa con otro script en **Python** que se encarga de subir el JSON y la hora de actualización a un **Bucket de S3** de **Amazon Web Services**.

Para poder simular el funcionamiento de Spark Streaming se usa un script de Shell que programado con el **crontab** de **Ubuntu** ejecuta estos dos scripts cada hora.


## Autores
   - Rodrigo de Miguel - [Github](https://github.com/hunzaGit) - [Linkedin](https://www.linkedin.com/in/rodrigo-de-miguel-gonzalez/)
   - Cesar Godino - [GitHub](https://github.com/cloudgrey)
   - Carmen López - [Github](https://github.com/calope03) - [Linkedin](https://www.linkedin.com/in/carmen-l%C3%B3pez-gonzalo/)


## Agradecimietos
   - Tomás Muñoz Testón - [Github](https://github.com/tomas-teston) - [Linkedin](https://www.linkedin.com/in/tom%C3%A1s-m-58597672/)
