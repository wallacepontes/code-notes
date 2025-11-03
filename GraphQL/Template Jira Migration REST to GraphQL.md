# Template Jira Migration REST to GraphQL

## Table of Contents
- [Template Jira Migration REST to GraphQL](#template-jira-migration-rest-to-graphql)
  - [Table of Contents](#table-of-contents)
  - [Migração de Endpoints REST para GraphQL](#migração-de-endpoints-rest-para-graphql)
    - [Estrutura Sugerida para Epics, Stories, Tasks e Subtasks no Jira para Migração de Endpoints REST para GraphQL (Apollo/Nexus/Apigee/AWS)](#estrutura-sugerida-para-epics-stories-tasks-e-subtasks-no-jira-para-migração-de-endpoints-rest-para-graphql-apollonexusapigeeaws)
      - [1. **Epic de Alto Nível: "Migração de APIs para GraphQL Federated Gateway"**](#1-epic-de-alto-nível-migração-de-apis-para-graphql-federated-gateway)
      - [2. **Por Que Focar no Novo Sistema?**](#2-por-que-focar-no-novo-sistema)
      - [3. **Dicas para Implementar no Jira**](#3-dicas-para-implementar-no-jira)
  - [Template Jira](#template-jira)
    - [Template Jira para o Projeto de Migração de Endpoints REST para GraphQL (POC e Full Migration)](#template-jira-para-o-projeto-de-migração-de-endpoints-rest-para-graphql-poc-e-full-migration)
      - [**Epic 1: Prova de Conceito (POC) - Migração de 5 Endpoints para GraphQL**](#epic-1-prova-de-conceito-poc---migração-de-5-endpoints-para-graphql)
      - [**Epic 2: Design e Implementação do Schema GraphQL Completo**](#epic-2-design-e-implementação-do-schema-graphql-completo)
      - [**Epic 3: Migração Incremental e Integração com Clientes**](#epic-3-migração-incremental-e-integração-com-clientes)
      - [**Epic 4: Testes, Monitoramento e Cutover**](#epic-4-testes-monitoramento-e-cutover)

## Migração de Endpoints REST para GraphQL

### Estrutura Sugerida para Epics, Stories, Tasks e Subtasks no Jira para Migração de Endpoints REST para GraphQL (Apollo/Nexus/Apigee/AWS)

Migrar endpoints REST do sistema antigo (WOA/Java8, com múltiplas URIs como 322) para um GraphQL unificado (com um único endpoint de gateway) exige uma abordagem focada no **novo sistema**, não no legado. Criar cards baseados em cada URI antiga criaria redundância e complexidade desnecessária, ignorando os benefícios do GraphQL (como schema unificado, resolvers modulares e federation). Em vez disso, estruture o backlog no Jira priorizando **features do novo schema**, **integrações microservices** e **migração incremental**, usando a hierarquia padrão: **Epic > Story > Task > Subtask**. Isso alinha com best practices ágeis (ex.: quebrar epics em stories entregáveis em sprints, medir progresso por completion de stories).

Baseado em práticas de migração REST-to-GraphQL (como incremental federation com Apollo), aqui vai uma proposta de estrutura. Use **labels** (ex.: "graphql-migration", "federation") e **components** (ex.: "Schema", "Gateway") para filtrar boards. Crie um board dedicado à migração, com swimlanes por epic, e configure Advanced Roadmaps para visão de dependências.

#### 1. **Epic de Alto Nível: "Migração de APIs para GraphQL Federated Gateway"**
   - **Descrição Geral do Epic**: Corpo de trabalho para migrar ~322 endpoints REST para um schema GraphQL unificado, usando Apollo Server/Nexus para resolvers, Federation para microservices, Apigee para proxy/gateway e AWS (ex.: Lambda/ECS) para hosting. Foco: schema-first design, migração incremental (manter REST legados como fallback). Métricas: Cobertura de schema (80% endpoints mapeados), performance (redução de requests em 70%).
   - **Duração Estimada**: 3-6 meses (múltiplos sprints).
   - **Stories Filhas** (exemplos de 5-8 stories por epic, cada uma sprint-sized):
     - **Story 1: Design e Implementação do Schema GraphQL Inicial**
       - **Aceitação**: Schema Nexus define tipos/queries/mutations para 20% dos endpoints (ex.: queries para "consultarProdutoInstalado" e "listarServicos"). Validação via GraphQL Playground.
       - **Tasks**:
         - Analisar 50 endpoints REST prioritários (ex.: WOA v1) e mapear para tipos GraphQL (ex.: `type Produto { id: ID, numeroTelefone: String }`).
         - Implementar resolvers iniciais com Nexus/Apollo.
       - **Subtasks**: Documentar schema em Markdown/Confluence; Testar queries básicas.
     - **Story 2: Setup da Infraestrutura AWS e Apigee Gateway**
       - **Aceitação**: Deploy de Apollo Gateway em AWS ECS/Lambda, proxy via Apigee com auth (OAuth/JWT). Endpoint único: `/graphql` responde com schema federado.
       - **Tasks**:
         - Configurar cluster ECS com Apollo Router (para federation).
         - Integrar Apigee para rate limiting e analytics.
       - **Subtasks**: Script de deploy via CDK/Terraform; Testar latency <200ms.
     - **Story 3: Migração Incremental de Endpoints para Subgraphs (Microservices)**
       - **Aceitação**: 30% dos endpoints (ex.: produto/v1 e aparelho/v1) migrados para 2-3 subgraphs federados (ex.: um para "Produto", outro para "Aparelho"). Clientes REST redirecionam via proxy.
       - **Tasks**:
         - Criar subgraph 1: Resolver para "verificarElegibilidade" (agregando dados de múltiplos REST).
         - Habilitar federation com `@key` directives em tipos compartilhados (ex.: `Produto`).
       - **Subtasks**: Migrar validações (ex.: numeroTelefoneType para input GraphQL); Refatorar adapters Java para Node.js resolvers.
     - **Story 4: Integração com Clientes e Migração de Queries**
       - **Aceitação**: Clientes (ex.: apps mobile/web) usam Apollo Client para queries GraphQL, com fallback para REST. Cobertura de testes: 90% queries/mutations.
       - **Tasks**:
         - Gerar queries GraphQL equivalentes a endpoints antigos (ex.: uma query `consultarConsumoEListarServicos` que resolve múltiplos dados).
         - Atualizar docs Swagger para GraphQL schema.
       - **Subtasks**: Implementar caching com Apollo (Redis em AWS); Migrar 50 URIs para schema unificado.
     - **Story 5: Testes, Monitoramento e Rollout**
       - **Aceitação**: Schema validado com load tests (ex.: 10k QPS via Artillery); Monitoramento via Apigee/AWS CloudWatch. Rollout canary para 50% tráfego.
       - **Tasks**:
         - Configurar schema registry e linting (GraphQL Code Generator).
         - Migrar restantes 50% endpoints, desabilitando REST obsoletos.
       - **Subtasks**: Escrita de schema tests; Setup alerts para erros de resolver.

#### 2. **Por Que Focar no Novo Sistema?**
   - **Evite Cards por URI Antiga**: Em vez de 322 stories (uma por endpoint), agrupe por **domínio funcional** (ex.: "Produto Instalado" como subgraph). Isso reflete o GraphQL: um schema unificado onde queries agregam dados (ex.: resolver que chama múltiplos microservices via federation). Cards legados (WOA/Java8) podem ser migrados para "Spike: Análise de Endpoint Legado" como subtasks iniciais.
   - **Benefícios**:
     - **Incremental**: Migre por batches (ex.: 20% por sprint), mantendo REST via proxy Apigee até cutover.
     - **Medição Ágil**: Use story points (Fibonacci) baseados em complexidade de resolver (não URIs). Track progresso via burndown de epic (completion % de stories).
     - **Colaboração**: Atribua stories a squads (ex.: backend para schema, DevOps para AWS/Apigee). Use links para dependências (ex.: story de schema linka a task de deploy).
   - **Hierarquia Ideal no Jira** (baseado em best practices):
     | Nível       | Propósito                          | Exemplo de Tamanho | Dicas de Configuração |
     |-------------|------------------------------------|--------------------|-----------------------|
     | **Epic**   | Grandes iniciativas (ex.: migração completa). | 3-6 meses         | Link stories; Use roadmap view para timeline. |
     | **Story**  | Entregas verticais (ex.: subgraph pronto). | 1 sprint (2 semanas) | Critérios de aceitação com Gherkin; Labels por fase (design/impl/test). |
     | **Task**   | Ações técnicas (ex.: implementar resolver). | 1-3 dias          | Assignee por skill (ex.: Nexus para schema). |
     | **Subtask**| Detalhes granulares (ex.: testar query específica). | <1 dia            | Checklist para code review/deploy. |

#### 3. **Dicas para Implementar no Jira**
   - **Board Setup**: Crie um Scrum board com colunas: Backlog > To Do > In Progress > Review > Done. Swimlanes por epic. Use Quick Filters: "is Epic" para visão high-level.
   - **Migração de Cards Existentes**: Audite cards WOA antigos como "Spike" tasks em uma story inicial ("Análise Legado"). Não duplique; linke-os como "blocks" para stories novas.
   - **Ferramentas Integradas**: Use Confluence para schema docs; Integre GitHub Actions para CI/CD (deploy schema changes). Para Apollo/Nexus, adicione webhooks no Jira para auto-atualizar issues em PRs.
   - **Riscos e Métricas**: Adicione tasks para "POCs" (ex.: federate 5 endpoints). Monitore com Jira reports: Epic progress burndown, velocity por story.
   - **Próximos Passos**: Comece com um spike epic ("Prova de Conceito: Migração de 10 Endpoints") para validar o schema unificado.

## Template Jira
### Template Jira para o Projeto de Migração de Endpoints REST para GraphQL (POC e Full Migration)

Abaixo, apresento um **template completo e pronto para uso no Jira** para o projeto de migração dos endpoints mencionados (ex.: `verificarElegibilidadeProdutoInstalado`, `listarServicos`, `consultarProdutoInstalado`, `consultarConsumoProdutoInstalado`, `definirGestorConsumo`). O template foca no **novo sistema GraphQL** (Apollo/Nexus para schema/resolvers, Apigee para gateway/proxy, AWS para hosting/federation), usando os endpoints como base para a **POC (Prova de Conceito)** para validar a infra (ex.: schema unificado, federation de 5 endpoints selecionados).

Estrutura hierárquica:
- **Epic**: Grandes fases (ex.: POC, Design, Implementação, Testes/Cutover).
- **Story**: Entregas incrementais (sized para 1 sprint/2 semanas, com critérios de aceitação em Gherkin).
- **Task**: Ações técnicas (1-3 dias).
- **Subtask**: Detalhes granulares (<1 dia, com checklists).

**Instruções para Implementar no Jira**:
1. Crie um **projeto Scrum** dedicado (ex.: "GRAPHQL-MIGRATION").
2. Use **Issue Templates for Jira** (addon sugerido nos resultados de busca) para pré-popular epics com stories/tasks. Copie os templates abaixo diretamente no campo "Description" ao criar issues.
3. **Labels**: `graphql-poc`, `federation`, `apigee`, `aws`.
4. **Components**: `Schema`, `Gateway`, `Resolvers`, `Testing`.
5. **Estimativa**: Use story points (Fibonacci: 1-13) e tempo (dias).
6. **Dependências**: Linke stories via "blocks" (ex.: POC blocks Design).
7. **Métricas**: Track via Epic Report (progresso %); Velocity por sprint.
8. **Roadmap**: Use Advanced Roadmaps para timeline (POC: Sprint 1-2; Cutover: Sprint 8-10).
9. **POC Foco**: Selecione 5 endpoints (ex.: os listados) para migrar em subgraphs iniciais, validando unificação em um endpoint `/graphql`.

#### **Epic 1: Prova de Conceito (POC) - Migração de 5 Endpoints para GraphQL**
   - **Summary**: Validar infraestrutura GraphQL federada com migração de 5 endpoints REST (verificarElegibilidade, listarServicos, consultarProdutoInstalado, consultarConsumoProdutoInstalado, definirGestorConsumo) para schema unificado.
   - **Description**: POC para testar Apollo/Nexus resolvers, Apigee proxy e AWS hosting. Sucesso: Queries agregadas respondem <200ms; Cobertura 100% dos 5 endpoints. Duração: 2 sprints. Risco: Integração com legacy adapters.
   - **Priority**: Highest | **Epic Color**: Red | **Story Points Total**: 40 | **Assignee**: Tech Lead.
   - **Acceptance Criteria**:
     - Schema Nexus define tipos para os 5 endpoints (ex.: `type Query { verificarElegibilidade(numeroTelefone: String!): Elegibilidade }`).
     - Federation: Subgraphs para "Produto" e "Internet" resolvem dados agregados.
     - Deploy em AWS ECS; Proxy via Apigee com auth.
     - Testes: 90% coverage; Load test 1k QPS.
   - **Stories Filhas** (Copie para Jira como linked issues):

     **Story 1.1: Análise e Mapeamento dos 5 Endpoints para Schema GraphQL**
       - **Summary**: Mapear lógica REST (ex.: adapters EAS.Produto/A parelho) para resolvers Nexus.
       - **Description**: Analisar validações (ex.: MSISDN vs. NumeroTelefoneType) e mapear para inputs GraphQL. Output: Schema draft em Markdown.
       - **Priority**: High | **Story Points**: 8 | **Sprint**: 1.
       - **Acceptance Criteria**: Given endpoints REST, when mapeados, then schema cobre 100% fields (ex.: `elegivel: Boolean, motivo: String`).
       - **Tasks**:
         - Task 1.1.1: Analisar endpoint `verificarElegibilidade` (Java adapters para resolvers Node.js). Est: 3 dias.
           - Subtask: Documentar inputs/outputs em Confluence.
         - Task 1.1.2: Mapear `listarServicos` e `consultarProdutoInstalado` para queries agregadas. Est: 2 dias.
           - Subtask: Gerar GraphQL schema com Nexus codegen.
         - Task 1.1.3: Incluir `consultarConsumo` e `definirGestor` em subgraph "Internet". Est: 3 dias.
           - Subtask: Checklist: Validações CNPJ/MSISDN migradas?

     **Story 1.2: Setup Inicial de Infra AWS e Apigee para POC**
       - **Summary**: Configurar Apollo Gateway em AWS com proxy Apigee.
       - **Description**: Deploy endpoint único `/graphql`; Testar federation básica.
       - **Priority**: High | **Story Points**: 13 | **Sprint**: 1.
       - **Acceptance Criteria**: Given deploy, when query executada, then responde schema dos 5 endpoints sem erros.
       - **Tasks**:
         - Task 1.2.1: Provisionar ECS/Lambda em AWS para Apollo Server. Est: 4 dias.
           - Subtask: Script CDK para infra as-code.
         - Task 1.2.2: Configurar Apigee proxy com OAuth e rate limiting. Est: 3 dias.
           - Subtask: Testar auth com Postman.
         - Task 1.2.3: Integrar Nexus para schema stitching. Est: 2 dias.
           - Subtask: Lint schema com GraphQL ESLint.

     **Story 1.3: Implementação de Resolvers para os 5 Endpoints na POC**
       - **Summary**: Desenvolver resolvers Apollo para queries/mutations dos endpoints.
       - **Description**: Migrar lógica Java (ex.: validações Business) para Node.js resolvers; Usar federation @key para Produto/Aparelho.
       - **Priority**: High | **Story Points**: 13 | **Sprint**: 2.
       - **Acceptance Criteria**: Given query GraphQL, when resolvida, then retorna dados equivalentes a REST (ex.: elegibilidade com motivos).
       - **Tasks**:
         - Task 1.3.1: Resolver para `verificarElegibilidade` (agregando EAS adapters via HTTP Data Sources). Est: 3 dias.
           - Subtask: Mock legacy REST calls.
         - Task 1.3.2: Resolver para `listarServicos` e `consultarProdutoInstalado` (agregação de saldos/plano). Est: 4 dias.
           - Subtask: Handle erros (ex.: ProdutoInvalidoBusinessException -> GraphQL errors).
         - Task 1.3.3: Resolver para `consultarConsumo` e `definirGestor` (com transacaoId). Est: 3 dias.
           - Subtask: Test unitário com Jest.

     **Story 1.4: Testes e Validação da POC**
       - **Summary**: Executar testes end-to-end para validar migração dos 5 endpoints.
       - **Description**: Cobertura funcional/performance; Comparar outputs REST vs. GraphQL.
       - **Priority**: Medium | **Story Points**: 5 | **Sprint**: 2.
       - **Acceptance Criteria**: Given load test, when 1k QPS, then latency <200ms; 100% match com REST.
       - **Tasks**:
         - Task 1.4.1: Escrever schema tests com GraphQL Testing. Est: 2 dias.
           - Subtask: Testar queries agregadas (ex.: consultarProduto + consumo em uma query).
         - Task 1.4.2: Load test com Artillery; Monitorar via CloudWatch. Est: 1 dia.
           - Subtask: Relatório de discrepâncias.

#### **Epic 2: Design e Implementação do Schema GraphQL Completo**
   - **Summary**: Expandir schema para todos ~322 endpoints, agrupados em subgraphs por domínio (ex.: Produto, Aparelho, Internet).
   - **Description**: Schema-first: Definir tipos, queries/mutations; Federation para 10 subgraphs. Duração: 3 sprints. Dependência: POC aprovada.
   - **Priority**: High | **Epic Color**: Orange | **Story Points Total**: 80.
   - **Acceptance Criteria**: Schema cobre 100% endpoints; Resolvers modulares com caching.
   - **Stories Filhas** (Exemplos; Expanda para batches de 50 endpoints):
     - **Story 2.1: Design de Schema para Domínio Produto (100 endpoints)**: Mapear adapters EAS para resolvers. Points: 20.
       - Tasks: Analisar Business layers; Gerar schema com Nexus.
     - **Story 2.2: Federation e Subgraphs para Aparelho/Internet**: Configurar @key/@provides. Points: 21.
       - Tasks: Migrar validações MSISDN/IMEI.
     - **Story 2.3: Integração com Legacy via Proxy Apigee**: Fallback REST. Points: 20.
       - Tasks: Rules para redirecionamento.
     - **Story 2.4: Otimização de Resolvers (Batch Loading)**: Evitar N+1. Points: 19.

#### **Epic 3: Migração Incremental e Integração com Clientes**
   - **Summary**: Migrar endpoints em waves (ex.: 50 por sprint); Atualizar clientes para Apollo Client.
   - **Description**: Rollout por feature flag; Docs GraphQL. Duração: 4 sprints.
   - **Priority**: Medium | **Epic Color**: Yellow | **Story Points Total**: 100.
   - **Acceptance Criteria**: 80% tráfego em GraphQL; Clientes mobile/web usam queries unificadas.
   - **Stories Filhas**:
     - **Story 3.1: Migração Wave 1 (50 Endpoints - Produto)**: Deploy subgraphs. Points: 25.
       - Tasks: Refatorar Java para Node resolvers.
     - **Story 3.2: Atualização de Clientes (Apps/Mobile)**: Apollo Client integration. Points: 25.
       - Tasks: Gerar queries codegen.
     - **Story 3.3: Migração Wave 2-6 (Restante Endpoints)**: Batch por domínio. Points: 50.

#### **Epic 4: Testes, Monitoramento e Cutover**
   - **Summary**: Testes completos, monitoramento e switch-off de REST.
   - **Description**: E2E, security; Canary deploy. Duração: 2 sprints.
   - **Priority**: Highest | **Epic Color**: Green | **Story Points Total**: 50.
   - **Acceptance Criteria**: Zero downtime; 99.9% uptime; Desabilitar 100% URIs legadas.
   - **Stories Filhas**:
     - **Story 4.1: Testes E2E e Security (OWASP)**: GraphQL-specific (ex.: introspection). Points: 15.
       - Tasks: Schema validation; Penetration tests.
     - **Story 4.2: Monitoramento com Apigee/CloudWatch**: Alerts para erros resolvers. Points: 10.
       - Tasks: Dashboards para QPS/latency.
     - **Story 4.3: Cutover e Rollback Plan**: Switch tráfego; Pós-mortem. Points: 25.
       - Tasks: Blue-green deploy; Decom REST APIs.

**Template Geral para Stories/Tasks (Copie para Description)**:
```
**Summary**: [Breve descrição]
**Description**: [Detalhes, dependências]
**Acceptance Criteria**:
* Given [contexto], when [ação], then [resultado].
**Definition of Done**:
- Code reviewed.
- Tests pass (90% coverage).
- Deployed to staging.
**Attachments**: [Schema draft, Wireframes]
```
