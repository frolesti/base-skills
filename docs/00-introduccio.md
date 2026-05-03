# 00 — Introducció

## El problema

GitHub Copilot (i en general qualsevol agent LLM) **carrega les seves
instruccions globals al context a cada torn de conversa**. Si poses tot
el coneixement del teu projecte (convencions, comandes, anti-patrons,
procediments, exemples…) en un únic fitxer gegant:

- Pagues el cost en tokens d'aquest fitxer **a cada pregunta**, encara que
  facis "hola".
- L'agent es despista amb informació irrellevant per a la tasca actual.
- El fitxer es torna inmantenible i contradictori.

## La solució d'aquest repo

Una **arquitectura per capes** on cada peça es carrega només quan toca:

| Capa | Es carrega... | Per a què |
|---|---|---|
| Instruccions globals | Sempre (ha de ser **molt curt**) | Stack, idioma, comandes bàsiques |
| Instruccions per glob (`applyTo`) | Quan edites un fitxer que matcheja el patró | Convencions per llenguatge/àrea |
| Skills | Sota demanda (l'agent les llegeix conscientment) | Procediments llargs |
| Prompts (`/comanda`) | Quan tu els disparis | Workflows reutilitzables |
| Chatmodes | Quan tu tries el mode | Agents amb tools restringides |

## Què guanyes

- **Menys cost** per pregunta (les instruccions globals queden petites).
- **Més precisió** (l'agent rep només el context rellevant).
- **Reutilització**: crea un projecte des de la template, executa el bootstrap i tens un projecte
  nou amb tot configurat per al stack que toqui.
- **Coherència entre projectes**: les mateixes comandes `/`, els mateixos
  modes, les mateixes skills meta.

## El mecanisme d'auto-adaptació

El repo no té stack propi. Quan creïs un projecte des de la template i obris Copilot per primer cop,
una skill anomenada `bootstrap` s'executa automàticament i:

1. Detecta artefactes (`package.json`, `pyproject.toml`…) o et pregunta.
2. Activa només els paquets d'instruccions/skills del stack triat (de
   `.github/_stacks/<stack>/` cap a les carpetes "actives").
3. Escriu `.copilot/stack.json` perquè la decisió persisteixi.

Llegeix [01-conceptes.md](./01-conceptes.md) per entendre els blocs
fonamentals abans de continuar.
