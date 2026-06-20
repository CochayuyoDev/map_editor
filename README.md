# 🗺️ Editor de Zonas de Cobertura — GSG Corp

Editor visual para **diseñar, editar y exportar** las zonas de cobertura/tarifas
de GSG Corp como archivos **KMZ/KML** compatibles con **Google My Maps**.

Es una sola página HTML (`editor_zonas.html`) que se abre directo en el navegador.
No necesita instalación, ni servidor, ni base de datos.

---

## 🚀 Cómo abrirlo

**Opción rápida (recomendada):** doble clic en `editor_zonas.html`.
Se abre en el navegador y funciona al 100% (todo corre del lado del cliente).

**Por XAMPP (opcional):** si tienes Apache encendido, también está disponible en:

```
http://localhost/GSG/sandboxes/Mapa/editor_zonas.html
```

> Necesita conexión a internet: las librerías (mapa, dibujo, KMZ) se cargan desde
> CDN. Sin internet, el mapa no carga.

---

## 📁 Archivos de esta carpeta

| Archivo | Qué es |
|---|---|
| `editor_zonas.html` | **La aplicación.** Es lo único que abres. |
| `Cobertura GSG Corp (1).kmz` · `Cobertura_GSG_Corp (*).kmz` | Exportaciones del mapa de cobertura (distintas versiones). |
| `Cobertura_GSG_datos.kml` | Datos de cobertura en KML. |
| `VMT_limite.kml` | Contorno del distrito de Villa María del Triunfo. |
| `README.md` | Este documento. |

> Los `.kmz` son ZIPs que contienen un `doc.kml` adentro. Puedes subir cualquiera
> de ellos al editor con **“1 · Cargar mapa actual”**.

---

## 🧭 Flujo de trabajo típico

1. **Cargar** tu KMZ exportado de My Maps (panel 1).
2. **Editar** zonas: dibujar nuevas, mover vértices, recortar, unir, etc.
3. **Ajustar tarifas y colores** (panel “Tarifas”).
4. **Resolver choques** entre zonas si hay solapes (ver abajo).
5. **Descargar KMZ** (panel 5) y volver a subirlo a Google My Maps.

El trabajo se **guarda solo** en el navegador (localStorage); si cierras por
error, usa **♻️ Recuperar último trabajo**.

---

## 🛠️ Funciones (panel lateral)

### 1 · Cargar mapa actual
Sube un `.kmz` o `.kml` exportado de My Maps. Las zonas se cargan **conservando
su color original** y se detecta la tarifa por su color.

### 2 · Tarifa activa
La tarifa (y color) que tomarán las **zonas nuevas** que dibujes.

### 3 · Dibujar / seleccionar
- **🖱️ Modo selección** (Esc) · **✏️ Dibujar zona nueva**
- **Editar vértices** · **Mover**
- **✂️ Recortar**: dibujas una forma y se resta de las zonas que toca.
- **🗑️ Borrar** · **↶ Deshacer (Ctrl+Z)** · **↷ Rehacer (Ctrl+Y)**
- **Snapping** activo: los vértices se pegan a otras zonas para no dejar huecos.

### Capas y superposición
- **⬆️ Al frente / ⬇️ Al fondo**: orden de dibujo de la zona seleccionada.
- **➖ Restar de abajo**: la zona de encima recorta a las de abajo donde se solapan.
- **🔗 Unir**: fusiona 2+ zonas seleccionadas en una (toma la tarifa de la primera).
- **⚖️ Resolver choques** + **Margen (m)**: ver sección dedicada más abajo.
- Selección: **clic** = una zona · **Shift+clic** = varias.

### 4 · Traer límite de distrito
Escribe el nombre de un distrito (ej. *Villa María del Triunfo*) y trae su
contorno exacto para editarlo, en vez de calcarlo a mano.

### 🛣️ Calles (pegar bordes)
Carga las calles de la zona visible. Al dibujar o recortar, los vértices se
**pegan a las calles** para que el borde siga las pistas reales.

### 5 · Exportar / Guardar
- **💾 Descargar KMZ** (para subir a My Maps).
- **Descargar GeoJSON**.
- **♻️ Recuperar último trabajo** (autoguardado en el navegador).

### Tarifas
Lista/leyenda de tarifas con su color. Cada tarifa se puede **renombrar,
recolorear, agregar o eliminar** (ver abajo).

---

## ⚖️ Resolver choques de zonas

Cuando dos zonas se **solapan** (un “choque”, p.ej. una zona de **10** pegada a
una de **20**), la regla del negocio es: **esa franja se cobra con la tarifa más
barata**, porque está muy cerca y cobrar la cara sería demasiado.

El botón **⚖️ Resolver choques** (con el campo **Margen**) hace exactamente eso:

- La zona **más barata crece** los metros del margen **hacia la zona más cara**.
- La zona **cara NO se recorta**: queda completa, debajo.
- La barata queda **al frente**, así su color y tarifa **mandan** en esa franja.

**Ejemplo:** zona de **10** junto a una de **20**, margen `50` → la de 10 se
agranda 50 m metiéndose sobre la de 20; la de 20 sigue intacta por debajo, pero
en esos 50 m manda la tarifa de 10.

Notas:
- “Más barata” = el **número de tarifa más bajo** (10 < 15 < 20 …).
- Margen `0` = no crece; solo reordena para que la barata quede al frente.
- La barata crece **solo hacia zonas más caras**, nunca hacia espacio vacío.
- Se puede deshacer con **Ctrl+Z**.
- Como la zona cara sigue debajo, en la franja hay dos polígonos superpuestos;
  por la transparencia del relleno el color puede verse un poco mezclado en el
  editor, pero la barata es la que está encima.

---

## 🎨 Colores y compatibilidad con Google My Maps

Google My Maps usa una **paleta fija de colores**. Si exportas un color que **no
existe** en esa paleta, My Maps lo **cambia** al más parecido (por eso “se veían
distintos” los colores al re-subir).

Para evitarlo, el editor:

1. Trae los **colores por defecto de las tarifas tomados de la paleta real de My Maps**:

   | Tarifa | Color | Hex |
   |---|---|---|
   | 10 | verde | `#558B2F` |
   | 15 | amarillo | `#FFD600` |
   | 20 | rojo oscuro | `#A52714` |
   | 25 | naranja | `#E65100` |
   | 30 | rojo | `#FF5252` |

2. Ofrece la **paleta de My Maps como cuadritos** (en el editor de tarifas y en el
   popup de cada zona). Si eliges desde esos cuadritos, el color se ve **idéntico**
   al volver a subir el KMZ.
3. Conserva los **colores originales** de las zonas que importas.
4. Migra automáticamente colores viejos guardados en el navegador a los de My Maps.

> Si usas un color **fuera** de la paleta de My Maps (con el selector libre), My
> Maps puede ajustarlo al subirlo — es una limitación de My Maps, no del editor.

### Tarifas editables
En el panel **Tarifas** puedes:
- Cambiar el **nombre/etiqueta** (ej. “Tarifa plana 15 soles”).
- Cambiar el **color** (paleta de My Maps o selector libre).
- **Agregar** o **eliminar** tarifas.

Al cambiar el color de una tarifa, todas las zonas con esa tarifa que **no** tengan
un color personalizado se actualizan solas. Cada zona puede además tener su **color
propio** desde su popup (clic en la zona).

---

## 💾 Guardado y recuperación

- Todo se guarda en el **navegador** (localStorage), no en archivos:
  - `zonas_autosave` / `zonas_autosave_ts` — las zonas y su fecha.
  - `tarifas_config` — la configuración de tarifas/colores.
- **♻️ Recuperar último trabajo** restaura la última sesión.
- Para tener un respaldo “de verdad”, usa **💾 Descargar KMZ** y guarda el archivo.

> El autoguardado es por navegador y por equipo. Si cambias de PC o navegador, no
> verás el autoguardado; usa el KMZ descargado.

---

## 🧱 Tecnología

Todo se carga por CDN (no hay `node_modules` ni build):

| Librería | Uso |
|---|---|
| [Leaflet 1.9.4](https://leafletjs.com/) | Mapa base e interacción |
| [Leaflet-Geoman 2.17.0](https://geoman.io/) | Dibujar y editar polígonos |
| [JSZip 3.10.1](https://stuk.github.io/jszip/) | Leer/escribir KMZ (ZIP) |
| [Turf.js 7.1.0](https://turfjs.org/) | Geometría: unir, restar, buffer, intersección |

### Detalle técnico: color KML
KML guarda el color como `aabbggrr` (alfa, azul, verde, rojo), al revés del hex
HTML `#rrggbb`. El editor convierte en ambos sentidos al importar/exportar.
My Maps codifica el color en el **id del estilo** (ej. `poly-558B2F-1601-64`).

---

## ⚠️ Limitaciones

- Necesita **internet** (librerías por CDN).
- El autoguardado vive en **un solo navegador/equipo**; el respaldo real es el KMZ.
- Colores fuera de la paleta de My Maps pueden cambiar al subirlos a My Maps.
- “Resolver choques” deja la zona cara **completa debajo** de la barata; si
  necesitas que la cara realmente pierda esa área en el archivo, usa
  **➖ Restar de abajo** o **✂️ Recortar**.
