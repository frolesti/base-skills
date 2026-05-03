---
mode: 'ask'
description: 'Audita ús de skills (telemetria local) per detectar skills mortes.'
---

# /skills-audit

Llegeix `.copilot/usage.json` i mostra un informe de quines skills s'usen
i quines no. Suggereix candidates a esborrar.

## Procediment

1. Llegeix `.copilot/usage.json`. Si no existeix, informa que la telemetria encara no s'ha inicialitzat (ho fa el bootstrap).
2. Llista totes les skills presents a `.github/skills/` i `.github/_stacks/*/skills/`.
3. Compara amb les claus de `usage.json` i mostra:

```
### Usades
- bootstrap: 1 (fa X dies)
- add-route: 7

### No usades mai
- deploy-fly
- add-migration

### Suggeriment
Considera moure les no usades a un branca-arxiu o eliminar-les si fa > 90 dies del repo.
```

4. **No esborris res automàticament.** Només informa.
