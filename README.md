# Microsoft Fabric AI Functions

### From Fundamentals to Advanced Architecture & Engineering

[![Microsoft Fabric](https://img.shields.io/badge/Microsoft-Fabric-117865)](https://learn.microsoft.com/en-us/fabric/) [![PySpark](https://img.shields.io/badge/PySpark-AI%20Functions-E25A1C)](https://spark.apache.org/) [![Pandas](https://img.shields.io/badge/Pandas-AI%20Functions-150458)](https://pandas.pydata.org/) ![Notebooks](https://img.shields.io/badge/Notebooks-34-orange) [![License](https://img.shields.io/badge/License-Apache%202.0-blue)](LICENSE)

**34 notebooks · 2 processing engines · 17 technical topics**

Repositorio educativo y técnico para explorar, implementar y evaluar las capacidades nativas de inteligencia artificial de Microsoft Fabric mediante PySpark y Pandas. Desde las primeras inferencias hasta el diseño de arquitecturas de datos e IA con observabilidad, gobernanza, escalabilidad y FinOps.

> **English overview:** A hands-on educational repository covering Microsoft Fabric AI Functions with PySpark and Pandas, from native AI capabilities to advanced engineering, token accounting, capacity monitoring, centralized auditing, governance and custom endpoints. Notebook explanations are primarily in Spanish.

## Objetivos

- Integrar funciones de IA generativa y embeddings en procesos de ingeniería de datos.
- Comparar los patrones de ejecución de PySpark y Pandas.
- Analizar estadísticas, latencia, concurrencia, tokens y consumo estimado de capacidad.
- Diseñar mecanismos de auditoría, gobernanza y optimización de costos.
- Explorar la integración de endpoints personalizados de IA.

## Programa de aprendizaje

| Módulo                                            | PySpark | Pandas |  Total |
|---------------------------------------------------|--------:|-------:|-------:|
| Fundamentals (NB_01–NB_09)                        |       9 |      9 |     18 |
| Advanced Architecture & Engineering (NB_10–NB_17) |       8 |      8 |     16 |
| **Total**                                         |  **17** | **17** | **34** |

### Module 01 — Fundamentals

| Notebook | AI Function            | Aplicación                                       |
|----------|------------------------|--------------------------------------------------|
| NB_01    | `ai.classify`          | Clasificación de textos mediante categorías      |
| NB_02    | `ai.extract`           | Extracción de información estructurada           |
| NB_03    | `ai.analyze_sentiment` | Análisis de sentimientos                         |
| NB_04    | `ai.summarize`         | Resumen de textos                                |
| NB_05    | `ai.translate`         | Traducción automática                            |
| NB_06    | `ai.fix_grammar`       | Corrección gramatical                            |
| NB_07    | `ai.generate_response` | Generación de respuestas y salidas estructuradas |
| NB_08    | `ai.embed`             | Representaciones vectoriales                     |
| NB_09    | `ai.similarity`        | Comparación semántica                            |

### Module 02 — Advanced Architecture & Engineering

| Notebook | Tema                  | Aplicación                                        |
|----------|-----------------------|---------------------------------------------------|
| NB_10    | AI Functions Chaining | Encadenamiento y trazabilidad por etapa           |
| NB_11    | Concurrency           | Concurrencia y throughput                         |
| NB_12    | Benchmark             | Comparación experimental de rendimiento           |
| NB_13    | Token Consumption     | Optimización de entradas y consumo                |
| NB_14    | Centralized Audit     | Contrato de auditoría y persistencia Delta        |
| NB_15    | Capacity Monitoring   | Conciliación con métricas de capacidad            |
| NB_16    | Data Governance       | Minimización, enmascaramiento y controles         |
| NB_17    | Custom Endpoint       | Configuración de endpoints y facturación separada |

## Organización propuesta

``` text
Microsoft-Fabric-AI-Functions/
├── README.md
├── LICENSE
├── NOTICE
├── Fundamentals/
│   ├── PySpark/       # NB_01–NB_09
│   └── Pandas/        # NB_01–NB_09
└── Advanced/
    ├── PySpark/       # NB_10–NB_17
    └── Pandas/        # NB_10–NB_17
```

Las revisiones V2 de NB_03 y NB_04 sustituyen a las versiones originales en el itinerario principal; no se cuentan como temas adicionales. El notebook de referencia NB_AIFUNCTIONS, si se incluye, tampoco forma parte del total de 34 laboratorios.

## Arquitectura conceptual

``` mermaid
flowchart TD
    A[Datos de entrada] --> B[Validación y gobernanza]
    B --> C[PySpark]
    B --> D[Pandas]
    C --> E[AI Functions]
    D --> E
    E --> F[Endpoint integrado de Fabric]
    E --> G[Endpoint personalizado]
    F --> H[Resultados y estadísticas]
    G --> H
    H --> I[Auditoría Delta opcional]
    H --> J[Análisis de tokens y rendimiento]
    J --> K[Conciliación con Capacity Metrics]
```

## Requisitos y ejecución

1.  Disponer de un workspace con capacidad Microsoft Fabric compatible, permisos de ejecución y configuración de tenant requerida para AI Functions.
2.  Descargar o clonar el repositorio e importar el archivo `.ipynb` deseado en Microsoft Fabric.
3.  Seleccionar el runtime PySpark o Python correspondiente y revisar los requisitos de instalación de SynapseML indicados en el notebook y en Microsoft Learn.
4.  Revisar el modelo, la concurrencia y los datos de entrada antes de ejecutar las celdas.
5.  En los laboratorios avanzados, habilitar `RUN_AI` únicamente cuando se autorice el consumo. Habilitar `SAVE_DELTA` solo si se desea persistir la auditoría y se dispone de un Lakehouse y dependencias compatibles.
6.  Validar resultados y estadísticas. Para NB_15, incorporar las métricas observadas de la capacidad; para NB_17, configurar un endpoint autorizado y credenciales seguras.

> Los notebooks fundamentales pueden ejecutar inferencias al correr todas las celdas. Revisa cada laboratorio antes de ejecutarlo. Las llamadas al modelo pueden generar costos.

## Ejemplo de uso

**PySpark**

``` python
import synapse.ml.spark.aifunc as aifunc

aifunc.default_conf.set_deployment_name("gpt-5-mini")
df = spark.createDataFrame([(1, "No puedo acceder al portal")], ["id", "texto"])
resultado = df.ai.classify(
    input_col="texto",
    labels=["Acceso", "Otros"],
    output_col="categoria"
)
display(resultado)
```

**Pandas**

``` python
import pandas as pd
import synapse.ml.aifunc as aifunc

aifunc.default_conf.model_deployment_name = "gpt-5-mini"
df = pd.DataFrame({"texto": ["No puedo acceder al portal"]})
resultado = df["texto"].ai.classify(labels=["Acceso", "Otros"])
stats = resultado.ai.stats
df["categoria"] = resultado
display(df)
display(stats)
```

## Observabilidad, capacidad y FinOps

Los laboratorios exploran `ai.stats`, los tokens de entrada, caché, salida y razonamiento cuando están disponibles, el tiempo de ejecución y la estimación de CU-segundos. Los valores no reportados no deben interpretarse como cero.

Para GPT-5-mini, el cálculo ilustrativo distingue tokens de entrada no cacheados, cacheados y de salida. Para Ada 002 utiliza tokens de entrada. Las tarifas deben comprobarse en la documentación oficial antes de utilizar las estimaciones para presupuestos.

La estimación de inferencia no representa el costo total del notebook ni sustituye las métricas reales de capacidad. El cómputo, almacenamiento, smoothing y otras cargas deben evaluarse por separado. Los endpoints personalizados pueden generar cargos independientes del proveedor.

El módulo avanzado propone una ubicación de auditoría común: `Files/audit_ai_functions_advanced/`, con identificadores de ejecución y experimento, motor, etapa, modelo, estadísticas y consumo estimado.

## Seguridad y gobernanza

- Utilizar datos ficticios o autorizados y minimizar la información enviada al modelo.
- No publicar secretos, tokens de acceso, credenciales ni información corporativa en notebooks, salidas o historial Git.
- Revisar permisos, región de procesamiento, políticas de tenant y condiciones del proveedor.
- Validar las respuestas de IA antes de incorporarlas a procesos productivos.
- Considerar los controles de NB_16 como demostrativos, no como garantía de anonimización o cumplimiento normativo.

## Documentación oficial

- [Microsoft Fabric AI Functions — Overview](https://learn.microsoft.com/en-us/fabric/data-science/ai-functions/overview)
- [PySpark — Configuration](https://learn.microsoft.com/en-us/fabric/data-science/ai-functions/pyspark/configuration)
- [Pandas — Configuration](https://learn.microsoft.com/en-us/fabric/data-science/ai-functions/pandas/configuration)
- [AI Functions — Billing](https://learn.microsoft.com/en-us/fabric/data-science/ai-functions/billing)
- [Microsoft Fabric Capacity Metrics](https://learn.microsoft.com/en-us/fabric/enterprise/metrics-app)

## Contribuciones

Las contribuciones de la comunidad son bienvenidas. Puedes abrir un Issue para reportar errores, proponer mejoras o compartir resultados reproducibles, y enviar un Pull Request con cambios documentados. Evita incluir datos sensibles y especifica el runtime y la configuración utilizados al reportar resultados.

## Autor

**Eduardo Osorio Venegas**  
Data & AI Architect \| University Professor \| Microsoft Fabric Community

Proyecto desarrollado con fines educativos y de colaboración comunitaria, con apoyo de inteligencia artificial generativa de ChatGPT (OpenAI) en la preparación de documentación y ejemplos de código.

## Licencia

Licenciamiento propuesto: [Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0). Consulta el archivo `LICENSE` del repositorio una vez incorporado. Los componentes de terceros conservan sus licencias respectivas.

Copyright © 2026 Eduardo Osorio Venegas, respecto del material original sobre el que posea derechos.

## Disclaimer

Este es un proyecto educativo independiente, no un repositorio oficial de Microsoft. La generación del material no equivale a una certificación de funcionamiento: los ejemplos requieren validación en un entorno real de Microsoft Fabric. Las funcionalidades, modelos, requisitos y tarifas pueden cambiar. El material se proporciona sin garantías, conforme a los términos de la licencia aplicable.

------------------------------------------------------------------------

**From AI Functions to Enterprise Data & AI Architecture.**
