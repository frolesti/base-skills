---
applyTo: 'app/**/route.{ts,js}'
description: 'Regles per a Route Handlers de Next.js.'
---

# Route Handlers

- Exporta funcions amb el nom HTTP en majúscules: `GET`, `POST`, `PUT`, `PATCH`, `DELETE`.
- Tipa explícitament `Request` / `NextRequest` i retorna `Response` / `NextResponse`.
- **Valida l'entrada** (body, query, params) amb schema abans de processar.
- Maneja errors retornant codis HTTP correctes amb `Response.json({ error }, { status })`.
- No barregis lògica de negoci dins el handler — extreu-la a `lib/` i fes que el handler només orquestri.
- Configura cache explícitament si cal: `export const dynamic = 'force-dynamic'` o `revalidate = N`.

## Què NO fer

- No retornis `any` ni objectes sense estructura definida.
- No exposis stack traces ni missatges d'error interns en producció.
- No facis servir `route.ts` com a substitut de Server Actions per mutacions del propi frontend.
