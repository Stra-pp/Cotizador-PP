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

## Primer ingreso y PIN
1. El **desarrollador** abre la URL → **Conectar con Microsoft** → entra con su PIN de la app principal.
2. Configuración → **Usuarios y accesos** (visible solo para el desarrollador) → **Restablecer PIN** genera un **PIN provisional de 6 dígitos** para esa persona.
   Se muestra una sola vez y en SharePoint solo queda cifrado; se le entrega por correo.
3. Al ingresar con el PIN provisional, la app **obliga a crear un PIN personal** (4 a 8 números, sin repetir el mismo dígito ni secuencias tipo 1234). Queda guardado en SharePoint y el provisional deja de servir.
4. Cualquier persona puede cambiar su PIN cuando quiera con el botón **Cambiar PIN** de la cabecera. Si alguien lo olvida, el desarrollador le restablece uno provisional.
5. **Probar conexión** (en Usuarios y accesos) revisa sesión, sitio, lista, configuración y padrón paso por paso.
   Si la cuenta no puede crear listas, crear en el sitio una lista genérica `PP_Cotizaciones` con una columna de varias líneas llamada `Data` y dos de texto: `Fecha` y `Estado`.

## Roles
| Rol | Quién | Puede |
|---|---|---|
| Desarrollador | — | Todo, incluido Calendario & Tarifas y **Usuarios y accesos** (roles, PIN, altas y bajas): esa opción solo la ve el desarrollador |
| Aprobadora | Maira Suarez | Ver todas las cotizaciones e índices, abrir/ajustar cualquiera y Calendario & Tarifas. No administra usuarios ni PIN, y no aprueba nada: es solo el rol. |
| Colaborador/a | Ariana V., Sheila B., Keyla B., Jonathan M., Daniela M., Lady R. | Cotizar; ver y ajustar solo **sus** cotizaciones; sin Calendario & Tarifas |

## Cotizaciones, ajustes e índice
- Cada cotización pide **cómo escribió el cliente**: por WhatsApp o llamada se identifica con su **teléfono**; por **correo electrónico**, con su **correo** (se valida que tenga formato de correo y se reconoce igual en mayúsculas o minúsculas); si llegó por **Instagram, Facebook o TikTok** no hace falta teléfono: se identifica con su **nombre de usuario** (se reconoce igual con o sin @, en mayúsculas o minúsculas, o pegando el enlace del perfil). El mismo usuario en dos redes distintas cuenta como dos clientes.
- Se guarda con folio `PP-00001-2026`, en orden, desde la lista (sin choques).
- Guardar crea la **V1**. Volver a guardar la misma cotización con cambios de **precio, fechas, habitaciones/personas o servicio** crea una versión nueva = **1 ajuste**.
  Guardar sin cambios no cuenta; cambiar solo el nombre o el teléfono tampoco.
- Descargar PDF/imagen o copiar el WhatsApp guarda automáticamente la cotización.
- **Índice = ajustes ÷ cotizaciones**, por cliente y por usuario. Cada colaboradora ve el suyo; Maira y el desarrollador ven todos y pueden filtrar.

## Congruencia de la información
- Check-in / check-out: Hospedaje y paquetes festivos usan 15h00 / 12h00; el **Day Use usa 10h00 / 18h00** (se editan por separado en Configuración → Day Use y Eventos). La tarjeta, el PDF/imagen y el WhatsApp muestran siempre los mismos horarios.
- Las cuatro pestañas (Hospedaje, Pool Day, Day Use y Eventos) comparten el mismo orden en el texto de WhatsApp: lo que incluye, el resumen con el total, la frase de moneda, las condiciones y el mismo cierre. En la tarjeta, el pie siempre dice "Cotización emitida el …".
- **Adicionales por persona** (Early check-in, Late check-out, uso anticipado/extendido de instalaciones): en cada uno se indica **cuántas personas** lo usan, o se toca **"Todos (N)"**, donde N es la suma de las personas de todas las habitaciones agregadas. Lo marcado con "Todos" se ajusta solo al agregar o quitar habitaciones; lo escrito a mano no se toca.
- **Paquetes de Navidad y Fin de Año**: el paquete de 1, 2, 3 o 4 noches se puede cotizar con **cualquier fecha de llegada** que incluya la noche especial (24-dic en Navidad, 31-dic en Fin de Año). Por ejemplo, 4 noches de Fin de Año: del 28-dic al 1-ene o del 30-dic al 3-ene. El total del paquete depende solo de las noches y se toma del PDF; si la llegada no incluye la noche especial, la app avisa que no aplica.
- **Pool Day Full Board** (entre semana): adulto $35 y niño $25 **con impuestos** ($28 y $20 sin impuestos en la configuración). Incluye uso de instalaciones, cóctel de bienvenida y almuerzo. Solo se cotiza de **lunes a jueves**, sin feriados ni puentes; en otras fechas la app avisa que no está disponible.
- **Tarifa por fecha de llegada**: al final de la cotización de hospedaje (tarjeta, PDF/imagen y WhatsApp) se muestran 7 fechas con la pedida al centro (3 antes y 3 después) y el valor del hospedaje de la misma habitación y noches; las fechas pasadas salen como "No disp.".
- **Planes por día** (Configuración → Planes por día): define qué planes (Bed & Breakfast, Full Board) se pueden cotizar cada día de la semana y en feriados/puentes, por defecto o por temporada. All Inclusive siempre está disponible. En el cotizador solo se habilitan los planes que aplican; si una estadía mezcla días (por ejemplo jueves y viernes) se arma un plan combinado y se detalla qué incluye cada noche. Por defecto, Bed & Breakfast y Full Board aplican de domingo a jueves y no en feriados.
- **PDF e imagen**: la información importante (check-in/out con su hora, total, condiciones en viñetas) se muestra en tamaño grande para leerse bien en el celular. La condición "Los valores abonados no son reembolsables" ya no aparece en las cotizaciones de hospedaje.
- Los **días puente** (fines de semana largos) llevan tarifa de feriado: feriado en lunes → sábado y domingo anteriores; en viernes → sábado y domingo siguientes; en martes → sábado, domingo y lunes; en jueves → viernes, sábado y domingo. Se puede apagar en Configuración → Feriados.

## Límites a tener presentes
- El control de acceso es **de pantalla** (igual que en las otras apps): oculta lo que no corresponde y bloquea la edición, pero cualquier persona con acceso al sitio SharePoint
  podría leer la lista o el archivo con herramientas técnicas. Para blindarlo, restringir los permisos de la lista `PP_Cotizaciones` y de la carpeta `COTIZADOR` a las personas autorizadas.
- Los PIN se guardan con un hash simple; es un candado de uso diario, no seguridad criptográfica. El PIN provisional viaja por correo: por eso se obliga a cambiarlo en el primer ingreso. La mejora siguiente es el ingreso por correo de cada persona, que también está pendiente en la app principal.
- Los folios y la configuración empiezan de cero en este sistema; las tarifas y reglas vigentes al día de hoy vienen incluidas en el código.
- Modo demostración: abrir `index.html?demo=1` guarda todo solo en el navegador (para probar sin SharePoint). PIN del desarrollador en demo: `9999`.
