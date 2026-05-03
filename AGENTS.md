# AGENTS.md

Fitxer de compatibilitat amb agents de codi (Cursor, Cline, Codex, Aider…).
La font de veritat completa és [`.github/copilot-instructions.md`](.github/copilot-instructions.md).
Aquest fitxer en repeteix el mínim imprescindible perquè altres agents també hi tinguin accés.

## Què és aquest repo

Plantilla agnòstica al stack per iniciar projectes amb un agent de codi
preconfigurat. **No té stack propi** fins que la skill `bootstrap`
s'executa sobre un projecte creat des de template.

## Regles per a l'agent

- **No assumeixis cap stack** abans que `.copilot/stack.json` existeixi.
- Si `.copilot/stack.json` no existeix, llegeix `.github/skills/bootstrap/SKILL.md` abans de qualsevol altra acció no trivial.
- Idioma per defecte de respostes i nova documentació: **català**.
- No creïs fitxers nous fora de l'estructura documentada al [README](README.md) sense confirmar-ho.
- No afegeixis dependències ni gestors de paquets al repo base.

## Estructura rellevant per agents

```
.github/
├── copilot-instructions.md   ← instruccions globals (curtes)
├── instructions/             ← per glob (applyTo)
├── skills/                   ← procediments sota demanda
├── prompts/                  ← workflows /comanda
├── chatmodes/                ← modes amb tools restringides
└── _stacks/                  ← paquets latents per stack
```

## Ordre de prioritat si hi ha conflicte

1. Petició explícita de l'usuari en el torn actual.
2. `.copilot/stack.json` (decisions d'aquest projecte).
3. `.github/copilot-instructions.md`.
4. Aquest fitxer (`AGENTS.md`).
