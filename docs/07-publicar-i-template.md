# 07 — Publicar el repo i arrencar un projecte real des de template

Aquest document cobreix dos casos:

1. **Publicar aquest repo base** al teu perfil de GitHub.
2. **Fer-lo servir per arrencar un projecte real** des de template.

---

## Part A — Publicar aquest repo base al teu perfil de GitHub

Si la carpeta local encara no té `.git/`, primer cal inicialitzar git,
fer el primer commit i publicar.

### Opció ràpida amb GitHub CLI (`gh`)

```powershell
git init
git add .
git commit -m "chore: initialize base skills repository"
gh repo create <repo-name> --source . --remote origin --push --public
gh repo edit <repo-name> --template
```

Si el vols privat, canvia `--public` per `--private`.

### Verificació mínima

Comprova que el repo de GitHub mostra:

- `README.md`
- `.github/`
- `docs/`
- `.vscode/`

I confirma que el repo és template:

```powershell
gh repo view <owner>/<repo-name> --json isTemplate,url
```

---

## Part B — Crear un projecte real amb aquest template

Aquesta és l'única via suportada en aquest repositori.

### Pas a pas

1. Entra al repo base a GitHub.
2. Fes clic a **Use this template**.
3. Crea un repo nou (per exemple `my-real-project`).
4. Clona'l en local:

   ```powershell
   git clone https://github.com/<user>/my-real-project
   cd my-real-project
   code .
   ```

5. Obre Copilot Chat i descriu el producte que vols construir.
6. Com que encara no existirà `.copilot/stack.json`, el bootstrap hauria de:
   - llegir la skill `bootstrap`
   - detectar si el projecte és buit
   - preguntar stack base i complements
   - activar només els paquets necessaris

7. Quan acabi, executa:

   ```text
   /plan Vull el primer milestone funcional amb autenticació, dashboard i flux principal.
   ```

8. Revisa el pla, ajusta'l i després demana execució.
9. Tanca cada bloc de feina amb:

   ```text
   /review
   /test
   /commit
   ```

---

## Part C — Flux recomanat de treball

1. Defineix el problema de negoci (no comencis pel stack).
2. Deixa que el bootstrap fixi el stack inicial.
3. Treballa per milestones petits amb `/plan`.
4. Usa els modes segons fase:
   - `planner` per planificació
   - `reviewer` per revisió
   - `test-writer` per cobertura
   - `doc-writer` per documentació
5. Fes auditories periòdiques amb `/skills-audit`.

---

## Errors a evitar

- Començar amb prompts massa oberts tipus "fes-me tota l'app".
- Activar massa complements el primer dia.
- Saltar-se la planificació abans d'editar.
- No validar amb `/review` i `/test` abans de commit.

---

## Resum curt

- Aquest repo és **template-only**.
- Crea sempre projectes nous amb **Use this template**.
- Primer producte, després stack, després execució per milestones.
