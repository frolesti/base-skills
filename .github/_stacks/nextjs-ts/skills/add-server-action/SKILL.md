# Skill: add-server-action

> Crear una Next.js Server Action amb validació i retorn segur.

## Procediment

1. **Ubicació**: `app/<feature>/actions.ts` o `app/actions/<feature>.ts`. Tria una convenció i mantén-la per tot el projecte.
2. **Capçalera**: el fitxer comença amb `'use server'`.
3. **Schema d'entrada** amb Zod (o equivalent):

   ```ts
   import { z } from 'zod'
   const Input = z.object({ /* ... */ })
   ```

4. **Funció**:

   ```ts
   export async function myAction(formData: FormData) {
     const parsed = Input.safeParse(Object.fromEntries(formData))
     if (!parsed.success) {
       return { ok: false as const, error: 'Invalid input' }
     }
     // ... lògica
     return { ok: true as const, data: /* ... */ }
   }
   ```

5. **Auth**: si requereix usuari, comprova-ho aquí (no confiïs que el frontend ho hagi fet).
6. **Side effects**: després de mutar, crida `revalidatePath` o `revalidateTag` per refrescar caches.
7. **Crida des del client**:
   - Form: `<form action={myAction}>`.
   - Programàtica: `const res = await myAction(fd)`.
8. **Tracta el retorn al client**: comprova `res.ok` i ensenya error si cal.

## Errors comuns

- Oblidar `'use server'` → l'acció s'executa al client (insegur).
- Fer `throw` enlloc de retornar `{ ok: false }` → el client veu un 500 genèric.
- Oblidar `revalidatePath` → la UI no es refresca després de mutar.
