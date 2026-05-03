# 04 — Per què aquesta arquitectura estalvia tokens

## El càlcul mental

Cada token que l'LLM "veu" costa diners (i és un tros del seu context
limitat). Si poses 800 línies a `copilot-instructions.md`, paguem ~3000
tokens **a cada torn**, encara que el torn sigui "afegeix un punt aquí".

Multiplicat per 200 torns/dia → **600.000 tokens/dia "fixos"** abans de
codi, només d'instruccions.

## Què fem en aquest repo

| Tècnica | Estalvi típic |
|---|---|
| `copilot-instructions.md` ≤ 40 línies | 80-90% sobre l'enfoc monolític |
| `applyTo` estret | Les instruccions de TSX no es carreguen quan edites Python |
| Skills sota demanda | Procediments de 200 línies només es llegeixen quan calen (potser 1 cop al dia) |
| Prompts (`/comanda`) | Disparen workflows precisos sense haver de re-explicar context |
| Chatmodes restringits | L'agent no carrega tools que no necessita |
| Memòria de repo (`memories/repo/`) | L'agent recorda decisions sense que tu les hagis de repetir |
| Telemetria local (`.copilot/usage.json`) | NO es carrega al context normal — cost 0 |

## Anti-patrons que aquest repo evita

❌ **Posar exemples llargs a `copilot-instructions.md`.**
   → Els exemples van a skills, que es llegeixen quan calen.

❌ **`applyTo: '**/*'` a una instrucció específica.**
   → Si aplica a tot, posa-ho a `copilot-instructions.md`. Si no,
   ajusta el glob.

❌ **Skills de 30 línies amb consells genèrics.**
   → Ocupen tokens quan es llegeixen i no aporten res. Esborra-les.

❌ **Documentació duplicada entre instruccions i skills.**
   → Una sola font de veritat per cada concepte.

❌ **Carregar tools a chatmodes que no s'usen.**
   → Cada tool descriu el seu schema al model. Menys tools = menys context.

## Mesures concretes que pots prendre

1. **Audita mensualment** amb `/skills-audit`. Esborra skills no usades.
2. **Mira la mida** de `.github/copilot-instructions.md` cada cert temps.
   Si supera 60 línies, mou contingut a skills.
3. **Revisa els `applyTo`** de les instruccions. Fes-los més estrets si
   pots.
4. **Usa modes restringits** quan facis sessions monogràfiques.
5. **Confia en les memòries** (`memories/repo/`): si l'agent ja ha après
   una convenció, no l'has de repetir cada cop.

## Vull veure quants tokens gasto

Veure secció dedicada al [README principal](../README.md#decisions-obertes)
i al document [06-faq.md](./06-faq.md) per la discussió sobre comptadors
de tokens (i per què avui no és senzill fer-ho de manera fiable).
