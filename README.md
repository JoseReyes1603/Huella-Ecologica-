# 🌎 Calculadora de huella ecológica

**Práctica 4.5-P2 · Desarrollo Sustentable (ACD-0908) · TecNM**
Unidad 4. Escenario económico — Tema 4.5 Estrategias de sustentabilidad (análisis de ciclo de vida y huella ecológica)

| Campo | Detalle |
|---|---|
| Alumno | José Reyes Valdés Zamudio |
| Profesor | Miguel Ángel Barrón |
| Modalidad | Individual |
| Página publicada | [Ver calculadora](https://TU_USUARIO.github.io/TU_REPO/) |

---

## 📌 Descripción

Página web con un cuestionario de estilo de vida (alimentación, transporte, energía, consumo y residuos). Las respuestas se envían a un escenario de **Make**, donde un **Make AI Agent** estima la huella ecológica, la clasifica por área y propone acciones para reducirla. El resultado regresa a la página y se compara con la calculadora oficial de **Global Footprint Network**.

**Flujo:**

```
Página web (GitHub Pages)
   │  POST (URLSearchParams)
   ▼
Webhooks → Custom webhook
   ▼
Make AI Agent (Make's AI Provider)
   ▼
Webhooks → Webhook Response (text/plain + CORS)
   │
   ▼
Página web muestra el resultado
```

## 🛠️ Herramientas

- IA generativa (generación de la página)
- Make: Webhooks, Make AI Agent, Webhook Response
- GitHub Pages
- Calculadora oficial: https://www.footprintcalculator.org

---

## 💻 Código

La página es un solo archivo `index.html` (HTML + CSS + JavaScript, sin librerías externas).

Fragmento del envío de datos a Make:

```javascript
// ⬇️ PEGA AQUI LA URL DE TU WEBHOOK DE MAKE
const WEBHOOK_URL = "https://hook.us2.make.com/xxxxxxxx";

const CAMPOS = ["carne","local","transporte","km","vuelos",
                "aire","personas","ropa","celular","separa"];

form.addEventListener("submit", async (e) => {
  e.preventDefault();
  const datos = new URLSearchParams();
  CAMPOS.forEach(c => datos.append(c, document.getElementById(c).value));

  boton.disabled = true;
  estado.textContent = "Calculando tu huella...";
  try {
    const resp = await fetch(WEBHOOK_URL, { method: "POST", body: datos });
    if (!resp.ok) throw new Error("HTTP " + resp.status);
    resultado.textContent = await resp.text();   // texto plano, white-space: pre-line
  } catch (err) {
    resultado.textContent = "No se pudo calcular, intenta de nuevo.";
  } finally {
    boton.disabled = false;
  }
});
```

### Configuración del escenario en Make (`Huella_Ecologica`)

| # | Módulo | Configuración |
|---|---|---|
| 1 | Webhooks → Custom webhook | Recibe los 10 campos del formulario |
| 2 | Make AI Agent | Connection: Make's AI Provider · Input: una línea por campo mapeado · Instructions: analista de huella ecológica (TecNM Mazatlán) |
| 3 | Webhooks → Webhook Response | Status `200` · Body: respuesta del agente · Headers: `Content-Type: text/plain; charset=utf-8`, `Access-Control-Allow-Origin: *` |

Programación: **Immediately as data arrives** · Escenario **Active**.

Reglas agregadas a las Instructions del agente para mejorar la respuesta:

```
- Reemplaza todo lo que esta entre corchetes con informacion real; no copies las instrucciones de la plantilla.
- En cada area escribe el nivel y la razon en la misma linea.
- En POR AREA, ordena las cinco areas de mayor a menor impacto.
- La primera linea de tu respuesta debe ser: Nota: esta es una estimacion aproximada, no un calculo oficial.
```

---

## 🖼️ Imágenes

> Guarda las capturas en una carpeta `img/` del repositorio.

**Escenario en Make**
![Escenario en Make](img/escenario_make.png)

**Datos recibidos por el webhook**
![Webhook con los 10 campos](img/webhook_datos.png)

**Página web**
![Página de la calculadora](img/pagina.png)

**Resultado de la IA**
![Resultado de la IA](img/resultado_ia.png)

**Calculadora oficial (Global Footprint Network)**
![Resultado calculadora oficial](img/calculadora_oficial.png)

---

## 🎥 Video

[![Ver video de la práctica](img/miniatura_video.png)](https://youtu.be/TU_VIDEO)

> Reemplaza el enlace por tu video (YouTube, Drive, etc.). En GitHub también puedes arrastrar un `.mp4` directo al editor del README.

---

## 📊 Resultados

Respuestas capturadas: carne 1-2 veces por semana · la mitad de la comida local · moto · 13 km/día · 10 vuelos/año · 6 h/día de aire acondicionado · 6 prendas/mes · cambio de celular cada año · sí separa la basura.

| Prueba | Hectáreas globales | Planetas (página) | Planetas (calculadora oficial) | Diferencia | Área de mayor impacto |
|---|---|---|---|---|---|
| 1 | 3.2 | 1.9 | ____ | ____ | Transporte / Energía |
| 2 | 2.8 | 1.6 | ____ | ____ | Transporte / Energía |

**Comparación con promedios (Global Footprint Network):**

| Referencia | Planetas | gha por persona |
|---|---|---|
| Mi resultado (promedio de 2 pruebas) | 1.75 | 3.0 |
| México | ~1.7 | ~2.5 (aprox.) |
| Mundo | 1.73 | ~2.6 |
| Límite sostenible | 1.0 | 1.48 (biocapacidad) |

---

## 📚 Investigación

- **Huella ecológica:** superficie biológicamente productiva necesaria para producir los recursos que consume una persona y absorber sus residuos; se mide en hectáreas globales (GFN; WWF, 2024).
- **Biocapacidad:** capacidad de los ecosistemas para producir materiales biológicos útiles y absorber residuos humanos (GFN, Footprint FAQs).
- **Hectárea global (gha):** hectárea biológicamente productiva con la productividad promedio mundial (GFN).
- **Día del Sobregiro de la Tierra:** fecha en que la demanda humana supera lo que la Tierra regenera en el año. En **2026 cayó el 30 de julio** (en 2025 fue el 24 de julio).
- **México:** Día del Sobregiro 2026 el 31 de julio; ~1.7 planetas si todos vivieran como el mexicano promedio.
- **Mundo:** la humanidad usa la naturaleza 73% más rápido de lo que se regenera (1.73 planetas); biocapacidad mundial 1.48 gha/persona.

Fuentes:
- Global Footprint Network – Earth Overshoot Day 2026: https://overshoot.footprintnetwork.org/newsroom/press-release-june-2026-english/
- Global Footprint Network – Country Overshoot Days: https://overshoot.footprintnetwork.org/newsroom/country-overshoot-days/
- Global Footprint Network – Glossary: https://www.footprintnetwork.org/resources/glossary/
- Global Footprint Network – Footprint FAQs: https://www.footprintnetwork.org/content/documents/Footprint_FAQs.pdf
- El Imparcial (2026) – Día del Sobregiro y México

---

## ✅ Conclusión

La huella estimada (1.6–1.9 planetas) está ligeramente por encima del promedio de México. Las áreas con más impacto son **transporte** (moto y vuelos) y **energía** (aire acondicionado por el clima de Mazatlán). La IA es útil para concientizar, pero sus estimaciones varían entre ejecuciones, por lo que la calculadora oficial es la referencia confiable.
