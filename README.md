# Infografía — Ascenso de Educación Básica 2024 (MINEDU + MINDEF)

Análisis estadístico del Concurso de Ascenso de Escala en la Carrera Pública Magisterial para la Educación Básica 2024, con cobertura conjunta de docentes del **MINEDU** (89 754 inscritos) y **MINDEF** (178 inscritos).

---

## Fuente de datos

| Archivo | Descripción |
|---|---|
| `data/Consolidado_Ascenso-EB-2024_Minedu+Mindef_actualizado 1.sav` | Base consolidada en formato SPSS (.sav). Versión actualizada usada a partir del 10/06/2026. |

La base es cargada con `pyreadstat` y procesada en pandas. Contiene una variable `BASE` que distingue entre postulantes de MINEDU y MINDEF.

---

## Estructura del proyecto

```
.
├── data/          # Base de datos fuente (.sav)
├── notebook/
│   └── exploratorio.ipynb   # Notebook principal de análisis
└── output/        # Tablas generadas en Excel (.xlsx)
```

---

## Tablas generadas

### T1 — Participación docente por etapas

Resumen del embudo de participación por institución (MINEDU / MINDEF / Total):

| Columna | Descripción |
|---|---|
| Inscritos | Total de postulantes inscritos |
| Evaluados Etapa Nac. | Participaron en la Etapa Nacional (tienen valor en `ESTADO_PUN`) |
| Evaluados Etapa Des. | Participaron en la Etapa de Desempeño (`CONDICION_FINAL` con valor habilitado o no cumple) |
| Ganadores | `GANADOR_ASCENSO == 1` |

---

### T2 — Evaluados, aprobados y ganadores por escala magisterial

Desagregación por escala (2.ª a 8.ª) de:
- Evaluados Etapa Nacional
- Aprobados Etapa Nacional (`ESTADO_PUN == "SUPERÓ PUNTAJE MÍNIMO"`)
- Ganadores
- Ratio Ganadores / Evaluados (%)

---

### T3 — Ganadores por sexo

Cruce de `BASE` × `SEXO_POSTULANTE` para el universo de ganadores. Incluye porcentajes por sexo dentro de cada institución y total.

---

### T4 — Ganadores según rango de edad

Distribución por rangos etarios (`RANGO_EDAD_IDR`: 25-29, 30-34, …, 60 a más) comparando evaluados vs. ganadores y ratio de conversión.

---

### T5 — Ganadores según discapacidad

Cruza dos fuentes de información:
- `TIENE_DISCAPACIDAD` para identificar evaluados
- `BONIFICACION_DISCAPACIDAD_A != 0` → variable derivada `RECIBIO_BONIF_DISPAC_IDR` para identificar ganadores con bonificación por discapacidad

---

### T6_A — Evaluados y aprobados Etapa Nacional por región

Agrupación por `BASE` × `REGION_LEGAJO_ACTUALIZADO` con conteo de evaluados, aprobados, reprobados y tasa de aprobación.

### T6_B — Evaluados y aprobados Etapa Nacional por unidad ejecutora

Misma lógica que T6_A pero agrupando por `DESC_UNIDAD_EJECUTORA`.

---

### T7 — Evaluados y aprobados por grupo de inscripción

Agrupación por `GRUPO_INSCRIPCION_FINAL` con evaluados, aprobados, reprobados y tasa de aprobación en la Etapa Nacional.

---

### T8 — Cumplimiento de criterios en la Etapa de Desempeño

Universo: docentes con `CONDICION_FINAL` igual a `"HABILITADO PARA ASIGNACIÓN DE VACANTE"` o `"NO CUMPLE REQUISITOS"`.

Utiliza la variable **`COLUMNAS_ED`** (actualizada el 10/06/2026), que define los 22 criterios de trayectoria evaluados:

```python
COLUMNAS_ED = [
    'TRAYECTORIA_1_1_MASTER',   # Grado de Maestría (derivado de TRAYECTORIA_1_1_GRADO)
    'TRAYECTORIA_1_1_DOCTOR',   # Grado de Doctor   (derivado de TRAYECTORIA_1_1_GRADO)
    'TRAYECTORIA_1_2',
    'TRAYECTORIA_1_3',
    'TRAYECTORIA_1_4',
    'TRAYECTORIA_1_5',
    'TRAYECTORIA_2_1',
    'TRAYECTORIA_2_2',
    'TRAYECTORIA_2_3',
    'TRAYECTORIA_2_4',
    'TRAYECTORIA_2_5A',
    'TRAYECTORIA_2_5B',
    'TRAYECTORIA_2_5C',
    'TRAYECTORIA_2_5D',
    'TRAYECTORIA_3_1A',
    'TRAYECTORIA_3_1B',
    'TRAYECTORIA_3_1C',
    'TRAYECTORIA_3_2',
    'TRAYECTORIA_3_3',
    'TRAYECTORIA_3_4',
    'TRAYECTORIA_3_5',
    'TRAYECTORIA_3_6'
]
```

Para cada criterio se reporta: cantidad de evaluados que cumplieron (`Cumplieron`), total de evaluados y porcentaje de cumplimiento.

> **Nota:** `TRAYECTORIA_1_1_MASTER` y `TRAYECTORIA_1_1_DOCTOR` son variables derivadas creadas en el notebook a partir de `TRAYECTORIA_1_1_GRADO` antes de construir la tabla.

---

## Outputs generados

| Archivo | Tabla | Generado |
|---|---|---|
| `output/T1.xlsx` | Participación por etapas | 10/06/2026 |
| `output/T2.xlsx` | Por escala magisterial | 10/06/2026 |
| `output/T3.xlsx` | Por sexo | 10/06/2026 |
| `output/T4.xlsx` | Por rango de edad | 10/06/2026 |
| `output/T5.xlsx` | Por discapacidad | 10/06/2026 |
| `output/T6_A.xlsx` | Por región | 10/06/2026 |
| `output/T6_B.xlsx` | Por unidad ejecutora | 10/06/2026 |
| `output/T7.xlsx` | Por grupo de inscripción | 10/06/2026 |
| `output/T8.xlsx` | Criterios Etapa Desempeño | 10/06/2026 |
| `output/Tablas de validación_ASCENSO EB 24_validacion_1006.xlsx` | Validación consolidada | 10/06/2026 |

---

## Historial de cambios relevantes

| Fecha | Cambio |
|---|---|
| 24/05/2026 | Creación del repositorio y carga inicial del notebook de análisis |
| 10/06/2026 | Actualización de la base de datos al archivo `.sav` consolidado (`Consolidado_Ascenso-EB-2024_Minedu+Mindef_actualizado`). Regeneración completa de tablas T1–T8. Revisión de la variable `COLUMNAS_ED` en T8 (22 criterios de trayectoria para la Etapa de Desempeño). |

---

## Requisitos

```
pandas
numpy
pyreadstat
openpyxl
```
