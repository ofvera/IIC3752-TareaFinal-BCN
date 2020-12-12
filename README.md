# IIC3752 - Tarea Final: Visualización Datos BCN

## Descripción

La solución diseñada corresponde a la Tarea Final para el curso IIC3752 Tecnologias para Inteligencia de Negocios. Esta busca resolver la necesidad de la Biblioteca del Congreso Nacional de desarrollar un sistema de gestión institucional que le permita visualizar información generada en distintos sectores. A diferencia del proyecto final entregado, la tarea final es una solución de Inteligencia de Negocios realizada completamente mendiante el uso de herramientas de Microsoft. En particular el uso de Azure, tanto por su capacidad de almacenamiento como por su herramienta para ocupar un Data Factory, y luego el uso de Power BI como herramienta de analisis y visualización de datos.

## Solución Implementada

Para cumplir con los requerimientos de esta tarea, se utilizó la plataforma Azure, donde se crearon Data Factories para procesar los archivos csv que extraen información desde la página del fogape, se procesan y cargan en una Base de Datos SQL Server que creamos para consolidar nuestros datos en Azure. Ademas se extrajeron los datos detallados de de servicios de infromación realizados a parlamentarios, por los asesores de la BCN. De esta forma consolidamos un datawarehouse en Azure con todos los datos recuperados. Finalmente realizamos visualizaciones utilizando PowerBI, de las cuales se inscruyen print screen en el directorio imagenes de este repositorio 

## Uso de la plataforma Azure

Para el uso de la plataforma Azure es necesario que se le otorge permiso al usuario para acceder a los recursos creados. Para esta entrega utilizamos la cuenta de Azure de uno de los integrantes del grupo, y resulta simple transferir la propiedad de los recursos a otro usuario administrador para la Biblioteca del Congreso Nacional si es deseado. Luego, se podrá acceder al Data Factory y Base de Datos y StorageBlob creados.

Una vez establecido el uso de la plataforma Azure, es posible acceder las visualizaciones y los analisis realizados en Power BI Desktop mediante una cuenta y los archivos encontrados en la carpeta `/powerbi_views`. Es posible que se pida reingresar los datos de autenticacion para acceder a la base de datos.

## Proceso de Trabajo

Para empezar con la recolección de datos, utilizamos los csv obtenidos desde el crawler entregado para el proyecto final. Por lo tanto, para efectos de esta entrega, utilizaremos los csv obtenidos y asumiremos que tenemos como obtener sus actualizaciones desde los crawlers ya creados. Estos csv se pueden encontrar en la carpeta `/csvs` y corresponden a los datos extraidos del fogape.

Estos archivos deben almacenarse en alguna cuenta de almacenamiento, por lo que creamos un recurso de Blob Storage, donde eventualmente podriamos automatizar la extracción del crawler. Sin embargo, por ahora simplemente es un espacio para almacenar los archivos csv obtenidos.

![Blob Storage](images/BlobStorage.png)

Por otro lado, necesitamos crear el recurso del Servidor de Azure Database for PostgreSQL. Acá se ubicará nuestra Base de Datos y resulta importante definir los permisos necesarios para que se pueda acceder desde distintas herramientas.

![PSQL Server](images/ServidorPSQL.png)

Luego, procedimos a crear un recurso de Data Factory para gestionar la integración de los datos extraidos. Para esto fue necesario crear 4 pipelines, correspondientes a cada tabla creada en la base de datos creada. Cada pipeline requiere de Datasets que le permitan ubicar los archivos csv y ubicar las tablas donde estos deben ser insertados. Luego, cada pipeline se encarga de monitorear el proceso de integración, con la finalidad de estandarizar, uniformar y automatizar la forma en que los datos son recolectados.

![Data Factory: Pipelines](images/DataFactoryPipelines.png)

Además, aprovechamos de crear un trigger para las pipelines. Esto automatiza parte del proceso de integración, ya que cada semana buscara los archivos csv del Blob Storage y integrara los datos en la Base de Datos deseada mientras se monitorea el proceso.

![Data Factory: Trigger](images/DataFactoryTriggers.png)

