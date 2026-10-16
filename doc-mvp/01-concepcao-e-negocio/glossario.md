# Glossário (Linguagem Ubíqua)

> **Escopo:** MVP (TCC) · **Status:** Rascunho v0.1 · **Atualizado em:** 10/10/2026

Termos do domínio e seu nome no código. Pacotes, módulos e termos do domínio usam **português** (pacotes em minúsculas e sem acentos, conforme o README); eventos de domínio também usam português (ex.: `AtividadeDelegadaEvent`).

| Termo | Definição | Nome no código |
|---|---|---|
| Escritório / Tenant | Organização cliente da plataforma; unidade de isolamento de dados | `escritorio` (módulo) / `tenant_id` |
| Proprietário | Representante legal e administrador principal do escritório; possui todas as permissões funcionais e operações exclusivas | `proprietario` |
| Usuário | Pessoa com acesso ao sistema, vinculada a um escritório | `usuario` |
| Grupo | Perfil que agrega permissões padrão (Proprietário, Gerente, Advogado, Estagiário, Usuário ou personalizado) | `grupo` |
| Permissão | Autorização para uma ação, no formato `RECURSO_ACAO` (ex.: `PROCESSO_CRIAR`) | `permissao` |
| Permissão individual | Exceção por usuário: concedida ou revogada em relação ao grupo | `permissaoIndividual` |
| Equipe | Conjunto de usuários (ex.: Equipe Trabalhista) que pode receber atividades | `equipe` |
| Cliente | Pessoa física ou jurídica atendida pelo escritório | `cliente` |
| Processo | Processo judicial identificado pelo número CNJ | `processo` |
| Número CNJ | Numeração única de processos (formato `NNNNNNN-DD.AAAA.J.TR.OOOO`) | `numeroCnj` |
| Parte | Envolvido no processo (autor, réu, terceiro), cliente ou não | `parte` |
| Movimentação | Evento na linha do tempo do processo (ex.: sentença publicada) | `movimentacao` |
| Prazo | Data-limite processual com responsável e status | `prazo` |
| Audiência | Ato processual agendado com data, horário e local | `audiencia` |
| Atividade | Tarefa de trabalho do escritório, com status, prioridade e responsável | `atividade` |
| Delegação | Atribuição de uma atividade a um usuário ou equipe | `delegacao` |
| Honorário | Valor cobrado pelos serviços advocatícios (fixo, parcelado, por etapa) | `honorario` |
| Receita | Entrada financeira realizada | `receita` |
| Despesa | Saída financeira realizada | `despesa` |
| Conta a receber | Valor que o escritório tem direito a receber | `contaReceber` |
| Conta a pagar | Obrigação financeira do escritório | `contaPagar` |
| Parcela | Fração de uma conta com valor e vencimento próprios | `parcela` |
| Pagamento | Liquidação (total ou parcial) de uma conta ou parcela | `pagamento` |
| Categoria | Classificação de receitas e despesas | `categoria` |
| Centro de custo | Área onde receitas/despesas se concentram (ex.: Trabalhista) | `centroCusto` |
| Auditoria | Registro imutável das operações relevantes | `auditoria` / `registro_auditoria` |
| Backup do escritório | Cópia completa dos dados de um tenant, solicitada pelo proprietário | `portabilidade` |
| Exportação | Extração parcial ou total dos dados do escritório | `portabilidade` |
| RLS | Row-Level Security do PostgreSQL; segunda camada de isolamento | — |
