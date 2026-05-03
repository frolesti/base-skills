# Skill: bootstrap

> **Activador**: aquesta skill s'executa quan l'agent detecta que **no existeix `.copilot/stack.json`** al repo.
> És l'única skill que pot crear `.copilot/stack.json`. Un cop creat, no torna a córrer.

## Objectiu

Fer un bootstrap **realment agnòstic** i no estàtic:

- entendre primer el producte que es vol construir
- fer preguntes adaptades al context (no un formulari fix)
- persistir les decisions perquè guiïn tot el desenvolupament
- activar només les instruccions i skills que tinguin sentit

## Procediment

### 1. Comprova precondició

- Si `.copilot/stack.json` ja existeix → atura't i informa que el bootstrap ja s'ha executat.
- Si no existeix → continua.

### 2. Detecció inicial (context + artefactes)

Mira l'arrel i un nivell de profunditat per pistes:

| Fitxer detectat | Stack candidat |
|---|---|
| `package.json` amb `"next"` | `nextjs-ts` |
| `package.json` amb `"vite"` + `react` | `vite-react-ts` |
| `package.json` amb `"astro"` | `astro` |
| `pyproject.toml` / `requirements.txt` | `python` (preguntar framework) |
| `Cargo.toml` | `rust` |
| `go.mod` | `go` |
| `pubspec.yaml` | `flutter` |
| `Gemfile` | `ruby` |
| Cap dels anteriors | repo buit → preguntar tot |

També detecta complements:
- `supabase/` o `@supabase/*` a `package.json` → afegeix `supabase`.
- `prisma/` o `"prisma"` → afegeix `prisma`.
- `tailwind.config.*` → afegeix `tailwind`.
- `drizzle.config.*` → afegeix `drizzle`.

També detecta context de firmware/IoT:
- `*.ino` o carpeta `arduino/` → candidat `arduino`.
- `platformio.ini` → candidat `platformio`.
- `CMakeLists.txt` amb `idf_component_register` → candidat `esp-idf`.
- Fitxers amb `#include <Arduino.h>` → candidat `arduino`.

### 3. Interrogatori conscient (fase producte)

Abans de preguntar stack, pregunta primer pel **què** i el **per què**.

Preguntes base (sempre):

1. Quin problema resol el projecte?
2. Quin resultat final esperes (web, API, firmware, automatització, app, llibreria...)?
3. Quines restriccions tens (hardware, temps, pressupost, coneixements, offline, seguretat)?
4. Què és explícitament fora d'abast ara (non-goals)?

Amb aquestes respostes, classifica el projecte en un **arquetip**.

Arquetips mínims:
- `web-app`
- `backend-api`
- `mobile`
- `firmware-iot`
- `data-ml`
- `cli-tool`
- `library-sdk`
- `game-graphics`

### 4. Preguntes adaptatives (fase tècnica)

Fes només preguntes rellevants per l'arquetip detectat.
No facis preguntes que no tenen sentit al context.

Exemple per `firmware-iot`:
- Placa/objectiu (`Arduino Uno`, `ESP32`, `RP2040`...).
- Entorn (`Arduino IDE` / `PlatformIO`).
- Llenguatge (`C++` habitualment).
- Sensors/actuadors previstos.
- Alimentació i connectivitat (`Wi-Fi`, `BLE`, `LoRa`, cap).
- Política d'errors (fail-safe, watchdog, logs sèrie).

Exemple per `web-app`:
- Framework base.
- Runtime i gestor de paquets.
- Persistència (DB sí/no, quina).
- Deploy target.

Límit recomanat: **màxim 8 preguntes totals** entre fase producte + fase tècnica.
Si calen més decisions, deixa-les per `/plan`.

### 5. Materialitza decisions permanents

Després de l'interrogatori, escriu dos fitxers:

1. `.copilot/project-profile.json` (visió i restriccions de producte)
2. `.copilot/stack.json` (decisions tècniques i comandes)

Exemple mínim de `project-profile.json`:

```json
{
  "version": 1,
  "createdAt": "<ISO timestamp>",
  "projectIntent": "Sistema de reg automàtic per horts petits",
  "archetype": "firmware-iot",
  "targetOutcome": "Firmware Arduino + guia de calibratge",
  "constraints": ["Baix consum", "Sense cloud obligatori"],
  "nonGoals": ["App mòbil nativa en fase inicial"],
  "hardware": {
    "board": "ESP32",
    "sensors": ["humitat sòl"],
    "actuators": ["relé bomba"]
  }
}
```

### 6. Activa paquets només si encaixen

Per cada `<stack>` triat (base + complements):

1. Comprova que existeix `.github/_stacks/<stack>/`.
   - Si no existeix, informa i ofereix crear-lo via skill `add-stack`. Atura aquí.
2. **Copia** (no enllaços simbòlics — incompatibles a Windows) el contingut de:
   - `.github/_stacks/<stack>/instructions/*` → `.github/instructions/`
   - `.github/_stacks/<stack>/skills/*` → `.github/skills/`
3. Si dos paquets aporten el mateix fitxer, **avisa l'usuari** abans de sobreescriure.

Si no existeix un paquet per l'arquetip (p.ex. `firmware-iot`),
NO forcis un stack web. Informa-ho i proposa crear paquet amb `add-stack`.

### 7. Personalitza `copilot-instructions.md`

Afegeix una secció `## Stack actual` al final de `.github/copilot-instructions.md`
amb:

- Nom del stack base + complements.
- Comandes reals: `dev`, `build`, `test`, `lint`, `format`, `typecheck`.
- Ruta del gestor de paquets.

Afegeix també una secció `## Directrius permanents` (molt curta) que indiqui:

- "Llegeix sempre `.copilot/project-profile.json` i `.copilot/stack.json` abans de canvis no trivials."
- "Aquestes directrius només es poden sobreescriure amb petició explícita de l'usuari."

Manté el fitxer **per sota de 60 línies totals**. Si el supera, mou la part
nova a una instrucció amb `applyTo` adequat.

### 8. Escriu `.copilot/stack.json`

```json
{
  "version": 1,
  "bootstrappedAt": "<ISO timestamp>",
  "base": "nextjs-ts",
  "addons": ["supabase", "tailwind"],
  "packageManager": "pnpm",
  "runtime": { "node": "20" },
  "deploy": ["vercel"],
  "commands": {
    "dev": "pnpm dev",
    "build": "pnpm build",
    "test": "pnpm test",
    "lint": "pnpm lint",
    "typecheck": "pnpm typecheck"
  }
}
```

`stack.json` ha de ser coherent amb `project-profile.json`.
No hi posis camps irrellevants per l'arquetip.

### 9. Inicialitza telemetria

Crea `.copilot/usage.json` buit `{}` si no existeix. (Veure
[skills-audit](../../prompts/skills-audit.prompt.md) per com s'usa.)

### 10. Resum

Dona a l'usuari un resum: paquets activats, fitxers creats/modificats,
i suggereix la primera comanda a provar (ex: `/plan` per planificar la
primera tasca).

Inclou sempre:
- arquetip detectat
- decisions de producte persistides
- decisions tècniques persistides

## Errors comuns

- **Stack inexistent a `_stacks/`**: no improvisis, dispara `add-stack`.
- **Reexecució accidental**: si l'usuari realment vol re-bootstrapar, ha
  d'esborrar manualment `.copilot/stack.json` i confirmar.
- **Conflicte de fitxers entre paquets**: pregunta abans de sobreescriure.
- **Pregunta fora de context**: si una pregunta no aporta per l'arquetip,
  elimina-la. Millor menys preguntes i més rellevants.
