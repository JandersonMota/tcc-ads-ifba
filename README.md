# 📌 Sistema de Gestão Jurídica SaaS

**Versão**: 1.0

**Tipo**: SaaS — Software as a Service

**Segmento**: Gestão para escritórios de advocacia

**Modelo de negócio**: Freemium / SaaS

**Aplicação acadêmica**: MVP utilizado como projeto de TCC em Análise e Desenvolvimento de Sistemas

# 📌 Problema
Escritórios de advocacia precisam administrar informações jurídicas, atividades e informações financeiras, muitas vezes utilizando ferramentas separadas.

Além disso, a utilização das ferramentas jurídicas não são intuitivas, dificultando o desenvolvimento do trabalho.

# 📌 Visão do Produto

Apresentar a visão completa da plataforma **SaaS de gestão para escritórios de advocacia**, destinada a centralizar informações jurídicas, operacionais e financeiras em um único ambiente.

O objetivo é permitir que um escritório gerencie:

- clientes;
- processos judiciais;
- partes processuais;
- movimentações;
- prazos;
- audiências;
- atividades;
- equipes;
- documentos;
- contratos;
- honorários;
- receitas;
- despesas;
- contas a receber;
- contas a pagar;
- pagamentos;
- notificações;
- usuários;
- permissões;
- auditoria;
- backup dos dados do escritório;
- exportação de dados.

O sistema será concebido desde o início como uma plataforma multi-tenant, permitindo que diferentes escritórios utilizem a mesma infraestrutura sem que exista compartilhamento indevido de dados.

# 📌 Objetivos do Produto
## 2.1 Objetivo geral

Desenvolver um MVP funcional capaz de demonstrar a viabilidade técnica da solução para escritórios de advocacia na **organização, acompanhamento e administração de suas atividades jurídicas e financeiras**.

## 2.2 Objetivos específicos

O sistema deverá:

- Centralizar informações de clientes e processos.
- Facilitar o acompanhamento de processos judiciais.
- Permitir o gerenciamento de atividades e tarefas.
- Permitir a delegação de atividades para usuários e equipes.
- Controlar prazos e audiências.
- Permitir o gerenciamento financeiro do escritório.
- Disponibilizar controle granular de permissões.
- Garantir isolamento entre diferentes escritórios.
- Registrar operações relevantes por meio de auditoria.
- Permitir exportação dos dados.
- Permitir geração de backup completo dos dados do escritório (tenant).
- Permitir notificações aos usuários.
- Criar uma base tecnológica capaz de evoluir para uma plataforma comercial.

# 📌 Implementação funcional
```
TCC
 │
 ├── Documento acadêmico
 │
 └── MVP funcional
       │
       ├── Backend
       ├── Frontend
       ├── Banco de dados
       ├── Docker
       ├── Documentação
       └── Testes
```

# 📌 Stack Tecnológica

| Item | Tecnologia | Situação |
|---|---|---|
| Linguagem | Java 25 (LTS) | ✅ Definido |
| Build e dependências | Maven 3.10.0 | ✅ Definido |
| IDE | IntelliJ IDEA | ✅ Definido |
| Banco de dados | PostgreSQL | ✅ Definido |
| Migrações de banco | Flyway | ✅ Definido |
| Framework backend | Spring Boot (versão compatível com Java 25) | ⏳ Versão a definir |
| Persistência | Spring Data JPA / Hibernate | ⏳ Versão a definir |
| Modularidade | Spring Modulith | ⏳ Versão a definir |
| Autenticação/autorização | Spring Security + JWT | ⏳ Emissor do token a definir (próprio ou Spring Authorization Server) |
| Testes | JUnit, Testcontainers, Spring Modulith Test / ArchUnit | ⏳ Versões a definir |
| Containers | Docker | ✅ Definido |
| Frontend | — | ⏳ Framework a definir |

# 📌 MVP
## Fluxo de utilização de um escritório
```
Proprietário cria escritório
        ↓
Cadastra usuários
        ↓
Define grupos/permissões
        ↓
Cadastra cliente
        ↓
Cadastra processo
        ↓
Cria atividade
        ↓
Delega atividade
        ↓
Usuário executa atividade
        ↓
Registra movimentação
        ↓
Controla honorário/receita
        ↓
Consulta informações no dashboard
```

## O grande diferencial do MVP
> Multi-tenancy + autorização baseada em permissões.

```
ESCRITÓRIO A
├── João
├── Maria
├── Cliente A
└── Processo A


ESCRITÓRIO B
├── Carlos
├── Pedro
├── Cliente B
└── Processo B
```
E provar que:
> João, mesmo que tente manipular uma requisição para acessar o processo do Escritório B, não consegue.

## Estratégia de multi-tenancy
O principal diferencial do MVP é o isolamento entre escritórios. As estratégias avaliadas foram:

| Estratégia | Avaliação para o MVP |
|---|---|
| Schema compartilhado + coluna `tenant_id` | ✅ **Adotada** |
| Schema por tenant | Complica Flyway, testes e operação |
| Banco por tenant | Excessivo para o MVP |

Estratégia adotada: **schema compartilhado + coluna `tenant_id`**.

Regras:
- Toda tabela de negócio possui a coluna `tenant_id`.
- Filtro na aplicação (Hibernate `@TenantId` / `@Filter`).
- **Row-Level Security (RLS) do PostgreSQL** como segunda camada de defesa.
- O tenant é obtido **sempre do token JWT** do usuário autenticado, nunca de parâmetro, header ou corpo da requisição.

```
Requisição → JWT → tenant_id → Filtro da aplicação → RLS no PostgreSQL
```

Assim, mesmo que um desenvolvedor esqueça um `WHERE tenant_id = ?`, o banco impede o acesso a dados de outro escritório.

## Escopo
As funcionalidades do MVP são classificadas em **obrigatórias** (necessárias para demonstrar a proposta do TCC) e **desejáveis** (implementadas se houver prazo disponível).

| Área | Obrigatórias | Desejáveis |
|---|---|---|
| 🔐 Segurança | Cadastro/login, usuários, escritórios/tenants, grupos, permissões, controle de acesso, isolamento entre escritórios, auditoria básica | — |
| ⚖️ Jurídico | Clientes, processos, partes, movimentações | — |
| 📋 Operacional | Atividades, delegação para usuários, status | Equipes (delegação para equipes), comentários, histórico |
| 💰 Financeiro | Honorários, receitas, despesas, contas a receber, contas a pagar, pagamentos (baixa de contas) | Contratos, categorias, centros de custo, relatórios básicos |
| 📊 Dashboard | Indicadores básicos | — |
| 📁 Outros | — | Documentos, notificações, exportação/backup dos dados do escritório |

Indicadores básicos do dashboard:
- quantidade de clientes;
- processos ativos;
- atividades pendentes;
- prazos;
- receitas;
- despesas;
- contas a receber;
- contas a pagar.

> O maior risco do projeto não é a arquitetura, e sim o escopo. O módulo financeiro é extenso, por isso apenas o núcleo financeiro é obrigatório.

## Plano
```
Plano
└── FREE
```

## O foco do MVP
> SaaS multi-tenant + controle de permissões + gestão jurídica + delegação de atividades + financeiro básico.

# 📌 Teste
## Testes unitários
Regras de negócio (JUnit).

## Testes de integração
Banco de dados, APIs etc., executados com **Testcontainers** (PostgreSQL real, e não banco em memória).

## Testes de autorização
Por exemplo:
```
Usuário A → Cliente do próprio escritório → PERMITIDO

Usuário A → Cliente de outro escritório → NEGADO

Usuário sem permissão → Criar processo → NEGADO

Usuário com permissão → Criar processo → PERMITIDO
```

## Teste de isolamento multi-tenant
É o teste mais importante do projeto, pois comprova o principal diferencial do MVP.
- Executado com **Testcontainers** (PostgreSQL real), incluindo as **políticas de RLS**.
- Deve validar as duas camadas de defesa: o filtro da aplicação e o RLS do banco (por exemplo, uma consulta sem filtro de `tenant_id` não pode retornar dados de outro escritório).

## Testes de arquitetura
- Verificação das fronteiras entre módulos com **Spring Modulith** (complementado por **ArchUnit** quando necessário).
- Garante que nenhum módulo acesse o `domain` ou o `adapter` interno de outro módulo.

# 📌 Roadmap futuro
- integração real com todos os tribunais;
- WhatsApp;
- Telegram;
- IA;
- API pública;
- Webhooks;
- SSO;
- Business/Enterprise completo;
- automações avançadas;
- infraestrutura empresarial;
- integrações contábeis;
- recursos avançados de documentos.

# 📌 Documento
## Documento 1 — Visão do Produto
Descreve o sistema completo:
> Free + Pago + Business/Enterprise + integrações + financeiro completo + notificações + futuras funcionalidades.

## Documento 2 — Especificação do MVP
Define exatamente o que será implementado para o TCC.

## Documento 3 — Roadmap
Define o que será implementado depois:
```
MVP
 ↓
V1
 ↓
V2
 ↓
Business
 ↓
Enterprise
 ↓
IA
```

# 📌 Arquitetura do Software 🏗️
Arquitetura escolhida:
- Arquitetura Monólito Modular;
- DDD;
- Hexagonal (Ports and Adapters).

Essa escolha é particularmente escolhida devido o MVP precisa ser suficientemente simples para o TCC.

E irá evoluir posteriormente para microserviços quando houver uma justificativa real.

```
                       ┌─────────────────────────────┐
                       │          FRONTEND           │
                       │      Web Application        │
                       └──────────────┬──────────────┘
                                      │
                                    HTTPS
                                      │
                                      ▼
┌───────────────────────────────────────────────────────────────────────────┐
│                             MONÓLITO MODULAR                              │
│                                                                           │
│  ┌─────────────────────────────────────────────────────────────────────┐  │
│  │        REST API (adaptadores web de entrada de cada módulo)         │  │
│  └─────────────────────────────────────────────────────────────────────┘  │
│                                                                           │
│  ┌───────────────┐ ┌───────────────┐ ┌───────────────┐ ┌───────────────┐  │
│  │   Identity    │ │    Tenant     │ │ Authorization │ │    Client     │  │
│  └───────────────┘ └───────────────┘ └───────────────┘ └───────────────┘  │
│                                                                           │
│  ┌───────────────┐ ┌───────────────┐ ┌───────────────┐ ┌───────────────┐  │
│  │ LegalProcess  │ │   Activity    │ │    Finance    │ │   Document    │  │
│  └───────────────┘ └───────────────┘ └───────────────┘ └───────────────┘  │
│                                                                           │
│  ┌───────────────┐ ┌───────────────┐ ┌───────────────┐                    │
│  │ Notification  │ │     Audit     │ │DataPortability│                    │
│  └───────────────┘ └───────────────┘ └───────────────┘                    │
│                                                                           │
│  Adaptadores de saída: persistência, e-mail, tribunais e storage          │
└───────────────┬───────────────────────────────────────┬───────────────────┘
                │                                       │
                ▼                        ┌──────────────┼──────────────┐
        ┌───────────────┐                ▼              ▼              ▼
        │  PostgreSQL   │           E-mail API    Judicial APIs     Storage
        └───────────────┘
```

Observações sobre o diagrama:
- Não existe uma camada "API / BFF" separada: na arquitetura hexagonal, os controllers REST são **adaptadores de entrada dentro de cada módulo** (`adapter/input/web`).
- E-mail API, Judicial APIs e Storage são acessados pelo **monólito**, por meio de adaptadores de saída, e não pelo banco de dados.

O ponto principal é:
> Um único deploy no MVP, mas módulos de negócio fortemente isolados internamente.

## Fronteiras entre módulos
Sem verificação automática, o monólito modular tende a se tornar um monólito acoplado. Por isso:
- As fronteiras são verificadas automaticamente com **Spring Modulith** (dependências entre módulos, eventos e documentação), complementado por **ArchUnit** quando necessário.
- Cada módulo é dono das suas próprias tabelas; um módulo não faz JOIN nem acessa diretamente tabelas de outro módulo.
- Um módulo só acessa outro pela sua **API pública** (interfaces expostas), nunca pelo `domain` ou `adapter` interno.

Essa regra é o que torna viável a futura extração para microserviços.

## Comunicação entre módulos
- **Chamada síncrona**: via API pública do módulo (interface Java), quando a resposta é necessária imediatamente.
- **Eventos de domínio**: para efeitos colaterais, como `Notification` e `Audit`.

```
Activity ──(AtividadeDelegadaEvent)──► Notification
Finance  ──(PagamentoRegistradoEvent)──► Audit
```

Os eventos utilizam `ApplicationEvent` do Spring com o **registro de publicação de eventos do Spring Modulith** (Event Publication Registry), que garante que o evento não seja perdido caso o consumidor falhe.

## Aplicação pragmática de DDD e Hexagonal
Aplicar a mesma estrutura completa em todos os módulos geraria excesso de código para o prazo do TCC. Por isso, o nível de rigor varia conforme a complexidade do módulo:

| Módulo | Abordagem |
|---|---|
| `authorization`, `tenant`, `activity`, `legalprocess`, `finance` | DDD tático completo (agregados, invariantes, portas e adaptadores) |
| `client`, categorias, `dataportability` | Estrutura simplificada (mais próxima de CRUD) |

## Frontend
```
src/
├── core
├── shared
├── auth
├── dashboard
├── clients
├── processes
├── activities
├── finance
├── documents
├── notifications
├── users
├── permissions
└── settings
```

## Backend
```
src/main/java/com/seuprojeto
│
├── shared
│   ├── domain
│   ├── exception
│   └── infrastructure
│
├── identity
│   ├── domain
│   ├── application
│   ├── adapter
│   └── configuration
│
├── tenant
│   ├── domain
│   ├── application
│   ├── adapter
│   └── configuration
│
├── authorization
│   ├── domain
│   ├── application
│   └── adapter
│
├── client
│   ├── domain
│   ├── application
│   ├── adapter
│   └── configuration
│
├── legalprocess
│   ├── domain
│   ├── application
│   ├── adapter
│   └── configuration
│
├── activity
│   ├── domain
│   ├── application
│   ├── adapter
│   └── configuration
│
├── finance
│   ├── domain
│   ├── application
│   ├── adapter
│   └── configuration
│
├── document
│   ├── domain
│   ├── application
│   └── adapter
│
├── notification
│   ├── domain
│   ├── application
│   └── adapter
│
├── audit
│   ├── domain
│   ├── application
│   └── adapter
│
└── dataportability
    ├── application
    └── adapter
```

Os módulos com DDD tático completo possuem a seguinte arquitetura interna. Por exemplo:
```
legalprocess
│
├── domain
│   ├── model
│   ├── service
│   └── repository
│
├── application
│   ├── usecase
│   ├── command
│   └── query
│
├── adapter
│   ├── input
│   │   └── web
│   └── output
│       └── persistence
│
└── configuration
```

Os módulos simplificados (como `client`) mantêm a separação entre `domain`, `application` e `adapter`, mas sem agregados elaborados nem separação entre `command` e `query`.

Como funciona a comunicação Hexagonal:
```
             ADAPTER
                │
                ▼
          INPUT PORT
                │
                ▼
          APPLICATION
                │
                ▼
           DOMAIN
                │
                ▼
          OUTPUT PORT
                │
                ▼
             ADAPTER
```

## Banco de dados
**PostgreSQL**.

Motivos:
- excelente suporte relacional;
- robusto;
- open source;
- ótimo suporte no ecossistema Java/Spring;
- adequado para SaaS;
- suporta crescimento;
- excelente para containers;
- suporta Row-Level Security (RLS), utilizado no isolamento entre escritórios;
- permite evoluir posteriormente.

O banco não é criado automaticamente pelo Hibernate em produção e sim com **Flyway**. As migrações também criam as **políticas de RLS** das tabelas de negócio.
```
V1__create_tenant.sql
V2__create_user.sql
V3__create_client.sql
V4__create_process.sql
```

## Auditoria
- Implementada pelo módulo `audit` como **listener de eventos de domínio** publicados pelos demais módulos (mesmo mecanismo da seção [Comunicação entre módulos](#comunicação-entre-módulos)).
- Os registros são gravados na tabela `audit_log`:

| Coluna | Descrição |
|---|---|
| `id` | Identificador do registro |
| `tenant_id` | Escritório ao qual o registro pertence |
| `user_id` | Usuário que executou a ação |
| `action` | Ação executada (criação, alteração, exclusão etc.) |
| `entity` / `entity_id` | Entidade afetada e seu identificador |
| `occurred_at` | Data/hora da operação |
| `changed_data` | Dados alterados (JSONB) |

## Backup e portabilidade de dados
O backup oferecido pelo sistema refere-se aos **dados do escritório** (todos os registros do seu `tenant_id`), e não ao banco de dados inteiro.

- **Backup completo do escritório**: em schema compartilhado, consiste na extração de todos os registros com o `tenant_id` do escritório. Por isso, backup e exportação são unificados em um único módulo: **`dataportability`**.
- **Exportação de dados**: utiliza o mesmo módulo, permitindo exportar o conjunto completo ou parte dos dados do escritório.
- O acesso é restrito a usuários com a permissão correspondente (por exemplo, o proprietário do escritório).
- **Backup da infraestrutura** (banco inteiro, com todos os escritórios) é responsabilidade operacional (`pg_dump`, snapshot), não é um módulo de negócio e não fica disponível aos usuários do sistema.
