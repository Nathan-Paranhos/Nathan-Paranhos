<div align="center">

<img src="https://raw.githubusercontent.com/Nathan-Paranhos/Nathan-Paranhos/main/atom.svg" width="180" alt="Nathan Paranhos"/>

<br/>

<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=700&size=24&duration=3500&pause=1200&color=00D4FF&center=true&vCenter=true&width=800&height=60&lines=Nathan+Paranhos;Software+Developer+%7C+Quality+Engineer;APIs+%7C+Integracoes+%7C+Sistemas+Distribuidos;Estabilidade+%7C+Qualidade+%7C+RCA" alt="Typing SVG"/>

<br/>

[![Portfolio](https://img.shields.io/badge/Portfolio-nathan--paranhos.com.br-0d1117?style=for-the-badge&logo=google-chrome&logoColor=00C7B7)](https://nathan-paranhos.com.br/)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0d1117?style=for-the-badge&logo=linkedin&logoColor=0A66C2)](https://www.linkedin.com/in/nathan-paranhos-55807831a/)
[![GitHub](https://img.shields.io/badge/GitHub-Follow-0d1117?style=for-the-badge&logo=github&logoColor=white)](https://github.com/Nathan-Paranhos)
[![Lucro Certo](https://img.shields.io/badge/Produto_em_Prod-lucrocerto.cloud-22c55e?style=for-the-badge&logo=rocket&logoColor=white)](https://lucrocerto.cloud)

<br/>

![Status](https://img.shields.io/badge/Status-Open_to_Opportunities-22c55e?style=flat-square)
&nbsp;
![Location](https://img.shields.io/badge/Jundiai_SP-0d1117?style=flat-square)
&nbsp;
![Focus](https://img.shields.io/badge/Focus-Quality_%26_Dev-0d1117?style=flat-square)

</div>

---

## About me

I'm a **Software Developer Intern** at **Fagron Tech** and **Founder** of **[Aithos Tech](https://lucrocerto.cloud)**, where I build and maintain software running in production.

My focus is on **system stability**, **Root Cause Analysis (RCA)** and **API integration** — always connecting technical depth with business impact.

- Incident investigation and root cause analysis (RCA)
- - Validation of integrations between **REST / GraphQL APIs** and internal services
  - - Analytical queries with **SQL / Firebird**
    - - Async flow monitoring with **RabbitMQ**
      - - Product in production: **[lucrocerto.cloud](https://lucrocerto.cloud)** — SaaS for pricing and financial management for food businesses
       
        - ---

        ## Produto em Producao — Lucro Certo

        <div align="center">

        [![Lucro Certo](https://img.shields.io/badge/Em_Producao-lucrocerto.cloud-22c55e?style=for-the-badge)](https://lucrocerto.cloud)

        </div>

        **[Lucro Certo](https://lucrocerto.cloud)** is a **smart pricing and financial management SaaS** built and maintained by me, focused on food businesses (restaurants, food trucks, bakeries, dark kitchens).

        | Feature | Description |
        |---|---|
        | **Smart Pricing** | Calculates the ideal price considering all costs: ingredients, packaging, fees and waste |
        | **Full Financial Management** | Tracks revenue, expenses and margin in real time |
        | **AI Agent** | Financial advisor that analyzes numbers and suggests actions |
        | **iFood Integration** | Automatically pulls orders and deducts fees to show real profit |
        | **Reports and DRE** | PDF export, scenario simulator and advanced reports |
        | **LGPD Compliant** | Data protection with export and account deletion options |

        > Stack: **Next.js · React 19 · Node.js · PostgreSQL · Supabase · Stripe · Groq AI · iFood API**
        >
        > ---
        >
        > ## Arquitetura — Lucro Certo
        >
        > ```mermaid
        > flowchart TB
        >
        >     subgraph CLIENTE["Cliente - Browser / PWA"]
        >         UI["Next.js App Router · React 19 · Tailwind · PWA"]
        >     end
        >
        >     subgraph EDGE["Edge Middleware"]
        >         E1{"Rota senssivel?"}
        >         E1 -->|sim| E1R["404 imediato"]
        >         E1 -->|nao| E2{"Sessao valida?"}
        >         E2 -->|nao - rota publica| EFAST["Fast-path sem auth"]
        >         E2 -->|sim| E3["Valida usuario"]
        >         E3 --> E4{"Autenticado?"}
        >         E4 -->|nao| E4R["redirect /login"]
        >         E4 -->|sim| E5{"Exige assinatura?"}
        >         E5 -->|sim| E6{"Assinatura ativa?"}
        >         E6 -->|nao - pagina| E6P["redirect /planos"]
        >         E6 -->|nao - API| E6A["402 SUBSCRIPTION_REQUIRED"]
        >         E6 -->|sim| EHDRS
        >         E5 -->|nao| EHDRS
        >         EFAST --> EHDRS
        >         EHDRS["Headers de seguranca: CSP - HSTS - X-Frame-Options"]
        >     end
        >
        >     subgraph AUTH["Autenticacao"]
        >         AC["Cadastro: anti-enumeracao - captcha - consentimento LGPD"]
        >         AL["Login: valida credenciais - sincroniza plano"]
        >         AF["Recuperacao e logout: rate limit - respostas neutras"]
        >     end
        >
        >     subgraph TRIAL["Trial"]
        >         TR1{"Assinatura existe?"}
        >         TR1 -->|nao| TR2["Cria: plano basico - 7 dias - 1 por conta"]
        >         TR1 -->|sim| TR3["Mantem estado atual"]
        >         TR2 --> TR4{"Trial expirado?"}
        >         TR4 -->|nao| TR6["Acessa o app"]
        >         TR4 -->|sim| TR5["Paywall - redirect /planos"]
        >     end
        >
        >     subgraph BILLING["Billing - Stripe"]
        >         BCO["Checkout: cria sessao - referencia ao usuario"]
        >         BWH["Webhook: valida assinatura - idempotente"]
        >         BWH --> BEV{"Evento"}
        >         BEV -->|assinatura criada/atualizada| BACT["Atualiza plano - ativa assinatura"]
        >         BEV -->|pagamento falhou| BGRC["Periodo de carencia - nao rebaixa imediatamente"]
        >         BEV -->|assinatura cancelada| BCAN["Plano cancelado - volta ao basico"]
        >     end
        >
        >     subgraph CHAT["IA Chat - Basico e Pro"]
        >         CH1["Sanitiza entrada - verifica limite do plano"]
        >         CH2["Gera resposta via LLM - stream SSE"]
        >         CH3{"Produto detectado?"}
        >         CH3 -->|sim| CH4["Valida schema - calcula precificacao - salva produto"]
        >         CH3 -->|nao| CH5["Salva historico - finaliza"]
        >         CH4 --> CH5
        >         CH1 --> CH2 --> CH3
        >     end
        >
        >     subgraph AGENTE["IA Agente - Pro"]
        >         AGR{"Plano do usuario?"}
        >         AGR -->|Basico| AGBL["403 - Upgrade necessario"]
        >         AGR -->|Pro| AGL["Executa loop do agente com contexto"]
        >         AGL --> AGT{"LLM precisa de tool?"}
        >         AGT -->|leitura| AGTL["Executa tool - dados do negocio"] --> AGL
        >         AGT -->|acao| AGTA["Propoe acao - aguarda confirmacao do usuario"]
        >         AGT -->|resposta final| AGTS["Stream SSE - salva historico"]
        >     end
        >
        >     subgraph LLM["Cliente LLM - Resiliencia"]
        >         GC1{"Slot disponivel?"}
        >         GC1 -->|nao| GCQ["Fila FIFO - backpressure"]
        >         GCQ -.->|slot liberado| GC1
        >         GC1 -->|sim| GCR["Round-robin entre chaves - prioriza disponiveis"]
        >         GCR --> GCS["Chama API - timeout 120s"]
        >         GCS --> GCO{"Resultado"}
        >         GCO -->|ok| GCOK["Retorna stream"]
        >         GCO -->|rate limit| GCC["Coloca chave em cooldown"]
        >         GCC --> GCN{"Outra chave disponivel?"}
        >         GCO -->|outro erro| GCN
        >         GCN -->|sim| GCR
        >         GCN -->|nao| GCFAIL["Degrada graciosamente - mensagem amigavel"]
        >         GCOK --> GCREL["Libera slot - acorda proximo na fila"]
        >         GCFAIL --> GCREL
        >     end
        >
        >     subgraph IFOOD["iFood - Conexao e Sync"]
        >         ICS["Inicia device flow OAuth - exibe codigo ao usuario"]
        >         ICF["Troca codigo por tokens - salva criptografado"]
        >         ICM["Vincula estabelecimento - status conectado"]
        >         ICS --> ICF --> ICM
        >         IS1["Polling de eventos - token valido"]
        >         IS1 -->|expirado| ISR["Renova token"] --> IS1
        >         IS1 -->|tem eventos| IS2["Processa pedidos em ordem"]
        >         IS2 --> IS4["Busca detalhes do pedido - retry em 404"]
        >         IS4 --> IS5{"Cancelamento?"}
        >         IS5 -->|sim| IS6["status CANCELLED - excluido da receita"]
        >         IS5 -->|nao| IS7["Salva status dos detalhes"]
        >         IS6 --> IS8["Salva pedido - atualiza metricas"]
        >         IS7 --> IS8
        >         IS8 --> IS9["Confirma recebimento para o iFood"]
        >     end
        >
        >     subgraph NEGOCIO["CRUD - Produtos, Vendas, Custos"]
        >         NL{"Limite do plano?"}
        >         NL -->|excedido| NLE["403 PLAN_LIMIT_REACHED"]
        >         NL -->|ok| NW["Escreve no banco - isolado por usuario"]
        >         NW --> NA["Registra auditoria - nao bloqueia operacao"]
        >     end
        >
        >     subgraph CALC["Motor de Calculos - puro, sem side-effects"]
        >         CLP["Precificacao: custo unitario - preco sugerido - break-even - markup"]
        >         CLD["DRE: Receita - Custos - Fixos - Prejuizos = Lucro liquido - alertas"]
        >     end
        >
        >     subgraph REL["Relatorios - Pro"]
        >         RP["Busca paralela: vendas - custos - produtos - pedidos iFood - prejuizos"]
        >         RP --> RB["Calcula KPIs - DRE - series temporais - mix - canais - comparativo"]
        >     end
        >
        >     subgraph LGPD["Privacidade - LGPD"]
        >         LG1["Exportacao e exclusao de conta - anonimiza dados - logout"]
        >         LG2["Consentimentos - retencao - limpeza de historico por idade"]
        >     end
        >
        >     subgraph SEC["Seguranca - Defesa em Camadas"]
        >         S1["Camada 1 - Borda: CSP - HSTS - bloqueio de rotas sensiveis - paywall"]
        >         S2["Camada 2 - Rota: rate limit - validacao de usuario - Zod - sem IDOR"]
        >         S3["Camada 3 - Banco: RLS por usuario - operacoes privilegiadas so via service role"]
        >         S1 --> S2 --> S3
        >     end
        >
        >     subgraph CRON["Cron Jobs"]
        >         CR1["A cada 2 min: sincroniza pedidos iFood"]
        >         CR2["Diario: limpeza de dados por politica de retencao"]
        >     end
        >
        >     subgraph SB["Supabase - Postgres + RLS"]
        >         DB[("usuarios · assinaturas · produtos · vendas\ncustos_fixos · pedidos_ifood · historico_chat\naudit_logs · consentimentos_lgpd")]
        >     end
        >
        >     subgraph EXT["Servicos Externos"]
        >         STRIPE_E["Stripe - Checkout hospedado - Webhooks assinados"]
        >         GROQ_E["Groq API - LLMs open-weight"]
        >         IFOOD_E["iFood API - OAuth device flow - polling de eventos"]
        >         TURN_E["Cloudflare Turnstile - Captcha no cadastro"]
        >     end
        >
        >     CLIENTE -->|HTTPS| EDGE
        >     EDGE -->|request autorizado| AUTH
        >     EDGE -->|request autorizado| TRIAL
        >     EDGE -->|request autorizado| BILLING
        >     EDGE -->|request autorizado| CHAT
        >     EDGE -->|request autorizado| AGENTE
        >     EDGE -->|request autorizado| IFOOD
        >     EDGE -->|request autorizado| NEGOCIO
        >     EDGE -->|request autorizado| REL
        >     EDGE -->|request autorizado| LGPD
        >
        >     CHAT --> LLM
        >     AGENTE --> LLM
        >     LLM -->|stream| GROQ_E
        >
        >     NEGOCIO --> CALC
        >     REL --> CALC
        >
        >     AUTH --> SB
        >     TRIAL --> SB
        >     BILLING --> SB
        >     CHAT --> SB
        >     AGENTE --> SB
        >     IFOOD --> SB
        >     NEGOCIO --> SB
        >     REL --> SB
        >     LGPD --> SB
        >
        >     AC -->|captcha| TURN_E
        >     BCO -->|cria sessao| STRIPE_E
        >     STRIPE_E -.->|webhook| BWH
        >     IFOOD -->|OAuth e polling| IFOOD_E
        >
        >     CRON -->|dispara sync| IFOOD
        >     CRON -->|dispara retencao| LGPD
        >
        >     SEC -.->|camada 1| EDGE
        >     SEC -.->|camada 2| NEGOCIO
        >     SEC -.->|camada 3| SB
        > ```
        >
        > ---
        >
        > ## Tech Stack
        >
        > <div align="center">

        ![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
        ![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white)
        ![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)
        ![React](https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=black)
        ![Next.js](https://img.shields.io/badge/Next.js-000000?style=for-the-badge&logo=nextdotjs&logoColor=white)
        ![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=for-the-badge&logo=postgresql&logoColor=white)
        ![Supabase](https://img.shields.io/badge/Supabase-3ECF8E?style=for-the-badge&logo=supabase&logoColor=white)
        ![Stripe](https://img.shields.io/badge/Stripe-635BFF?style=for-the-badge&logo=stripe&logoColor=white)
        ![SQL](https://img.shields.io/badge/SQL_Firebird-CC2927?style=for-the-badge&logo=databricks&logoColor=white)
        ![RabbitMQ](https://img.shields.io/badge/RabbitMQ-FF6600?style=for-the-badge&logo=rabbitmq&logoColor=white)
        ![GraphQL](https://img.shields.io/badge/GraphQL-E10098?style=for-the-badge&logo=graphql&logoColor=white)
        ![Git](https://img.shields.io/badge/Git-F05032?style=for-the-badge&logo=git&logoColor=white)

        </div>

        ---

        ## GitHub Stats

        <div align="center">

        <img height="160" src="https://github-readme-stats.vercel.app/api?username=Nathan-Paranhos&show_icons=true&theme=github_dark&hide_border=true&include_all_commits=true&count_private=true" alt="GitHub Stats"/>
        &nbsp;
        <img height="160" src="https://github-readme-stats.vercel.app/api/top-langs/?username=Nathan-Paranhos&layout=compact&theme=github_dark&hide_border=true&langs_count=8" alt="Top Languages"/>

        </div>

        ---

        ## Experiencia

        **Estagiario de Desenvolvimento de Software** · Fagron Tech *(atual)*
        > Investigacao de incidentes, RCA, validacao de APIs REST/GraphQL, analise com SQL/Firebird e monitoramento de filas RabbitMQ em ambiente de producao com times ageis (Scrum/Kanban).
        >
        > **Fundador e Desenvolvedor** · Aithos Tech *(atual)*
        > > Criacao e manutencao do **Lucro Certo** — SaaS em producao para negocios de alimentacao. Responsavel por arquitetura, desenvolvimento full-stack, integracoes e infraestrutura.
        > >
        > > ---
        > >
        > > <div align="center">

        *"Qualidade nao e um ato, e um habito."*

        [![Contato](https://img.shields.io/badge/Entre_em_contato-0d1117?style=for-the-badge&logo=gmail&logoColor=EA4335)](mailto:contato@nathan-paranhos.com.br)

        </div>
