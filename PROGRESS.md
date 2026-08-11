# PROGRESS — Optimización PDP clyren.store

Registro de cambios, más reciente arriba.

## 2026-08-11 — Sesión 2 (cont.): implementación completa en tema dev

Desbloqueado el acceso (egreso + `SHOPIFY_FLAG_STORE` corregidos por el
propietario; además el CLI requiere `NODE_USE_ENV_PROXY=1` — ver CLAUDE.md).
Creado el duplicado no publicado **"CLYREN PDP v2 (dev)" (id 186409550160)**
desde el live y aplicados allí todos los cambios. El tema live NO se ha tocado.

Cambios implementados (archivos del tema):

1. **Hero con hook** — `sections/main-product.liquid`: el bloque `title` acepta
   ajustes `hook_heading`/`hook_subheading` (definidos en
   `templates/product.json`); H1 "Higiene moderna para una rutina más limpia",
   subtítulo con separadores azules, nombre técnico debajo en pequeño. En móvil
   se renderiza un hero duplicado ANTES de la galería (`.clyren-hero-mobile`)
   y se oculta el del bloque title, para que sea lo primero above-the-fold.
2. **Pack 2 preseleccionado** — `templates/product.json` (bloque
   `clyren_pack_selector`): si no hay `?variant=` en la URL, al cargar se marca
   Formato="Pack 2" (server-side highlight + `checked`+`change` en
   DOMContentLoaded, sin `label.click()` para no robar el foco). Se mantiene la
   posición de scroll ~1,2 s porque la galería de Sense hace scrollIntoView al
   re-renderizar. Precio, input oculto del formulario, galería y sticky quedan
   sincronizados en Pack 2 (verificado: variante 53257776136528, 24,99 €).
3. **Precio/ud y ahorro** — selector de packs existente: redondeo hacia arriba
   (12,50 €/ud y 11,00 €/ud en vez de 12,49/10,99), "Ahorra"→"Ahorras", borde
   azul #5FB3E6 permanente en la fila Pack 2 (jerarquía visual), miniaturas con
   `&width=120` (de ~50 KB a ~4 KB cada una).
4. **Sticky ATC móvil** — nuevo `snippets/clyren-sticky-atc.liquid` (renderizado
   desde `main-product.liquid`): barra fija inferior <990px con imagen mini,
   variante+precio sincronizados y botón que envía el formulario nativo
   (respeta variante y cantidad). Visibilidad por scroll-listener (no
   IntersectionObserver: los saltos de scroll se saltan la transición). En
   previews se eleva 72px para no quedar bajo la barra "Draft" de Shopify.
5. **Entrega estimada** — `snippets/buy-buttons.liquid`: "Recíbelo entre el
   [+6 días] y el [+9 días]" calculado en Liquid con meses en español +
   "Envío con seguimiento incluido". Verificado: 17–20 de agosto (hoy día 11).
6. **Garantía bajo el botón** — mismo snippet: "✓ Garantía de devolución de
   30 días · ✓ Derecho de desistimiento de 14 días".
7. **"¿Y la espuma?"** — nueva sección `custom-liquid` en `product.json`,
   colocada justo antes de la sección de reseñas de Loox.
8. **Velocidad** — hallazgos: las PNG de la galería (~1,9 MB) las sirve el CDN
   como WebP de 70–120 KB a navegadores modernos → no crítico; galería ya con
   srcset+lazy. Acciones: eliminada la sección `apps` vacía huérfana,
   miniaturas del selector a `width=120`. Fuera del tema (no tocado): JS de
   Loox + Hoppy Trust Badges, y el banner de cookies de Shopify que cubre la
   PDP en la primera visita (ver "Pendiente").

Verificación automatizada en Chromium 375×812 (iPhone UA) contra el preview:
hero above-the-fold ✓ · Pack 2 preseleccionado con precio 24,99 € ✓ · sticky
aparece al pasar el botón y añade al carrito Negro/Pack 2 (24,99 €) ✓ ·
fechas 17–20 agosto ✓ · garantía ✓ · sección espuma ✓.

Pendiente / notas para el propietario:
- Revisar en móvil real y decidir publicación (manual, desde el admin).
- `compare_at_price` roto (7,11–7,23 €) — corregir en admin.
- El banner de cookies de Shopify tapa toda la PDP en la primera visita:
  revisar su configuración antes de la campaña (impacto directo en conversión
  de tráfico frío).
- La burbuja de chat "Ayuda" pisa el borde derecho del sticky en móvil:
  valorar subirla o desactivarla en PDP desde la app de chat.
- Regenerar el token de Theme Access al terminar.

## 2026-08-11 — Sesión 2: verificación de estado y bloqueos de acceso

**Sin cambios en el tema todavía** (pendiente de aprobación del plan y de
resolver bloqueos de red).

Verificado vía storefront público (`clyren.store`):

- Producto confirmado: 6 variantes (Color × Formato), precios 14,99 / 24,99 /
  32,99 €. `compare_at_price` roto (7,11–7,23 €, menor que el precio) →
  corregir en admin.
- Tema publicado: "CLYREN Experimento PDP" (Sense 15.4.1, id 185898008912),
  ya customizado por sesión anterior: selector de packs custom (`clyren-ps`)
  con precio/ud, ahorros y badge "★ Más vendido" en Pack 2; badge de urgencia;
  marquesina de anuncios en header; marquesina de beneficios; sección
  comparativa; sección vacía huérfana (`1777196258ca75d439`).
- App de reseñas: **Loox**. App embed adicional: Hoppy Trust Badges.
- Estado frente a los objetivos: Pack 2 NO preseleccionado (carga "Negro /
  1 unidad"); sin hero-hook H1; sin sticky ATC (Sense no trae opción nativa);
  sin línea de entrega estimada ni garantía junto al botón; objeción de la
  espuma no presente como sección propia.
- Velocidad: imágenes son PNG ~1,9 MB pero el CDN de Shopify sirve WebP
  (~70–120 KB) a navegadores modernos → no es el incendio que parecía.
  HTML de PDP 270 KB; JS de apps: Loox + Hoppy (4 ficheros).

**Bloqueos encontrados (impiden usar Shopify CLI):**

1. `SHOPIFY_FLAG_STORE=clyren.myshopify.com` es incorrecto — la tienda real
   es `x0gchz-0n.myshopify.com` (clyren.myshopify.com devuelve 402, es otra
   tienda). Corregir la variable de entorno.
2. La política de egreso deniega `theme-kit-access.shopifyapps.com`
   (obligatorio para CUALQUIER comando `shopify theme *` con token
   `shptka_`) y también `x0gchz-0n.myshopify.com`. Añadir ambos hosts a la
   lista de dominios permitidos del entorno.
3. Hasta resolver 1 y 2 no se puede ni validar el token.

Creados `CLAUDE.md` y `PROGRESS.md` en la rama
`claude/clyren-pdp-conversion-optimization-fy1pvo` (nota: la rama indicada
en el encargo, `...-1c72ey`, no existe en el remoto; esta sesión tiene
asignada `...-fy1pvo` y se documenta aquí).
