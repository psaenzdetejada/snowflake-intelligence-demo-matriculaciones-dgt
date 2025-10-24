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
A continuación, será necesario [crear Semantic Views](https://docs.snowflake.com/en/user-guide/views-semantic/overview) para poder realizar preguntas en lenguaje natural a los datos estructurados del CRM y matriculaciones de la DGT. Podemos crear estas vistas semánticas de dos maneras:

- A través de la [UI de Snowsight](https://docs.snowflake.com/en/user-guide/views-semantic/ui#label-views-semantic-ui-create-wizard)
- A través de los [ficheros YAML](https://docs.snowflake.com/en/user-guide/views-semantic/ui#label-views-semantic-ui-create-upload) disponibles en este repositorio de Github

Se recomienda utilizar los ficheros YAML si se quiere ahorrar tiempo de crear relaciones entre tablas, añadir sinónimos, etc. Pero siempre se pueden completar posteriormente las Semantic Views si fuese necesario.

## Paso 4: Crear el agente
A continuación crearemos el Agente, y añadiremos al agente todas las tools o herramientas que queramos que tenga a su disposición. En nuestro caso, podemos incluir los dos servicios de Cortex Search creados con el Notebook de los pasos 1 y 2, y las dos Semantic Views creadas en el paso 3. Es importante añadir cuanto más detalle posible al Agente, y entender los [conceptos que forman parte de los Agentes de Snowflake](https://docs.snowflake.com/en/user-guide/snowflake-cortex/cortex-agents#cortex-agent-concepts) por ejemplo en la [orquestación](https://docs.snowflake.com/en/user-guide/snowflake-cortex/cortex-agents-manage#specify-orchestration) para especificar cuándo tiene que usar cada una de las tools que le facilitamos. Por ejemplo:

- Usa la semantic view X para responder preguntas sobre oportunidades y clientes.
- Usa la semantic view Y para responder preguntas relacionadas con matriculaciones de vehiculos y preguntas relacionadas con datos de la DGT y el mercado del automóvil.
- Usa cortex search Z para responder a preguntas técnicas relacionadas con el uso, mantenimiento o cualquier información técnica sobre un modelo de coche específico.

Para ver un ejemplo de cómo crear un Agente en Snowflake mediante la UI de Snowsight recomiendo revisar [este quickstart](https://quickstarts.snowflake.com/guide/getting_started_with_cortex_agents/index.html?index=../..index#2)

## Paso 5: Configurar y usar Snowflake Intelligence
Por último, si no tenemos Snowflake Intelligence configurado en nuestra cuenta de Snowflake, podemos hacerlo siguiendo [estos pasos](https://docs.snowflake.com/en/user-guide/snowflake-cortex/snowflake-intelligence#set-up-sf-intelligence)

Con todo ello, tendríamos ya un ejemplo práctico de buena parte de las capacidades de Snowflake Cortex AI: AISQL, Cortex Search, Cortex Analyst, Agents y Snowflake Intelligence.

La demo puede personalizarse todo lo necesario, pudiendo realizar modificaciones en las tablas para que las fechas en el CRM sean dinámicas por ejemplo y asegurarnos de que existan ciertas oportunidades para la fecha actual, o cualquier otro aspecto que necesitemos.
