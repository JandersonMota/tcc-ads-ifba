# Visão do Produto (MVP)

> **Escopo:** MVP (TCC) · **Status:** Rascunho v0.1 · **Atualizado em:** 10/10/2026
> **Fonte:** [`README.md`](../../README.md)

## 1. Declaração de visão

> **Para** advogados e escritórios de advocacia **que** precisam administrar informações jurídicas, atividades e finanças em ferramentas separadas e pouco intuitivas, **o** Sistema de Gestão Jurídica SaaS **é uma** plataforma web multi-tenant **que** centraliza clientes, processos, atividades e financeiro em um único ambiente, com isolamento total entre escritórios e controle granular de permissões.

## 2. Problema

| Problema | Impacto |
|---|---|
| Informações jurídicas, operacionais e financeiras ficam em ferramentas separadas | Retrabalho, dados duplicados, perda de informação |
| Ferramentas jurídicas pouco intuitivas | Curva de aprendizado alta, baixa adoção pela equipe |
| Pouco controle sobre quem faz o quê no escritório | Dificuldade de delegar, acompanhar e responsabilizar |
| Financeiro desconectado dos processos e clientes | Dificuldade de saber quanto cada cliente/processo gera e deve |

## 3. Público-alvo do MVP

Advogados autônomos e escritórios de pequeno porte. Perfis de usuário: Proprietário, Gerente, Advogado, Estagiário e Usuário (ver [Personas](../03-ux-e-design/personas.md)).

## 4. Objetivo do MVP

Desenvolver um MVP funcional que demonstre a **viabilidade técnica** de um SaaS multi-tenant para escritórios de advocacia, provando que:

> um usuário, mesmo manipulando uma requisição, não consegue acessar dados de outro escritório.

**Foco:** SaaS multi-tenant + controle de permissões + gestão jurídica + delegação de atividades + financeiro básico.

## 5. Princípio central

> Todo dado operacional pertence a um Escritório (Tenant), e toda operação sobre esse dado precisa passar simultaneamente por autorização e isolamento de tenant.

```
REQUISIÇÃO → AUTENTICAÇÃO → IDENTIFICAÇÃO DO TENANT → AUTORIZAÇÃO → REGRA DE NEGÓCIO → ACESSO AO DADO → AUDITORIA
```

## 6. O que o MVP entrega

| Área | Obrigatório | Desejável |
|---|---|---|
| 🔐 Segurança | Cadastro/login, usuários, escritórios, grupos, permissões, controle de acesso, isolamento, auditoria básica | Permissões individuais |
| ⚖️ Jurídico | Clientes, processos, partes, movimentações | Prazos e audiências *(a classificar)* |
| 📋 Operacional | Atividades, delegação a usuários, status | Equipes, comentários, histórico |
| 💰 Financeiro | Honorários, receitas, despesas, contas a receber/pagar, pagamentos | Contratos, categorias, centros de custo, relatórios |
| 📊 Dashboard | Indicadores básicos | — |
| 📁 Outros | — | Documentos, notificações, backup/exportação do escritório |

Plano disponível: somente **Free**. Detalhes em [Documento de Requisitos](../02-requisitos-e-backlog/documento-de-requisitos.md).

## 7. Indicadores de sucesso

| Indicador | Meta |
|---|---|
| Testes de isolamento multi-tenant | 100% passando, cobrindo todos os recursos de negócio |
| Testes de autorização | 4 cenários (próprio tenant, outro tenant, sem permissão, com permissão) por endpoint |
| Fluxo principal | Executável de ponta a ponta (criar escritório → dashboard) |
| Fronteiras de módulos | `ApplicationModules.verify()` passando |
| TCC | Aprovação pela banca do IFBA |

## 8. Fora do escopo do MVP

Planos pagos e assinaturas, integração com tribunais, WhatsApp/Telegram, MFA/2FA, SSO, recuperação de senha por e-mail, API pública, webhooks, calendário, pesquisa global, versionamento de documentos, automações e IA. Ver [Roadmap](../05-planejamento-e-gestao/roadmap.md).
