# Diccionario CSV 2021: Partidos y Candidatos

## Alcance

Este documento describe el mapeo entre las columnas `VOTOS_Pn` de los archivos CSV de la ONPE (versión PCM) y los partidos / candidatos presidenciales del Perú 2021.

Archivos cubiertos en este repositorio:

- [data/Resultados_1ra_vuelta_Version_PCM.csv](../data/Resultados_1ra_vuelta_Version_PCM.csv)
- [data/Resultados_2da_vuelta_Version_PCM.csv](../data/Resultados_2da_vuelta_Version_PCM.csv)

## Método de verificación

El mapeo `VOTOS_Pn → partido / candidato` fue verificado sumando columna por columna las **86,488 mesas** del archivo de 1ra vuelta y contrastando los totales contra los resultados oficiales publicados por la ONPE. La columna **Votos verificados** en la tabla de 1ra vuelta refleja esos totales y permite reproducir la validación.

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

| Columna CSV | partido_id | Partido | Candidato | Votos verificados |
|---|---|---|---|---:|
| VOTOS_P1  | PNP | Partido Nacionalista Peruano  | Ollanta Humala Tasso         |   230,831 |
| VOTOS_P2  | FA  | Frente Amplio                 | Marco Arana Zegarra          |    65,300 |
| VOTOS_P3  | PM  | Partido Morado                | Julio Guzmán                 |   325,608 |
| VOTOS_P4  | PPS | Perú Patria Segura            | Rafael Santos Alvarado       |    55,644 |
| VOTOS_P5  | VN  | Victoria Nacional             | George Forsyth Sommer        |   814,516 |
| VOTOS_P6  | AP  | Acción Popular                | Yonhy Lescano Ancieta        | 1,306,288 |
| VOTOS_P7  | AP2 | Avanza País                   | Hernando de Soto             | 1,674,201 |
| VOTOS_P8  | PP  | Podemos Perú                  | Daniel Urresti               |   812,721 |
| VOTOS_P9  | JP  | Juntos por el Perú            | Verónika Mendoza             | 1,132,577 |
| VOTOS_P10 | SP  | Somos Perú                    | Daniel Salaverry             |   286,447 |
| VOTOS_P11 | K   | Fuerza Popular                | Keiko Fujimori Higuchi       | 1,930,762 |
| VOTOS_P12 | DD  | Democracia Directa            | Andrés Alcántara             |   101,267 |
| VOTOS_P13 | RL  | Renovación Popular            | Rafael López Aliaga          | 1,692,279 |
| VOTOS_P14 | PPC | Partido Popular Cristiano     | Alberto Beingolea Delgado    |    89,376 |
| VOTOS_P15 | RUN | Renacimiento Unido Nacional   | Ciro Gálvez Herrera          |   240,234 |
| VOTOS_P16 | PC  | Perú Libre                    | Pedro Castillo Terrones      | **2,724,752** |
| VOTOS_P17 | UPP | Unión por el Perú             | José Vega Antonio            |    50,802 |
| VOTOS_P18 | APP | Alianza para el Progreso      | César Acuña Peralta          |   867,025 |

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

- Las cabeceras documentadas fueron verificadas contra los archivos CSV reales presentes en [data/](../data/).
- El mapeo `VOTOS_Pn → partido_id` de **1ra vuelta** se validó sumando los 86,488 registros del CSV y comparando contra resultados oficiales ONPE (ver columna *Votos verificados*).
- Para **2da vuelta**, el mapeo (`VOTOS_P1` = Pedro Castillo / Perú Libre, `VOTOS_P2` = Keiko Fujimori / Fuerza Popular) se confirma con los totales nacionales: 8,835,970 vs 8,791,730 respectivamente.
- El delimitador del CSV es `;` y la codificación original es Windows-1252. En Power BI se carga con `Encoding=1252` y `QuoteStyle=QuoteStyle.None` (ver ejemplo en [README.md](../README.md)).
