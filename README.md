# 📊 Precio vs. Ingresos Trimestrales — Tesla & GameStop (Dashboard)

**Notebook final:** `peer_review_tsla_gme_FINAL_ALLFIX.ipynb`  
> Proyecto didáctico para comparar el **precio histórico** de una acción con sus **ingresos (revenue) trimestrales** y generar un **dashboard** con dos paneles (precio e ingresos). Incluye *fallbacks* y parches para ejecutarse incluso cuando las fuentes web cambian o bloquean el acceso.

---

## 🧭 Tabla de contenidos
- [Objetivo](#objetivo)
- [Qué hace](#qué-hace)
- [Fuentes de datos](#fuentes-de-datos)
- [Metodología](#metodología)
- [Estructura del repo](#estructura-del-repo)
- [Requisitos y ejecución](#requisitos-y-ejecución)
- [Resultados / Imágenes](#resultados--imágenes)
- [Conclusiones](#conclusiones)
- [Limitaciones y ética](#limitaciones-y-ética)
- [Solución de problemas](#solución-de-problemas)
- [FAQ](#faq)
- [Licencia y créditos](#licencia-y-créditos)

---

## 🎯 Objetivo
Responder de forma visual a la pregunta: **¿Cómo se relaciona el precio de mercado de una empresa con su desempeño operativo (ingresos trimestrales)?**  
El proyecto te entrega dashboards para **Tesla (TSLA)** y **GameStop (GME)** que puedes abrir, capturar y compartir.

---

## 🧩 Qué hace
1. Descarga **precio histórico** (Yahoo Finance).
2. Extrae **ingresos trimestrales** (Macrotrends → espejo → *yfinance*), con **tres planes** para que no se rompa.
3. Limpia y **normaliza fechas** (sin zona horaria) y valores numéricos.
4. Construye un **dashboard** con:
   - **Panel izquierdo:** Precio (línea, diario).
   - **Panel derecho:** Ingresos (barras, trimestral).  
     > Si los ingresos provienen de un plan de contingencia extremo, el panel indica **(proxy)**.
5. Exporta **CSV** y **HTML** para entregar evidencia o pegar capturas.

---

## 🌐 Fuentes de datos
- **Precio:** Yahoo Finance vía [`yfinance`](https://github.com/ranaroussi/yfinance).  
- **Ingresos (Quarterly Revenue):**
  - A) **Macrotrends** (tabla HTML)
  - B) **Espejo de texto** (r.jina.ai) cuando la tabla no está disponible
  - C) **Yahoo Finance (quarterly financials)** como respaldo final

> **Transparencia:** Cuando el proyecto usa el respaldo C, el gráfico lo marca como **(proxy)** en el título del panel derecho.

---

## 🛠️ Metodología
- **Extracción:** HTTP `requests` + `pandas.read_html` (cuando hay tabla) o texto + *regex* (espejo), o *yfinance* financials.
- **Limpieza:** quitar símbolos monetarios, comas y unidades (`M`/`B`), convertir a números, **alinear fechas** a `datetime64[ns]` (sin TZ).
- **Alineación temporal:** el panel de precio se recorta hasta la **última fecha** con ingresos.
- **Visualización:** `plotly` (línea para precio, barras para ingresos).
- **Resiliencia:** si no hay ingresos reales, **proxy trimestral** desde el cierre (para que el dashboard no falle).

---

## 🗂️ Estructura del repo
```
.
├── peer_review_tsla_gme_FINAL_ALLFIX.ipynb   # Notebook final (ejecutar en orden Q1–Q7)
├── tesla_data.csv                             # Se genera al ejecutar Q1
├── tesla_quarterly_revenue.csv                # Se genera al ejecutar Q2
├── gme_data.csv                               # Se genera al ejecutar Q3
├── gme_quarterly_revenue.csv                  # Se genera al ejecutar Q4
├── tsla_dashboard.html                        # Se genera al ejecutar Q5
└── gme_dashboard.html                         # Se genera al ejecutar Q6
```

---

## 💻 Requisitos y ejecución

### Entorno
- **Python 3.10+** recomendado
- Conexión a Internet

### Instalar dependencias (opción simple)
```bash
pip install pandas yfinance requests beautifulsoup4 lxml plotly
```

### Ejecutar
1. Abre `peer_review_tsla_gme_FINAL_ALLFIX.ipynb` en Jupyter / VSCode / Watson Studio.
2. Ejecuta las celdas **en orden** (Q1 → Q7).  
3. Al finalizar, encontrarás **CSV** y **HTML** listos en la carpeta.

---

## 🖼️ Resultados / Imágenes

### Tesla
**Pega aquí tu captura del HTML de Tesla**  
```markdown
![Dashboard Tesla: Precio vs. Ingresos](ruta/tu_captura_tsla.png)
```

### GameStop
<img width="961" height="461" alt="image" src="https://github.com/user-attachments/assets/f5b3b966-bbe5-4a06-ac5a-eac97009f88f" />
```markdown
![Dashboard GameStop: Precio vs. Ingresos](ruta/tu_captura_gme.png)
```

> Las capturas se obtienen abriendo `tsla_dashboard.html` y `gme_dashboard.html` en tu navegador.

---

## 🧪 Conclusiones
- El **precio** y los **ingresos** se mueven a **ritmos diferentes** (diario vs. trimestral). No esperes “calce perfecto” punto a punto.
- Pueden aparecer **desacoples** (precio alto con ingresos planos o viceversa) que valen la pena analizar con contexto (noticias, expectativas).
- El proyecto demuestra cómo construir un **pipeline robusto** de datos financiero–operativos y **comunicar** insights de manera visual.

---

## ⚖️ Limitaciones y ética
- **Disponibilidad:** webs de terceros pueden cambiar/bloquear; por eso hay *fallbacks*.
- **Proxy:** solo se usa para no romper gráficos cuando la fuente no responde; **no** es sustituto analítico.
- **No es asesoría financiera.** Es una herramienta educativa para explorar y enseñar.

---

## 🧯 Solución de problemas
- **403 Forbidden (Macrotrends):** el notebook usa headers de navegador y *fallbacks*; sigue ejecutando las celdas, la fuente se ajusta sola.
- **“No hay revenue para graficar” (Q5/Q6):** el notebook garantiza *proxy* para el gráfico si faltan ingresos reales.
- **Error de fechas “tz-naive vs tz-aware”:** solucionado normalizando todo a `datetime64[ns]` **sin timezone**.

---

## ❓ FAQ
**¿Puedo cambiar de empresa?** Sí, sustituyendo el ticker (por ejemplo, `AAPL`).  
**¿Necesito saber programar?** No, ejecuta el notebook como “caja negra”.  
**¿Puedo compartir los HTML?** Sí, son archivos estáticos; úsalos para capturas o publicación.

---

## 📄 Licencia y créditos
- Código y contenido educativo: licencia **MIT** (ajusta si lo deseas).  
- Datos: pertenecen a sus **fuentes originales** (Yahoo Finance, Macrotrends). Úsalos conforme a sus términos.  
- Agradecimientos: comunidad de **pandas**, **plotly** y **yfinance**.

---

### ✍️ Notas para revisores / instructores
- El notebook guarda **artefactos** (CSV y HTML) por cada sección, facilitando revisión y auditoría.
- Los gráficos marcan **(proxy)** cuando corresponde, manteniendo **transparencia** con el lector.
