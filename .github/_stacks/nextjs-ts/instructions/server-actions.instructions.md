---
applyTo: '**/actions/**/*.ts'
description: 'Regles per a Next.js Server Actions.'
---

# Server Actions

- Cada fitxer comença amb `'use server'`.
- **Valida sempre** l'entrada amb un schema (Zod o equivalent) abans de tocar res.
- Retorna sempre un objecte serializable: `{ ok: true, data } | { ok: false, error }`. No throw cap a `'use client'`.
- Crida `revalidatePath` / `revalidateTag` quan modifiquis dades cachejades.
- No exposis secrets ni IDs interns al retorn.

## Què NO fer

- No usis server actions per a lectures pures (usa Server Components).
- No facis redirects sense validació prèvia.
- No assumeixis que l'usuari està autenticat — comprova-ho dins l'acció.
