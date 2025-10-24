# Demo de Snowflake Intelligence con datos reales de Matriculaciones de la Dirección General de Tráfico (DGT)
Demo de Snowflake Intelligence y Cortex AI con datos ficticios de un CRM y datos reales de matriculaciones de vehículos de la Dirección General de Tráfico (DGT) de España

## Paso 1: Pasos para usar la demo en una cuenta de Snowflake

1. [Importar el Python Notebook](https://docs.snowflake.com/en/user-guide/ui-snowsight/notebooks-create) "Demo Set Up.ipynb" en Snowflake.
2. Antes de ejecutar todo el notebook, recomiendo echar un vistazo a las primeras celdas, especialmente hasta la sección "Cargar datos desde S3". Ya que será importante también descargar unos manuales de vehículos y cargarlos en el esquema DEMO_VENTA_VEHICULOS.RAW y en el stage MANUALES que se crea en el paso anterior tal como se indica en la celda "fs_3".
3. Una vez se cargan los ficheros PDF en el STAGE, puede ejecutarse el resto del notebook seguido.

## Paso 2: Detalle de los procesos realizados en el Notebook 
El Notebook carga los [datos reales mensuales de la DGT de matriculaciones de vehículos](https://www.dgt.es/menusecundario/dgt-en-cifras/dgt-en-cifras-resultados/dgt-en-cifras-detalle/Microdatos-de-Matriculaciones-de-Vehiculos-mensual/) previamente subidos a un bucket de S3 para que sea más sencillo la carga en Snowflake. El Notebook también creará tablas adicionales que parseen y preparen tablas en distintas capas RAW | REFINED | CURATED hasta tener una tabla limpia y preparada DEMO_VENTA_VEHICULOS.CURATED.MATRICULACIONES_VEHICULOS

Posteriormente se añaden también una serie de tablas con datos ficticios de un CRM de ventas, con información creada a través de modelos LLM sobre clientes, oportunidades, vehiculos, tareas, etc.

El Notebook también contiene ejemplos sencillos de uso de [AISQL de Snowflake](https://docs.snowflake.com/en/user-guide/snowflake-cortex/aisql) y la creación de un servicio de [Cortex Search](https://docs.snowflake.com/en/user-guide/snowflake-cortex/cortex-search/cortex-search-overview) para realizar preguntas con lenguaje natural a varios ejemplos de manuales de vehículos descargados anteriormente tal como se indica en el Paso 1.

## Paso 3: Creación de Semantic Views
A continuación, será necesario [crear Semantic Views](https://docs.snowflake.com/en/user-guide/views-semantic/overview) para poder realizar preguntas en lenguaje natural a los datos estructurados del CRM y matriculaciones de la DGT. Podemos crear estas vistas semánticas de dos maneras, a través de la [UI de Snowsight](https://docs.snowflake.com/en/user-guide/views-semantic/ui#label-views-semantic-ui-create-wizard)
