# Stack: nextjs-ts

Paquet base per a projectes **Next.js (App Router) + TypeScript**.

## Aporta

- `instructions/tsx.instructions.md` — convencions per a components React.
- `instructions/server-actions.instructions.md` — regles per a server actions.
- `instructions/route-handlers.instructions.md` — regles per a `app/**/route.ts`.
- `skills/add-route/SKILL.md` — afegir nova ruta App Router.
- `skills/add-server-action/SKILL.md` — crear server action amb validació.

## Requereix

- Node ≥ 20.
- Un gestor de paquets JS (`npm`, `pnpm`, `yarn`, `bun`).

## Comandes que el bootstrap afegeix a `copilot-instructions.md`

```
dev:        <pm> run dev
build:      <pm> run build
test:       <pm> run test
lint:       <pm> run lint
typecheck:  <pm> exec tsc --noEmit
```

(`<pm>` = el gestor triat al bootstrap)

## Combinabilitat

- ✅ Compatible amb: `tailwind`, `supabase`, `prisma`, `drizzle`, `shadcn`.
- ❌ Incompatible amb: altres bases (`vite-react-ts`, `astro`, etc.) — només una base per projecte.
