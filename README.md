<div align="center">

<img src="https://raw.githubusercontent.com/Nathan-Paranhos/Nathan-Paranhos/main/atom.svg" width="160" alt="Nathan Paranhos"/>

<br/>

<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=700&size=22&duration=3500&pause=1200&color=00D4FF&center=true&vCenter=true&width=800&height=55&lines=Nathan+Paranhos;Engenheiro+de+Software+%C2%B7+Full-Stack;APIs+%7C+SaaS+%7C+Integra%C3%A7%C3%B5es+%7C+Testes;Sistemas+est%C3%A1veis+em+produ%C3%A7%C3%A3o" alt="Typing SVG"/>

<br/>

[![Portfolio](https://img.shields.io/badge/Portfolio-nathan--paranhos.com.br-0d1117?style=for-the-badge&logo=google-chrome&logoColor=00C7B7)](https://nathan-paranhos.com.br/)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Nathan_Paranhos-0d1117?style=for-the-badge&logo=linkedin&logoColor=0A66C2)](https://www.linkedin.com/in/nathan-paranhos-55807831a/)
[![GitHub](https://img.shields.io/badge/GitHub-Nathan--Paranhos-0d1117?style=for-the-badge&logo=github&logoColor=white)](https://github.com/Nathan-Paranhos)
[![Lucro Certo](https://img.shields.io/badge/SaaS_em_Producao-lucrocerto.cloud-22c55e?style=for-the-badge)](https://lucrocerto.cloud)
[![Aithos Tech](https://img.shields.io/badge/Empresa-aithostech.com.br-0d1117?style=for-the-badge)](https://aithostech.com.br)

<br/>

![Status](https://img.shields.io/badge/Status-Aberto_a_Oportunidades-22c55e?style=flat-square)
![Local](https://img.shields.io/badge/Local-Jundiai_SP-0d1117?style=flat-square)
![Foco](https://img.shields.io/badge/Foco-Full--Stack_%26_Integracoes-0d1117?style=flat-square)

</div>

---

## Sobre

Engenheiro de Software Full-Stack. Desenvolvo sistemas web, SaaS, APIs e integrações conectando front-end, back-end, banco de dados, requisitos e testes para entregar software estável em produção.

Na **Fagron Tech**, atuo com serviços internos, APIs, RabbitMQ, SQL/Firebird, Azure DevOps e análise de código Delphi. Pela **[Aithos Tech](https://aithostech.com.br)**, construo produtos digitais full-stack do planejamento ao deploy, incluindo o **[Lucro Certo](https://lucrocerto.cloud)** em produção.

**7+** projetos publicados &nbsp;|&nbsp; **3** incidentes críticos analisados com RCA &nbsp;|&nbsp; **2** empresas simultâneas

---

## Produto em Produção — Lucro Certo

<div align="center">

[![Lucro Certo](https://img.shields.io/badge/Em_Produ%C3%A7%C3%A3o-lucrocerto.cloud-22c55e?style=for-the-badge)](https://lucrocerto.cloud)

</div>

**[Lucro Certo](https://lucrocerto.cloud)** é um SaaS de **precificação inteligente e gestão financeira** para negócios de alimentação (restaurantes, food trucks, confeitarias, dark kitchens).

| Funcionalidade | Descrição |
|---|---|
| **Precificação Inteligente** | Calcula o preço ideal: ingredientes, embalagens, taxas e perdas |
| **Gestão Financeira Completa** | Acompanha receitas, despesas e margem em tempo real |
| **Agente IA** | Consultor financeiro que analisa números e sugere ações |
| **Integração iFood** | Puxa pedidos automaticamente e deduz taxas para exibir o lucro real |
| **Relatórios e DRE** | Exportação PDF, simulador de cenários e relatórios avançados |
| **LGPD Compliant** | Dados protegidos com exportação e exclusão de conta |

Stack: **Next.js · React 19 · Node.js · Python · PostgreSQL · MongoDB · Supabase · Stripe · Groq AI · iFood API**

### Arquitetura

```mermaid
flowchart TB
subgraph CLIENTE["Cliente - Browser / PWA"]
UI["Next.js App Router - React 19 - Tailwind - PWA"]
end
subgraph EDGE["Edge Middleware"]
E1{"Rota sensivel?"} -->|sim| E1R["404 imediato"]
E1 -->|nao| E2{"Sessao valida?"}
E2 -->|nao| EFAST["Fast-path sem auth"]
E2 -->|sim| E3["Valida usuario"] --> E4{"Autenticado?"}
E4 -->|nao| E4R["redirect /login"]
E4 -->|sim| E5{"Exige assinatura?"}
E5 -->|sim| E6{"Assinatura ativa?"} -->|nao pag.| E6P["redirect /planos"]
E6 -->|nao API| E6A["402 SUBSCRIPTION_REQUIRED"]
E6 -->|sim| EHDRS
E5 -->|nao| EHDRS
EFAST --> EHDRS["Headers: CSP - HSTS - X-Frame-Options"]
end
subgraph AUTH["Autenticacao"]
AC["Cadastro: anti-enumeracao - captcha - LGPD"]
AL["Login: valida credenciais - sincroniza plano"]
end
subgraph TRIAL["Trial"]
TR1{"Assinatura existe?"} -->|nao| TR2["Cria: basico - 7 dias - 1 por conta"]
TR1 -->|sim| TR3["Mantem estado atual"]
TR2 --> TR4{"Trial expirado?"} -->|nao| TR6["Acessa o app"]
TR4 -->|sim| TR5["Paywall - redirect /planos"]
end
subgraph BILLING["Billing - Stripe"]
BCO["Checkout: cria sessao - referencia ao usuario"]
BWH["Webhook: valida assinatura - idempotente"] --> BEV{"Evento"}
BEV -->|assinatura criada/atualizada| BACT["Atualiza plano - ativa assinatura"]
BEV -->|pagamento falhou| BGRC["Periodo de carencia"]
BEV -->|assinatura cancelada| BCAN["Plano cancelado - volta ao basico"]
end
subgraph CHAT["IA Chat - Basico e Pro"]
CH1["Sanitiza entrada - verifica limite"] --> CH2["LLM stream SSE"] --> CH3{"Produto detectado?"}
CH3 -->|sim| CH4["Valida schema - calcula - salva produto"] --> CH5["Salva historico"]
CH3 -->|nao| CH5
end
subgraph AGENTE["IA Agente - Pro"]
AGR{"Plano?"} -->|Basico| AGBL["403 - Upgrade necessario"]
AGR -->|Pro| AGL["Executa loop com contexto"] --> AGT{"LLM precisa tool?"}
AGT -->|leitura| AGTL["Executa tool"] --> AGL
AGT -->|acao| AGTA["Propoe acao - aguarda confirmacao"]
AGT -->|final| AGTS["Stream SSE - salva historico"]
end
subgraph LLM["Cliente LLM - Resiliencia"]
GC1{"Slot disponivel?"} -->|nao| GCQ["Fila FIFO - backpressure"] -.->|liberado| GC1
GC1 -->|sim| GCR["Round-robin entre chaves"] --> GCS["Chama API - timeout 120s"] --> GCO{"Resultado"}
GCO -->|ok| GCOK["Retorna stream"] --> GCREL["Libera slot"]
GCO -->|rate limit| GCC["Cooldown na chave"] --> GCN{"Outra chave?"}
GCO -->|erro| GCN
GCN -->|sim| GCR
GCN -->|nao| GCFAIL["Degrada graciosamente"] --> GCREL
end
subgraph IFOOD["iFood - Conexao e Sync"]
ICS["Device flow OAuth"] --> ICF["Troca tokens - salva criptografado"] --> ICM["Vincula estabelecimento"]
IS1["Polling de eventos"] -->|expirado| ISR["Renova token"] --> IS1
IS1 -->|eventos| IS2["Processa pedidos"] --> IS4["Busca detalhes - retry 404"]
IS4 --> IS5{"Cancelamento?"} -->|sim| IS6["CANCELLED - excluido da receita"]
IS5 -->|nao| IS7["Salva status"]
IS6 --> IS8["Salva pedido - metricas"] --> IS9["Confirma para o iFood"]
IS7 --> IS8
end
subgraph NEGOCIO["CRUD - Produtos Vendas Custos"]
NL{"Limite do plano?"} -->|excedido| NLE["403 PLAN_LIMIT_REACHED"]
NL -->|ok| NW["Escreve no banco - isolado por usuario"] --> NA["Auditoria - nao bloqueia"]
end
subgraph CALC["Motor de Calculos"]
CLP["Precificacao: custo - preco sugerido - break-even - markup"]
CLD["DRE: Receita - Custos - Fixos = Lucro - alertas"]
end
subgraph SEC["Seguranca - Defesa em Camadas"]
S1["Camada 1 - Borda: CSP - HSTS - paywall"] --> S2["Camada 2 - Rota: rate limit - Zod - sem IDOR"] --> S3["Camada 3 - Banco: RLS por usuario"]
end
subgraph SB["Supabase - Postgres + RLS"]
DB[("usuarios - assinaturas - produtos - vendas - pedidos iFood - historico chat - audit logs")]
end
subgraph EXT["Servicos Externos"]
STRIPE_E["Stripe - Webhooks assinados"]
GROQ_E["Groq API - LLMs open-weight"]
IFOOD_E["iFood API - OAuth + polling"]
TURN_E["Cloudflare Turnstile - Captcha"]
end
CLIENTE -->|HTTPS| EDGE
EDGE --> AUTH & TRIAL & BILLING & CHAT & AGENTE & IFOOD & NEGOCIO
CHAT --> LLM
AGENTE --> LLM
LLM -->|stream| GROQ_E
NEGOCIO --> CALC
AUTH --> SB
TRIAL --> SB
BILLING --> SB
CHAT --> SB
AGENTE --> SB
IFOOD --> SB
NEGOCIO --> SB
AC --> TURN_E
BCO --> STRIPE_E -.->|webhook| BWH
IFOOD --> IFOOD_E
SEC -.-> EDGE
SEC -.-> NEGOCIO
SEC -.-> SB
```

---

## Cases

| # | Projeto | Papel | Stack |
|---|---|---|---|
| **01** | **[PME OS](https://pmeos-tech.netlify.app)** — SaaS multitenant com RLS, IA generativa por tenant e pagamentos | Arquitetura, dev full-stack, integrações, testes E2E | Next.js 14 · TypeScript · Supabase · Groq AI · Asaas · Playwright |
| **02** | **[Lucro Certo](https://lucrocerto.cloud)** — Precificação inteligente para MEIs do setor alimentício | Dev full-stack, SaaS, IA conversacional, freemium, pagamentos | Next.js · TypeScript · Supabase · Groq AI · Stripe |
| **03** | **[Aithos LabCode](https://github.com/Nathan-Paranhos)** — Bancada desktop para engenharia de software | Arquitetura, interface, módulos, publicação open source | Electron · Node.js · TypeScript |

---

## Experiência

### Engenheiro de Software (Estágio) · Fagron Tech
**fev. 2026 - atual** · São Paulo, SP · Engenharia de Software

- Validação e refinamento de requisitos funcionais, regras de negócio e critérios de aceite
- Investigação de incidentes em produção com análise de logs, SQL/Firebird e RCA
- Validação de integrações entre APIs, payloads, contratos e filas com RabbitMQ
- Testes funcionais/regressivos e validação pós-implantação em ambiente produtivo
- Gestão de demandas com Azure DevOps, Kanban, plannings e retrospectivas
- Análise de código legado em Delphi para correções, estabilização e documentação

![C#](https://img.shields.io/badge/C%23-239120?style=flat-square&logo=csharp&logoColor=white) ![Delphi](https://img.shields.io/badge/Delphi-EE1F35?style=flat-square) ![SQL/Firebird](https://img.shields.io/badge/SQL%2FFirebird-CC2927?style=flat-square) ![RabbitMQ](https://img.shields.io/badge/RabbitMQ-FF6600?style=flat-square&logo=rabbitmq&logoColor=white) ![Azure DevOps](https://img.shields.io/badge/Azure_DevOps-0078D7?style=flat-square&logo=azuredevops&logoColor=white) ![Scrum](https://img.shields.io/badge/Scrum%2FKanban-0052CC?style=flat-square)

---

### Fundador & Engenheiro de Software · Aithos Tech
**2025 - atual** · Jundiaí, SP · [aithostech.com.br](https://aithostech.com.br)

- Ciclo completo de produtos digitais: requisitos, arquitetura, dev full-stack, integrações e deploy
- Desenvolvimento do PME OS (SaaS multitenant com RLS, IA e pagamentos)
- Desenvolvimento e manutenção do Lucro Certo em produção
- Entrega de projetos para clientes: SaaS, landing pages, dashboards e sites
- Testes E2E com Playwright e documentação técnica

![Next.js](https://img.shields.io/badge/Next.js-000000?style=flat-square&logo=nextdotjs&logoColor=white) ![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white) ![Node.js](https://img.shields.io/badge/Node.js-339933?style=flat-square&logo=nodedotjs&logoColor=white) ![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white) ![Supabase](https://img.shields.io/badge/Supabase-3ECF8E?style=flat-square&logo=supabase&logoColor=white) ![Playwright](https://img.shields.io/badge/Playwright-45ba4b?style=flat-square&logo=playwright&logoColor=white)

---

### Software Support (Estágio) · Fagron Tech
**abr. 2025 - fev. 2026** · São Paulo, SP

- Liderança em projetos de implantação de ERP/CRM e homologação de sistemas
- Consultas e validações em banco Firebird/SQL para sustentação e decisão técnica
- Automação de processos internos com Power Automate e SharePoint
- Gestão de backlog com Monday, Azure DevOps e Microsoft Project
- Experiência com Microsoft Dynamics 365 e mapeamento de processos

![Firebird](https://img.shields.io/badge/Firebird-CC2927?style=flat-square) ![Dynamics 365](https://img.shields.io/badge/Dynamics_365-0078D4?style=flat-square&logo=microsoft&logoColor=white) ![Power Automate](https://img.shields.io/badge/Power_Automate-0066FF?style=flat-square&logo=powerautomate&logoColor=white) ![SharePoint](https://img.shields.io/badge/SharePoint-0078D4?style=flat-square&logo=microsoftsharepoint&logoColor=white)

---

### Assistente ADM / TI · Abrylar Imoveis
**ago. 2023 - jan. 2025** · Jundiaí, SP

- Automação de tarefas repetitivas, relatórios e organização de dados
- Suporte em sistemas de gestão imobiliária e atendimento a usuários
- Desenvolvimento de landing pages responsivas e dashboards de indicadores

![HTML/CSS](https://img.shields.io/badge/HTML%2FCSS-E34F26?style=flat-square&logo=html5&logoColor=white) ![Dashboards](https://img.shields.io/badge/Dashboards-0d1117?style=flat-square) ![Automacao](https://img.shields.io/badge/Automacao-0d1117?style=flat-square)

---

## Projetos

| # | Projeto | Descrição | Stack |
|---|---|---|---|
| 01 | **[KV Cache QJL](https://github.com/Nathan-Paranhos)** | Simulação de compressão de KV Cache com rotação Johnson-Lindenstrauss e quantização polar | C# · .NET 8 · WinForms |
| 02 | **[Loja Fake API](https://github.com/Nathan-Paranhos)** | Catálogo com Fake Store API, custom hooks, service layer, filtros e carrinho | React 18 · TypeScript · Vite · Puppeteer |
| 03 | **[GroqNote v1](https://github.com/Nathan-Paranhos)** | Editor Markdown e testador REST com análise por IA, persistência local e MVVM | C# · .NET 8 · Avalonia UI · Groq API |
| 04 | **LaudoBot** | Geração de laudos em PDF via WhatsApp com validação e organização automática | Node.js · Evolution API · Firebase · PDFKit |
| 05 | **[Visionaria Vistorias](https://visionariavistorias.com.br)** | Landing page responsiva para geração de leads | Next.js · Tailwind CSS |
| 06 | **[Aithos Tech](https://aithostech.com.br)** | Site institucional com animações CSS nativas e identidade visual | Next.js · TypeScript · Tailwind |
| 07 | **[Darlan Goncalves](https://darlan-goncalves.com.br)** | Site pessoal para analista de sistemas com identidade personalizada | Next.js · TypeScript · Tailwind CSS |
| 08 | **[Sobral Crédito Seguro](https://sobralcreditoseguro.com.br)** | Site institucional para empresa de crédito | Next.js · Tailwind CSS |
| 09 | **TCC Arduino** | Monitoramento de ambiente com Arduino e sensores (projeto acadêmico) | Arduino · C++ · IoT |

---

## Stack

<div align="center">

**Front-end**

![React](https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=black)
![Next.js](https://img.shields.io/badge/Next.js-000000?style=for-the-badge&logo=nextdotjs&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-06B6D4?style=for-the-badge&logo=tailwindcss&logoColor=white)
![Vite](https://img.shields.io/badge/Vite-646CFF?style=for-the-badge&logo=vite&logoColor=white)
![Framer Motion](https://img.shields.io/badge/Framer_Motion-0055FF?style=for-the-badge&logo=framer&logoColor=white)

**Back-end e Integrações**

![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![C#/.NET 8](https://img.shields.io/badge/C%23_.NET_8-239120?style=for-the-badge&logo=csharp&logoColor=white)
![GraphQL](https://img.shields.io/badge/GraphQL-E10098?style=for-the-badge&logo=graphql&logoColor=white)
![RabbitMQ](https://img.shields.io/badge/RabbitMQ-FF6600?style=for-the-badge&logo=rabbitmq&logoColor=white)
![JWT](https://img.shields.io/badge/JWT-000000?style=for-the-badge&logo=jsonwebtokens&logoColor=white)
![OAuth](https://img.shields.io/badge/OAuth-4285F4?style=for-the-badge)
![Delphi](https://img.shields.io/badge/Delphi-EE1F35?style=for-the-badge&logo=delphi&logoColor=white)

**Testes e Qualidade**

![Playwright](https://img.shields.io/badge/Playwright-45ba4b?style=for-the-badge&logo=playwright&logoColor=white)
![Puppeteer](https://img.shields.io/badge/Puppeteer-40B5A4?style=for-the-badge&logo=puppeteer&logoColor=white)
![RCA](https://img.shields.io/badge/RCA-0d1117?style=for-the-badge)
![Testes Funcionais](https://img.shields.io/badge/Testes_Funcionais-0d1117?style=for-the-badge)

**Banco de Dados Relacionais**

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=for-the-badge&logo=postgresql&logoColor=white)
![SQL/Firebird](https://img.shields.io/badge/SQL%2FFirebird-CC2927?style=for-the-badge)
![Row-Level Security](https://img.shields.io/badge/Row--Level_Security-0d1117?style=for-the-badge)

**BaaS e Banco NoSQL**

![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=for-the-badge&logo=mongodb&logoColor=white)
![Supabase](https://img.shields.io/badge/Supabase-3ECF8E?style=for-the-badge&logo=supabase&logoColor=white)
![Firebase](https://img.shields.io/badge/Firebase-FFCA28?style=for-the-badge&logo=firebase&logoColor=black)

**DevOps e Plataformas**

![Vercel](https://img.shields.io/badge/Vercel-000000?style=for-the-badge&logo=vercel&logoColor=white)
![Netlify](https://img.shields.io/badge/Netlify-00C7B7?style=for-the-badge&logo=netlify&logoColor=white)
![Azure DevOps](https://img.shields.io/badge/Azure_DevOps-0078D7?style=for-the-badge&logo=azuredevops&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?style=for-the-badge&logo=githubactions&logoColor=white)
![Stripe](https://img.shields.io/badge/Stripe-635BFF?style=for-the-badge&logo=stripe&logoColor=white)

**IA e Automação**

![Groq API](https://img.shields.io/badge/Groq_API-F55036?style=for-the-badge)
![Llama 3.3 70B](https://img.shields.io/badge/Llama_3.3_70B-0d1117?style=for-the-badge)
![Power Automate](https://img.shields.io/badge/Power_Automate-0066FF?style=for-the-badge&logo=powerautomate&logoColor=white)

**Desktop**

![Electron](https://img.shields.io/badge/Electron-47848F?style=for-the-badge&logo=electron&logoColor=white)
![WinForms](https://img.shields.io/badge/WinForms-.NET_8-512BD4?style=for-the-badge)
![Avalonia UI](https://img.shields.io/badge/Avalonia_UI-8B45FF?style=for-the-badge)
![MVVM](https://img.shields.io/badge/MVVM-0d1117?style=for-the-badge)

**Corporativo**

![Dynamics 365](https://img.shields.io/badge/Dynamics_365-0078D4?style=for-the-badge&logo=microsoft&logoColor=white)
![SharePoint](https://img.shields.io/badge/SharePoint-0078D4?style=for-the-badge&logo=microsoftsharepoint&logoColor=white)
![Monday.com](https://img.shields.io/badge/Monday.com-FF3750?style=for-the-badge&logo=mondaydotcom&logoColor=white)

</div>

---

## GitHub Stats

<div align="center">

<img height="160" src="https://github-readme-stats-sigma-seven.vercel.app/api?username=Nathan-Paranhos&show_icons=true&theme=github_dark&hide_border=true&include_all_commits=true&count_private=true" alt="GitHub Stats"/>
&nbsp;
<img height="160" src="https://github-readme-stats-sigma-seven.vercel.app/api/top-langs/?username=Nathan-Paranhos&layout=compact&theme=github_dark&hide_border=true&langs_count=8" alt="Top Languages"/>

</div>

---

## Formação

| Período | Curso | Instituição |
|---|---|---|
| 2025 - 2028 | Bacharelado em Engenharia de Software | Universidade Estácio |
| 2021 - 2022 | Técnico em Tecnologia da Informação | FAACG |

Inglês: leitura técnica de documentações, APIs e releases; escrita básica para comunicação.

---

## Contato

<div align="center">

Atuo em oportunidades que envolvem front-end, back-end, APIs, banco de dados, automações, integrações e testes.

[![WhatsApp](https://img.shields.io/badge/WhatsApp-%2B55_11_99696--1151-25D366?style=for-the-badge&logo=whatsapp&logoColor=white)](https://wa.me/5511996961151)
[![E-mail](https://img.shields.io/badge/E--mail-paranhoscontato.n%40gmail.com-EA4335?style=for-the-badge&logo=gmail&logoColor=white)](mailto:paranhoscontato.n@gmail.com)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Nathan_Paranhos-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/nathan-paranhos-55807831a/)
[![Portfolio](https://img.shields.io/badge/Portfolio-nathan--paranhos.com.br-00C7B7?style=for-the-badge&logo=google-chrome&logoColor=white)](https://nathan-paranhos.com.br/)
[![Aithos Tech](https://img.shields.io/badge/Empresa-aithostech.com.br-0d1117?style=for-the-badge)](https://aithostech.com.br)

</div>

---

<div align="center">

*"Qualidade não é um ato, é um hábito."*

**2026 Nathan Paranhos · Jundiaí, SP · [Aithos Tech](https://aithostech.com.br)**

</div>
