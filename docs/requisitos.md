# Requisitos — (login e catálogo)

> Atualizado em 04/11/2026 · SaaS de gestão para lojas de aluguel de vestidos
> Versão do documento: 1.0.0

A v1 entrega login e catálogo para a loja piloto. Os requisitos de reservas, financeiro e evolução do SaaS ficam no documento **Requisitos — próximas versões**.

**Prioridade:** **Essencial** = sem isso a v1 não vai ao ar · **Importante** = entra na v1 se couber · **Desejável** = pode ficar para depois.

## Sumário

1. [Requisitos funcionais](#1-requisitos-funcionais)
2. [Requisitos não funcionais](#2-requisitos-não-funcionais)
3. [Restrições](#3-restrições)
4. [Pendências](#4-pendências)

---

## 1. Requisitos funcionais

A v1 tem 38 requisitos funcionais: 28 essenciais, 7 importantes e 3 desejáveis.

### Acesso e usuários

O administrador da loja é o usuário cujo perfil tem permissão para gerenciar os outros usuários.

| ID | Requisito | Prioridade |
| --- | --- | --- |
| RF-01 | Entrar no sistema com e-mail e senha. | Essencial |
| RF-02 | Sair do sistema, encerrando a sessão. | Essencial |
| RF-03 | Liberar telas e ações conforme o perfil do usuário. | Essencial |
| RF-04 | O administrador da loja cadastra usuários com nome, e-mail, perfil e senha inicial. | Importante |
| RF-05 | O administrador da loja altera o nome e o perfil dos usuários e os desativa; a desativação encerra as sessões abertas. | Importante |
| RF-06 | O administrador da loja redefine a senha dos usuários que gerencia. | Importante |
| RF-07 | O usuário não altera o próprio nome nem a própria senha; essas mudanças são feitas pelo administrador. | Essencial |
| RF-08 | Exigir a troca de senha no primeiro acesso de contas criadas com senha provisória. *(em decisão — ver Pendências)* | Essencial |
| RF-09 | Confirmar o e-mail do usuário por um link. | Desejável |

### Administração da plataforma

Feita pelo dono do SaaS, não pelas lojas.

| ID | Requisito | Prioridade |
| --- | --- | --- |
| RF-10 | Criar uma loja numa única operação: tenant, primeiro usuário administrador e módulo catálogo ligado. | Essencial |
| RF-11 | Redefinir a senha de qualquer usuário de uma loja, inclusive do administrador da loja. | Essencial |
| RF-12 | Suspender e reativar uma loja. | Importante |
| RF-13 | Ligar e desligar módulos por loja. | Importante |
| RF-14 | Agendar a desativação de um módulo para uma data e hora, em uma loja ou em todas. | Importante |
| RF-15 | Desligar um módulo para todas as lojas de uma vez. | Desejável |

### Configuração da loja

| ID | Requisito | Prioridade |
| --- | --- | --- |
| RF-16 | Cadastrar, editar e desativar tipos de item: nome, prefixo do código e se usa tamanho. | Essencial |
| RF-17 | Definir a grade de tamanhos de cada tipo de item, numérica ou em letras (ex.: vestidos de 32 a 56, bolsas P, M e G): incluir, reordenar e desativar tamanhos. | Essencial |
| RF-18 | Impedir a mudança de prefixo e de "usa tamanho" em um tipo que já tem peças. | Essencial |
| RF-19 | Cadastrar, editar e desativar categorias. | Essencial |

### Etiquetas

Etiquetas são atributos adicionados aos itens para facilitar a busca e a filtragem, como "Longo", "Renda" ou "Azul". Um item pode ter várias etiquetas.

| ID | Requisito | Prioridade |
| --- | --- | --- |
| RF-20 | Cadastrar, editar e excluir etiquetas numa tela própria, sem duplicar nomes por diferença de acento ou maiúscula. Etiquetas em uso não podem ser excluídas. | Essencial |
| RF-21 | Relacionar uma ou mais etiquetas a um item ao cadastrá-lo ou editá-lo, e remover essa relação. | Essencial |

### Catálogo

| ID | Requisito | Prioridade |
| --- | --- | --- |
| RF-22 | Cadastrar item com tipo, nome, cor, marca, descrição, preço e categorias. | Essencial |
| RF-23 | Editar item, impedindo a troca de tipo quando já houver peças. | Essencial |
| RF-24 | Desativar e reativar item. | Essencial |
| RF-25 | Enviar várias fotos por item, com compressão automática e escolha da capa e da ordem. | Essencial |
| RF-26 | Excluir foto do item, removendo também o arquivo. | Essencial |
| RF-27 | Cadastrar peças de um item com um tamanho da grade do seu tipo; o código (ex.: V-0001) é gerado automaticamente. | Essencial |
| RF-28 | Cadastrar várias peças de um item de uma vez, uma para cada tamanho informado. | Importante |
| RF-29 | Editar peça: tamanho, situação, anotações de ajuste e observações. | Essencial |
| RF-30 | Desativar e reativar peça, mantendo o histórico. | Essencial |
| RF-31 | Ver a ficha do item com fotos, dados, etiquetas e peças por tamanho e situação. | Essencial |

### Importação por planilha

| ID | Requisito | Prioridade |
| --- | --- | --- |
| RF-32 | Importar itens e peças de uma planilha de uma só vez, a partir de um modelo de planilha baixado do sistema. As fotos são adicionadas depois, no sistema. | Essencial |
| RF-33 | Validar a planilha antes de gravar e mostrar os erros por linha; nada é gravado enquanto houver erro. | Essencial |

### Busca

| ID | Requisito | Prioridade |
| --- | --- | --- |
| RF-34 | Buscar itens por nome, cor, marca e etiqueta, sem diferenciar acentos e maiúsculas. | Essencial |
| RF-35 | Filtrar por tipo, categoria, uma ou mais etiquetas, situação da peça e tamanho (exato ou faixa, como "do 38 ao 44"). | Essencial |
| RF-36 | Encontrar uma peça pelo código. | Essencial |
| RF-37 | Listar itens com foto de capa, paginação e ordenação por nome ou data de cadastro. | Essencial |

### Compartilhamento

| ID | Requisito | Prioridade |
| --- | --- | --- |
| RF-38 | Enviar as fotos de um item pelo WhatsApp a partir do celular. | Desejável |

---

## 2. Requisitos não funcionais

Os valores numéricos são metas iniciais; ajuste conforme o uso real da loja piloto.

### Segurança

| ID | Requisito |
| --- | --- |
| RNF-01 | Uma loja nunca acessa dados de outra. O isolamento é garantido pelo banco (RLS e chaves estrangeiras com `tenant_id`) e coberto por teste automatizado em todas as rotas. |
| RNF-02 | Senhas guardadas só de forma criptografada (hash), nunca em texto puro; não aparecem em logs nem em respostas da API. |
| RNF-03 | Todo tráfego usa HTTPS. |
| RNF-04 | O login bloqueia tentativas repetidas: no máximo 5 erros em 15 minutos por e-mail e por IP. |
| RNF-05 | A sessão expira após 24 horas sem acesso. |
| RNF-06 | Permissões e módulos são verificados no backend em toda requisição; o frontend apenas esconde o que não está liberado. |
| RNF-07 | As fotos só podem ser vistas por usuários logados da loja; não ficam acessíveis por um endereço público na internet. |

### Privacidade (LGPD)

| ID | Requisito |
| --- | --- |
| RNF-08 | O sistema coleta só os dados pessoais necessários ao funcionamento. |
| RNF-09 | Os dados de uma loja podem ser exportados e excluídos ao fim do contrato. |

### Desempenho

| ID | Requisito |
| --- | --- |
| RNF-10 | A busca no catálogo responde em até 1 segundo em 95% das requisições, para lojas com até 5.000 peças. |
| RNF-11 | A primeira requisição após um período parado (servidor e banco "acordando") pode levar até 10 segundos. |
| RNF-12 | As fotos são reduzidas no navegador antes do envio: WebP, até 1.600 px no maior lado. |

### Usabilidade

| ID | Requisito |
| --- | --- |
| RNF-13 | Todas as telas funcionam no celular a partir de 360 px de largura; o celular é o aparelho principal. |
| RNF-14 | Cadastrar um item com uma peça e uma foto leva até 1 minuto. |
| RNF-15 | Interface em português do Brasil, com valores em R$, datas em dd/mm/aaaa e fuso de São Paulo. |
| RNF-16 | Contraste de cores e navegação por teclado no nível AA da WCAG. |

### Disponibilidade e dados

| ID | Requisito |
| --- | --- |
| RNF-17 | Backup diário do banco, guardado por 30 dias em armazenamento separado. |
| RNF-18 | A restauração do backup é testada uma vez por mês. |
| RNF-19 | Meta de disponibilidade de 99% ao mês na fase piloto, sem SLA contratual. |

### Manutenção e evolução

| ID | Requisito |
| --- | --- |
| RNF-20 | TypeScript em modo estrito no frontend e no backend, com tipos e validações compartilhados entre os dois. |
| RNF-21 | Monólito modular: cada módulo tem fronteiras próprias e só se comunica com outro por serviços públicos ou eventos. |
| RNF-22 | Toda mudança no banco é feita por migration versionada; nada é alterado à mão em produção. |
| RNF-23 | Regras de negócio críticas têm testes automatizados, executados a cada push pelo CI. |
| RNF-24 | Decisões de arquitetura são registradas em ADRs no repositório. |
| RNF-25 | Uma loja nova entra no sistema sem mudança de código nem deploy. |

### Observabilidade e custo

| ID | Requisito |
| --- | --- |
| RNF-26 | Erros do frontend e do backend são enviados ao Sentry. |
| RNF-27 | Logs estruturados, sempre com `tenant_id` e o identificador da requisição. |
| RNF-28 | Alerta de orçamento na nuvem para avisar sobre qualquer cobrança acima de zero. |

---

## 3. Restrições

Decisões já tomadas que limitam como o sistema é construído. Mudar qualquer uma delas exige um novo ADR.

| ID | Restrição | Motivo |
| --- | --- | --- |
| R-01 | TypeScript no frontend e no backend. | Uma linguagem só, com tipos e validações compartilhados. |
| R-02 | Backend em NestJS, como monólito modular, empacotado em container Docker. | Organização por módulos e portabilidade entre provedores. |
| R-03 | Frontend em React (Vite), como site estático. | Sistema com login, sem necessidade de SEO; hospedagem gratuita. |
| R-04 | PostgreSQL no Neon, num único banco compartilhado entre as lojas, com `tenant_id` e RLS. | Multi-tenant com isolamento garantido pelo banco e custo zero. |
| R-05 | Hospedagem: Cloudflare Pages (frontend), Google Cloud Run (backend) e Cloudflare R2 (fotos). | Planos gratuitos que permitem uso comercial e escalam pagando por uso. |
| R-06 | Autenticação com biblioteca open source rodando no próprio backend. | Sem custo por usuário e sem dependência de fornecedor. |
| R-07 | Custo de infraestrutura zero na fase piloto; o único gasto previsto é o domínio. | Validar o produto antes de investir. |
| R-08 | Backend e banco hospedados nos EUA. | O plano gratuito do Cloud Run só vale em regiões americanas; a latência maior é aceita. |
| R-09 | Sem SLA de disponibilidade e com atraso na primeira requisição após inatividade. | Limitações dos planos gratuitos. |
| R-10 | Um único desenvolvedor. | Prioriza ferramentas produtivas e evita infraestrutura que não seja necessária agora. |
| R-11 | Escopo da v1: login e catálogo. Reservas e financeiro ficam para a v2 e a v3. | Colocar a loja piloto para usar o sistema o quanto antes. |
| R-12 | Criação de lojas e liberação de módulos feitas por script ou painel interno, sem cadastro self-service. | Há uma única loja piloto. |
| R-13 | Sistema só em português do Brasil. | Público-alvo são lojas brasileiras. |
| R-14 | Tratamento de dados pessoais conforme a LGPD. | Obrigação legal. |

---

## 4. Pendências

- **RF-08 × RF-07:** o RF-07 diz que o usuário não altera a própria senha, mas o RF-08 exige a troca no primeiro acesso. Decidir se a troca no primeiro acesso fica como exceção ou se o RF-08 sai. Sem o RF-08, o administrador conhece a senha de todos os usuários.
