# Tareas por Satoshis — Cada Satoshi cuenta

Sistema de gestión de tareas con recompensas en satoshis para un equipo de trabajo.
Frontend estático que lee datos en vivo desde Google Sheets.

## Estructura del repo

```
/
├── index.html       ← La web entera (HTML + CSS + JS en un solo archivo)
├── fotos/           ← Carpeta con las fotos de perfil de cada persona
│   ├── neo.jpg
│   ├── trinity.jpg
│   └── ...
└── README.md
```

## Cómo desplegar en GitHub Pages

1. Crear un repo público en GitHub (ej: `panel-tareas`).
2. Subir `index.html` al raíz del repo.
3. Crear la carpeta `fotos/` y subir las fotos de perfil ahí (formato cuadrado recomendado, tamaño entre 200x200 y 400x400 px).
4. En el repo: **Settings → Pages → Source: Deploy from branch → Branch: main / root → Save**.
5. En unos minutos la web va a estar en `https://TUUSUARIO.github.io/panel-tareas/`.

## Cómo conectar con Google Sheets

1. Abrir la planilla en Google Sheets.
2. **Archivo → Compartir → Publicar en la web.**
3. Para cada hoja (`Personas`, `Tareas`, `Config`):
   - Seleccionar la hoja específica (no "Documento entero").
   - Seleccionar formato **CSV**.
   - Hacer clic en **Publicar** y copiar la URL.
4. Abrir el panel en el navegador.
5. Hacer clic en el ícono de engranaje (abajo a la derecha).
6. Pegar las 3 URLs y guardar.

> Las URLs se guardan en el navegador (localStorage). Cualquiera que abra la web ve los datos, pero las URLs configuradas son por dispositivo.

## Cargar las fotos de perfil

En la columna `foto_url` de la hoja `Personas`, escribir la ruta relativa:

```
fotos/neo.jpg
fotos/trinity.jpg
```

Si una foto no existe o está mal escrita, se muestra la inicial del nickname.

## Cómo usar día a día

### Crear una tarea nueva
Agregar fila en la hoja `Tareas`:
- `id`: T006, T007, etc.
- `titulo`, `descripcion`
- `asignado_a`: vacío (todavía nadie la tomó)
- `fecha_inicio`, `fecha_limite`: formato `YYYY-MM-DD HH:MM`
- `satoshis_base`: lo que pagás si entrega a tiempo
- `satoshis_bonus`: extra si además no tiene errores
- `estado`: `abierta`

### Cuando alguien la toma
- Cambiar `asignado_a` al nickname de la persona.
- Cambiar `estado` a `en_progreso`.

### Cuando se entrega
- Si está bien y a tiempo:
  - `estado` = `completada`
  - `a_tiempo` = `SI`
  - `sin_errores` = `SI` o `NO`
- Si no llegó a tiempo:
  - `estado` = `no_completada`
  - `a_tiempo` = `NO`

### Cuando le pagás los satoshis
- `pagado` = `SI` (esto es solo para tu control interno).

## Funcionalidades

- **Vista por tareas** (principal): tarjetas con estado, asignado, tiempo restante, satoshis en juego.
- **Ranking**: leaderboard de las 15 personas ordenado por satoshis ganados.
- **Historial**: tareas completadas y fallidas con badges (PERFECTA / OK / TARDE / FALLÓ).
- **Precio BTC en vivo**: traído de CoinGecko, equivalente USD calculado automáticamente.
- **Auto-refresh**: cada 60 segundos se actualizan los datos.
- **Responsive**: vista optimizada para celular.

## Reglas del sistema

- Una tarea solo la puede tomar una persona (la primera que se anota).
- El pago en satoshis solo se da si `estado = completada` Y `a_tiempo = SI`.
- El bonus extra solo se da si además `sin_errores = SI`.
- Los satoshis acumulados por persona se calculan automáticamente sumando sus tareas completadas a tiempo.
