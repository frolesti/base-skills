# 03 — Modes (chatmodes)

Els chatmodes són **agents personalitzats** amb tools restringides i
comportament definit. Quan triïs un mode, l'agent **NO podrà** sortir-se
del seu rol encara que li ho demanis.

## Com triar un mode

1. Obre **Copilot Chat** (panel lateral o `Ctrl+Alt+I`).
2. A la part inferior del panel hi ha un selector que diu **"Agent"**,
   **"Ask"** o el nom d'un mode personalitzat.
3. Fes-hi clic → desplegable amb tots els modes disponibles, inclosos
   els d'aquest repo.
4. Tria'n un. La conversa passa a respectar les seves regles.

> Si no hi veus els modes personalitzats, tanca i reobre VS Code. La
> primera detecció després de crear-los pot trigar.

---

## `planner` — Planifica sense editar

**Tools:** només lectura + `manage_todo_list` + `vscode_askQuestions`.

**Quan usar-lo:**
- Sessions de disseny on vols pensar en veu alta amb l'agent sense risc
  que toqui codi.
- Per garantir que l'agent no es llanci abans d'entendre la tasca.

**Diferència amb `/plan`:**
- `/plan` és una comanda de **un torn** que genera un pla i s'atura.
- El mode `planner` manté **tota la sessió** en mode planificació. Pots
  tenir 10 torns de conversa refinant el pla.

**Exemple de flux:**

```
[Tries mode: planner]
Tu: vull migrar de Pages Router a App Router
Agent: explora codi... 3 preguntes...
Tu: [respons]
Agent: pla v1
Tu: i si en lloc d'això fem...
Agent: pla v2
[Quan estiguis content, tornes al mode Agent normal i diuses:
  "Executa el pla anterior."]
```

---

## `reviewer` — Revisa sense editar

**Tools:** només lectura + accés a canvis (`git`).

**Quan usar-lo:**
- Code review formal (no només `/review`).
- Sessions on l'usuari vol discutir un diff sense risc d'edicions.

**Diferència amb `/review`:**
- `/review` torna un informe i s'atura.
- El mode `reviewer` permet **dialogar** sobre el diff: "i aquesta funció,
  per què la consideres segura?", "què passaria si X?", etc.

---

## `test-writer` — Només toca tests

**Tools:** lectura + edició restringida + execució de tests.

**Restricció clau:** només pot editar fitxers que matchegin patrons de
test (`*.test.*`, `*.spec.*`, `__tests__/**`, `test_*.py`, `*_test.go`…).

**Quan usar-lo:**
- Quan vols garantir que el codi de producció **no es modifica** mentre
  s'afegeixen tests.
- Per fer "test gardening" massiu (afegir cobertura a un mòdul sencer).

Si li demanes "arregla el bug i afegeix tests", **et dirà que canvïs de
mode** per al primer pas.

---

## `doc-writer` — Només toca `.md`

**Tools:** lectura + edició restringida a `.md`/`.mdx`.

**Quan usar-lo:**
- Sessions de documentació pura (README, ADRs, guies).
- Quan vols evitar que l'agent "aprofiti" per refactoritzar codi mentre
  documentes.

---

## Quan NO usar un mode personalitzat

L'agent per defecte (**Agent**) té totes les tools. Per a tasques
multi-disciplinàries (codi + tests + docs alhora), **deixa l'agent per
defecte** i usa les comandes `/` per orientar-lo.

Els modes són per **sessions monogràfiques**.

---

## Combinant modes i comandes

Els modes són ortogonals a les comandes `/`:

| Vull... | Mode | Comanda |
|---|---|---|
| Planejar una feature gran | `planner` | `/plan` (al mode `planner`) |
| Revisar diff llest per commit | `reviewer` | `/review` |
| Generar missatge de commit | (qualsevol) | `/commit` |
| Afegir tests a un fitxer | `test-writer` | `/test` |
| Documentar el README | `doc-writer` | — |

Pots combinar-ho. Exemple: entres al mode `test-writer`, escrius `/test`
i estàs doblement protegida (la comanda diu què fer + el mode garanteix
que no surt de l'scope).
