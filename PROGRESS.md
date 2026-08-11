# PROGRESS — Optimización PDP clyren.store

Registro de cambios, más reciente arriba.

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
