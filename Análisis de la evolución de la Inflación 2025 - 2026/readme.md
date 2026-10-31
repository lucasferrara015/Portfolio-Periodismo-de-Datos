# Análisis automatizado de la inflación en Argentina (datos.gob.ar) 2025-2026

Repositorio del proyecto de **Periodismo de Datos** centrado en la extracción, auditoría y visualización de la evolución mensual e interanual del Índice de Precios al Consumidor (IPC) a nivel nacional.

---

## 📌 El Hallazgo Periodístico

El análisis del ciclo 2025-2026 revela una marcada **tendencia hacia la desaceleración del nivel general de inflación**, acompañada de picos estacionales previsibles en los meses de marzo (comienzo del ciclo lectivo y ajustes de servicios) y julio (receso invernal y turismo). Al comparar el comportamiento interanual, la serie temporal evidencia una consolidación de rangos mensuales más bajos respecto a 2025, alcanzando en el período de junio-agosto de 2026 sus valores porcentuales más estables, lo que sugiere un cambio de dinámica en la estructura de precios del consumidor.

---

## 🔬 Ficha Metodológica y Transparencia

- **Fuente oficial:** [API de Series de Tiempo del Portal Nacional de Datos Abiertos](https://apis.datos.gob.ar/series/api/series/) (`apis.datos.gob.ar`).
- **Serie utilizada:** IPC Nivel General (Serie base 2016) — ID: `101.1_I2NG_2016_M_22`.
- **Rango temporal procesado:** Enero de 2024 a Agosto de 2026 (tomando 2024 como año base de comparación para el cálculo interanual de 12 meses).

### Proceso de Auditoría y Control de Calidad
Para garantizar la máxima precisión y rigor periodístico, los valores absolutos e índices devueltos por la API automatizada se **cruzaron y auditaron directamente contra los Informes Técnicos oficiales del INDEC** (Índice de Precios al Consumidor) correspondientes a los meses de **junio, julio y agosto de 2026**. 

Se constató que las variaciones porcentuales mensuales registradas en los reportes en PDF coinciden plenamente con los datos de la API, confirmando que las discrepancias menores corresponden estrictamente al método de redondeo estándar en decimales y no a divergencias en la serie original.

---

## ⚙️ Pipeline de Datos (ETL con Python)

El procesamiento de datos está totalmente automatizado a través de un script en Python (`main.py` / Google Colab) estructurado en tres etapas principales:

1. **Extracción (API):** Consumo automatizado del endpoint CSV del Portal de Datos Abiertos haciendo uso de la librería `requests` con cabeceras HTTP personalizadas para evitar bloqueos del servidor.
2. **Transformación (Pandas):** 
   - Conversión de tipos de datos temporales (`pd.to_datetime`).
   - Cálculo de la tasa de variación mensual en porcentaje: `pct_change() * 100`.
   - Cálculo de la variación interanual comparando exactamente 12 períodos hacia atrás: `pct_change(periods=12) * 100`.
   - Filtrado y estructuración horizontal del dataset enfocado en la comparativa 2025 vs. 2026.
3. **Carga y Exportación:** Generación del archivo normalizado `ipc_nacional_2025_2026.csv` listo para su ingesta en plataformas de visualización.

```python
# Extracto del procesamiento en Pandas
df['Inflacion_Mensual (%)'] = df['IPC_Nacional_Indice'].pct_change() * 100
df['Inflacion_Interanual (%)'] = df['IPC_Nacional_Indice'].pct_change(periods=12) * 100
```

---

## 📊 Visualizaciones e Historias Interactivas

Para adaptar el análisis tanto a la lectura rápida como a la exploración en profundidad, se crearon dos formatos complementarios:

* **📈 Gráfico Comparativo Interactivo (Datawrapper):** [Ver Gráfico Interactivo en Datawrapper](https://datawrapper.dwcdn.net/y3LiA/1/)
  * *Objetivo:* Permitir al lector explorar de un vistazo la comparación exacta mes a mes entre 2025 y 2026 pasando el cursor por cada punto de la serie.

* **🎬 Narrativa Animada de la Serie (Flourish):** [Ver Animación en Flourish](https://flourish-user-preview.com/30636592/mrnaGC80RqmEdtW9xM0xEaEQ1fgP-aaRvUtLCyzzIdCN235nfu_3Jx2TLdBlFTTb/)
  * *Objetivo:* Generar una dinámica de scrollytelling/video que muestra el recorrido acumulativo y la velocidad del cambio en los precios a lo largo del tiempo.

---

## 🎥 Explicación Técnica y Presentación

Podés ver el **video explicativo en LinkedIn** donde muestro el paso a paso del proyecto, la ejecución del script en Google Colab y la interacción con las visualizaciones finales:

👉 **[[Ver video explicativo]([url](https://drive.google.com/file/d/1pBm1pEkwiFszzhMiOAce2oJuVw8U2IU0/view?usp=sharing)**

---

## 🛠️ Herramientas Utilizadas

- **Lenguaje:** Python 3.x
- **Librerías de Datos:** Pandas, Requests, IO
- **Entorno:** Google Colab / Jupyter Notebook
- **Visualización:** Datawrapper, Flourish
- **Fuente de Datos:** Datos Argentina (INDEC)

---

### 📩 Contacto y Colaboraciones
**Lic. Lucas Ferrara** — *Data Analyst & Periodista de Datos*  
Mendoza, Argentina  
- **LinkedIn:** [linkedin.com/in/lucasferrara-data-comunicacion](https://www.linkedin.com/in/lucasferrara-data-comunicacion)  
- **GitHub:** [github.com/lucasferrara0115](https://github.com/lucasferrara0115)
