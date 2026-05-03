# base-skills-repo

Repositori de partida (**template-first**) per iniciar projectes amb GitHub Copilot
ja configurat per **maximitzar la precisió** de l'agent i **minimitzar el consum
de tokens** des del primer commit.

> Idea clau: el repo és **agnòstic al stack**. Un cop creat des de template, una skill de
> *bootstrap* analitza el projecte (o conversa amb tu si està buit), decideix
> el stack i **activa només les instruccions i skills rellevants**. La resta
> queden latents (no consumeixen tokens fins que no es necessiten).

> 📚 **Nova aquí?** Comença per [docs/](./docs/) — hi ha una guia pas a pas
> de conceptes, comandes `/`, modes i estendre el repo.

---

## Per què aquest repo

GitHub Copilot (i la majoria d'agents moderns) carreguen al context **totes
les instruccions globals a cada torn**. Si poses tot el coneixement del
projecte en un únic `copilot-instructions.md` de 800 línies:

- **Cada pregunta** paga el cost d'aquestes 800 línies.
- L'agent es despista amb informació irrellevant per la tasca actual.
- És impossible mantenir-ho coherent.

La solució és una **arquitectura per capes** on cada peça es carrega només
quan toca.

---

## Arquitectura per capes

| Capa | Ubicació | Quan es carrega | Per a què |
|---|---|---|---|
| **1. Instruccions globals** | `.github/copilot-instructions.md` | Sempre | Stack, comandes, què NO fer. **≤ 40 línies.** |
| **2. Instruccions per glob** | `.github/instructions/*.instructions.md` | Quan s'edita un fitxer que matcheja `applyTo` | Convencions per llenguatge/àrea |
| **3. Skills** | `.github/skills/<nom>/SKILL.md` | Sota demanda (l'agent decideix llegir-les) | Procediments llargs i repetitius |
| **4. Prompts** | `.github/prompts/*.prompt.md` | Quan l'usuari escriu `/nom` | Workflows reutilitzables |
| **5. Chatmodes (agents)** | `.github/chatmodes/*.chatmode.md` | Quan l'usuari tria el mode | Agents amb tools restringides |
| **6. Memòria de repo** | `/memories/repo/` | Quan l'agent la consulta | Decisions i convencions verificades |
| **7. AGENTS.md** | Arrel | Altres agents (Cursor, Cline, Codex…) | Compatibilitat cross-tool |

### Regla d'or de col·locació

```
És imperatiu i curt (< 5 línies) i aplica SEMPRE? → copilot-instructions.md
Aplica només a un tipus de fitxer?                → instructions/ amb applyTo
És un procediment de > 3 passos?                  → skills/
És un workflow que l'usuari dispararà?            → prompts/
Cal restringir tools de l'agent per la tasca?     → chatmodes/
```

---

## Mecanisme d'auto-adaptació al stack

Quan creïs un projecte des de template, l'estat inicial és **mode neutre**: només les
instruccions globals genèriques i la skill `bootstrap` estan actives.

### Flux

1. **Primera interacció amb Copilot** al repo creat des de template.
2. La instrucció global indica a l'agent: *"Si encara no s'ha executat el
   bootstrap (no existeix `.copilot/stack.json`), llegeix la skill
   `bootstrap` abans de res."*
3. La skill `bootstrap` fa, en aquest ordre:
   - **Detecta** artefactes (`package.json`, `pyproject.toml`, `Cargo.toml`,
     `go.mod`, `pubspec.yaml`…) i n'infereix candidats de stack.
   - Si el repo és buit, **pregunta** amb `vscode_askQuestions`:
     llenguatge, framework, gestor de paquets, BD, deploy…
   - **Activa** els paquets d'instruccions corresponents copiant/enllaçant
     els fitxers latents de `.github/_stacks/<nom>/` cap a
     `.github/instructions/` i `.github/skills/`.
   - **Escriu** `.copilot/stack.json` amb la configuració decidida (perquè
     el pas 2 no es repeteixi).
   - **Actualitza** `.github/copilot-instructions.md` amb les comandes
     reals del stack triat (build, test, lint, dev).
4. A partir d'aquí, el repo està **adaptat** i es comporta com si l'haguessin
   fet a mida per al projecte.

### Estructura de `_stacks/` (latent)

```
.github/_stacks/
├── nextjs-ts/
│   ├── instructions/
│   │   ├── tsx.instructions.md       (applyTo: **/*.tsx)
│   │   └── server-actions.instructions.md
│   └── skills/
│       ├── add-route/SKILL.md
│       └── add-i18n-key/SKILL.md
├── python-fastapi/
│   ├── instructions/
│   └── skills/
├── supabase/
│   ├── instructions/
│   └── skills/
└── ...
```

Els paquets es poden **combinar** (p.ex. `nextjs-ts` + `supabase` + `tailwind`).

---

## Agents inclosos

Tots els chatmodes són **read-only o restringits per defecte** per evitar
edicions inesperades.

| Agent | Tools permeses | Quan usar-lo |
|---|---|---|
| **planner** | només lectura + `manage_todo_list` | Abans de tasques grans, per generar pla revisable |
| **reviewer** | només lectura + `get_changed_files` | Revisar diff actual o un PR |
| **test-writer** | edició restringida a `**/*.{test,spec}.*` i `__tests__/**` | Afegir/arreglar tests sense tocar producció |
| **doc-writer** | edició restringida a `**/*.md` | Documentació, README, ADRs |

L'agent per defecte segueix tenint accés complet — aquests modes són per
a sessions on vols **garantir** un scope concret.

---

## Principis de redacció (per maximitzar precisió)

1. **Imperatiu, no descriptiu.**
   - ✅ "Usa `pnpm`. No facis servir `npm install`."
   - ❌ "Aquest projecte normalment utilitza pnpm com a gestor de paquets."
2. **Llista negra explícita.** Un "DO NOT" estalvia més errors que tres "DO".
3. **Comandes reals**, copy-paste-ables, no descripcions.
4. **Una idea per línia.** Facilita que l'agent la citi i la respecti.
5. **`applyTo` el més estret possible.** Un `**/*` per defecte és malgastar tokens.
6. **Skills > 3 passos repetits.** Si una cosa s'ha explicat 2 vegades, és skill.
7. **No expliquis el "perquè" llarg dins instruccions globals.** Va a skills o ADRs.

---

## Estructura final prevista

```
base-skills-repo/
├── README.md                          ← aquest fitxer
├── AGENTS.md                          ← compatibilitat cross-agent
├── LICENSE
├── .editorconfig
├── .gitignore
├── .gitattributes
├── .vscode/
│   ├── settings.json
│   └── extensions.json
├── .copilot/
│   └── stack.json                     ← (generat pel bootstrap)
├── .github/
│   ├── copilot-instructions.md        ← global, curt
│   ├── instructions/                  ← actives (omplert pel bootstrap)
│   ├── skills/
│   │   ├── bootstrap/SKILL.md         ← sempre present
│   │   ├── add-skill/SKILL.md         ← meta: com afegir una skill nova
│   │   └── add-instruction/SKILL.md
│   ├── prompts/
│   │   ├── plan.prompt.md
│   │   ├── review.prompt.md
│   │   ├── commit.prompt.md
│   │   └── test.prompt.md
│   ├── chatmodes/
│   │   ├── planner.chatmode.md
│   │   ├── reviewer.chatmode.md
│   │   ├── test-writer.chatmode.md
│   │   └── doc-writer.chatmode.md
│   └── _stacks/                       ← paquets latents (no carregats)
│       ├── nextjs-ts/
│       ├── python-fastapi/
│       ├── supabase/
│       └── ...
└── memories/
    └── repo/
        └── .gitkeep
```

---

## Estat actual

Aquest repositori ja està **operatiu i publicat** com a template:

- Repo públic: `https://github.com/frolesti/base-skills`
- Template repository: **activat**
- Base implementada: instruccions globals, skills meta, prompts, chatmodes
- Paquet de referència implementat: `nextjs-ts`

Per al flux real de publicació i arrencada de projectes nous, segueix
la guia pas a pas a `docs/07-publicar-i-template.md`.

---

## Com s'utilitza (un cop publicat)

```bash
# 1. Crea un projecte nou des de template
gh repo create my-new-project --template TEU-USUARI/base-skills-repo

# 2. Clona i obre amb VS Code
git clone https://github.com/TEU-USUARI/my-new-project
code my-new-project

# 3. Obre Copilot Chat i escriu qualsevol cosa.
#    El bootstrap s'executarà automàticament la primera vegada.
```

---

## Decisions aplicades

1. Llicència: **cap** (de moment).
2. Gestor per defecte al repo base: **cap** (es decideix per projecte fill).
3. Memòria de repo: `memories/repo/.gitkeep` versionat.
4. Stacks: model **monorepo** dins `.github/_stacks/`.
5. Telemetria: fitxer local `.copilot/usage.json` previst per auditories.
