# Backlog do Produto

> **Escopo:** MVP (TCC) · **Status:** Rascunho v0.1 · **Atualizado em:** 03/11/2026

Formato das histórias: *Como [perfil], quero [ação] para [benefício].* Critérios de aceitação em Dado/Quando/Então. Estimativas em pontos (1, 2, 3, 5, 8) são **propostas** para planejamento.

## 1. Épicos

| Épico | Descrição | Requisitos | Prioridade | Fase |
|---|---|---|---|---|
| EP01 – Fundação | Projeto Maven, módulos, Flyway, Docker, CI, multi-tenancy base | RNF01, RNF06–RNF09 | Obrigatória | 0 |
| EP02 – Identidade e escritório | Cadastro de escritório, login, usuários | RF01–RF04 | Obrigatória | 1 |
| EP03 – Autorização | Grupos, permissões, controle de acesso | RF05–RF08 | Obrigatória | 1 |
| EP04 – Auditoria | Registro e consulta de auditoria | RF10 | Obrigatória | 1 |
| EP05 – Clientes | Cadastro de clientes PF/PJ | RF11 | Obrigatória | 2 |
| EP06 – Processos | Processos, partes, movimentações | RF12–RF15 | Obrigatória | 2 |
| EP07 – Prazos e audiências | Prazos e audiências do processo | RF16, RF17 | A classificar | 2 |
| EP08 – Atividades | Atividades, delegação, status | RF18–RF20 | Obrigatória | 3 |
| EP09 – Colaboração | Equipes, comentários, histórico | RF21–RF23 | Desejável | 5 |
| EP10 – Financeiro núcleo | Honorários, receitas, despesas, contas, pagamentos | RF25–RF30 | Obrigatória | 4 |
| EP11 – Financeiro complementar | Contratos, categorias, centros de custo, relatórios, parcelas | RF31–RF35 | Desejável | 5 |
| EP12 – Dashboard | Indicadores | RF24 | Obrigatória | 4 |
| EP13 – Documentos e notificações | Upload de documentos, central de notificações | RF36, RF37 | Desejável | 5 |
| EP14 – Portabilidade de dados | Backup e exportação do escritório | RF38, RF39 | Desejável | 5 |

## 2. Histórias de usuário (obrigatórias)

### EP01 – Fundação

| ID | História | Pontos |
|---|---|---|
| HU01 | Como desenvolvedor, quero o projeto Maven com Java 25, Spring Boot e Spring Modulith configurados para iniciar os módulos com fronteiras verificadas. | 3 |
| HU02 | Como desenvolvedor, quero PostgreSQL em Docker e Flyway configurados para versionar o schema. | 2 |
| HU03 | Como desenvolvedor, quero a infraestrutura de tenant (`TenantContext`, `@TenantId`, `set_config` + RLS) para isolar os dados desde a primeira tabela. | 8 |
| HU04 | Como desenvolvedor, quero um pipeline de CI que execute `mvn verify` a cada push para garantir que testes e verificação de módulos passem. | 2 |

**HU03 – Critérios de aceitação**
- Dado dois escritórios com dados, quando o usuário do escritório A consulta pela aplicação, então só recebe dados de A.
- Dado `app.tenant_id` de A, quando uma consulta SQL nativa sem `WHERE tenant_id` é executada, então só retorna linhas de A.
- Dado nenhum tenant definido, quando uma consulta é executada, então nenhuma linha é retornada.

### EP02 – Identidade e escritório

| ID | História | Pontos |
|---|---|---|
| HU05 | Como advogado, quero cadastrar meu escritório para começar a usar o sistema como proprietário. | 5 |
| HU06 | Como usuário, quero entrar com e-mail e senha para acessar os dados do meu escritório. | 5 |
| HU07 | Como usuário, quero sair do sistema para encerrar minha sessão. | 1 |
| HU08 | Como proprietário, quero cadastrar, editar e desativar usuários para montar minha equipe. | 5 |

**HU05 – Critérios de aceitação**
- Dado um e-mail não cadastrado, quando informo nome do escritório, meu nome, e-mail e senha válidos, então o escritório é criado no plano Free e eu sou seu Proprietário.
- Dado um e-mail já cadastrado, quando tento cadastrar, então recebo erro de validação.
- Os grupos padrão são criados automaticamente para o novo escritório.

**HU06 – Critérios de aceitação**
- Dado credenciais válidas, quando faço login, então recebo um token contendo meu `tenant_id`.
- Dado senha incorreta ou usuário desativado, então o acesso é negado sem indicar qual dado está errado.
- Dado várias tentativas falhas seguidas, então novas tentativas são bloqueadas temporariamente.

### EP03 – Autorização

| ID | História | Pontos |
|---|---|---|
| HU09 | Como proprietário, quero criar grupos personalizados e editar as permissões dos grupos para controlar o que cada perfil pode fazer. | 5 |
| HU10 | Como proprietário, quero atribuir um grupo a cada usuário para definir suas permissões. | 2 |
| HU11 | Como proprietário, quero que toda ação verifique as permissões do usuário para proteger os dados do escritório. | 5 |

**HU11 – Critérios de aceitação**
- Usuário com a permissão → ação permitida.
- Usuário sem a permissão → HTTP 403.
- Recurso de outro escritório → HTTP 404.
- Usuário com todas as permissões funcionais, mas que não é proprietário → não acessa operações exclusivas do proprietário.

### EP04 – Auditoria

| ID | História | Pontos |
|---|---|---|
| HU12 | Como proprietário, quero consultar o registro de auditoria (quem, quando, o quê, valores antes/depois) para acompanhar as alterações. | 5 |

### EP05 e EP06 – Jurídico

| ID | História | Pontos |
|---|---|---|
| HU13 | Como advogado, quero cadastrar clientes PF e PJ para centralizar os dados de quem atendo. | 3 |
| HU14 | Como advogado, quero cadastrar um processo com seus dados (número CNJ, tribunal, vara etc.) para acompanhá-lo. | 5 |
| HU15 | Como advogado, quero vincular clientes, partes e advogados responsáveis a um processo para refletir todos os envolvidos. | 5 |
| HU16 | Como advogado, quero registrar movimentações e vê-las em linha do tempo para acompanhar o andamento. | 3 |
| HU17 | Como advogado, quero ver os processos de um cliente para ter a visão completa do atendimento. | 2 |

**HU14 – Critérios de aceitação**
- Número CNJ em formato inválido → erro de validação.
- Número CNJ já cadastrado no mesmo escritório → erro; em outro escritório → permitido.

### EP08 – Atividades

| ID | História | Pontos |
|---|---|---|
| HU18 | Como advogado, quero criar atividades vinculadas a cliente e/ou processo para organizar o trabalho. | 3 |
| HU19 | Como gerente, quero delegar uma atividade a um usuário para distribuir o trabalho. | 3 |
| HU20 | Como responsável, quero mudar o status da atividade para informar o andamento. | 3 |
| HU21 | Como usuário, quero ver as atividades atribuídas a mim, filtrando por status e prioridade. | 3 |

**HU19 – Critérios de aceitação**
- Sem `ATIVIDADE_DELEGAR` → 403.
- Responsável de outro escritório ou desativado → erro de validação.
- A delegação publica `AtividadeDelegadaEvent` e é registrada na auditoria.

### EP10 e EP12 – Financeiro e dashboard

| ID | História | Pontos |
|---|---|---|
| HU22 | Como proprietário, quero registrar honorários de um cliente/processo para controlar o que será cobrado. | 3 |
| HU23 | Como financeiro, quero cadastrar contas a receber e a pagar com vencimento para controlar o fluxo financeiro. | 5 |
| HU24 | Como financeiro, quero registrar o pagamento de uma conta para dar baixa e gerar a receita/despesa. | 5 |
| HU25 | Como financeiro, quero registrar receitas e despesas avulsas para manter o caixa completo. | 3 |
| HU26 | Como proprietário, quero ver um dashboard com indicadores jurídicos e financeiros para acompanhar o escritório. | 5 |

**HU24 – Critérios de aceitação**
- Pagamento que excede o saldo → erro.
- Pagamento que quita o saldo → conta passa a PAGA.
- Conta com vencimento passado e não paga é exibida como ATRASADA.

## 3. Histórias desejáveis (resumo)

| ID | História | Épico |
|---|---|---|
| HU27 | Gerenciar prazos e audiências do processo *(a classificar)* | EP07 |
| HU28 | Criar equipes e delegar atividades a equipes | EP09 |
| HU29 | Comentar em atividades e ver seu histórico | EP09 |
| HU30 | Conceder/revogar permissões individuais | EP03 |
| HU31 | Contratos, categorias, centros de custo e parcelas | EP11 |
| HU32 | Relatórios financeiros básicos | EP11 |
| HU33 | Anexar documentos | EP13 |
| HU34 | Central de notificações | EP13 |
| HU35 | Backup e exportação dos dados do escritório | EP14 |

## 4. Fora do escopo do MVP

MFA/2FA, recuperação de senha por e-mail, assinaturas e planos pagos, integrações judiciais e consulta automática, calendário, pesquisa global, versionamento de documentos, notificações por e-mail/WhatsApp/Telegram com preferências, API pública, webhooks, SSO, auditoria avançada, IA.
