---
mode: 'agent'
description: 'Genera contingut per promocionar aquest repo al portfolio personal i a una newsletter.'
---

# /promote

Genera material de promoció per a aquest projecte. **No publiquis res
tu**, només genera el contingut perquè l'usuari el copïi al seu lloc/
butlletí.

## Procediment

1. Llegeix [README.md](../../README.md) i [docs/00-introduccio.md](../../docs/00-introduccio.md) per entendre el projecte.
2. Pregunta a l'usuari (amb `vscode_askQuestions` si cal) **només** el que no es pugui inferir:
   - Stack del seu portfolio (Next.js, Astro, Hugo, manual…).
   - URL pública del repo (per als enllaços).
   - To desitjat: tècnic-pur, divulgatiu, o híbrid.
   - Idioma del portfolio i de la newsletter (CA/ES/EN). Per defecte CA.
3. Genera **tres peces** en blocs de codi separats, llests per copiar:

### Peça 1 — Targeta del projecte per al portfolio

Markdown o HTML segons l'stack del portfolio. Camps:

- **Títol**.
- **Subtítol** (1 línia).
- **Descripció curta** (3-4 línies).
- **Tags / tecnologies** (Copilot, AI agents, DX, plantilla).
- **CTA**: link al repo + "Forka i prova".
- **Captura suggerida**: descriu què hauria de mostrar la imatge
  d'acompanyament (no la generis).

### Peça 2 — Article curt per al portfolio (~250 paraules)

Estructura:

- **Hook**: el problema (instruccions globals = cost a cada torn).
- **Solució**: arquitectura per capes + auto-adaptació al stack.
- **Què aporta a la lectora**: 3 beneficis concrets.
- **Crida a l'acció**: forka el repo, prova-ho, comparteix feedback.

### Peça 3 — Secció per a la newsletter de maig

Format compacte (≤ 120 paraules) amb:

- Títol cridaner però honest.
- 2-3 línies de context.
- Bullet amb 3 features destacades.
- Link al repo.
- (Opcional) cita d'un fragment del README que faci enveja llegir.

## Què NO fer

- No inventis mètriques (`80% menys tokens`) si no estan al repo.
- No promotis funcionalitats que encara no existeixen (Fase 5 paquets
  addicionals, comptador de tokens, etc.). Si vols mencionar-les, deixa
  clar que són roadmap.
- No facis servir hype buit: "revolutionary", "game-changing"…

## Context addicional (opcional)

${input:context:Detalls addicionals (audiència, longitud, focus)}
