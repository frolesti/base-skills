# 06 — FAQ i errors comuns

## Q: He creat un repo des de template però Copilot no fa res especial. Què passa?

Comprova:

1. **VS Code reiniciat?** Els chatmodes/prompts nous es detecten després
   de reiniciar el panel de Chat (o el VS Code sencer la primera vegada).
2. **`.vscode/settings.json` present?** És el que diu a Copilot on
   trobar-ho tot. Si l'has borrat, restaura'l.
3. **Tens GitHub Copilot Chat instal·lat i actiu?**

## Q: He escrit `/` i no veig totes les comandes esperades

- Verifica que `.github/prompts/` conté els fitxers `*.prompt.md`.
- Verifica `chat.promptFiles: true` i `chat.promptFilesLocations` a
  `.vscode/settings.json`.
- Tanca i reobre el panel de Chat.

## Q: No veig els modes personalitzats al selector

- Verifica que `.github/chatmodes/` conté els fitxers `*.chatmode.md`.
- Verifica `chat.modeFilesLocations` a `.vscode/settings.json`.
- Reinicia VS Code (la primera detecció pot trigar).

## Q: L'agent no executa el bootstrap automàticament

- Revisa `.github/copilot-instructions.md` — la secció "Bootstrap
  obligatori" ha d'estar-hi.
- Si `.copilot/stack.json` ja existeix, el bootstrap no torna a córrer
  (és per disseny). Esborra'l si vols re-bootstrapar.
- Demana-li explícitament: *"Llegeix `.github/skills/bootstrap/SKILL.md`
  i executa'l."*

## Q: L'agent edita fitxers fora del seu rol al mode `test-writer` / `doc-writer`

- Comprova que la propietat `tools` del chatmode no inclogui res que no
  hauria. Revisa el frontmatter.
- Si el problema persisteix, és una limitació del model (els models més
  petits respecten pitjor les restriccions). Canvia a un model més gran.

## Q: Vull saber quants tokens estic gastant

Avui dia VS Code Copilot **no exposa una API pública fiable** perquè una
extensió o el propi repo llegeixi el cost en tokens per torn. Opcions
parcials:

- **GitHub Copilot Usage Dashboard** (per a comptes de pagament): mostra
  consum agregat, no per torn ni per fitxer.
- **Estimació manual** amb tokenizers locals (com `tiktoken`): pots
  comptar tokens dels teus fitxers d'instruccions per veure si t'estàs
  passant. Útil com a alerta preventiva, no com a comptador real.
- **Telemetria local d'ús de skills** (`/skills-audit`): et diu quines
  skills es llegeixen, però no quants tokens en surten.

Veure la discussió completa al final del README. **Decisió actual: no
implementar comptador automàtic** fins que aparegui una API estable. Sí
que pots fer una **alerta de mida** (script local que avisi si
`copilot-instructions.md` supera N línies, o si una instrucció supera M).

## Q: Puc usar aquest repo amb Cursor / Cline / Codex?

Parcialment. El fitxer `AGENTS.md` és reconegut per molts agents. Les
skills (`SKILL.md`) i instruccions (`*.instructions.md`) tenen format
prou estàndard perquè altres agents les puguin llegir si les hi
assenyales explícitament. Els chatmodes són específics de Copilot Chat.

Per Cursor, per exemple, hauràs de duplicar algunes instruccions a
`.cursor/rules/` (és una limitació de cada agent, no d'aquest repo).

## Q: Què faig si dues skills es contradiuen?

Aplica l'ordre de prioritat documentat a `AGENTS.md`:

1. Petició explícita de l'usuari en el torn actual.
2. `.copilot/stack.json`.
3. `.github/copilot-instructions.md`.
4. `AGENTS.md`.

Si la contradicció és **dins** del nivell 3 (dues skills), arregla-ho
manualment: una de les dues és redundant o errònia.
