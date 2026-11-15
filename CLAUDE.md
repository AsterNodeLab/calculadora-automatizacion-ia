# CLAUDE.md — Guía del proyecto para agentes de código

Este archivo le da contexto a cualquier agente (Claude Code, OpenCode, etc.) que trabaje en este repositorio. Léeelo completo antes de hacer cambios.

## Qué es este proyecto

**Calculadora interactiva de automatización de procesos con IA.** El usuario describe un proceso empresarial (horas, frecuencia, costo por hora, personas) y la calculadora estima: potencial de automatización, tiempo ahorrado, ahorro económico, ROI/payback y la herramienta recomendada (n8n, Make, Zapier, agentes de IA).

- **Archivo único:** todo vive en `index.html` (HTML + CSS en `<style>` + JS en `<script>`). **Sin build, sin frameworks, sin dependencias.** Mantén esa estructura salvo que el usuario pida lo contrario.
- **Público:** materiales de clase y dueños de procesos PyME. Idioma: español neutro.
- **Publicado en:** https://asternodelab.github.io/calculadora-automatizacion-ia/ (GitHub Pages desde `main`, deploy automático al hacer `git push`).

## Cómo correrlo y verificarlo

```bash
npm start          # o: node serve.js → http://localhost:3000
```

Verificación mínima antes de dar algo por terminado:

1. La página carga **sin errores ni warnings de consola**.
2. Los KPIs renderizan valores reales (no "—"): con los valores por defecto debe salir esto (MXN, moneda MXN):

| KPI | Valor esperado |
|---|---|
| Horas hoy | 48.0 h/sem (3 h × 8 veces × 2 personas) |
| Potencial | 75 % |
| Ahorradas | 30.6 h/sem (48 × 75 % × (1 − 15 %)) |
| Ahorro anual | $190,797 (132.5 h/mes × $120 × 12) |
| ROI 12 meses | +1,370 % |
| Payback | menos de 1 mes (inversión $12,000, costo $1,200/mes) |

3. Casos borde que **no deben romperse**:
   - Potencial = 0 % → tiempo ahorrado 0 h, ahorro $0, recomendación con aviso de ahorro pequeño.
   - Inversión enorme (ej. $5,000,000) → payback se expresa en **años** (ej. "28.3 años") y ROI negativo con signo.
   - Cambio de moneda → sufijos de los inputs y formato de todas las cifras cambian (`Intl.NumberFormat` con `narrowSymbol`).
4. Si el entorno tiene Lighthouse: Accesibilidad, Best Practices y SEO deben quedarse en 100.
5. Responsive: sin scroll horizontal; la grilla se colapsa a 1 columna ≤ 1020 px.

## Arquitectura interna de index.html

- **Tokens de diseño:** al inicio de `<style>`, en `:root` (modo oscuro por defecto, `@media (prefers-color-scheme: light)` redefine los tokens). **Todos los colores/fuentes/espacios salen de tokens**; no metas valores sueltos.
- **Motor de cálculo:** función `calcular()` — todas las fórmulas en un solo lugar. Las fórmulas visibles de la sección "Cómo se calcula" se generan en `renderFormulas()` **con los mismos resultados**, así que si cambias una fórmula en `calcular()` también debes revisar `renderFormulas()` para que sigan consistentes.
- **Render:** `renderKPIs`, `renderDonut`, `renderBars`, `renderLinea` (gráfica SVG generada por JS, sin librerías), `renderRecomendacion` (scoring en `puntuar()`), `renderFormulas`.
- **Inversión sugerida:** `inversionSugerida()` ≈ 3 meses de ahorro neto (regla editable y transparente). Solo se auto-actualiza si el usuario no editó el campo (flag `inversionEditada`).
- **Tipos de proceso:** objeto `TIPOS` (porcentaje de automatización sugerido por tipo + si es estructurado). `DEFAULTS` tiene los supuestos base (4.33 semanas/mes, 15 % de supervisión).

## Principios que no se negocian

1. **No inventar cifras.** Los resultados son estimaciones con supuestos editables y transparentes: toda fórmula nueva debe reflejarse en "Cómo se calcula" con los números sustituidos.
2. **Accesibilidad AA:** contraste suficiente en ambos modos, foco visible, HTML semántico, `aria-live="polite"` en resultados, `prefers-reduced-motion` respetado.
3. **Iconos:** SVG inline de trazo (estilo Lucide), nunca emojis como iconos.
4. **Dirección estética:** "Tech premium oscuro" — fondo `#0A0C10`, superficies `#12151C`, acento lima `#C6F432` usado con disciplina, tipografías Space Grotesk (títulos) + Inter (texto) + JetBrains Mono (cifras con `tabular-nums`). Cambios de diseño deben mantener esta identidad o definirla de nuevo con intención y tokens.
5. **Idioma:** todo el texto visible y los comentarios en español.

## Pendientes conocidos (buenos primeros tickets)

- La regla "inversión ≈ 3 meses de ahorro neto" está en el hint del campo; podría explicarse mejor en la UI (tooltip o línea en "Cómo se calcula").
- El sufijo del costo por hora sí se actualiza al cambiar la moneda (`actualizarSufijos()`), pero al primer vistazo dice "$MX/h": considera una aclaración visual de que es dinámico.
- Animaciones 3D solicitadas por el usuario: hazlo con CSS (`transform: perspective/rotateY`) o librería por CDN **con graceful degradation** si no hay internet (la calculadora debe seguir funcionando offline); respeta `prefers-reduced-motion`.

## Publicación

- `git push` a `main` → Pages redespliega solo (tarda ~1 min en estar en línea).
- El repo es público; no subas claves ni datos personales.
