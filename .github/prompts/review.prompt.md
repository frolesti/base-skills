---
mode: 'agent'
description: 'Revisa els canvis no commitejats (o un PR) amb checklist.'
---

# /review

Fes una revisió crítica dels canvis. **No editis res**, només informa.

## Procediment

1. Obté els canvis:
   - Per defecte: diff local (`git diff` + `git diff --cached`).
   - Si l'usuari proporciona un PR/branca, fes `git diff <base>...<head>`.
2. Per a cada fitxer canviat, comprova:
   - **Correctesa**: la lògica fa el que diu el commit/PR?
   - **Seguretat**: secrets, injecció, validació d'entrada, OWASP top 10.
   - **Consistència**: segueix les convencions del repo i `.copilot/stack.json`?
   - **Tests**: hi ha cobertura per al canvi?
   - **Smells**: codi mort, duplicació, abstraccions prematures.
3. Estructura la sortida així:

```
### Resum
<2-3 línies>

### Bloquejants
- [fitxer:línia] descripció + suggeriment

### Suggeriments
- [fitxer:línia] descripció

### LGTM
- ...
```

4. No proposis grans refactors si no hi ha bug. **Mantingues l'scope del diff.**

## Context addicional (opcional)

${input:context:Branca, PR o focus específic (opcional)}
