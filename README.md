# Movilitizate · Brief de Descubrimiento

Formulario HTML **autónomo** para responder el cuestionario de **Memoria de Marca** de Movilitizate y guardar todas las respuestas en una pestaña llamada **Respuestas enviadas**.

- Sin dependencias, sin build, sin servidor: un solo archivo `index.html`.
- Funciona abriéndolo con doble clic y también desplegado en GitHub Pages o Vercel.

---

## 1 · Qué incluye

- **29 preguntas** del brief (7 secciones + cierre), con numeración visible.
- **21 preguntas esenciales** marcadas con **★**, tal como vienen en el brief.
- Pestañas **Encuesta** y **Respuestas enviadas**.
- Botón **«Cerrar encuesta y guardar»**: al cerrarlo se guardan **todas** las respuestas (también las vacías, marcadas como pendientes) y la encuesta queda registrada como **Cerrada** en la pestaña de respuestas.
- **Borrador automático**: mientras respondes se guarda en el navegador; si cierras la pestaña sin querer, al volver se recupera.
- Cada encuesta cerrada muestra: fecha, número, cuántas se respondieron, cuántas ★ se respondieron, y todas las preguntas con la respuesta **tal como la escribió el usuario**.
- Acciones: **copiar respuestas**, **descargar JSON**, **eliminar encuesta**, **exportar todo (JSON)**, **exportar todo (CSV para Excel/Sheets)**, **borrar todo**.
- Diseño: fondo negro, marco degradado **oro / azul metálico**, alto contraste (AA), responsive (móvil/escritorio), navegación por teclado, `prefers-reduced-motion` y estilos de impresión.
- Guarda **localmente** en el navegador (`localStorage`). No envía datos a ningún servidor.

## 2 · Cómo usarlo

1. Abre `index.html` (doble clic).
2. Responde a tu ritmo. En “Valores” puedes elegir hasta 4 y en “Adjetivos” hasta 3.
3. Pulsa **«Cerrar encuesta y guardar»** en la barra inferior.
4. La encuesta queda en **Respuestas enviadas**. Desde ahí puedes copiarla o exportarla.

> Si cierras la encuesta con ★ pendientes, la app te avisa y te deja continuar: esas respuestas quedan como *«Sin respuesta (pendiente)»*.

## 3 · Dónde se guardan las respuestas

- En el **almacenamiento local del navegador** de ese dispositivo (`localStorage`):
  - `movilitizate.respuestas.v1` → encuestas cerradas.
  - `movilitizate.borrador.v1` → borrador en curso.
- No hay servidor ni base de datos, así que:
  - Sirve para que **una persona** responda el brief en su dispositivo y luego exporte el JSON/CSV.
  - Si se borran los datos del navegador, o se abre en otro dispositivo/navegador, no aparecerán las respuestas anteriores.
- **Respaldos:** usa «Exportar todo (JSON)» o «Exportar todo (CSV)». El CSV abre en Excel o Google Sheets.

### Si necesitas recibir respuestas de varias personas en un solo lugar

Esta página es 100 % local. Para centralizar respuestas de varias personas/equipos hay que conectar un backend ligero, por ejemplo:

- **Supabase** (tabla `respuestas` + RLS) o **Firebase**.
- **Google Apps Script + Google Sheets** (gratis y rápido para un formulario interno).
- **Formspree / Netlify Forms / Getform**.

El formulario ya está aislado en `index.html`; solo habría que sustituir `guardarEncuesta()` para enviar el JSON a ese servicio además de (o en lugar de) guardarlo en local.

## 4 · Desplegar en GitHub Pages

1. Crea un repositorio (por ejemplo `movilitizate-brief`).
2. Sube **el contenido de esta carpeta** (que `index.html` quede en la raíz del repo).
3. En GitHub: **Settings → Pages → Build and deployment**.
   - Source: **Deploy from a branch**.
   - Branch: `main` y carpeta `/ (root)` → **Save**.
4. En 1–2 minutos estará en `https://TU-USUARIO.github.io/TU-REPO/`.

> Alternativa: si lo subes dentro de una carpeta (p. ej. `/brief`), la URL será `https://TU-USUARIO.github.io/TU-REPO/brief/`.

## 5 · Desplegar en Vercel

**Opción A — desde Git (recomendada)**

1. Sube la carpeta a GitHub/GitLab/Bitbucket.
2. En [vercel.com/new](https://vercel.com/new) importa el repositorio.
3. Configuración del proyecto:
   - **Framework Preset:** `Other`
   - **Build Command:** *(vacío)*
   - **Output Directory:** `.` *(o déjalo vacío; es estático)*
4. **Deploy**. Vercel servirá `index.html` en la raíz del dominio.

**Opción B — CLI**

```bash
npm i -g vercel
cd ruta/a/movilitizate/brief
vercel        # despliegue de prueba
vercel --prod # producción
```

## 6 · Integrarlo al sitio Next.js existente (opcional)

El proyecto ya tiene un sitio Next.js en `movilitizate/web`. Si quieres publicar el brief dentro de ese dominio sin tocar el código:

1. Copia `index.html` a `movilitizate/web/public/brief.html`.
2. Al desplegar el sitio, quedará disponible en `https://TU-DOMINIO/brief.html`.

Es puramente estático: no requiere rutas, componentes ni configuración de Next.

## 7 · Editar preguntas o textos

Cada pregunta es un bloque `<article class="pregunta">` con:

| Atributo | Uso |
|---|---|
| `data-codigo` | Identificador único (se guarda en el historial). |
| `data-tipo` | `texto` (textarea) u `opciones` (chips). |
| `data-esencial` | `si` si lleva ★ y cuenta como esencial. |

- El texto del enunciado va en `<span class="texto">…</span>`.
- La numeración **se genera automáticamente** según el orden de los bloques; no hay que renumerar a mano.
- El contador de progreso y los resúmenes (★ respondidas / total) se recalculan solos.
- Para las preguntas con chips, agrega/quita `<label class="chip"><input type="checkbox" name="valores" value="…"><span>…</span></label>` dentro de `.chips`. El límite se controla con `data-max`.
- Si agregas una sección, usa `<section class="seccion" data-numero="9" data-titulo="…">` y mantén el `data-numero` / `data-titulo` para que el historial agrupe bien.

> Importante: conserva `data-codigo` únicos y los `textarea id="…"` con su `<label for="…">` correspondiente.

## 8 · Verificación realizada

Se validó con pruebas automatizadas (jsdom) el flujo completo: estructura del cuestionario, numeración, límites de chips, progreso, guardado al cerrar, borrador recuperado, pestaña de respuestas, exportación JSON/CSV, copiado, eliminación, borrado total, HTML sin errores de parseo y contraste AA de la paleta. También se verificó que la página funciona abriendo el archivo directamente (sin servidor).

---

**Archivo principal:** [`index.html`](index.html)

Tizayuca, Hidalgo · Memoria de Marca Movilitizate
