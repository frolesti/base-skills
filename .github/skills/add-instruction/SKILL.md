# Skill: add-instruction

> Crea un fitxer `*.instructions.md` amb `applyTo` ben ajustat.

## Quan crear una instrucció (vs altres capes)

Crea instrucció **només si**:
- És una regla **curta** (idealment ≤ 30 línies).
- Aplica a un **subconjunt** de fitxers identificable per glob.
- Ha de carregar-se **automàticament** quan es treballa amb aquests fitxers.

Si aplica a tot el repo sempre → `copilot-instructions.md`.
Si és un procediment de molts passos → `skills/`.

## Procediment

1. **Ubicació**: `.github/instructions/<tema>.instructions.md` (o dins `_stacks/<stack>/instructions/` si és específic d'un stack).
2. **Nom**: kebab-case curt (`tsx.instructions.md`, `tests.instructions.md`, `server-actions.instructions.md`).
3. **Plantilla**:

```markdown
---
applyTo: '<glob>'
description: '<una línia>'
---

# <Títol>

- Regla 1 (imperativa).
- Regla 2.
- ...

## Què NO fer

- Anti-patró 1.
- Anti-patró 2.
```

4. **Ajusta `applyTo`** al màxim:
   - ✅ `'**/*.tsx'` per components React.
   - ✅ `'**/*.{test,spec}.{ts,tsx}'` per tests.
   - ✅ `'app/api/**/route.ts'` per route handlers Next.js.
   - ❌ `'**/*'` → equival a global; usa `copilot-instructions.md`.

5. **Verifica** que cap altra instrucció ja cobreixi el mateix glob — si sí, fusiona o re-ajusta els globs perquè no se solapin.

## Anti-patrons

- `applyTo: '**/*'` (és global, posa-ho a copilot-instructions).
- Instruccions de més de 50 línies (parteix-ho o convertint en skill).
- Repetir el mateix consell en diverses instruccions (DRY: extreu a una sola amb glob més ampli).
