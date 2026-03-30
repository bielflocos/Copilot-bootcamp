# 🧩 Modos do Copiloto — Sentinela HIS

![Sentinela](https://img.shields.io/badge/Sentinela-HIS-0ea5e9)
![IA](https://img.shields.io/badge/IA-Copiloto%20Hospitalar-blue)
![Prompt](https://img.shields.io/badge/Prompt-engineering-yellow)
![Node](https://img.shields.io/badge/Node.js-v22-339933)
![Express](https://img.shields.io/badge/Express-5.x-000000)
![SQLite](https://img.shields.io/badge/SQLite-sql.js-003B57)

O Copiloto do Sentinela opera em **5 modos de interação**, cada um customizado para o contexto real do sistema hospitalar: stack (Node.js + Express + SQLite + WebSocket), estrutura de arquivos (18 rotas, 6 services, 60+ tabelas), padrões de código (`getDb`, `createLog`, `authMiddleware`, `req.wss`) e domínio clínico (triagem, prescrição, internação, faturamento).

Todos os prompts foram gerados a partir da análise direta do código-fonte do Sentinela — não são templates genéricos.

---

# ❓ Ask

O modo **Ask** é para fazer perguntas e entender coisas, **sem alterar seu código**. Você pode perguntar sobre um arquivo específico, um erro, uma função, uma stack trace ou até conceitos gerais.

**O que foi customizado para o Sentinela:**

- Conhece a stack real (CommonJS, sql.js, Express 5, WebSocket `ws`)
- Sabe como funciona a autenticação (JWT + cookies httpOnly + tabela `sessoes`)
- Sabe como funciona o banco (`getDb().prepare().run()`, sem ORM)
- Sabe como funciona o WebSocket (`req.wss`, mensagens com campo `type`)
- Sabe como funciona o RBAC (7 perfis, `hasPermission`, `requirePermission`)
- Sabe como funciona o frontend (`app.js` → `initApp()` → `checkAuth()` → `init(user)`)
- Referencia os 18 arquivos de rotas e 60+ tabelas para diagnóstico preciso
- Formato: Resumo → Explicação → Como confirmar → Opções → Oferta de snippet

📄 **Prompt:** [prompts/prompt-ask.md]((https://github.com/bielflocos/Copilot-bootcamp/blob/main/prompts/prompt-agent.md))

---

# ✏️ Edit (Agent Code)

O modo **Edit/Agent** é o mais autônomo. Ele **navega pelo projeto**, **cria arquivos**, **modifica múltiplos pontos** e **mantém contexto entre passos**.

**O que foi customizado para o Sentinela:**

- Padrão de rota completo com exemplo real (validação → operação → auditoria → WebSocket → resposta)
- Padrão de página HTML completo (layout, sidebar, `init(user)`, scripts)
- Sabe onde encaixar cada tipo de mudança (qual arquivo de rotas, qual service, qual util)
- Ciclo de trabalho obrigatório: Descobrir → Planejar → Implementar → Verificar → Finalizar
- Mentalidade hospitalar: avalia risco ao paciente, auditoria, RBAC, duplicidade, integridade clínica
- Conhece os 27 módulos já implementados para não recriar o que existe
- Gera código pronto para colar, usando os padrões reais (`require`, `getDb`, `createLog`)

📄 **Prompt:** [prompts/prompt-agent.md](https://github.com/bielflocos/create-copilot/blob/main/prompts/prompt-agent.md)

---

# 🧭 Plan

O modo **Plan** produz um **plano de implementação revisável** antes de qualquer código. Ele pensa, estrutura e só implementa quando você aprovar.

**O que foi customizado para o Sentinela:**

- Formato obrigatório: Objetivo → Contexto → Escopo → Estratégia → Arquivos → Passos → Testes → Riscos → Próximo passo
- Diretrizes específicas para: nova rota, nova tabela, nova página frontend, segurança/medicação
- Conhece os padrões de query do sql.js (`.prepare().run()`, `.getAsObject()`, `.exec()`)
- Conhece os 3 níveis de auditoria (`nivel: 0` técnico, `1` operacional, `2` crítico)
- Avalia rollback: o que acontece no banco se a implementação falhar no meio
- Mentalidade hospitalar: risco ao paciente, duplicidade, integridade entre módulos
- Regra rígida: no máximo pseudocódigo e assinaturas — código completo só quando aprovado

📄 **Prompt:** [prompts/prompt-plan.md](https://github.com/bielflocos/create-copilot/blob/main/prompts/prompt-plan.md)

---

# 📚 Study

O modo **Study** é focado em **aprendizado ativo**. Funciona como um tutor que ensina conceitos usando o próprio Sentinela como material didático.

**O que foi customizado para o Sentinela:**

- Formato de explicação em 7 passos: Nome → Definição → Analogia → Exemplo mínimo → Armadilhas → Quando usar → Checkpoint
- Analogias hospitalares (triagem como middleware, crachá como JWT)
- Exemplos de código usando arquivos reais do projeto (`core.routes.js`, `server.js`, `rbac.js`)
- Catálogo de 10 categorias de temas mapeados ao projeto (Auth, Banco, WebSocket, Express, Node core, Arquitetura, Frontend, IA, Domínio hospitalar)
- Adaptação automática de nível (iniciante → intermediário → avançado)
- Checkpoints de compreensão após cada explicação
- Dois exemplos completos de resposta mostrando o tom exato

📄 **Prompt:** [prompts/prompt-study.md](https://github.com/bielflocos/create-copilot/blob/main/prompts/prompt-study.md)

---

# 🧠 Resumo rápido

- **Ask** → entender o que está acontecendo no código
- **Edit/Agent** → implementar mudanças com código pronto
- **Plan** → planejar antes de agir, com plano revisável
- **Study** → aprender conceitos usando o Sentinela como contexto

---

# 📁 Estrutura dos prompts

```
prompts/
├── prompt-agent.md    # Edit/Agent — implementa código
├── prompt-ask.md      # Ask — diagnostica e explica
├── prompt-plan.md     # Plan — planeja sem implementar
└── prompt-study.md    # Study — ensina conceitos
```

---
Todos os prompts foram gerados a partir da análise real do projeto: `package.json`, `server.js`, `schema.sql`, `database.js`, `routes/`, `services/`, `utils/`, `static/` e `utils/rbac.js`.
