# Skill: add-skill

> Crea una skill nova de manera consistent. Usa-la sempre que vulguis afegir un procediment reutilitzable.

## Quan crear una skill (vs altres capes)

Crea skill **només si**:
- És un procediment de **> 3 passos** que es repetirà.
- O conté coneixement de domini que **no aplica sempre** (només quan toca aquesta tasca).

Si és una regla curta i sempre activa → `copilot-instructions.md`.
Si és per un tipus de fitxer → `instructions/` amb `applyTo`.
Si és un workflow disparat per l'usuari → `prompts/`.

## Procediment

1. **Decideix la ubicació**:
   - Skill genèrica del repo base → `.github/skills/<nom>/SKILL.md`.
   - Skill específica d'un stack → `.github/_stacks/<stack>/skills/<nom>/SKILL.md`.
2. **Nom**: kebab-case, verb + objecte (`add-route`, `create-migration`, `deploy-vercel`).
3. **Crea `SKILL.md`** seguint aquesta plantilla:

```markdown
# Skill: <nom>

> Quan aplica aquesta skill (1-2 línies).

## Objectiu

Què aconseguim en executar-la (2-3 línies).

## Precondicions

- Què ha d'existir al repo abans (fitxers, env vars, dependències).

## Procediment

1. Pas 1 (imperatiu, una acció).
2. Pas 2.
3. ...

## Errors comuns

- Cas X → solució Y.
```

4. **Recursos addicionals** (opcional): fitxers de plantilla dins la mateixa carpeta (`template.tsx`, `example.json`…). Referencia'ls amb ruta relativa des del `SKILL.md`.

5. **Registra-la** mentalment: les skills es descobreixen pel sistema automàticament — no cal llistat. Però si vols accelerar la descoberta, cita-la des de `copilot-instructions.md` o des d'una altra skill relacionada.

## Anti-patrons

- Skills genèriques tipus "general best practices" → no aporten res, ocupen tokens quan es llegeixen.
- Skills que dupliquen documentació oficial → enllaça-la, no la copiïs.
- Skills sense procediment numerat → l'agent no les sap seguir bé.
