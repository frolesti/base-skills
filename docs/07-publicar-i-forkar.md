# 07 — Publicar el repo i arrencar un projecte real des d'un fork

Aquest document cobreix dos casos:

1. **Publicar aquest repo base** al teu perfil de GitHub.
2. **Fer-lo servir per arrencar un projecte real** a partir d'un fork o d'un repo creat des de template.

---

## Part A — Publicar aquest repo base al teu perfil de GitHub

En aquest moment, la carpeta local pot existir sense `.git/`. En aquest cas,
primer cal inicialitzar git, fer el primer commit i després crear el repo remot.

### Opció ràpida amb GitHub CLI (`gh`)

Si tens `gh` autenticat, el flux és aquest:

```powershell
git init
git add .
git commit -m "chore: initialize base skills repository"
gh repo create <repo-name> --source . --remote origin --push --public
```

Si el vols privat, canvia `--public` per `--private`.

### Què fa cada pas

1. `git init`
   Crea el repositori git local.
2. `git add .`
   Afegeix tots els fitxers actuals a l'índex.
3. `git commit -m ...`
   Crea el primer snapshot versionat del projecte.
4. `gh repo create ... --push`
   Crea el repo al teu compte de GitHub, configura `origin` i puja el commit.

### Si ho fas des de VS Code

1. Obre el panell **Source Control**.
2. Fes clic a **Initialize Repository**.
3. Stageja tots els fitxers.
4. Crea el commit inicial.
5. Obre la Command Palette.
6. Executa **GitHub: Publish to GitHub**.
7. Tria si ha de ser públic o privat.

### Verificació mínima després de publicar

Comprova que al repo de GitHub hi apareixen:

- `README.md`
- `.github/`
- `docs/`
- `.vscode/`

I comprova també que el repo obre correctament a GitHub i que el README es veu bé.

---

## Part B — Com usar aquest repo per començar un projecte nou

Hi ha dues estratègies. La recomanada és la **template**, no el fork pur.

### Opció recomanada — "Use this template"

És la millor opció si vols començar un projecte real teu sense arrossegar
la relació de fork a GitHub.

#### Pas a pas

1. Publica aquest repo base a GitHub.
2. A GitHub, activa l'opció **Template repository** a la configuració del repo.
3. Quan vulguis iniciar un projecte nou, entra al repo base.
4. Fes clic a **Use this template**.
5. Crea un repo nou, per exemple `my-real-project`.
6. Clona aquest repo nou localment:

   ```powershell
   git clone https://github.com/<user>/my-real-project
   cd my-real-project
   code .
   ```

7. Obre Copilot Chat i explica què vols construir.
8. Com que encara no existirà `.copilot/stack.json`, l'agent hauria de:
   - llegir `bootstrap`
   - detectar si el projecte és buit
   - preguntar-te el stack
   - activar el paquet de stack correcte

#### Quan convé aquesta opció

- Quan el projecte fill serà **independent** del repo base.
- Quan no vols que GitHub el mostri com a fork.
- Quan vols compartir el projecte final com una peça pròpia.

---

### Opció alternativa — Fork del repo base

És útil si vols mantenir una relació explícita amb el repo d'origen i et pot
interessar portar millores del repo base al futur.

#### Pas a pas

1. Obre el repo base a GitHub.
2. Fes clic a **Fork**.
3. Dona-li nom al fork (si vols).
4. Clona el fork:

   ```powershell
   git clone https://github.com/<user>/<fork-name>
   cd <fork-name>
   code .
   ```

5. Obre Copilot Chat.
6. Fes la primera petició real, per exemple:

   ```text
   Vull crear una aplicació web per gestionar reserves d'un estudi de ioga.
   ```

7. El bootstrap hauria de guiar la detecció o preguntar-te:
   - stack base
   - complements (`tailwind`, `supabase`, etc.)
   - gestor de paquets
   - deploy target

8. Un cop creat `.copilot/stack.json`, el projecte ja queda personalitzat.

#### Inconvenient del fork pur

GitHub marcarà el repo com a fork. Per a productes finals o portfolio, això
de vegades no és el que vols. Per això, per projectes reals, la template sol ser millor.

---

## Part C — Flux recomanat per arrencar un projecte real

Aquest és el flux que et recomano perquè el resultat sigui net i eficient.

### 1. Crea el projecte des de template

No forkis si el projecte serà una peça final del teu portfolio. Usa template.

### 2. Fes una primera petició de producte, no tècnica

En lloc de dir:

```text
Vull un Next.js amb Supabase i Tailwind.
```

és millor dir:

```text
Vull construir una plataforma per publicar recursos educatius amb àrees d'usuari, cerca i pagaments.
```

Per què? Perquè així el bootstrap i la planificació parteixen del **problema de negoci** i poden ajustar el stack a la mida real del projecte.

### 3. Executa `/plan`

Abans de crear res gros:

```text
/plan Vull construir una plataforma per publicar recursos educatius amb àrees d'usuari, cerca i pagaments.
```

Revisa el pla. Ajusta'l. Només després demana execució.

### 4. Fixa el stack amb el bootstrap

Quan el bootstrap et pregunti:

- no triïs complements perquè sí
- no afegeixis Prisma, Drizzle, Supabase i Tailwind tots alhora si encara no ho necessites
- comença amb el mínim viable

Regla pràctica: **base + 1 o 2 complements màxim** a l'inici.

### 5. Usa els modes correctes per cada fase

- Disseny i descomposició: `planner`
- Revisió abans de commit: `reviewer`
- Cobertura de tests: `test-writer`
- Documentació i README: `doc-writer`

### 6. Encadena les slash commands

Flux habitual:

```text
/plan
```

després execució normal, i quan acabis:

```text
/review
/commit
```

Si falten proves:

```text
/test
```

### 7. Fes auditories periòdiques

Cada cert temps:

```text
/skills-audit
```

Serveix per detectar skills mortes o massa específiques que ja no aporten valor.

---

## Part D — Exemple complet de primer dia en un projecte real

Suposem que vols crear una plataforma per gestionar encàrrecs de disseny.

### Sessió recomanada

1. Crees el repo nou des de template.
2. El clones i l'obres a VS Code.
3. Obres Copilot Chat.
4. Escrius:

   ```text
   Vull construir una plataforma per gestionar encàrrecs de disseny entre freelancers i clients, amb dashboard, missatgeria i pressupostos.
   ```

5. El bootstrap et fa preguntes i et proposa un stack.
6. Respon només amb el que saps segur. No especulis.
7. Quan acabi el bootstrap, escrius:

   ```text
   /plan Vull el primer milestone funcional amb autenticació, dashboard i creació d'encàrrecs.
   ```

8. Revises el pla.
9. Quan el validis, li dius a l'agent normal:

   ```text
   Executa aquest pla pas a pas i valida cada canvi abans de continuar.
   ```

10. Quan acabi:

   ```text
   /review
   /test
   /commit
   ```

Aquest flux redueix improvisació, repetició i soroll contextual.

---

## Part E — Errors que has d'evitar

- **No comencis pel stack si encara no tens clar el producte.**
- **No facis prompts massa oberts** tipus "fes-me tota l'app".
- **No activis 5 complements al dia 1** només perquè sonin bé.
- **No treballis sempre en mode Agent genèric** si la sessió és només de review o només de tests.
- **No oblidis revisar el pla abans de deixar que l'agent editi.**

---

## Resum curt

- Per projectes reals: **template > fork**.
- Primer defineix el problema.
- Després `/plan`.
- Després execució.
- Tanca amb `/review`, `/test`, `/commit`.

Si segueixes aquest flux, aquest repo et serveix tant de **motor d'arrencada** com de **sistema de disciplina** per treballar amb agents.