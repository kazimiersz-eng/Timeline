# Timeline
Timeline de filósofos para  PAU 2025-26
# Cronograma PAU · Filosofía

Visualización interactiva de los filósofos del temario de la PAU, sus relaciones de influencia y el tipo de vínculo entre ellos. Un único archivo HTML autocontenido, sin dependencias externas salvo dos fuentes de Google Fonts.

---

## Vistas

### Red de influencias (`◎ Red de influencias`)
Grafo canvas 2D navegable. Los filósofos aparecen posicionados en columnas por era histórica y a lo largo del eje vertical según su año de nacimiento (escala no lineal: la Antigüedad recibe más espacio proporcional para evitar que los presocráticos se amontonen).

- **Arrastra** para desplazarte por el grafo.
- **Rueda del ratón** para hacer zoom.
- **Clic en un nodo** para abrir el panel de detalle.
- **Clic en el fondo** para cerrar el panel.

### Eje temporal (`⏱ Eje temporal`)
Lista cronológica agrupada por era. Cada filósofo aparece como una tarjeta con nombre, fechas, contexto histórico e ideas principales. La leyenda se oculta automáticamente en esta vista para no tapar el contenido.

---

## Filtros

Dos chips en la cabecera controlan qué nodos se muestran en ambas vistas:

| Chip | Qué muestra |
|---|---|
| ★ PAU | Los 6 filósofos del examen: Platón, Tomás de Aquino, Descartes, Locke, Marx, Arendt |
| Secundarios | El resto: presocráticos, medievales y modernos que contextualizan las influencias |

Desactivar *Secundarios* oculta también los presocráticos, ya que comparten el mismo tipo.

---

## Tipos de nodo

| Color | Significado |
|---|---|
| 🟡 Dorado | Filósofo principal PAU (nodo más grande, borde grueso) |
| 🔵 Azul | Filósofo secundario / contexto |

---

## Flechas: tipos de relación

Las flechas indican influencia directa de un autor sobre otro. En reposo se muestran en gris neutro para no saturar el grafo; al seleccionar o pasar el cursor sobre un nodo, las flechas conectadas revelan su color real:

| Color | Significado |
|---|---|
| 🟢 Verde | Acuerdo o continuidad — el receptor recoge y desarrolla la idea |
| 🔴 Rojo | Desacuerdo o ruptura — el receptor reacciona contra o rechaza explícitamente |
| 🟡 Naranja | Relación compleja — hay tanto deuda como crítica, o una transformación profunda |

---

## Panel de detalle

Al hacer clic en cualquier nodo se abre un panel lateral con:

- **Era y tipo** (PAU / Secundario)
- **Fechas y periodo histórico**
- **Contexto histórico**
- **Ideas principales**
- **Influencias recibidas** — con barra de color lateral según el tipo de relación y una nota explicativa del vínculo concreto
- **Influye en** — ídem

Las píldoras de influencia son clicables y navegan al filósofo correspondiente.

---

## Eras históricas

| Color de banda | Era |
|---|---|
| 🟣 Púrpura | Filosofía Antigua |
| 🔵 Azul | Filosofía Medieval |
| 🟡 Dorado | Filosofía Moderna |
| 🩷 Salmón | Filosofía Contemporánea |

---

## Estructura del código

El archivo es un único `filosofia_grafo.html` con tres secciones:

```
<style>          → Estilos (variables CSS, componentes, vistas)
<body>           → HTML estático: cabecera, canvas, panel, leyenda
<script>
  ├── DATA       → Array PH[] con los filósofos
  ├── EDGES      → Tabla de relaciones arista por arista (from→to, rel, note)
  ├── STATE      → Cámara, filtros, nodo seleccionado
  ├── FILTERS    → toggleFilter()
  ├── VIEW SWITCH→ setView('graph' | 'tl')
  ├── GRAPH LAYOUT → layoutGraph() — posicionamiento con escala Y no lineal
  ├── RENDER     → drawEraBands / drawYearAxis / drawEdges / drawNodes
  ├── EVENTS     → Mouse, touch, wheel
  ├── PANEL      → openPanel() / closePanel()
  └── TIMELINE   → buildTL()
```

---

## Añadir o modificar un filósofo

En el array `PH`, cada entrada tiene esta forma:

```js
{
  id:    "locke",
  name:  "Locke",
  tipo:  "pau",           // "pau" | "sec"
  era:   "Moderna",       // "Antigua" | "Medieval" | "Moderna" | "Contemp"
  nac:   1632,            // año de nacimiento (negativo = a.C.)
  dates: "1632–1704",
  ctx:   "Contexto histórico breve.",
  ideas: "Ideas principales.",
  recv:  ["descartes","hobbes"],   // ids de quienes le influyen
  gives: ["rousseau","kant","marx"] // ids a quienes influye
}
```

---

## Añadir o modificar una relación

En el objeto `EDGES`, la clave es `"fromId→toId"`:

```js
"descartes→locke": {
  rel:  "disagree",   // "agree" | "disagree" | "complex"
  note: "Locke rechaza el innatismo cartesiano"
}
```

El array `gives` del filósofo fuente y el array `recv` del destino deben estar sincronizados con las entradas de `EDGES`.

---

## Requisitos

Ninguno. Abrir `filosofia_grafo.html` directamente en el navegador. Las fuentes (*Cormorant Garamond* y *Outfit*) se cargan de Google Fonts; sin conexión a internet se usará la fuente del sistema sin pérdida funcional.
