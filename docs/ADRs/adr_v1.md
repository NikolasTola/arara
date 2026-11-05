# ADRs — decisões de arquitetura

> Atualizado em 04/11/2026 · SaaS de gestão para lojas de aluguel de vestidos

Registro das 15 decisões técnicas tomadas até aqui. Cada ADR traz o contexto, a decisão, as alternativas descartadas e as consequências. Uma decisão aceita só muda com um novo ADR que a substitua.

## Índice

| ADR | Decisão | Data | Status |
| --- | --- | --- | --- |
| [ADR-001](#adr-001--multi-tenant-com-banco-compartilhado-e-rls) | Multi-tenant com banco compartilhado e RLS | 24/10/2026 | Aceita |
| [ADR-002](#adr-002--typescript-no-frontend-e-no-backend) | TypeScript no frontend e no backend | 24/10/2026 | Aceita |
| [ADR-003](#adr-003--monólito-modular-em-nestjs) | Monólito modular em NestJS | 24/10/2026 | Aceita |
| [ADR-004](#adr-004--frontend-react-vite-no-cloudflare-pages) | Frontend React (Vite) no Cloudflare Pages | 24/10/2026 | Aceita |
| [ADR-005](#adr-005--postgresql-no-neon) | PostgreSQL no Neon | 24/10/2026 | Aceita |
| [ADR-006](#adr-006--backend-em-container-no-google-cloud-run) | Backend em container no Google Cloud Run | 24/10/2026 | Em revisão |
| [ADR-007](#adr-007--fotos-no-cloudflare-r2) | Fotos no Cloudflare R2 | 24/10/2026 | Aceita |
| [ADR-008](#adr-008--infraestrutura-de-custo-quase-zero-e-portável) | Infraestrutura de custo quase zero e portável | 24/10/2026 | Aceita |
| [ADR-009](#adr-009--autenticação-com-biblioteca-open-source-no-backend) | Autenticação com biblioteca open source no backend | 24/10/2026 | Aceita |
| [ADR-010](#adr-010--dois-perfis-fixos-admin-e-atendente) | Dois perfis fixos: admin e atendente | 04/11/2026 | Aceita |
| [ADR-011](#adr-011--senhas-geridas-pelo-admin-com-troca-no-primeiro-acesso) | Senhas geridas pelo admin, com troca no primeiro acesso | 04/11/2026 | Aceita |
| [ADR-012](#adr-012--módulos-globais-ligados-por-loja) | Módulos globais ligados por loja | 03/11/2026 | Aceita |
| [ADR-013](#adr-013--catálogo-em-três-níveis-tipo-item-e-peça) | Catálogo em três níveis: tipo, item e peça | 03/11/2026 | Aceita |
| [ADR-014](#adr-014--convenções-de-dados) | Convenções de dados | 03/11/2026 | Aceita |
| [ADR-015](#adr-015--operação-e-qualidade) | Operação e qualidade | 04/11/2026 | Aceita |

O ADR-006 está em revisão porque a região do backend depende de confirmar se São Paulo tem franquia gratuita no Cloud Run.

---

## Arquitetura e infraestrutura

### ADR-001 · Multi-tenant com banco compartilhado e RLS

**Contexto:** o sistema é um SaaS para várias lojas, com custo quase zero e um único desenvolvedor.

**Decisão:** um único banco PostgreSQL e um único deploy para todas as lojas. Toda tabela da loja tem `tenant_id`, e o isolamento é feito por Row-Level Security, com `app.tenant_id` definido a cada transação. As chaves estrangeiras são compostas, `(tenant_id, x_id)` → `(tenant_id, id)`, para impedir referências entre lojas. Só `tenants`, `modulos` e `perfis` são globais.

**Alternativas descartadas:**

- Um banco por loja: custo e operação multiplicados.
- Um schema por loja: cada migration roda uma vez por loja.
- Uma instância do sistema por cliente: pode voltar no futuro para um cliente grande, via infraestrutura como código.

**Consequências:**

- Uma loja nova entra sem deploy, e o custo fica baixo.
- Uma falha de isolamento é o maior risco do sistema, por isso há teste automatizado obrigatório.
- O login precisa de uma função específica que ignora o RLS, porque a loja só é conhecida depois do login.

### ADR-002 · TypeScript no frontend e no backend

**Contexto:** a linguagem foi escolhida pelo encaixe com o projeto, não pela familiaridade. O sistema é de cadastro com regras de negócio, feito por uma pessoa.

**Decisão:** TypeScript em modo estrito nos dois lados, com tipos e validações compartilhados num pacote comum.

**Alternativas descartadas:** C#/.NET (robusto, mas duas linguagens), Python (tipagem opcional e duas linguagens), Java/Kotlin (pesado para um projeto solo), Go (verboso para cadastro) e PHP/Ruby (tipagem fraca).

**Consequências:** uma linguagem só reduz retrabalho; a validação em tempo de execução continua necessária. Recursos de IA no futuro podem ser um serviço separado em Python.

### ADR-003 · Monólito modular em NestJS

**Decisão:** um único backend em NestJS, dividido em módulos (acesso, plataforma, configuração e catálogo) que só se comunicam por serviços públicos ou eventos.

**Alternativas descartadas:** microsserviços, que trariam custo e complexidade sem benefício agora; Express ou Fastify sem estrutura.

**Consequências:** deploy simples; as fronteiras entre módulos permitem extrair um serviço no futuro, se for preciso.

### ADR-004 · Frontend React (Vite) no Cloudflare Pages

**Contexto:** o sistema exige login, não precisa de SEO e é usado principalmente no celular.

**Decisão:** aplicação React feita com Vite, publicada como site estático no Cloudflare Pages.

**Alternativas descartadas:** Next.js na Vercel (o plano gratuito proíbe uso comercial) e GitHub Pages (não permite SaaS comercial).

**Consequências:** hospedagem gratuita com uso comercial permitido; sem renderização no servidor, que não é necessária.

### ADR-005 · PostgreSQL no Neon

**Decisão:** Postgres serverless no Neon, no plano gratuito (100 CU-horas e 1 GB), com processamento máximo de 0,25 CU na fase piloto. Acesso pelo Drizzle, com migrations versionadas.

**Alternativas descartadas:** Supabase (pausa projetos sem uso por 7 dias e prende a recursos próprios), Render (apaga o banco gratuito em 30 dias) e RDS (custo fixo).

**Consequências:** Postgres padrão, fácil de migrar; a primeira requisição após 5 minutos parado demora alguns segundos; é o primeiro serviço a gerar custo, entre 3 e 10 lojas.

### ADR-006 · Backend em container no Google Cloud Run

**Decisão:** a API roda num container Docker no Cloud Run, que escala a zero quando não há uso. Região atual: EUA.

**Em revisão:** a tabela de preços mostra franquia gratuita também em regiões de segunda faixa. Se São Paulo tiver franquia, backend e banco devem ir para lá.

**Alternativas descartadas:** AWS ECS ou App Runner (custo fixo mensal) e planos gratuitos que hibernam a aplicação.

**Consequências:** custo zero no volume da loja piloto; atraso na primeira requisição após período parado; o tráfego para o Brasil é cobrado (cerca de R$ 0,29 por mês); o container roda em qualquer outro provedor.

### ADR-007 · Fotos no Cloudflare R2

**Decisão:** as fotos ficam no R2, no caminho `tenants/{tenant_id}/itens/{item_id}/{id}.webp`, e o banco guarda só a chave. O envio e a exibição usam links temporários gerados pelo backend. As fotos são reduzidas no navegador (WebP, até 1.600 px) e pertencem ao item, não à peça.

**Alternativas descartadas:** Amazon S3 (cobra o tráfego de saída) e imagens guardadas no banco.

**Consequências:** saída gratuita e 10 GB sem custo; as fotos não ficam públicas na internet; a API compatível com S3 permite trocar de provedor.

### ADR-008 · Infraestrutura de custo quase zero e portável

**Decisão:** na fase piloto, só planos gratuitos que permitem uso comercial. Cada componente usa um padrão aberto (Postgres, Docker, API do S3), para trocar de provedor sem reescrever código. O único custo fixo é o domínio, de R$ 40 por ano.

**Consequências:** custo estimado de R$ 3,62 por mês; sem garantia de disponibilidade; atrasos após períodos parados são aceitos; alerta de orçamento obrigatório no Google Cloud.

---

## Acesso e segurança

### ADR-009 · Autenticação com biblioteca open source no backend

**Decisão:** a autenticação usa uma biblioteca open source rodando no próprio backend, sobre a tabela `usuarios`. O e-mail é único no sistema todo; a senha é guardada só como hash; a sessão expira após 24 horas sem acesso; 5 tentativas erradas em 15 minutos bloqueiam o login.

**Alternativas descartadas:** Cognito, Auth0 ou Clerk (custo por usuário e dependência de fornecedor) e Supabase Auth.

**Consequências:** sem custo por usuário; manter a biblioteca atualizada passa a ser responsabilidade do projeto.

### ADR-010 · Dois perfis fixos: admin e atendente

**Contexto:** as lojas têm quem gerencia e quem só atende. Permissões configuráveis por loja não são necessárias na v1.

**Decisão:** a tabela global `perfis` tem dois registros, criados por migration, identificados pelo `codigo` fixo. Cada usuário aponta para um perfil por `usuarios.perfil_id`. As permissões de cada perfil ficam no código:

| Perfil | `codigo` | O que pode fazer |
| --- | --- | --- |
| Admin | `admin` | Toda a gestão da loja: usuários, tipos, grades, categorias, etiquetas, itens, fotos, peças e importação. |
| Atendente | `atendente` | Entrar no sistema e visualizar os itens: buscar, filtrar, ver a ficha e encontrar peça pelo código. Não cadastra nem altera nada. |

**Alternativas descartadas:** permissões configuráveis por loja, complexas demais para a v1; perfil como texto livre na tabela de usuários.

**Consequências:**

- Os casos de uso e as histórias de cadastro do catálogo passam a ser só do admin.
- Toda loja precisa ter ao menos um admin ativo.
- Um perfil novo exige migration e deploy.
- O código do perfil `dona`, usado nos documentos anteriores, passa a ser `admin`.

### ADR-011 · Senhas geridas pelo admin, com troca no primeiro acesso

**Contexto:** o RF-07 impedia o usuário de alterar a própria senha, e o RF-08 exigia a troca no primeiro acesso. Os dois estavam em conflito.

**Decisão:**

1. O admin cadastra o usuário, e o sistema gera uma senha aleatória.
2. O usuário nasce com `deve_trocar_senha = true`.
3. No primeiro acesso, o sistema exige uma nova senha antes de liberar qualquer tela.
4. Fora essa troca obrigatória, o usuário não altera o próprio nome nem a própria senha.
5. Não há recuperação de senha por e-mail: quem redefine é o admin da loja ou o administrador da plataforma.

**Consequências:**

- O RF-08 passa a ser uma exceção ao RF-07, e o conflito está resolvido.
- O admin nunca conhece a senha definitiva dos usuários.
- Quem esquece a senha depende do admin para voltar a entrar.

**Pendências:**

- Definir como a senha aleatória chega ao usuário: exibida uma única vez para o admin repassar, ou enviada por e-mail.
- Confirmar que a redefinição de senha pelo admin segue o mesmo fluxo: senha aleatória e troca obrigatória no acesso seguinte.

### ADR-012 · Módulos globais ligados por loja

**Decisão:** a tabela global `modulos` tem um `codigo` fixo (`catalogo`, `reservas`, `financeiro`), nome, descrição, `ativo` e `expira_em`; a tabela `tenant_modulos` liga cada módulo a cada loja, também com `ativo` e `expira_em`. Um módulo está disponível quando as duas linhas estão ativas e nenhuma data de expiração passou. O backend confere isso a cada requisição; o frontend só esconde o menu. As linhas de `modulos` nascem por migration, junto com o código do módulo.

**Alternativas descartadas:** uma coluna por módulo na tabela de lojas e uma lista de módulos só no código.

**Consequências:** a desativação agendada funciona sem tarefa programada; um módulo pode ser desligado para todas as lojas sem deploy; a estrutura serve de base para os planos pagos do SaaS. O catálogo é o módulo base e não pode ser desligado para uma loja.

---

## Dados

### ADR-013 · Catálogo em três níveis: tipo, item e peça

**Contexto:** a loja aluga vestidos e acessórios; o mesmo modelo existe em vários tamanhos; cada tipo usa uma grade diferente (vestidos de 32 a 56, bolsas P, M e G).

**Decisão:**

- `tipos_item` guarda as regras de cada tipo: prefixo do código, se usa tamanho e o próximo número.
- `itens` é o catálogo: preço, fotos, categorias e etiquetas valem para todas as peças do item.
- `pecas` é a unidade física, com tamanho e código próprio.
- A grade de tamanhos é uma tabela por tipo, com rótulo em texto e uma coluna `ordem` para ordenar e buscar por faixa.
- Etiquetas são cadastradas pela loja e ligadas aos itens (muitos para muitos), com nome normalizado sem acento.
- O código da peça é o prefixo do tipo mais um número sequencial (V-0001), nunca reaproveitado.

**Alternativas descartadas:**

- Tamanho como número inteiro: não aceita P/M/G e exigiria a lista de tamanhos fixa no código.
- Atributos livres num campo JSON: sem validação e difíceis de filtrar.
- Um único nível "vestido": não representa várias unidades do mesmo modelo nem os acessórios.

**Consequências:** cada loja define as próprias grades sem mudar código; o tipo do item, o prefixo e o "usa tamanho" ficam travados depois da primeira peça; a reserva da v2 aponta para a peça.

### ADR-014 · Convenções de dados

**Decisão:**

- Identificadores em UUID v7, ordenáveis por data de criação.
- Tabelas e colunas em português, sem acento, em snake_case.
- Valores fixos como `text` com `CHECK`, não enum do Postgres.
- Registros com histórico são desativados, nunca apagados; as chaves estrangeiras usam `RESTRICT`.
- Preços em centavos, como número inteiro.
- `criado_em` e `atualizado_em` em todas as tabelas, com `atualizado_em` mantido por trigger.
- Toda mudança no banco por migration versionada (Drizzle); nada alterado à mão em produção.

**Consequências:** incluir um valor novo numa lista fixa é uma migration simples; o histórico fica preservado para as reservas e o financeiro; não há erro de arredondamento em valores.

---

## Operação e qualidade

### ADR-015 · Operação e qualidade

**Decisão:**

- CI e deploy automático pelo GitHub Actions: lint, verificação de tipos e testes a cada push.
- Erros do frontend e do backend enviados ao Sentry.
- Logs estruturados, sempre com `tenant_id` e o identificador da requisição.
- Backup diário do banco no R2, guardado por 30 dias, com teste de restauração uma vez por mês.
- Testes automatizados obrigatórios para as regras críticas: isolamento entre lojas, código de peça, permissões e grade de tamanhos.
- Decisões de arquitetura registradas neste documento e na pasta `docs/adr` do repositório.

**Alternativas descartadas:** depender só dos backups do plano gratuito do Neon, que têm retenção curta.

**Consequências:** todas as ferramentas cabem nos planos gratuitos na fase piloto; um problema em produção pode ser rastreado até a loja e a requisição.
