# Skill: add-route (Next.js App Router)

> Afegir una nova ruta a `app/`.

## Procediment

1. **Decideix la ruta**:
   - Pública estàtica → `app/<segment>/page.tsx`.
   - Dinàmica → `app/<segment>/[param]/page.tsx`.
   - Agrupada (no afecta URL) → `app/(group)/<segment>/page.tsx`.
   - API → veure [add-route-handler](../add-route-handler/SKILL.md) si existeix, si no segueix `route-handlers.instructions.md`.
2. **Crea els fitxers necessaris**:
   - `page.tsx` (obligatori).
   - `layout.tsx` (si la secció necessita layout propi).
   - `loading.tsx` (per skeleton durant suspense).
   - `error.tsx` (per error boundary). Marca'l `'use client'`.
   - `not-found.tsx` (si la ruta pot retornar 404).
3. **Decideix Server vs Client**:
   - Per defecte Server Component.
   - Marca `'use client'` només si cal estat/efectes/APIs del navegador.
4. **Metadata**: exporta `metadata` (estàtica) o `generateMetadata` (dinàmica) amb títol i descripció.
5. **Tipa els params**: `{ params: Promise<{ slug: string }> }` (Next 15+) o `{ params: { slug: string } }` segons versió.
6. **Si depèn de dades**:
   - `fetch` amb opcions de cache explícites.
   - O `await` a una funció de `lib/`.
7. **Linka-la** des del lloc adequat (menú, llista…) amb `<Link>`.
8. **Verifica** amb `<pm> run dev` i navega-hi.

## Errors comuns

- Oblidar `'use client'` en `error.tsx` → fallida en runtime.
- Posar `'use client'` a `page.tsx` només per usar `<Link>` (no cal).
- Mutar dades dins un Server Component sense server action → no funciona.
