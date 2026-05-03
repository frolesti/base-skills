---
mode: 'agent'
description: 'Afegeix o millora tests per al fitxer/funció indicat.'
---

# /test

Afegeix tests per a l'objectiu indicat. Treballa **només** en fitxers de test.

## Procediment

1. Identifica l'objectiu:
   - Si l'usuari indica fitxer/funció, usa-ho.
   - Si no, usa el fitxer obert a l'editor.
2. Llegeix `.copilot/stack.json` per saber el framework de test (vitest, jest, pytest…).
3. Localitza tests existents propers; segueix-ne el patró (estructura, helpers, fixtures).
4. Genera tests que cobreixin:
   - **Camí feliç** principal.
   - **Casos límit** evidents (null, buit, límits numèrics).
   - **Errors esperats** (què ha de fallar i com).
5. **No modifiquis** el codi de producció. Si el codi no és testable, **informa-ho** com a bloquejant en lloc de refactoritzar-lo.
6. Executa els tests (si l'entorn ho permet) i mostra el resultat.

## Objectiu

${input:target:Fitxer, funció o àrea a testar (opcional, per defecte el fitxer obert)}
