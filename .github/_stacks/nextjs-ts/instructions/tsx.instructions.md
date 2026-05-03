---
applyTo: '**/*.tsx'
description: 'Convencions per a components React (App Router).'
---

# Components React / TSX

- **Server Components per defecte.** Marca `'use client'` només si necessites estat, efectes o APIs del navegador.
- Props tipades amb `interface` o `type`. Cap `any`.
- Noms en `PascalCase`. Un component per fitxer.
- Exporta el component com a **default** si el fitxer és una `page.tsx`/`layout.tsx`/`loading.tsx`/`error.tsx`/`not-found.tsx`. Si no, named export.
- Estils: classes utilitàries (Tailwind si està actiu) o CSS modules. **No** estils inline llargs.
- Fetch de dades: `async` Server Components > `use client` + `useEffect`.
- Imatges: `next/image`. Links interns: `next/link`.

## Què NO fer

- No facis `useState`/`useEffect` en un Server Component.
- No facis crides a la BD ni accedeixis a secrets dins components `'use client'`.
- No envoltis tota la pàgina en `'use client'` per estalviar marcadors.
