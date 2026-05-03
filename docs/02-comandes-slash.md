# 02 — Comandes `/` explicades

Aquestes comandes les dispares tu escrivint `/` al panel de Copilot Chat.
T'estalvien escriure el mateix context una i altra vegada.

---

## `/plan` — Planificar abans de tocar codi

**Quan usar-la:** abans de qualsevol tasca no trivial (afegir feature,
refactoritzar, migrar). T'estalvia que l'agent es llanci a editar a cegues.

**Què fa:**
1. Et demana fins a 3 preguntes si la tasca és ambigua.
2. Explora el codi en read-only.
3. Et torna un pla amb: objectiu, fitxers afectats, passos numerats,
   riscos i com validar.
4. Crea una llista de tasques (todo list) per anar-la marcant.
5. **S'atura i espera la teva confirmació.**

**Exemple:**

```
/plan Vull afegir login amb Google al projecte
```

L'agent et tornarà un pla revisable. Tu el confirmes o l'ajustes. Després,
quan diguis "endavant", l'execució real comença ja amb context net.

**Estalvi:** evites que l'agent toqui 15 fitxers i n'hagi de desfer 12.

---

## `/review` — Revisar canvis no commitejats o un PR

**Quan usar-la:** abans de fer commit, abans de demanar review humana, o
per analitzar un PR aliè.

**Què fa:**
1. Per defecte, revisa `git diff` + `git diff --cached` (canvis locals).
2. Si li passes una branca/PR, revisa `git diff <base>...<head>`.
3. Aplica una checklist: correctesa, seguretat (OWASP top 10),
   consistència amb les convencions del repo, cobertura de tests,
   code smells.
4. Et torna seccions: **Resum**, **Bloquejants**, **Suggeriments**,
   **LGTM** — amb cites `fitxer:línia`.

**Exemple:**

```
/review
```

o

```
/review compara contra main
```

**No edita res.** Només informa. Tu decideixes què aplicar.

---

## `/commit` — Generar missatge de commit Conventional Commits

**Quan usar-la:** quan ja has fet `git add` i necessites un missatge
estructurat sense pensar-hi.

**Què fa:**
1. Llegeix el diff *staged*.
2. Determina type (`feat`, `fix`, `refactor`…) i scope a partir dels paths.
3. Et torna un missatge format Conventional Commits, llest per copiar:

   ```
   feat(auth): add Google OAuth provider

   Replace mock provider with real Google OAuth client.
   Adds env vars: GOOGLE_CLIENT_ID, GOOGLE_CLIENT_SECRET.
   ```

4. **No fa `git commit` per tu.** Tu decideixes.

**Exemple:**

```
git add .
/commit
```

---

## `/test` — Escriure o ampliar tests

**Quan usar-la:** quan tens codi sense cobertura o quan acabes d'afegir
una funció i vols proves immediatament.

**Què fa:**
1. Identifica l'objectiu (fitxer obert per defecte, o el que li indiquis).
2. Llegeix `.copilot/stack.json` per saber el framework de test
   (vitest, jest, pytest…).
3. Genera tests per: camí feliç, casos límit, errors esperats.
4. **Només toca fitxers de test.** Si el codi de producció no és
   testable, t'ho diu en lloc de refactoritzar-lo silenciosament.
5. Executa els tests si l'entorn ho permet i et mostra el resultat.

**Exemple:**

```
/test
```

(usa el fitxer obert)

```
/test src/lib/parseDate.ts
```

---

## `/skills-audit` — Auditar quines skills s'usen i quines no

**Quan usar-la:** cada cert temps (mensualment, per exemple) per netejar
skills que no aporten res.

**Què fa:**
1. Llegeix `.copilot/usage.json` (telemetria local, **no** es carrega al
   context normal — cost zero al flux diari).
2. Llista skills usades vs no usades mai.
3. Suggereix candidates a esborrar si fa molt que no s'usen.
4. **No esborra res.** Només informa.

**Exemple:**

```
/skills-audit
```

---

## Truc d'optimització: encadenar comandes

```
/plan refactoritzar el sistema d'auth perquè usi cookies HttpOnly
```

→ revises el pla → confirmes → l'agent executa → quan acaba:

```
/review
/commit
```

Tres comandes, zero repetició de context, màxima precisió.
