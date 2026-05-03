# 05 — Estendre el repo

Tres operacions habituals: afegir una skill, afegir una instrucció,
afegir un stack nou.

## Afegir una skill nova

Demana a l'agent:

```
Llegeix .github/skills/add-skill/SKILL.md i crea una skill anomenada
<nom> per <descripció>.
```

Ell seguirà el procediment formal: ubicació, plantilla, anti-patrons.

**Tu has de decidir:**
- Skill genèrica del repo (`.github/skills/`) o específica d'un stack
  (`.github/_stacks/<stack>/skills/`)?
- Nom (kebab-case, verb + objecte: `add-route`, `deploy-vercel`).

**Quan és skill (vs altres capes):**
- Procediment > 3 passos.
- Es repetirà.
- No aplica sempre — l'agent l'ha de llegir conscientment.

## Afegir una instrucció per glob

Demana a l'agent:

```
Llegeix .github/skills/add-instruction/SKILL.md i crea una instrucció per
<glob> amb les regles següents: ...
```

**Tu has de decidir:**
- Quin glob `applyTo` (com més estret, millor).
- 5-15 regles imperatives (no descriptives).

**Quan és instrucció (vs altres capes):**
- Aplica a un subconjunt de fitxers identificable per glob.
- És curta (≤ 30 línies idealment).
- Ha de carregar-se automàticament.

## Afegir un stack

Demana a l'agent:

```
Llegeix .github/skills/add-stack/SKILL.md i crea el paquet <nom> amb
les instruccions i skills bàsiques per a <descripció>.
```

**Tu has de decidir:**
- Nom (kebab-case: `python-fastapi`, `astro`, `rust-axum`).
- Quines instruccions inicials (mínim 1-2 amb `applyTo` ben pensat).
- Quines skills inicials (mínim 1-2: típicament `add-<entitat>`,
  `deploy-<target>`).
- Si és **base** (només una per projecte) o **complement**
  (combinable: `tailwind`, `supabase`…).

Després, perquè el bootstrap el pugui activar a futurs forks, ja queda
disponible automàticament a `.github/_stacks/`.

## Crear un prompt nou

No tenim skill formal per a això (encara) — fes-ho manualment:

1. Crea `.github/prompts/<nom>.prompt.md`.
2. Frontmatter:
   ```yaml
   ---
   mode: 'agent'   # o 'ask' si no ha de poder editar
   description: 'Una línia.'
   ---
   ```
3. Cos: procediment imperatiu numerat. Acaba amb un placeholder per a
   l'input de l'usuari si cal:
   ```markdown
   ## Tasca
   ${input:task:Descriu què vols}
   ```

## Crear un chatmode nou

1. Crea `.github/chatmodes/<nom>.chatmode.md`.
2. Frontmatter:
   ```yaml
   ---
   description: 'Una línia.'
   tools: ['codebase', 'search', 'editFiles', ...]
   ---
   ```
3. Cos: defineix el rol, les regles i les prohibicions. Inclou
   explícitament què NO pot fer.

Mira els existents a [.github/chatmodes/](../.github/chatmodes/) com a
referència.
