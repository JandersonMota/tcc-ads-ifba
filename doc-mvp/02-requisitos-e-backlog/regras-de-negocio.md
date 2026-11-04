# Regras de Negócio

> **Escopo:** MVP (TCC) · **Status:** Rascunho v0.1 · **Atualizado em:** 03/11/2026

`Proposta` = regra sugerida neste documento, a confirmar.

## Escritório e acesso

| ID | Regra | Requisitos |
|---|---|---|
| RN01 | Todo dado operacional pertence a exatamente um escritório (tenant). | RF09 |
| RN02 | Um usuário nunca acessa dados de outro escritório, mesmo manipulando a requisição. Recursos de outro escritório são tratados como inexistentes. | RF09 |
| RN03 | Cada escritório possui um único **Proprietário**, criado no cadastro do escritório. | RF01 |
| RN04 | O Proprietário possui todas as permissões funcionais e as operações exclusivas: gerenciar grupos e permissões, remover usuários, solicitar backup, exportar dados, alterar configurações críticas, transferir propriedade e excluir o escritório. | RF05–RF07, RF38, RF39 |
| RN05 | Conceder 100% das permissões funcionais a um usuário **não** o torna proprietário. | RF06 |
| RN06 | Permissões efetivas = permissões do grupo + permissões concedidas individualmente − permissões revogadas individualmente. | RF06, RF07 |
| RN07 | Os grupos padrão não podem ser excluídos; grupos personalizados podem, se não tiverem usuários. *(Proposta)* | RF05 |
| RN08 | Usuário desativado não consegue autenticar; seus registros permanecem. *(Proposta)* | RF04 |
| RN09 | No MVP, todo escritório está no plano **Free**, limitado a **2 usuários ativos**, **50 clientes ativos** e **50 processos ativos** (arquivados não contam). Os limites vêm de configuração. Ao atingir um limite, apenas a criação é bloqueada; leitura, edição, arquivamento e exportação continuam permitidos. | RF01, RF04, RF11, RF12, RF39 |

## Jurídico

| ID | Regra | Requisitos |
|---|---|---|
| RN10 | Cliente é Pessoa Física (CPF) ou Pessoa Jurídica (CNPJ); o documento é válido e único dentro do escritório. *(Proposta)* | RF11 |
| RN11 | O número CNJ segue o formato `NNNNNNN-DD.AAAA.J.TR.OOOO`, tem dígito verificador válido e é único dentro do escritório. *(Proposta)* | RF12 |
| RN12 | Um processo pode ter múltiplos clientes, partes e advogados responsáveis (não se limita a Cliente → Processo). | RF13, RF14 |
| RN13 | Cliente vinculado a processo não pode ser excluído enquanto o vínculo existir. *(Proposta)* | RF11 |
| RN14 | No MVP, movimentações são inseridas manualmente; a integração com tribunais não faz parte do MVP. | RF15 |

## Operacional

| ID | Regra | Requisitos |
|---|---|---|
| RN15 | Fluxo de status da atividade: A FAZER → EM ANDAMENTO → EM REVISÃO → CONCLUÍDA; BLOQUEADA e CANCELADA podem ocorrer a partir de qualquer status não final; atividade concluída pode ser reaberta (volta para A FAZER). | RF20 |
| RN16 | Delegar exige a permissão `ATIVIDADE_DELEGAR`; o responsável deve ser usuário ativo do mesmo escritório. | RF19 |
| RN17 | Atividade delegada gera notificação ao responsável (quando RF37 for implementado). | RF19, RF37 |

## Financeiro

| ID | Regra | Requisitos |
|---|---|---|
| RN18 | Status de conta/parcela: ABERTA, PAGA, ATRASADA, CANCELADA. Fica ATRASADA quando o vencimento passou e não está paga. | RF28, RF29 |
| RN19 | Registrar pagamento de conta a receber gera uma **receita**; de conta a pagar gera uma **despesa**. *(Proposta)* | RF26, RF27, RF30 |
| RN20 | A soma dos pagamentos não pode ultrapassar o valor da conta; ao atingir o valor, a conta passa a PAGA. *(Proposta)* | RF30 |
| RN21 | Conta PAGA ou CANCELADA não pode ser editada nem receber pagamento. *(Proposta)* | RF28–RF30 |
| RN22 | Valores monetários em reais, com duas casas decimais, nunca em ponto flutuante. *(Proposta)* | RF25–RF30 |

## Dados e auditoria

| ID | Regra | Requisitos |
|---|---|---|
| RN23 | Registros de auditoria são somente inserção: não podem ser alterados nem excluídos. | RF10 |
| RN24 | A auditoria registra usuário, data/hora, ação, entidade, identificador e valores anterior/posterior. | RF10 |
| RN25 | Backup é **manual**, solicitado somente pelo proprietário, e contém todos os dados do escritório; não há backup automático no computador do proprietário. | RF38 |
| RN26 | Exportação é independente do backup e pode ser parcial (clientes, processos, atividades, financeiro, documentos, usuários) ou total, em pacote ZIP. | RF39 |
