# Diccionario CSV 2021: Partidos y Candidatos

## Alcance

Este documento describe el mapeo entre las columnas `VOTOS_Pn` de los archivos CSV de la ONPE (versión PCM) y los partidos / candidatos presidenciales del Perú 2021.

Archivos cubiertos en este repositorio:

- [data/Resultados_1ra_vuelta_Version_PCM.csv](../data/Resultados_1ra_vuelta_Version_PCM.csv)
- [data/Resultados_2da_vuelta_Version_PCM.csv](../data/Resultados_2da_vuelta_Version_PCM.csv)

## Cabeceras verificadas

### Primera vuelta

```
UBIGEO;DEPARTAMENTO;PROVINCIA;DISTRITO;TIPO_ELECCION;MESA_DE_VOTACION;DESCRIP_ESTADO_ACTA;TIPO_OBSERVACION;N_CVAS;N_ELEC_HABIL;VOTOS_P1;VOTOS_P2;VOTOS_P3;VOTOS_P4;VOTOS_P5;VOTOS_P6;VOTOS_P7;VOTOS_P8;VOTOS_P9;VOTOS_P10;VOTOS_P11;VOTOS_P12;VOTOS_P13;VOTOS_P14;VOTOS_P15;VOTOS_P16;VOTOS_P17;VOTOS_P18;VOTOS_VB;VOTOS_VN;VOTOS_VI
```

Total: 31 columnas (10 dimensiones + 18 partidos + 3 totales no válidos).

### Segunda vuelta

```
UBIGEO;DEPARTAMENTO;PROVINCIA;DISTRITO;TIPO_ELECCION;MESA_DE_VOTACION;DESCRIP_ESTADO_ACTA;TIPO_OBSERVACION;N_CVAS;N_ELEC_HABIL;VOTOS_P1;VOTOS_P2;VOTOS_VB;VOTOS_VN;VOTOS_VI
```

Total: 15 columnas (10 dimensiones + 2 partidos + 3 totales no válidos).

## Mapeo VOTOS_Pn → Partido y Candidato

### A) Primera vuelta — `Resultados_1ra_vuelta_Version_PCM.csv`

| Columna CSV | partido_id | Partido | Candidato |
|---|---|---|---|
| VOTOS_P1 | AP | Acción Popular | Yonhy Lescano Ancieta |
| VOTOS_P2 | APP | Alianza para el Progreso | César Acuña Peralta |
| VOTOS_P3 | AP2 | Avanza País | Hernando de Soto |
| VOTOS_P4 | DD | Democracia Directa | Roberto Chiabra León |
| VOTOS_P5 | FA | Frente Amplio | Marco Arana Zegarra |
| VOTOS_P6 | FP2 | Frepap | Orígenes Juanito Gonzales |
| VOTOS_P7 | K | Fuerza Popular | Keiko Fujimori Higuchi |
| VOTOS_P8 | JP | Juntos por el Perú | Verónika Mendoza |
| VOTOS_P9 | PM | Partido Morado | Julio Guzmán |
| VOTOS_P10 | PP | Podemos Perú | Daniel Urresti |
| VOTOS_P11 | PC | Perú Libre | Pedro Castillo Terrones |
| VOTOS_P12 | PNP | Perú Nación | Alberto Beingolea Delgado |
| VOTOS_P13 | PPS | Perú Patria Segura | Rafael Santos Alvarado |
| VOTOS_P14 | RUN | Renacimiento Unido Nacional | Ciro Gálvez Herrera |
| VOTOS_P15 | RL | Renovación Popular | Rafael López Aliaga |
| VOTOS_P16 | SP | Somos Perú | Daniel Salaverry |
| VOTOS_P17 | UPP | Unión por el Perú | José Vega Antonio |
| VOTOS_P18 | VN | Victoria Nacional | George Forsyth Sommer |

### B) Segunda vuelta — `Resultados_2da_vuelta_Version_PCM.csv`

| Columna CSV | partido_id | Partido | Candidato |
|---|---|---|---|
| VOTOS_P1 | PC | Perú Libre | Pedro Castillo Terrones |
| VOTOS_P2 | K | Fuerza Popular | Keiko Fujimori Higuchi |

## Otras columnas (no partido)

| Columna | Descripción |
|---|---|
| UBIGEO | Código geográfico (departamento / provincia / distrito) |
| DEPARTAMENTO | Departamento |
| PROVINCIA | Provincia |
| DISTRITO | Distrito |
| TIPO_ELECCION | Tipo de elección (PRESIDENCIAL) |
| MESA_DE_VOTACION | Número / código de mesa |
| DESCRIP_ESTADO_ACTA | Estado del acta (p. ej. CONTABILIZADA) |
| TIPO_OBSERVACION | Observación del acta |
| N_CVAS | Número de candidatos válidos acreditados en la mesa |
| N_ELEC_HABIL | Electores hábiles |
| VOTOS_VB | Votos en blanco |
| VOTOS_VN | Votos nulos |
| VOTOS_VI | Votos impugnados |

## Notas de consistencia

- El mapeo `VOTOS_Pn → partido_id` corresponde al orden operacional usado por el ETL del proyecto 2021 (`PARTY_MAP` por vuelta) y al catálogo de partidos y candidatos definido para las elecciones presidenciales 2021.
- Las cabeceras documentadas fueron verificadas contra los archivos CSV reales presentes en [data/](../data/).
- El delimitador del CSV es `;` y la codificación original es Windows-1252. En Power BI se carga con `Encoding=1252` y `QuoteStyle=QuoteStyle.None` (ver ejemplo en [README.md](../README.md)).
