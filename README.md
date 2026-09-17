# CasoEstudio1BI

CONTEXTO
Contexto de las Estructuras Actuales
1. Estructuras Relacionales (de Estructura.txt)

El sistema actual se compone de 4 fuentes de datos relacionales, que en total suman 28 tablas después de un proceso de reducción. Estas fuentes están claramente orientadas a un contexto financiero y crediticio:

Fuente 1 (MySQL): Es el núcleo del sistema. Contiene 15 tablas que modelan el ciclo de vida completo de un préstamo: catálogos (tipos de identificación, producto, garantía), entidades (cliente, producto, agencia) y transacciones (solicitud, préstamo, pago, garantía).

Fuente 2 (PostgreSQL): Se enfoca en el riesgo legal y penal. Con 5 tablas, modela procesos penales, tipos de delitos y un consolidado de antecedentes por persona.

Fuente 3 (SQL Server): Se centra en el riesgo crediticio externo. Con 4 tablas, gestiona información de deudores morosos, sus deudas en diferentes entidades financieras y un score de riesgo asociado.

Fuente 4 (MariaDB): Aborda el riesgo comercial y empresarial. Con 4 tablas, registra empresas, sus accionistas, representantes y su situación tributaria.

En resumen, las fuentes relacionales cubren el ciclo de vida del crédito interno, el riesgo legal, el riesgo crediticio externo y el riesgo comercial.

2. Estructuras No Relacionales (de FuenteWeb.txt y open_finance.external_products.json)

FuenteWeb.txt (Dataset UCI): Es un archivo de texto que describe un repositorio de Machine Learning. Contiene un script de Python para extraer un dataset sobre la predicción de incumplimiento de pago de clientes de tarjetas de crédito. Es una fuente de datos estructurada (tabular) pero no relacional, orientada al modelado predictivo de riesgo.

open_finance.external_products.json: Es un archivo JSON con datos de productos financieros externos (préstamos, hipotecas) de diferentes bancos. Cada objeto JSON representa un producto con detalles como banco, tipo, monto, tasa de interés, cliente asociado, etc. Es una fuente de datos semiestructurada, orientada a la visión 360 del cliente desde el sistema financiero abierto (Open Finance).


PROMPT A REALIZAR
rompt para Análisis y Diseño de Arquitectura de Datos
Rol y Objetivo
Actúa como un Arquitecto de Datos Senior especializado en soluciones analíticas end-to-end (Data Lake → Data Mart → Data Warehouse) con experiencia en integración de fuentes relacionales y no relacionales, modelado dimensional, y habilitación de casos de uso avanzados de Machine Learning, Data Mining, Procesamiento de Lenguaje Natural, Econometría y Series de Tiempo, con consumo final en Power BI.

Contexto del Caso
Se dispone de un ecosistema de datos compuesto por fuentes relacionales y no relacionales.

Las fuentes relacionales suman 28 tablas después de un proceso de reducción. La Fuente 1, en MySQL, es el núcleo del sistema con 15 tablas que modelan el ciclo de vida completo de un préstamo, incluyendo catálogos como tipos de identificación, producto y garantía, entidades como cliente, producto y agencia, y transacciones como solicitud, préstamo, pago y garantía. La Fuente 2, en PostgreSQL, se enfoca en el riesgo legal y penal con 5 tablas que modelan procesos penales, tipos de delitos y un consolidado de antecedentes por persona. La Fuente 3, en SQL Server, se centra en el riesgo crediticio externo con 4 tablas que gestionan información de deudores morosos, sus deudas en diferentes entidades financieras y un score de riesgo asociado. La Fuente 4, en MariaDB, aborda el riesgo comercial y empresarial con 4 tablas que registran empresas, sus accionistas, representantes y su situación tributaria.

Las fuentes no relacionales incluyen el archivo FuenteWeb.txt, que corresponde a un dataset UCI descrito como un repositorio de Machine Learning con un script de Python para extraer datos sobre predicción de incumplimiento de pago de clientes de tarjetas de crédito, siendo una fuente estructurada tabular pero no relacional orientada al modelado predictivo de riesgo. También se incluye el archivo open_finance.external_products.json, un JSON con datos de productos financieros externos como préstamos e hipotecas de diferentes bancos, donde cada objeto representa un producto con detalles como banco, tipo, monto, tasa de interés y cliente asociado, siendo una fuente semiestructurada orientada a la visión 360 del cliente desde el sistema financiero abierto.

Tareas a Realizar
1. Análisis de Datos y Estructuras
Realiza un inventario detallado de las 28 tablas relacionales, clasificándolas por tipo, ya sea catálogo, entidad, transacción o consolidado. Determina su granularidad, es decir, el nivel de detalle que manejan, ya sea cliente, préstamo, pago, evento legal o empresa. Identifica llaves primarias, foráneas y las relaciones existentes entre fuentes, incluyendo posibles joins cross-source. Señala los atributos clave para analítica, como fechas, montos, tasas, scores y categorías.

Para las fuentes no relacionales, analiza el esquema implícito del CSV de UCI, incluyendo columnas, tipos, valores nulos y distribución. Examina la estructura jerárquica y anidada del JSON de Open Finance, identificando claves, arrays y objetos.

Identifica problemas de calidad como duplicados, nulos, inconsistencias de formato, claves huérfanas y heterogeneidad de identificadores de cliente.

2. Diseño de la Arquitectura Medallion (Lake → Mart → DW)
2.1 Data Lake (Zona Bronze / Raw)
Define la ingesta de cada fuente. Para las relacionales, propón extracción batch o CDC, ya sea full o incremental, hacia formatos como Parquet o Avro. Para el CSV de UCI, plantea una ingesta directa como archivo estructurado. Para el JSON de Open Finance, define una ingesta semiestructurada que mantenga el anidamiento o lo aplane según convenga.

Propón particionamiento por fecha, fuente o dominio, y un catálogo de metadatos. Define políticas de retención, versionado y linaje.

2.2 Data Lake (Zona Silver / Cleaned & Conformed)
Establece reglas de limpieza, normalización y estandarización. Esto incluye la unificación de identificadores de cliente entre las cuatro fuentes relacionales y las dos no relacionales, la homologación de catálogos como tipos de identificación, productos, monedas y fechas, y el tratamiento de nulos, outliers y duplicados.

Realiza la integración cross-source creando una dimensión maestra de cliente que unifique los datos del ciclo de crédito interno, el riesgo legal, el riesgo crediticio externo, el riesgo comercial, los productos externos del JSON de Open Finance y las variables del dataset UCI como fuente de features para Machine Learning.

Genera tablas conformadas listas para modelado dimensional.

2.3 Data Marts (Zona Gold / Business-Oriented)
Diseña data marts temáticos según los casos de uso analíticos. Un data mart de crédito orientado al análisis del ciclo de préstamo, morosidad y pagos, habilitando series de tiempo, econometría y consumo en Power BI. Un data mart de riesgo que consolide riesgo legal, crediticio y comercial, habilitando scoring, Machine Learning y Data Mining. Un data mart de cliente 360 con visión unificada del cliente interno y de Open Finance, habilitando segmentación y Procesamiento de Lenguaje Natural en quejas u observaciones. Un data mart de cumplimiento con antecedentes penales, situación tributaria y listas restrictivas, habilitando alertas y PLN en textos legales. Un data mart de productos externos con comparativa de productos financieros del mercado, habilitando benchmarking y series de tiempo de tasas.

Para cada mart, define el modelo dimensional en estrella o copo de nieve, especificando tablas de hechos con su grain, medidas y llaves foráneas, y dimensiones como tiempo, cliente, producto, agencia, riesgo y empresa. Especifica métricas clave como tasa de morosidad, score promedio, monto desembolsado, ratio de endeudamiento y frecuencia de delitos.

2.4 Data Warehouse (Zona Gold / Integrado)
Integra los data marts en un Data Warehouse corporativo con modelo dimensional unificado. Define una capa semántica con vistas, cubos y métricas calculadas para Power BI. Propón una estrategia de actualización que contemple batch nocturno, near-real-time o streaming para eventos legales.

3. Habilitación de Casos de Uso Avanzados
3.1 Machine Learning
Identifica features desde el Data Warehouse para modelos de predicción de default basados en el dataset UCI y datos internos, scoring crediticio alternativo usando Open Finance y riesgo comercial, y detección de fraude combinando transacciones y riesgo legal. Define pipelines de feature engineering y un feature store.

3.2 Data Mining
Propón técnicas de segmentación, clustering y reglas de asociación sobre perfiles de clientes del Customer 360, patrones de morosidad y pagos, y relaciones entre empresa, accionista y representante.

3.3 Procesamiento de Lenguaje Natural
Identifica campos de texto libre como observaciones legales, descripciones de delitos y nombres de empresas. Propón tareas de PLN como extracción de entidades en textos legales, clasificación de documentos penales y análisis de sentimiento en observaciones de clientes. Define el almacenamiento de embeddings y su integración en el Data Warehouse.

3.4 Econometría y Series de Tiempo
Identifica series temporales disponibles como desembolsos, pagos y morosidad por mes, tasas de interés de productos externos, y eventos legales por período. Propón modelos como ARIMA o SARIMA para morosidad, VAR para relación entre variables macro y crédito, y regresiones con datos panel a nivel cliente-período. Define tablas de hechos temporales y agregaciones para Power BI.

4. Modelado para Power BI
Diseña el modelo tabular en esquema estrella óptimo para Power BI, especificando relaciones, cardinalidad y direccionalidad de filtros. Define medidas DAX clave como YTD, MoM, ratios y scoring. Propón jerarquías de tiempo, geografía, producto y riesgo. Plantea vistas materializadas o tablas agregadas para rendimiento. Define seguridad a nivel de fila por agencia o región. Especifica orígenes de datos, ya sea DirectQuery o Import, según volumen y frescura.

5. Entregables Esperados
Entrega un diagrama de arquitectura Lake → Mart → DW en formato textual o Mermaid. Un inventario de fuentes con esquemas, granularidad y relaciones. Un modelo dimensional por data mart con tablas de hechos y dimensiones. Un diccionario de datos consolidado con métricas y reglas de negocio. Un pipeline de integración ETL o ELT paso a paso. Un plan de habilitación analítica para Machine Learning, Data Mining, PLN, econometría y series de tiempo. Una especificación del modelo Power BI con tablas, relaciones, medidas y seguridad a nivel de fila. Y un roadmap de implementación por fases, distinguiendo quick wins de largo plazo.
