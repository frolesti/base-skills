# Copilot — instruccions globals

Repositori plantilla agnòstic. Sense stack propi fins que el bootstrap s'executi.

## Idioma

- Respon i documenta en **català**.
- Comandes, identificadors i noms de fitxer en anglès.

## Bootstrap obligatori

- Si **no existeix `.copilot/stack.json`**, abans de qualsevol acció no trivial llegeix `.github/skills/bootstrap/SKILL.md` i executa'l.
- Si existeix, llegeix-lo per saber stack, gestor de paquets i comandes reals.

## Què NO fer

- No assumeixis llenguatge, framework ni gestor de paquets sense `.copilot/stack.json`.
- No instal·lis dependències ni creïs `package.json`/`pyproject.toml` etc. al repo base. Això ho fa cada projecte fill.
- No moguis fitxers fora de l'estructura del [README](../README.md) sense confirmació.
- No escriguis instruccions globals llargues aquí. Si una explicació passa de 5 línies, va a una skill.

## Convencions de redacció (per quan editis aquest mateix sistema)

- Imperatiu, no descriptiu.
- Una idea per línia.
- `applyTo` el més estret possible a `.github/instructions/*.instructions.md`.
- Procediment de > 3 passos repetit → skill, no instrucció global.

## Capes (resum)

| Carpeta | Quan es carrega |
|---|---|
| `.github/copilot-instructions.md` | Sempre |
| `.github/instructions/` | Quan s'edita un fitxer que matcheja `applyTo` |
| `.github/skills/` | Sota demanda |
| `.github/prompts/` | Amb `/nom` |
| `.github/chatmodes/` | Quan l'usuari tria el mode |
| `.github/_stacks/` | Mai directament — el bootstrap copia a les altres |

## Memòria

- Decisions verificades del repo: `memories/repo/`.
- Notes de sessió actual: `memories/session/` (no versionat).
