# TASK-019: Implementar el tema Klima light/dark

## Objetivo

Una aplicación activa la marca Klima con `data-brand="klima"` y obtiene su tipografía y su acción primaria sin redefinir variables.

## Contexto

- TASK-016 incorporó las primitivas de Klima, incluida `brand.klima.font.family.base` (Be Vietnam Pro).
- El componente `button` del archivo `Sistema de diseño KLIMA` (`pEieSZyQwDGKiAbeADq3ah`, nodo `1:1508`) tiene las variantes de relleno Default, Indigo, Blue y Green.
- En la primera lectura (2026-10-22), la variante Blue usaba texto `#EBEBEB` sobre `#4185F4` (2.99:1) y no cumplía AA. Diseño corrigió el componente ese mismo día. Variables tras la corrección, confirmadas con captura:

  | Variante | Fondo                                     | Texto                                     | Texto/fondo | Fondo vs canvas light | Fondo vs canvas dark |
  | -------- | ----------------------------------------- | ----------------------------------------- | ----------- | --------------------- | -------------------- |
  | Indigo   | `Klima/blue/Normal-hover/600` (`#2764E9`) | `Klima/white/Light/100` (`#F5F5F5`)       | 4.71        | 5.14                  | 3.70                 |
  | Blue     | `Klima/white/Light/100` (`#F5F5F5`)       | `Klima/blue/Normal-hover/600` (`#2764E9`) | 4.71        | 1.09                  | 17.44                |
  | Green    | `Klima/black/Normal/900` (`#1E1E1E`)      | `Klima/blue/Normal/500` (`#4185F4`)       | 4.67        | 16.67                 | 1.14                 |

- Figma no define hover ni active para la acción de Klima.

## Lecturas requeridas

- `docs/tasks/010-sole-theme.md` (selectores, validación y fixture).
- `docs/tasks/016-klima-primitives.md`.
- `docs/decisions/004-brand-strategy.md`.
- `docs/decisions/006-font-delivery.md`.

## Dependencias

- TASK-010 y TASK-016 completadas.

## Decisiones del propietario

- **Acción primaria.** La variante Indigo en ambos modos, porque es la única que cumple todos los pares de `CONTRAST_PAIRS` en light y dark: fondo `brand.klima.blue.600` y texto `brand.klima.white.100`. Hover y active repiten el valor por defecto hasta que diseño los especifique.
- **Tipografía.** `font.family.base` se sobrescribe con `brand.klima.font.family.base` (ADR-006).
- El resto de colores, incluida la acción secundaria, mantiene el contrato neutral de `@klima-ds/tokens`.

## Alcance

- `packages/themes/src/klima/font.json`, `light.json` y `dark.json`.
- Marca `klima` en `themes.json`.
- Export `@klima-ds/themes/klima.css`.
- Pruebas unitarias y de consumo desde tarballs.

## Fuera de alcance

- Acción secundaria y demás colores de marca.
- Carga de Be Vietnam Pro en la fixture (ADR-006 lo deja a la aplicación).
- Estilos de texto `typography` (ADR-005).

## Criterios de aceptación

- `[data-brand="klima"]` resuelve, en light, en dark y sin `data-theme`:
  - `--klima-font-family-base` a `"Be Vietnam Pro", system-ui, sans-serif`;
  - `--klima-color-action-primary-{default,hover,active}` a `#2764E9`;
  - `--klima-color-action-primary-foreground` a `#F5F5F5`.
- El resto de variables coincide con el contrato neutral.
- El CSS solo contiene reglas acotadas a `data-brand="klima"` y no depende del orden de las hojas.
- Cargar `klima.css` y `sole.css` a la vez no altera Solé.

## Verificación

```bash
pnpm --filter @klima-ds/themes test
pnpm --filter themes-fixture test
pnpm --filter @klima-ds/themes check:generated
```

## Pendiente

- Diseño debe definir hover y active de la acción primaria de Klima.
- Confirmar con diseño que Indigo es la variante primaria de Klima; el nombre de la variante no lo indica.
