# 01 — Conceptes

Quatre capes, cadascuna amb un propòsit clar.

## 1. Instruccions globals (`.github/copilot-instructions.md`)

**Es carreguen a cada torn de conversa.** Per això han de ser **molt curtes**
(ideal ≤ 40 línies). Conté:

- Idioma de resposta.
- Stack del projecte (quan el bootstrap l'ha establert).
- Comandes bàsiques: `dev`, `build`, `test`, `lint`.
- Llista negra ("què NO fer") — sovint més útil que la blanca.

**Regla d'or**: si una explicació passa de 5 línies, **NO va aquí**. Va a
una skill.

## 2. Instruccions per glob (`.github/instructions/*.instructions.md`)

Igual que les globals, però **només es carreguen quan edites un fitxer
que matcheja el seu `applyTo`**. Ho declares al frontmatter del fitxer:

```markdown
---
applyTo: '**/*.tsx'
description: 'Convencions per a components React'
---

# Components React
- Usa Server Components per defecte.
- ...
```

Així, les regles de TSX només costen tokens quan estàs editant un `.tsx`.
Els `.py` o `.md` no en paguen el cost.

**Regla d'or**: `applyTo` el més estret possible. `'**/*'` equival a global —
si vols això, posa-ho a `copilot-instructions.md` directament.

## 3. Skills (`.github/skills/<nom>/SKILL.md`)

Procediments **llargs i específics** que NO es carreguen automàticament.
L'agent les llegeix **conscientment** quan detecta que la tasca actual
encaixa.

Una skill típica té:

- Quan aplica.
- Objectiu.
- Procediment numerat (passos atòmics).
- Errors comuns i com evitar-los.

Exemples al repo: `bootstrap`, `add-skill`, `add-instruction`, `add-stack`.
Al paquet `nextjs-ts`: `add-route`, `add-server-action`.

**Regla d'or**: si un procediment té > 3 passos i es repeteix, és skill.

## 4. Prompts (`.github/prompts/*.prompt.md`)

Workflows que **disparis tu mateix** escrivint `/<nom>` al chat. Útils per
a tasques freqüents on no vols reescriure el context cada cop:
revisar diff, generar missatge de commit, planificar una tasca, etc.

Es defineixen amb frontmatter:

```markdown
---
mode: 'agent'
description: 'Genera un pla per a una tasca.'
---

# /plan

Procediment que l'agent ha de seguir...
```

Les comandes disponibles al repo es detallen a
[02-comandes-slash.md](./02-comandes-slash.md).

## 5. Chatmodes (`.github/chatmodes/*.chatmode.md`)

Agents personalitzats amb **tools restringides**. Quan triïs un mode al
selector de Copilot Chat, l'agent només podrà usar les tools que aquell
mode declara.

Útil per a sessions on vols garantir l'scope: el mode `test-writer` no
podrà tocar codi de producció encara que li ho demanis, i el `reviewer`
no podrà editar res.

Es detallen a [03-chatmodes.md](./03-chatmodes.md).

## Resum visual

```
Pregunta → Copilot
            │
            ├─ Sempre: copilot-instructions.md
            ├─ Si edites *.tsx: tsx.instructions.md
            ├─ Si tries un mode: regles del mode
            ├─ Si escrius /plan: plan.prompt.md
            └─ Si l'agent decideix: SKILL.md específica
```

Amb això, cada conversa només "paga" pels fragments rellevants.
