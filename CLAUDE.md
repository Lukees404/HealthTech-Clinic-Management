# AGENTS.md

## Stack & Arquitetura
- **Frontend:** Angular (Standalone Components, Signals, Lazy Loading).
- **Backend/SSR:** Rotas de servidor Angular SSR em `src/server/api/`.
- **Banco de Dados:** PostgreSQL (Docker) via Prisma ORM (`src/server/db.ts`).
- **Testes & Deploy:** Vitest + TypeScript estrito; alvo de deploy na Vercel.

## Fronteiras de Arquivos (Isolamento Paralelo)
- **Onda 1 (Contratos):** Apenas o agente arquiteto edita `prisma/schema.prisma` e registra rotas *lazy* em `src/app/app.routes.ts`.
- **Onda 2 (Subagentes):** Edite **exclusivamente** os arquivos dentro da sua pasta designada (`src/app/features/<nome>/` ou `src/server/api/<nome>/`).
- **Proibido na Onda 2:** Alterar `schema.prisma`, `app.routes.ts` ou instalar novos pacotes.

## Fluxo OpenSpec & TDD Estrito
1. **Leia o Delta:** Consulte a especificação ativa em `openspec/changes/` antes de escrever código.
2. **Red:** Crie primeiro o `*.spec.ts` ao lado do arquivo alvo cobrindo os critérios de aceite e rode `npx vitest run <sua-pasta>` para confirmar a falha.
3. **Green:** Implemente o código mínimo usando os tipos gerados pelo Prisma até o teste passar.
4. **Refactor:** Remova duplicações mantendo os testes verdes.

## Critério de Conclusão
Só encerre a tarefa quando ambos os comandos rodarem sem erros no terminal:
- `npx vitest run <sua-pasta>`
- `npx tsc --noEmit`