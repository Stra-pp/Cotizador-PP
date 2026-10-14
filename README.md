# Cotizador Playa Paraíso — anexo de la App Playa Paraíso

Mismo esquema que el Inventario A&B: una página en GitHub Pages que usa la **misma app de Microsoft**
(Client ID `3f28a168-…`, tenant del hotel), el **mismo sitio SharePoint** (`/sites/Prueba7`) y el **mismo padrón de usuarios**
(solo lectura de `ILDS/NEXO_Config.json`). Lo propio del cotizador:

| Qué | Dónde |
|---|---|
| Cotizaciones (un elemento por versión) | Lista `PP_Cotizaciones` (se crea sola) |
| Configuración: tarifas, calendario, extras, usuarios y PIN | `COTIZADOR/NEXO_Cotizador_Config.json` (misma biblioteca de la app principal) |

## Publicar (una sola vez)
1. Crear el repositorio `stra-pp/Cotizador-PP` y subir `index.html` a la raíz (rama `main`).
2. Settings → Pages → *Deploy from a branch* → `main` / `(root)`. La URL queda `https://stra-pp.github.io/Cotizador-PP/`.
3. En Microsoft Entra → la app `3f28a168-96e0-4b84-9845-caad8d358beb` → Authentication → *Single-page application* →
   agregar el Redirect URI **exacto** `https://stra-pp.github.io/Cotizador-PP/` (con la barra final).
   No pide permisos nuevos: usa `Files.ReadWrite.All`, `Sites.ReadWrite.All` y `User.Read`, que ya están consentidos para las otras apps.

## Primer ingreso
1. Abrir la URL → **Conectar con Microsoft**.
2. Elegir **Desarrollador** e ingresar el PIN del desarrollador que ya existe en el padrón de la app principal.
3. Configuración → **Usuarios y accesos** → asignar un PIN a Maira y a cada colaboradora (4 a 8 números; se muestra una sola vez, anótalo y entrégalo).
4. En esa misma pantalla, **Probar conexión**: revisa sesión, sitio, lista, configuración y padrón paso por paso.
   Si la cuenta no puede crear listas, crear en el sitio una lista genérica `PP_Cotizaciones` con una columna de varias líneas llamada `Data` y dos de texto: `Fecha` y `Estado`.

## Roles
| Rol | Quién | Puede |
|---|---|---|
| Desarrollador | — | Todo, incluido Calendario & Tarifas y cambiar roles |
| Aprobadora | Maira Suarez | Ver todas las cotizaciones e índices, abrir/ajustar cualquiera, Calendario & Tarifas, asignar PIN. No aprueba nada: es solo el rol. |
| Colaborador/a | Ariana V., Sheila B., Keyla B., Jonathan M., Daniela M., Lady R. | Cotizar; ver y ajustar solo **sus** cotizaciones; sin Calendario & Tarifas |

## Cotizaciones, ajustes e índice
- Cada cotización pide el **teléfono del cliente** (identifica al cliente) y se guarda con folio `PP-00001-2026`, en orden, desde la lista (sin choques).
- Guardar crea la **V1**. Volver a guardar la misma cotización con cambios de **precio, fechas, habitaciones/personas o servicio** crea una versión nueva = **1 ajuste**.
  Guardar sin cambios no cuenta; cambiar solo el nombre o el teléfono tampoco.
- Descargar PDF/imagen o copiar el WhatsApp guarda automáticamente la cotización.
- **Índice = ajustes ÷ cotizaciones**, por cliente y por usuario. Cada colaboradora ve el suyo; Maira y el desarrollador ven todos y pueden filtrar.

## Límites a tener presentes
- El control de acceso es **de pantalla** (igual que en las otras apps): oculta lo que no corresponde y bloquea la edición, pero cualquier persona con acceso al sitio SharePoint
  podría leer la lista o el archivo con herramientas técnicas. Para blindarlo, restringir los permisos de la lista `PP_Cotizaciones` y de la carpeta `COTIZADOR` a las personas autorizadas.
- Los PIN se guardan con un hash simple; es un candado de uso diario, no seguridad criptográfica. La mejora siguiente es el ingreso por correo de cada persona, que también está pendiente en la app principal.
- Los folios y la configuración empiezan de cero en este sistema; las tarifas y reglas vigentes al día de hoy vienen incluidas en el código.
- Modo demostración: abrir `index.html?demo=1` guarda todo solo en el navegador (para probar sin SharePoint). PIN del desarrollador en demo: `9999`.
