---
mode: 'agent'
description: 'Genera un missatge de commit Conventional Commits a partir del diff staged.'
---

# /commit

Genera **només** el missatge de commit. No facis `git commit` tu — l'usuari decideix.

## Procediment

1. Obté el diff staged: `git diff --cached`.
   - Si està buit, avisa i atura't.
2. Determina:
   - **type**: `feat | fix | docs | style | refactor | perf | test | build | ci | chore | revert`.
   - **scope** (opcional): mòdul/àrea afectada (mira els paths).
   - **breaking change**: només si el diff trenca API pública.
3. Format Conventional Commits:

```
<type>(<scope>): <subject curt en imperatiu, ≤ 72 chars>

<body opcional explicant el "perquè", no el "què">

<footer opcional: BREAKING CHANGE: ..., Refs: #123>
```

4. Idioma del missatge: **anglès** (estàndard internacional) tret que `.copilot/stack.json` indiqui el contrari.
5. Mostra'l en un bloc de codi llest per copiar.
