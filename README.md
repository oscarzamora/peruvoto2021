# Proyecto Power BI - Elecciones Presidenciales Perú 2021

## Índice

- [Propósito y Contexto](#propósito-y-contexto)
- [Datos disponibles](#datos-disponibles)
- [Diccionario de partidos y candidatos](#diccionario-de-partidos-y-candidatos)
- [Origen de la data](#origen-de-la-data-fuente-oficial)
- [Contenido del reporte](#contenido-del-reporte)
- [Galería de Paneles](#galería-de-paneles)

## Descripción

Este repositorio documenta un ejercicio de análisis de datos desarrollado en 2021 con Power BI, utilizando información pública de la ONPE para la primera y segunda vuelta de las elecciones presidenciales del Perú.

## Propósito y Contexto

El ejercicio se desarrolló en un escenario electoral de alta sensibilidad pública. En 2021, Perú realizó sus elecciones presidenciales en dos vueltas:

**Primera Vuelta (11 de abril de 2021):** En una elección altamente fragmentada con 18 candidatos, Pedro Castillo (izquierda radical) obtuvo el primer lugar con aproximadamente 19% de los votos, seguido de Keiko Fujimori (derecha) con cerca de 13%. Ambos avanzaron a la segunda vuelta, lo cual sorprendió a muchos analistas dado el contexto político y económico del país.

**Segunda Vuelta (6 de junio de 2021):** En el ballotage final, Pedro Castillo resultó ganador con aproximadamente 50.1% de los votos frente a Keiko Fujimori con 49.9%. El resultado fue sumamente cerrado y controversial, lo que generó alegaciones de fraude por parte de sectores que respaldaban a Fujimori, aunque no se presentó evidencia tangible de irregularidades sistemáticas.

En ese momento, la ONPE publicó los datasets de votación de ambas vueltas, lo que permitió contrastar resultados y reducir espacios para desinformación mediante evidencia verificable. Los datos desagregados por mesa de votación facilitaban análisis independientes y verificación de los resultados reportados.

Los archivos utilizados en este proyecto se obtienen del portal de Datos Abiertos del Estado peruano, específicamente del grupo oficial de la ONPE.

## Datos disponibles

- `data/Resultados_1ra_vuelta_Version_PCM.csv` (primera vuelta)
- `data/Resultados_2da_vuelta_Version_PCM.csv` (segunda vuelta)
- `reports/ONPE.pbix` (reporte Power BI)
- `docs/CSV_PARTIDOS_CANDIDATOS.md` (diccionario de columnas, partidos y candidatos)

## Diccionario de partidos y candidatos

El mapeo entre las columnas `VOTOS_Pn` de los CSV y los partidos / candidatos presidenciales 2021 está documentado en [docs/CSV_PARTIDOS_CANDIDATOS.md](docs/CSV_PARTIDOS_CANDIDATOS.md).

Incluye:

- Cabeceras verificadas de los archivos de primera y segunda vuelta.
- Tabla de mapeo `VOTOS_Pn → partido_id → Partido → Candidato` para ambas vueltas, con **votos verificados** sumando columna por columna las 86,488 mesas de 1ra vuelta.
- Descripción del resto de columnas (UBIGEO, MESA_DE_VOTACION, totales de votos en blanco, nulos e impugnados).

## Origen de la data (fuente oficial)

- Portal de datos abiertos ONPE:
    - https://www.datosabiertos.gob.pe/group/oficina-nacional-de-procesos-electorales-onpe
- Referencia original del ejercicio:
    - https://ozamora.com/2021/07/peruvian-voting-data-with-powerbi/

## Propósito del proyecto

1. Exponer con transparencia la información oficial publicada por la ONPE, de modo que cualquier persona pudiera explorar y analizar los datos.
2. Reforzar y retomar práctica de desarrollo en Power BI sobre un caso real, incluyendo modelado, jerarquías, medidas y visualizaciones.

## Alcance técnico del ejercicio

- Integración de 2 archivos CSV oficiales (primera y segunda vuelta).
- Creación de un modelo de datos con relación principal por `MESA_DE_VOTACION`.
- Construcción de atributos derivados para análisis geográfico nacional e internacional.
- Diseño de páginas de análisis para resultados presidenciales, votos nulos/blancos y casos con baja votación.
- Implementación de medidas DAX para cálculo de porcentajes dinámicos según filtros.

## Ejemplos Técnicos

### Atributos Derivados (DAX)

**Continente** - Identifica el continente basado en el departamento. Si es un nombre conocido de continente, lo usa; si no, asume América donde reside Perú.

```dax
Continente = 
    IF(Resultados_1ra_vuelta_Version_PCM[DEPARTAMENTO] 
        IN {"AMERICA","ASIA", "OCEANIA","EUROPA", "AFRICA"},
        Resultados_1ra_vuelta_Version_PCM[DEPARTAMENTO],
    "AMERICA")
```

**País** - Determina el país basado en continente y provincia. Si está fuera de América o el departamento no es el continente, retorna la provincia como país.

```dax
País = 
    IF(Resultados_1ra_vuelta_Version_PCM[Continente]="AMERICA"
        && Resultados_1ra_vuelta_Version_PCM[Continente] 
            <> Resultados_1ra_vuelta_Version_PCM[DEPARTAMENTO],
        "PERU"
    , Resultados_1ra_vuelta_Version_PCM[PROVINCIA])
```

**Departamento Perú** - Normaliza el departamento para votos nacionales, asignando "Exterior" para votos internacionales.

```dax
Departamento Perú = 
    IF(Resultados_1ra_vuelta_Version_PCM[País] = "PERU", 
        Resultados_1ra_vuelta_Version_PCM[DEPARTAMENTO], 
        "Exterior")
```

### Medidas de Análisis

**Porcentaje de Votos (Ratio)** - Calcula el porcentaje dinámico de votos por candidato según los filtros aplicados. La función CALCULATE es esencial porque permite que el denominador y numerador se recalculen automáticamente según los cambios en filtros.

```dax
Ratio_K = 
    CALCULATE(
        SUM(Resultados_2da_vuelta_Version_PCM[VOTOS_P2])) 
            / (CALCULATE(SUM(Resultados_2da_vuelta_Version_PCM[VOTOS_P2])) 
                + CALCULATE(SUM(Resultados_2da_vuelta_Version_PCM[VOTOS_P1])))
```

### Componentes del Dashboard

## Contenido del reporte

El dashboard presenta 3 páginas principales:

1. **Presidenciales** - Información de votos para Keiko Fujimori y Pedro Castillo en ambas vueltas, con opciones de filtro y drill-down por mesa de votación.
2. **Nulos, Blancos** - Información de votos sin candidato o invalidados por error.
3. **Menos de 5 Votos** - Ubicaciones que registraron menos de 5 votos por candidato principal.

## Galería de Paneles

- Presidenciales
- Nulos, Blancos
- Menos de 5 Votos

## Contenido del proyecto

El proyecto contiene archivos fuente en formato CSV y el dashboard en Power BI.

## Configuración de ruta relativa en Power BI

Power BI Desktop no toma automáticamente la ubicación del archivo PBIX para resolver rutas relativas reales. Para lograr el mismo efecto, se utiliza un parámetro de carpeta base.

En este proyecto, esa configuración ya está preconfigurada en el archivo PBIX. La explicación de abajo se mantiene como documentación de referencia en caso se requiera actualizar, migrar o ajustar rutas en otro entorno.

Pasos recomendados:

1. En Power BI Desktop, abrir **Transformar datos**.
2. Ir a **Administrar parámetros** y crear el parámetro `BaseProjectPath` (tipo texto).
3. Asignar como valor la carpeta raíz del proyecto.
4. En cada consulta CSV, construir la ruta completa combinando el parámetro con el nombre del archivo.

Ejemplo en Power Query (M):

```powerquery
let
    FilePath = BaseProjectPath & "\data\Resultados_1ra_vuelta_Version_PCM.csv",
    Source = Csv.Document(
        File.Contents(FilePath),
        [Delimiter=";", Columns=32, Encoding=1252, QuoteStyle=QuoteStyle.None]
    )
in
    Source
```

Con este enfoque, si el proyecto cambia de carpeta o de equipo, solo se actualiza `BaseProjectPath` y no todas las consultas.

## Resumen de lo realizado en este repositorio

1. Se organizó la estructura final dejando los datos en `data/` y el archivo Power BI en `reports/ONPE.pbix`.
2. Se documentó el contexto histórico de la elección presidencial 2021 (primera y segunda vuelta).
3. Se incorporaron fuentes oficiales: artículo original y portal de Datos Abiertos (ONPE).
4. Se añadieron ejemplos técnicos en DAX (atributos derivados y medida de porcentaje).
5. Se corrigió ortografía y tildación del contenido en español.

## Aprendizajes (adaptado del artículo original)

1. Un dashboard simple puede generar alto valor cuando se parte de datos oficiales y trazables.
2. Con dos datasets y una relación bien definida es posible habilitar comparaciones útiles entre vueltas electorales.
3. Los atributos calculados (por ejemplo, continente, país y normalización geográfica) mejoran la capacidad de segmentación y el drill-down.
4. Las medidas DAX permiten calcular indicadores porcentuales dinámicos que se ajustan automáticamente al contexto de filtros.
5. La transparencia de la fuente y la reproducibilidad del modelo son claves para fomentar análisis abiertos y verificables.

## Próximo paso

1. Abrir el archivo Power BI en Power BI Desktop.
2. Validar el parámetro `BaseProjectPath` para lectura de archivos CSV (ya preconfigurado; actualizar solo si cambia la ubicación del proyecto).
3. Ejecutar refresco de datos, revisar relaciones y extender visualizaciones según necesidad.
