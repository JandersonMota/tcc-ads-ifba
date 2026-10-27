# CLAUDE.md

## Projeto
Sistema de Gestão Jurídica SaaS multi-tenant para escritórios de advocacia (MVP de TCC em Análise e Desenvolvimento de Sistemas). Visão, escopo e decisões arquiteturais estão no `README.md`.

## Ferramentas
- **Linguagem:** Java 25 (LTS)
- **Build:** Maven 3.10.0
- **IDE:** IntelliJ IDEA
- **Framework:** Spring Boot 4.1.2, Spring Data JPA / Hibernate 7.4, Spring Modulith 2.1.1, Spring Security 7.1 + JWT (OAuth2 Resource Server), SpringDoc OpenAPI 3.2.0, Spring Boot Actuator
- **Banco de Dados:** PostgreSQL, com migrações via Flyway
- **Testes:** JUnit 6, Testcontainers 2, Spring Modulith Test / ArchUnit
- **Código do backend:** repositório `tcc-backend-ads-ifba`, pacote raiz `br.com.leija`, classe principal `TccBackendAdsIfbaApplication`
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
