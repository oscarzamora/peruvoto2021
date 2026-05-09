# Proyecto Power BI - Elecciones Presidenciales Peru 2021

## Descripcion

Este repositorio documenta un ejercicio de analisis de datos desarrollado en 2021 con Power BI, utilizando informacion publica de la ONPE para la primera y segunda vuelta de las elecciones presidenciales del Peru.

Fuente y referencia original del ejercicio:
- https://ozamora.com/2021/07/peruvian-voting-data-with-powerbi/

Fuente oficial de datos abiertos (ONPE):
- https://www.datosabiertos.gob.pe/group/oficina-nacional-de-procesos-electorales-onpe

## Contexto

El ejercicio se desarrollo en un escenario electoral de alta sensibilidad publica. En 2021, Peru realizo sus elecciones presidenciales en dos vueltas:

**Primera Vuelta (11 de abril de 2021):** En una eleccion altamente fragmentada con 18 candidatos, Pedro Castillo (izquierda radical) obtuvo el primer lugar con aproximadamente 19% de los votos, seguido de Keiko Fujimori (derecha) con cerca de 13%. Ambos avanzaron a la segunda vuelta, lo cual sorprendio a muchos analistas dado el contexto politico y economico del pais.

**Segunda Vuelta (6 de junio de 2021):** En el ballotage final, Pedro Castillo resulto ganador con aproximadamente 50.1% de los votos frente a Keiko Fujimori con 49.9%. El resultado fue sumamente cerrado y controversial, lo que genero alegaciones de fraude por parte de sectores que respaldaban a Fujimori, aunque no se presento evidencia tangible de irregularidades sistematicas.

En ese momento, la ONPE publico los datasets de votacion de ambas vueltas, lo que permitio contrastar resultados y reducir espacios para desinformacion mediante evidencia verificable. Los datos desagregados por mesa de votacion facilitaban analisis independientes y verificacion de los resultados reportados.

Los archivos utilizados en este proyecto se obtienen del portal de Datos Abiertos del Estado peruano, especificamente del grupo oficial de la ONPE.

## Proposito del proyecto

1. Exponer con transparencia la informacion oficial publicada por la ONPE, de modo que cualquier persona pudiera explorar y analizar los datos.
2. Reforzar y retomar practica de desarrollo en Power BI sobre un caso real, incluyendo modelado, jerarquias, medidas y visualizaciones.

## Alcance tecnico del ejercicio

- Integracion de 2 archivos CSV oficiales (primera y segunda vuelta).
- Creacion de un modelo de datos con relacion principal por `MESA_DE_VOTACION`.
- Construccion de atributos derivados para analisis geografico nacional e internacional.
- Diseno de paginas de analisis para resultados presidenciales, votos nulos/blancos y casos con baja votacion.
- Implementacion de medidas DAX para calculo de porcentajes dinamicos segun filtros.

## Ejemplos Tecnicos

### Atributos Derivados (DAX)

**Continente** - Identifica el continente basado en el departamento. Si es un nombre conocido de continente, lo usa; si no, asume América donde reside Perú.

```dax
Continente = 
    IF(Resultados_1ra_vuelta_Version_PCM[DEPARTAMENTO] 
        IN {"AMERICA","ASIA", "OCEANIA","EUROPA", "AFRICA"},
        Resultados_1ra_vuelta_Version_PCM[DEPARTAMENTO],
    "AMERICA")
```

**Pais** - Determina el país basado en continente y provincia. Si está fuera de América o el departamento no es el continente, retorna la provincia como país.

```dax
Pais = 
    IF(Resultados_1ra_vuelta_Version_PCM[Continente]="AMERICA"
        && Resultados_1ra_vuelta_Version_PCM[Continente] 
            <> Resultados_1ra_vuelta_Version_PCM[DEPARTAMENTO],
        "PERU"
    , Resultados_1ra_vuelta_Version_PCM[PROVINCIA])
```

**Departamento Peru** - Normaliza el departamento para votos nacionales, asignando "Exterior" para votos internacionales.

```dax
Departamento Peru = 
    IF(Resultados_1ra_vuelta_Version_PCM[Pais] = "PERU", 
        Resultados_1ra_vuelta_Version_PCM[DEPARTAMENTO], 
        "Exterior")
```

### Medidas de Analisis

**Porcentaje de Votos (Ratio)** - Calcula el porcentaje dinamico de votos por candidato segun los filtros aplicados. La funcion CALCULATE es esencial porque permite que el denominador y numerador se recalculen automaticamente segun los cambios en filtros.

```dax
Ratio_K = 
    CALCULATE(
        SUM(Resultados_2da_vuelta_Version_PCM[VOTOS_P2])) 
            / (CALCULATE(SUM(Resultados_2da_vuelta_Version_PCM[VOTOS_P2])) 
                + CALCULATE(SUM(Resultados_2da_vuelta_Version_PCM[VOTOS_P1])))
```

### Componentes del Dashboard

El dashboard presenta 3 paginas principales:

1. **Presidenciales** - Informacion de votos para Keiko Fujimori y Pedro Castillo en ambas vueltas, con opciones de filtro y drill-down por mesa de votacion.
2. **Nulos, Blancos** - Informacion de votos sin candidato o invalidados por error.
3. **Menos de 5 Votos** - Ubicaciones que registraron menos de 5 votos por candidato principal.

## Contenido del proyecto

El proyecto contiene archivos fuente en formato CSV, el dashboard en Power BI, y documentacion de soporte para la configuracion y uso.

## Aprendizajes (adaptado del articulo original)

1. Un dashboard simple puede generar alto valor cuando se parte de datos oficiales y trazables.
2. Con dos datasets y una relacion bien definida es posible habilitar comparaciones utiles entre vueltas electorales.
3. Los atributos calculados (por ejemplo, continente, pais y normalizacion geografica) mejoran la capacidad de segmentacion y el drill-down.
4. Las medidas DAX permiten calcular indicadores porcentuales dinamicos que se ajustan automaticamente al contexto de filtros.
5. La transparencia de la fuente y la reproducibilidad del modelo son claves para fomentar analisis abiertos y verificables.

## Proximo paso

1. Abrir el archivo Power BI en Power BI Desktop.
2. Seguir la guia de configuracion de rutas relativas en la documentacion.
3. Ejecutar refresco de datos, revisar relaciones y extender visualizaciones segun necesidad.
