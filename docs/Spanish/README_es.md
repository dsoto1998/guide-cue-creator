# Guide Cue Creator

Lee una sesión de Ableton Live (`.als`), asocia los marcadores con archivos de cue de audio y genera un único archivo WAV estéreo (`guide_cues.wav`) listo para colocar en una pista de cue guía dedicada en el tiempo 0.

---

## Contenido incluido

```
cue_creator.html    ← la aplicación (abrir este archivo)
GUIDE CUES/         ← biblioteca de cues de audio (incluida)
```

---

## Requisitos

- **Chrome o Edge** (recomendado) — acceso a la carpeta sin clics adicionales después de la primera vez
- **Firefox** — funciona, volver a seleccionar la carpeta una vez por sesión del navegador si es necesario
- Conexión a Internet (carga pako, Fuse.js, Dexie desde CDN)

---

## Inicio rápido

1. Haz doble clic en `cue_creator.html` — se abre en tu navegador predeterminado
2. Haz clic en **Seleccionar carpeta** y elige la carpeta que contiene `GUIDE CUES/`
3. Selecciona un **idioma** del menú desplegable
4. Arrastra tu archivo `.als` al área de carga (o haz clic para buscar)
5. Revisa la tabla de cues — verde = buena coincidencia, amarillo = confianza baja, rojo = sin coincidencia
6. Haz clic en **Vista previa** (o presiona **Espacio**) para escuchar
7. Haz clic en **Renderizar WAV** — descarga `guide_cues.wav`
8. Arrastra `guide_cues.wav` a tu sesión de Ableton en el tiempo 0 en una pista dedicada

En Chrome/Edge, la selección de carpeta se recuerda. El paso 2 es una configuración única.

---

## Qué hace la aplicación

- Coloca cada cue de sección **1 compás antes** de su marcador
- Genera un **conteo de entrada** para los primeros 2 compases (tiempos 0–7 en 4/4)
- Suprime los cues de sección que caen dentro de la región del conteo
- Omite marcadores llamados `count off` o `next song`
- Compatible con automatización de tempo (sesiones con BPM variable)

---

## Compases de tiempo compatibles

3/4 · 4/4 · 6/8 · 12/8

---

## Idiomas compatibles

Inglés · Español · Francés · Indonesio · Coreano · Filipino · Portugués

---

## Controles de la tabla de cues

| Columna | Qué hacer |
|---------|-----------|
| Marcador | Solo lectura — desde tu .als |
| Cue asociado | Editar para cambiar la asociación automática |
| Confianza | Verde ≥ 80% · Amarillo ≥ 50% · Rojo < 50% |

---

## Controles de la línea de tiempo

| Acción | Resultado |
|--------|-----------|
| Clic | Ir a la posición |
| Espacio | Reproducir / Pausar |
| Ctrl+desplazamiento o pellizco | Zoom hacia el cursor |
| Botones +/− | Acercar/alejar |
| Restablecer | Ajustar sesión completa al ancho |
| Seguir cabezal | Activar/desactivar desplazamiento automático |

---

## Especificaciones de salida

| Ajuste | Valor |
|--------|-------|
| Frecuencia de muestreo | 48.000 Hz |
| Canales | Estéreo |
| Profundidad de bits | PCM de 16 bits |
| Archivo | `guide_cues.wav` |
