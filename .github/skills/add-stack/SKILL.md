# Skill: add-stack

> Afegeix un nou paquet de stack a `.github/_stacks/` perquè el bootstrap el pugui activar.

## Quan usar-la

- Quan el bootstrap detecta o l'usuari demana un stack que encara no existeix a `_stacks/`.
- Quan vols preparar un paquet per a futurs projectes creats des de template.

## Procediment

1. **Decideix el nom**: kebab-case curt i descriptiu. Exemples:
   - Base: `nextjs-ts`, `vite-react-ts`, `astro`, `python-fastapi`, `node-cli`, `rust-axum`.
   - Complement: `supabase`, `prisma`, `drizzle`, `tailwind`, `shadcn`, `tanstack-query`.

2. **Crea l'estructura**:

```
.github/_stacks/<nom>/
├── README.md            ← què aporta aquest paquet (1 paràgraf)
├── instructions/        ← *.instructions.md amb applyTo específics
└── skills/              ← <skill>/SKILL.md
```

3. **`README.md` del paquet** ha d'incloure:
   - **Aporta**: llista de fitxers que copia.
   - **Requereix**: si depèn d'un altre paquet (p.ex. `prisma` requereix una BD).
   - **Comandes** que el bootstrap ha d'afegir a `copilot-instructions.md` un cop activat.

4. **Instruccions del paquet**: segueix [add-instruction](../add-instruction/SKILL.md). `applyTo` ha de ser **específic del stack** (p.ex. `app/**/*.tsx` per a Next.js, no `**/*.tsx` genèric).

5. **Skills del paquet**: segueix [add-skill](../add-skill/SKILL.md). Bones candidates típiques:
   - `add-route` / `add-page` / `add-component`.
   - `add-migration` / `add-model`.
   - `deploy-<target>`.
   - `add-test`.

6. **Proves**: crea un projecte de prova des de la template en una carpeta local, executa el bootstrap escollint el nou paquet i verifica que:
   - Els fitxers es copien on toca.
   - `copilot-instructions.md` queda < 60 línies.
   - Els `applyTo` s'activen amb fitxers d'exemple del stack.

## Combinabilitat

Els paquets s'han de poder **combinar** sense col·lisions. Regles:

- Si dos paquets toquen el mateix glob → posa el contingut comú a una skill compartida i deixa que cada paquet la referenciï.
- Si un paquet sobreescriu un fitxer d'un altre, documenta-ho explícitament al `README.md` del paquet i marca'l com a **incompatible** amb l'altre.

## Anti-patrons

- Paquets monolítics que barregen base + complements (p.ex. `nextjs-supabase-tailwind` tot junt). **Separa'ls.**
- Paquets sense `README.md` → el bootstrap no sap què aporten.
- Skills genèriques al paquet que no usen res del stack → mou-les a `.github/skills/` arrel.
