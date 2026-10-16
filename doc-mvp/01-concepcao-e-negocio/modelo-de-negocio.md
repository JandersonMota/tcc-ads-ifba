# Modelo de Negócio (contexto do MVP)

> **Escopo:** MVP (TCC) · **Status:** Rascunho v0.2 · **Atualizado em:** 15/10/2026

## 1. Tipo de negócio

**SaaS (Software as a Service)** com modelo **Freemium**: um plano gratuito como porta de entrada e planos pagos por assinatura no futuro.

O MVP valida a **viabilidade técnica** da plataforma; a validação comercial fica para depois do TCC.

## 2. Lean Canvas (resumido)

| Bloco | Conteúdo |
|---|---|
| **Problema** | Ferramentas separadas para jurídico, operação e financeiro; ferramentas jurídicas pouco intuitivas; pouco controle sobre delegação |
| **Segmento de clientes** | Advogados autônomos e escritórios de pequeno porte |
| **Proposta de valor** | Gestão jurídica, operacional e financeira em um só lugar, simples de usar e com isolamento/segurança desde o início |
| **Solução (MVP)** | Plataforma web multi-tenant com clientes, processos, atividades, financeiro básico, permissões e auditoria |
| **Canais** | Cadastro self-service no plano Free *(a definir após o TCC)* |
| **Fontes de receita** | Nenhuma no MVP; assinaturas no futuro |
| **Estrutura de custos (MVP)** | Hospedagem da demonstração *(a definir; preferir gratuita ou local)* |
| **Métricas-chave (MVP)** | Indicadores técnicos da [Visão do Produto](visao-do-produto.md#7-indicadores-de-sucesso) |
| **Vantagem** | Arquitetura multi-tenant segura e modular; autorização granular com regras de proprietário |

## 3. Plano no MVP

Princípio: **o Free faz o trabalho; o Pago automatiza, escala e colabora.**

| Recurso | Plano Free (MVP) |
|---|---|
| Escritórios | Cadastro livre |
| Usuários | **2** (Proprietário + 1 usuário ativo) |
| Clientes | **50 ativos** |
| Processos | **50 ativos** (arquivados/baixados não contam) |
| Atividades, movimentações, lançamentos financeiros | Sem limite de quantidade |
| Funcionalidades | Todas as obrigatórias do MVP |
| Cobrança | Não há |

### 3.1 Justificativa dos limites

- **Usuários (2):** atende o advogado autônomo com um estagiário ou secretária e permite usar a delegação de atividades, mas um escritório inteiro não opera de graça. É a principal alavanca de conversão para o Pago (até 10 usuários).
- **Clientes e processos (50 ativos):** suficientes para o autônomo começar e perceber valor; processos arquivados não contam, para não punir o histórico.
- **Registros operacionais sem limite:** limitar o volume de atividades ou lançamentos desestimula o uso; no Free a limitação é de **funcionalidade**, não de volume.

### 3.2 Atividades básicas

| Free | Fora do Free (produto completo) |
|---|---|
| Criar, editar, concluir, cancelar e reabrir atividades | Recorrência |
| Prazo, prioridade e status | Modelos e checklists |
| Vínculo com processo ou cliente | Fluxos e automações |
| Delegação para outro usuário do escritório | Delegação para equipes |
| Listagem e filtros (abertas, atrasadas) | Relatórios de produtividade e notificações por e-mail |

### 3.3 Recursos essenciais

| Free | Fora do Free (produto completo) |
|---|---|
| Clientes (PF/PJ), processos, partes e movimentações manuais | Integrações judiciais automáticas |
| Financeiro núcleo (contas a pagar/receber, honorários) | Relatórios financeiros avançados |
| Dashboard com indicadores principais | Documentos e armazenamento ampliados |
| Usuários, grupos e permissões (modelo completo) | Backup ampliado |
| Auditoria básica (retenção de 90 dias) *(Proposta)* | Auditoria avançada e retenção longa |
| Exportação dos próprios dados | Notificações externas, API/Webhooks, SSO |

### 3.4 Comportamento ao atingir um limite

- Bloqueia apenas a **criação** de novos usuários, clientes ou processos ativos; leitura, edição e arquivamento continuam liberados.
- A **exportação** dos dados do escritório é sempre permitida (LGPD e portabilidade).
- Ao chegar a **80%** do limite, o sistema exibe um aviso. *(Proposta)*

### 3.5 Aplicação no MVP

Os limites são **modelados e aplicados** no MVP, sem cobrança:

- `Plano` / `LimitesDoPlano` no módulo `escritorio`, com valores vindos de configuração (seed Flyway ou `application.yml`), nunca fixos no código.
- Verificação de domínio na criação de usuário, cliente e processo ativo, com contagem sempre filtrada pelo `tenant_id` do token; ao exceder, `LimiteDoPlanoExcedidoException`.
- Fora do MVP: assinatura, pagamento, upgrade/downgrade.

Como os valores são configuráveis, é possível reduzir o limite na demonstração para exibir o bloqueio ao vivo.

## 4. Pendências

- [x] Quantidade limite de usuários, clientes e processos no Free — ver [seção 3](#3-plano-no-mvp).
- [x] Quais são as "atividades básicas" e os "recursos essenciais" do Free — ver [3.2](#32-atividades-básicas) e [3.3](#33-recursos-essenciais).
- [x] Decidir se os limites serão aplicados no MVP ou apenas documentados — aplicados, ver [3.5](#35-aplicação-no-mvp).
