# Skill: bootstrap

> **Activador**: aquesta skill s'executa quan l'agent detecta que **no existeix `.copilot/stack.json`** al repo.
> És l'única skill que pot crear `.copilot/stack.json`. Un cop creat, no torna a córrer.

## Objectiu

Adaptar aquest repo plantilla al stack real del projecte fill, activant
només les instruccions i skills rellevants i deixant la resta latents a
`.github/_stacks/`.

## Procediment

### 1. Comprova precondició

- Si `.copilot/stack.json` ja existeix → atura't i informa que el bootstrap ja s'ha executat.
- Si no existeix → continua.

### 2. Detecta artefactes

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

### 3. Confirma o pregunta

Usa `vscode_askQuestions` amb les pistes detectades com a opció recomanada.
Pregunta sempre, encara que la detecció sembli òbvia, per:

- **stack base** (un de sol)
- **complements** (multi-select)
- **gestor de paquets** (si aplica): `npm`, `pnpm`, `yarn`, `bun`, `uv`, `poetry`…
- **runtime version** si rellevant (Node, Python…)
- **deploy target** (opcional, multi-select): `vercel`, `netlify`, `fly`, `railway`, `docker`, `cap`…

### 4. Activa els paquets

Per cada `<stack>` triat (base + complements):

1. Comprova que existeix `.github/_stacks/<stack>/`.
   - Si no existeix, informa i ofereix crear-lo via skill `add-stack`. Atura aquí.
2. **Copia** (no enllaços simbòlics — incompatibles a Windows) el contingut de:
   - `.github/_stacks/<stack>/instructions/*` → `.github/instructions/`
   - `.github/_stacks/<stack>/skills/*` → `.github/skills/`
3. Si dos paquets aporten el mateix fitxer, **avisa l'usuari** abans de sobreescriure.

### 5. Personalitza `copilot-instructions.md`

Afegeix una secció `## Stack actual` al final de `.github/copilot-instructions.md`
amb:

- Nom del stack base + complements.
- Comandes reals: `dev`, `build`, `test`, `lint`, `format`, `typecheck`.
- Ruta del gestor de paquets.

Manté el fitxer **per sota de 60 línies totals**. Si el supera, mou la part
nova a una instrucció amb `applyTo` adequat.

### 6. Escriu `.copilot/stack.json`

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

### 7. Inicialitza telemetria

Crea `.copilot/usage.json` buit `{}` si no existeix. (Veure
[skills-audit](../../prompts/skills-audit.prompt.md) per com s'usa.)

### 8. Resum

Dona a l'usuari un resum: paquets activats, fitxers creats/modificats,
i suggereix la primera comanda a provar (ex: `/plan` per planificar la
primera tasca).

## Errors comuns

- **Stack inexistent a `_stacks/`**: no improvisis, dispara `add-stack`.
- **Reexecució accidental**: si l'usuari realment vol re-bootstrapar, ha
  d'esborrar manualment `.copilot/stack.json` i confirmar.
- **Conflicte de fitxers entre paquets**: pregunta abans de sobreescriure.
