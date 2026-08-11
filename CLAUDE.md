# Proyecto: Optimización de conversión PDP — clyren.store

## Qué es este repo
Este repositorio (`AsadSaidi/AsadSaidi`) es el README de perfil de GitHub del
propietario. **No contiene el tema de Shopify.** Se usa únicamente para
documentar el trabajo de optimización de la PDP de la tienda Shopify
`clyren.store`:

- `CLAUDE.md` — contexto del proyecto (este archivo).
- `PROGRESS.md` — registro de cambios con fecha.

El tema se trabaja directamente vía Shopify CLI (`shopify theme pull/push`)
contra un **tema no publicado**. Nunca se edita el tema Live directamente;
la publicación es siempre manual tras revisión del propietario.

## La tienda
- Dominio público: `clyren.store`
- Dominio interno real: **`x0gchz-0n.myshopify.com`** (¡ojo! NO es
  `clyren.myshopify.com` — ese dominio pertenece a otra tienda y devuelve
  402 "Store unavailable").
- Tema publicado: **"CLYREN Experimento PDP"** (id `185898008912`),
  base **Sense 15.4.1**, ya customizado por una sesión anterior.
- App de reseñas: **Loox** (bloque de app en la PDP + rating bajo el título).
  No tocar desde el tema: la purga de reseñas la hace el propietario en la app.
- Otra app activa: Hoppy Trust Badges (app embed).

## El producto
- Handle: `clyren-clean-brush-core-sin-mango` — "CLYREN Clean Brush Core (sin mango)"
- Producto id: `10521503236432`
- Dos opciones: **Color** (Negro, Gris) × **Formato** (1 unidad, Pack 2, Pack 3)
  → 6 variantes.
- Precios (ambos colores): 1 unidad **14,99 €** · Pack 2 **24,99 €** ·
  Pack 3 **32,99 €**.
- ⚠️ Dato roto: `compare_at_price` = 7,11–7,23 € (MENOR que el precio).
  Debe corregirse desde el admin (vaciarlo o ponerlo por encima del precio);
  no es editable con el token de Theme Access.

## Autenticación / entorno
- Variables: `SHOPIFY_CLI_THEME_TOKEN` (token Theme Access `shptka_...`) y
  `SHOPIFY_FLAG_STORE` (debe ser `x0gchz-0n.myshopify.com`, no clyren.myshopify.com).
- El Shopify CLI con token Theme Access enruta TODO por
  `theme-kit-access.shopifyapps.com` — ese host debe estar permitido en la
  política de egreso del entorno, además de `x0gchz-0n.myshopify.com`.
- ⚠️ En este entorno remoto el CLI necesita `NODE_USE_ENV_PROXY=1` y
  `NODE_EXTRA_CA_CERTS=/root/.ccr/ca-bundle.crt` (el fetch nativo de Node
  ignora HTTPS_PROXY); sin eso da el error engañoso "don't have access to
  this dev store". El `read ECONNRESET` final tras cada comando es solo la
  telemetría bloqueada — inofensivo.
- Copia de trabajo del tema: `/home/user/clyren-theme` (repo git local propio,
  fuera del repo de docs).
- Seguridad: el token fue pegado en un chat en algún momento →
  **regenerarlo al terminar el proyecto**.

## Tema de desarrollo
- **"CLYREN PDP v2 (dev)"** (id `186409550160`), duplicado no publicado del
  live con todos los cambios de la sesión 2026-08-11.
- Preview: `https://clyren.store/products/clyren-clean-brush-core-sin-mango?preview_theme_id=186409550160`

## Restricción legal absoluta en el copy
PROHIBIDO: "elimina bacterias", "antibacteriano", "mata gérmenes",
"35 veces más higiénico", comparaciones porcentuales de higiene, claims
médicos (acné, curar, tratar), "resultados garantizados".
PERMITIDO: "no retiene humedad", "se seca rápido/solo", "fácil de aclarar",
"más higiénico", "alternativa moderna", "material duradero", "suave con la
piel", "diseñado para durar".

## Paleta / estilo
Texto `#2E2A39` · acento azul `#5FB3E6` · fondo suave `#8ECDF5` al 15% ·
tipografía del tema.

## Flujo de trabajo acordado
1. Verificar estado real antes de tocar nada; presentar hallazgos y plan.
2. Mostrar el diff de cada archivo antes de aplicarlo; no ejecutar sin
   aprobación del plan inicial.
3. Todos los cambios sobre un duplicado no publicado del tema.
4. Actualizar `PROGRESS.md` al terminar cada cambio.
5. Verificación final en viewport móvil 375px + URL de preview del tema
   no publicado para revisión manual antes de publicar.
