---
mode: 'agent'
description: 'Genera un pla revisable per a una tasca abans de tocar codi.'
---

# /plan

Genera un pla per a la tasca descrita. **No editis cap fitxer encara.**

## Procediment

1. Si la petició és ambigua, fes **fins a 3 preguntes** crítiques amb `vscode_askQuestions`. No més.
2. Explora el codi necessari (read-only). Usa l'agent `Explore` si la cerca és àmplia.
3. Produeix un pla amb:
   - **Objectiu** (1 línia).
   - **Fitxers afectats** (llista amb ruta i tipus de canvi).
   - **Passos** numerats (cadascun = un canvi atòmic).
   - **Riscos / decisions obertes**.
   - **Com validar** (test, comanda, comprovació visual).
4. Crida `manage_todo_list` amb els passos.
5. Atura't i espera confirmació de l'usuari abans de continuar.

## Tasca

${input:task:Descriu la tasca a planificar}
