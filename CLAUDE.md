# CLAUDE.md

## Projeto
Sistema de Gestão Jurídica SaaS multi-tenant para escritórios de advocacia (MVP de TCC em Análise e Desenvolvimento de Sistemas). Visão, escopo e decisões arquiteturais estão no `README.md`.

## Ferramentas
- **Linguagem:** Java 25 (LTS)
- **Build:** Maven 3.10.0
- **IDE:** IntelliJ IDEA
- **Framework:** Spring Boot (versão a definir), Spring Data JPA / Hibernate, Spring Modulith, Spring Security + JWT
- **Banco de Dados:** PostgreSQL, com migrações via Flyway
- **Testes:** JUnit, Testcontainers, Spring Modulith Test / ArchUnit
- **Containers:** Docker
- **Frontend:** a definir

## Regras de Arquitetura
- Monólito modular + DDD + Hexagonal (Ports and Adapters).
- **Multi-tenancy:** schema compartilhado com coluna `tenant_id` em toda tabela de negócio; filtro na aplicação (Hibernate `@TenantId` / `@Filter`) e Row-Level Security no PostgreSQL.
- O `tenant_id` vem **sempre do token JWT**, nunca de parâmetro, header ou corpo da requisição.
- Cada módulo é dono das suas tabelas: sem JOIN ou acesso direto a tabelas de outro módulo.
- Um módulo só acessa outro pela API pública; nunca pelo `dominio` ou `adaptador` interno.
- Efeitos colaterais entre módulos (notificação, auditoria) via eventos de domínio.
- Schema do banco criado apenas por migrações Flyway (incluindo políticas de RLS), nunca pelo Hibernate.
- DDD tático completo em `autorizacao`, `escritorio`, `atividade`, `processo`, `financeiro`; estrutura simplificada em `cliente`, categorias e `portabilidade`.
- Pacotes e módulos (backend e frontend) em **português**, minúsculas e sem acentos (ex.: `processo`, `notificacao`, `adaptador.entrada.web`).

## Regras do Sistema de Design
Essas regras previnem as inconsistências visuais encontradas e corrigidas na auditoria de UI/UX. Siga-as rigorosamente ao escrever ou editar qualquer componente.
