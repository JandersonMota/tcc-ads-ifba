# Documento de Requisitos (Especificação do MVP)

> **Escopo:** MVP (TCC) · **Status:** Rascunho v0.1 · **Atualizado em:** 03/11/2026
> **Fontes:** [`README.md`](../../README.md) (seção Escopo)

## 1. Convenções

- **Prioridade:** `Obrigatória` (necessária para demonstrar a proposta do TCC) ou `Desejável` (se houver prazo), conforme o README.
- **Origem:** `README` quando a classificação vem do README; `Proposta` quando foi sugerida neste documento e precisa ser confirmada.
- Regras de negócio estão em [regras-de-negocio.md](regras-de-negocio.md); histórias e critérios de aceitação em [backlog-do-produto.md](backlog-do-produto.md).

## 2. Requisitos funcionais

### 2.1 Segurança e administração

| ID | Requisito | Prioridade | Origem |
|---|---|---|---|
| RF01 | Cadastrar escritório, criando o tenant e o usuário **Proprietário** | Obrigatória | README |
| RF02 | Autenticar usuário com e-mail e senha, emitindo token JWT com o `tenant_id` | Obrigatória | README |
| RF03 | Encerrar sessão (logout) | Obrigatória | Proposta |
| RF04 | Gerenciar usuários do escritório (criar, editar, desativar) | Obrigatória | README |
| RF05 | Gerenciar grupos: padrão (Proprietário, Gerente, Advogado, Estagiário, Usuário) e personalizados | Obrigatória | README |
| RF06 | Configurar permissões de cada grupo | Obrigatória | README |
| RF07 | Conceder ou revogar permissões individualmente por usuário | Desejável | Proposta |
| RF08 | Verificar permissão em toda operação de negócio | Obrigatória | README |
| RF09 | Isolar dados entre escritórios (aplicação + RLS) | Obrigatória | README |
| RF10 | Registrar auditoria das operações relevantes e permitir consulta ao proprietário | Obrigatória | README |

### 2.2 Gestão jurídica

| ID | Requisito | Prioridade | Origem |
|---|---|---|---|
| RF11 | Gerenciar clientes pessoa física e jurídica (cadastrar, editar, visualizar, excluir) | Obrigatória | README |
| RF12 | Gerenciar processos (número CNJ, tribunal, comarca, vara, classe, assunto, valor da causa, data de distribuição, status, advogados responsáveis) | Obrigatória | README |
| RF13 | Associar um ou mais clientes a um processo e listar os processos de um cliente | Obrigatória | README |
| RF14 | Gerenciar partes do processo (polo ativo, passivo, terceiro) | Obrigatória | README |
| RF15 | Registrar movimentações manualmente e exibi-las em linha do tempo | Obrigatória | README |
| RF16 | Gerenciar prazos do processo (responsável, data inicial/final, prioridade, status) | **A classificar** | — |
| RF17 | Gerenciar audiências (tipo, data, horário, local, responsável) | **A classificar** | — |

> ⚠️ **Pendência:** prazos e audiências constam nos objetivos e no dashboard do README, mas não na tabela de escopo. Classificar como obrigatória ou desejável.

### 2.3 Gestão operacional

| ID | Requisito | Prioridade | Origem |
|---|---|---|---|
| RF18 | Gerenciar atividades (título, descrição, tipo, prioridade, prazo, cliente e processo vinculados) | Obrigatória | README |
| RF19 | Delegar atividade a um usuário | Obrigatória | README |
| RF20 | Alterar status da atividade (A fazer, Em andamento, Em revisão, Concluída, Bloqueada, Cancelada) | Obrigatória | README |
| RF21 | Gerenciar equipes e delegar atividades a equipes | Desejável | README |
| RF22 | Comentar em atividades | Desejável | README |
| RF23 | Exibir histórico de alterações da atividade | Desejável | README |
| RF24 | Exibir dashboard com indicadores (clientes, processos ativos, atividades pendentes, prazos, receitas, despesas, contas a receber e a pagar) | Obrigatória | README |

### 2.4 Gestão financeira

| ID | Requisito | Prioridade | Origem |
|---|---|---|---|
| RF25 | Registrar honorários (fixo, parcelado, por etapa) vinculados a cliente e/ou processo | Obrigatória | README |
| RF26 | Registrar receitas | Obrigatória | README |
| RF27 | Registrar despesas | Obrigatória | README |
| RF28 | Gerenciar contas a receber (valor, vencimento, status) | Obrigatória | README |
| RF29 | Gerenciar contas a pagar (fornecedor, valor, vencimento, status) | Obrigatória | README |
| RF30 | Registrar pagamentos (baixa) de contas: data, valor, forma de pagamento | Obrigatória | README |
| RF31 | Gerenciar contratos com clientes | Desejável | README |
| RF32 | Gerenciar categorias de receitas e despesas | Desejável | README |
| RF33 | Gerenciar centros de custo | Desejável | README |
| RF34 | Emitir relatórios financeiros básicos (fluxo de caixa, receitas e despesas por período) | Desejável | README |
| RF35 | Dividir contas em parcelas | Desejável | Proposta |

### 2.5 Outros

| ID | Requisito | Prioridade | Origem |
|---|---|---|---|
| RF36 | Anexar documentos (upload/download) a cliente, processo ou atividade | Desejável | README |
| RF37 | Central de notificações no sistema (ex.: atividade delegada) | Desejável | README |
| RF38 | Gerar backup completo dos dados do escritório (somente proprietário) | Desejável | README |
| RF39 | Exportar dados do escritório (parcial ou total) | Desejável | README |

## 3. Requisitos não funcionais

| ID | Categoria | Requisito | Origem |
|---|---|---|---|
| RNF01 | Segurança | Isolamento multi-tenant em duas camadas: filtro Hibernate `@TenantId` e RLS do PostgreSQL | README |
| RNF02 | Segurança | `tenant_id` obtido somente do JWT, nunca da requisição | README |
| RNF03 | Segurança | Senhas armazenadas com hash forte (BCrypt ou Argon2); comunicação via HTTPS | Proposta |
| RNF04 | Segurança | Proteção contra tentativas repetidas de login | Proposta |
| RNF05 | Auditabilidade | Operações relevantes registradas em `registro_auditoria`, sem possibilidade de alteração | README |
| RNF06 | Manutenibilidade | Monólito modular com fronteiras verificadas por Spring Modulith | README |
| RNF07 | Manutenibilidade | Schema do banco versionado exclusivamente com Flyway | README |
| RNF08 | Testabilidade | Testes de integração e isolamento com Testcontainers (PostgreSQL real) | README |
| RNF09 | Portabilidade | Aplicação executável via Docker | README |
| RNF10 | Usabilidade | Interface intuitiva: fluxo principal executável sem treinamento | README (problema) |
| RNF11 | Desempenho | Operações de CRUD com resposta < 1 s (p95) no volume de dados de demonstração | Proposta |
| RNF12 | Compatibilidade | Funcionar nas versões atuais de Chrome, Firefox e Edge | Proposta |
| RNF13 | Conformidade | Tratamento de dados pessoais conforme a LGPD (ver [Conformidade LGPD](../10-seguranca-e-conformidade/conformidade-lgpd.md)) | Proposta |
| RNF14 | Tecnologia | Java 25 (LTS), Maven 3.10.0, Spring Boot, PostgreSQL | README |

## 4. Restrições

- Desenvolvimento individual, no prazo do TCC do curso de ADS do IFBA.
- Apenas o plano Free.
- Sem integrações externas obrigatórias (tribunais, WhatsApp, Telegram).
- Dados usados no desenvolvimento e na demonstração devem ser **fictícios**.

## 5. Premissas

- Cada usuário pertence a **um único escritório** no MVP; o e-mail de login é único na plataforma.
- Cada usuário pertence a **um grupo** (ex.: "Maria – Grupo: Gerente").
- Cada escritório tem **um único proprietário**.

## 6. Rastreabilidade

| Objetivo específico (README) | Requisitos |
|---|---|
| Centralizar clientes e processos | RF11–RF15 |
| Acompanhar processos judiciais | RF12, RF15 |
| Gerenciar atividades e delegar a usuários e equipes | RF18–RF23 |
| Controlar prazos e audiências | RF16, RF17 |
| Gerenciamento financeiro | RF25–RF35 |
| Controle granular de permissões | RF05–RF08 |
| Isolamento entre escritórios | RF09, RNF01, RNF02 |
| Auditoria | RF10, RNF05 |
| Exportação e backup | RF38, RF39 |
| Notificações | RF37 |
