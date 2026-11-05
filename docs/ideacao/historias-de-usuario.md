# Histórias de usuário

> Atualizado em 04/11/2026 · SaaS de gestão para lojas de aluguel de vestidos
> Versão do documento: 1.0.0

A v1 tem 26 histórias de usuário, agrupadas por épico. Cada uma segue o formato **como / quero / para**, traz critérios de aceite verificáveis e aponta os casos de uso (UC, em **casos-de-uso-v1.md**) e os requisitos (RF, em **requisitos-v1.md**) que cobre. Os atores são os mesmos dos casos de uso: usuário da loja, administrador da loja e administrador da plataforma.

## Sumário

| Épico | Histórias |
| --- | --- |
| [1 — Acesso](#épico-1--acesso) | US-01 a US-04 |
| [2 — Usuários da loja](#épico-2--usuários-da-loja) | US-05 a US-07 |
| [3 — Administração da plataforma](#épico-3--administração-da-plataforma) | US-08 a US-11 |
| [4 — Configuração da loja e etiquetas](#épico-4--configuração-da-loja-e-etiquetas) | US-12 a US-16 |
| [5 — Catálogo](#épico-5--catálogo) | US-17 a US-22 |
| [6 — Importação, busca e compartilhamento](#épico-6--importação-busca-e-compartilhamento) | US-23 a US-26 |

---

## Épico 1 — Acesso

### US-01 · Entrar no sistema

**Como** usuário da loja, **quero** entrar com meu e-mail e senha, **para** acessar o sistema da minha loja.

Prioridade: Essencial · UC-01 · RF-01, RF-03

Critérios de aceite:

- Com e-mail e senha corretos, abre a tela inicial mostrando só as funções liberadas para o meu perfil.
- Com dados incorretos, aparece "E-mail ou senha incorretos", sem dizer qual dos dois está errado.
- Após 5 tentativas erradas em 15 minutos, o login fica bloqueado temporariamente.
- Usuário desativado ou de loja suspensa não entra e vê uma mensagem explicando o motivo.
- A sessão expira após 24 horas sem acesso.

### US-02 · Sair do sistema

**Como** usuário da loja, **quero** sair do sistema, **para** que outra pessoa não use minha conta no mesmo aparelho.

Prioridade: Essencial · UC-04 · RF-02

Critérios de aceite:

- A opção "Sair" está acessível em todas as telas.
- Depois de sair, voltar no navegador não mostra dados da loja.
- A sessão é encerrada também no servidor.

### US-03 · Trocar senha no primeiro acesso

**Como** usuário da loja, **quero** definir minha própria senha no primeiro acesso, **para** que só eu conheça a senha da minha conta.

Prioridade: Essencial · UC-02 · RF-08 · **Em decisão:** conflita com o RF-07.

Critérios de aceite:

- Ao entrar com senha provisória, aparece a tela de nova senha antes de qualquer outra.
- A nova senha tem no mínimo 8 caracteres e precisa ser digitada duas vezes.
- Depois da troca, o sistema segue para a tela inicial.

### US-04 · Confirmar e-mail

**Como** usuário da loja, **quero** confirmar meu e-mail por um link, **para** garantir que o endereço cadastrado é meu.

Prioridade: Desejável · UC-03 · RF-09

Critérios de aceite:

- Ao ser cadastrado, recebo um e-mail com um link de confirmação.
- O link confirma o e-mail e deixa de funcionar depois de usado.
- O administrador vê na lista de usuários quem ainda não confirmou o e-mail.

---

## Épico 2 — Usuários da loja

### US-05 · Cadastrar usuário

**Como** administrador da loja, **quero** cadastrar as pessoas que trabalham comigo, **para** que cada uma tenha o próprio acesso.

Prioridade: Importante · UC-13 · RF-04

Critérios de aceite:

- Nome, e-mail, perfil e senha inicial são obrigatórios.
- Se o e-mail já existir no sistema, aparece uma mensagem e o cadastro não é feito.
- O novo usuário pertence à minha loja e aparece na lista de usuários.

### US-06 · Editar e desativar usuário

**Como** administrador da loja, **quero** alterar o nome e o perfil de um usuário e desativá-lo, **para** controlar quem acessa o sistema e o que cada pessoa pode fazer.

Prioridade: Importante · UC-13 · RF-05, RF-07

Critérios de aceite:

- Altero nome e perfil; a mudança de perfil vale a partir da próxima ação do usuário.
- Ao desativar, o usuário é desconectado de todos os aparelhos na hora.
- Um usuário desativado aparece marcado na lista e pode ser reativado.
- Não é possível desativar nem mudar o perfil do último administrador ativo da loja.
- O próprio usuário não tem opção para alterar o nome nem a senha.

### US-07 · Redefinir senha de um usuário

**Como** administrador da loja, **quero** redefinir a senha de um usuário, **para** que ele volte a entrar quando esquecer a senha.

Prioridade: Importante · UC-14 · RF-06

Critérios de aceite:

- Defino uma nova senha com no mínimo 8 caracteres.
- As sessões abertas do usuário são encerradas.
- Só consigo redefinir a senha de usuários da minha loja.

---

## Épico 3 — Administração da plataforma

### US-08 · Criar loja

**Como** administrador da plataforma, **quero** criar uma loja numa única operação, **para** que um novo cliente comece a usar o sistema no mesmo dia.

Prioridade: Essencial · UC-19 · RF-10

Critérios de aceite:

- Informo o nome da loja, o identificador curto (slug) e os dados do primeiro administrador.
- Loja, primeiro administrador e módulo catálogo são criados juntos; se algo falhar, nada é criado.
- O identificador curto é único; se já existir, aparece uma mensagem.
- A loja nasce sem tipos de item, grades ou categorias, para ela cadastrar os seus.

### US-09 · Suspender e reativar loja

**Como** administrador da plataforma, **quero** suspender e reativar uma loja, **para** bloquear o acesso de um cliente sem perder os dados dele.

Prioridade: Importante · UC-20 · RF-12

Critérios de aceite:

- Com a loja suspensa, nenhum usuário dela entra, e quem estava conectado é desconectado.
- Os dados da loja ficam intactos.
- Ao reativar, os usuários ativos voltam a entrar normalmente.

### US-10 · Gerenciar módulos

**Como** administrador da plataforma, **quero** ligar, desligar e agendar a desativação de módulos, **para** controlar o que cada loja usa, como num período de teste.

Prioridade: Importante · UC-21 · RF-13, RF-14, RF-15

Critérios de aceite:

- Ligo e desligo um módulo para uma loja específica.
- Agendo uma data e hora de desativação, para uma loja ou para todas.
- A partir da data agendada, o módulo some do menu e o sistema recusa o acesso a ele, sem ação manual.
- Desligo um módulo para todas as lojas de uma vez.
- O módulo catálogo não pode ser desligado para uma loja.

### US-11 · Redefinir senha de qualquer usuário

**Como** administrador da plataforma, **quero** redefinir a senha de qualquer usuário, **para** ajudar o administrador de uma loja que esqueceu a própria senha.

Prioridade: Essencial · UC-22 · RF-11

Critérios de aceite:

- Encontro o usuário pelo e-mail e defino uma nova senha.
- As sessões abertas do usuário são encerradas.

---

## Épico 4 — Configuração da loja e etiquetas

### US-12 · Cadastrar tipos de item

**Como** administrador da loja, **quero** cadastrar os tipos de item que alugo, **para** organizar o catálogo e gerar os códigos das peças.

Prioridade: Essencial · UC-15 · RF-16, RF-18

Critérios de aceite:

- Informo nome, prefixo do código e se o tipo usa tamanho.
- Nome e prefixo não se repetem dentro da loja.
- Se o tipo já tem peças, o prefixo e o "usa tamanho" ficam bloqueados para edição.
- Um tipo desativado some das listas de cadastro, mas os itens dele continuam no sistema.

### US-13 · Definir a grade de tamanhos

**Como** administrador da loja, **quero** definir a grade de tamanhos de cada tipo, **para** cadastrar as peças com os tamanhos que a loja realmente usa.

Prioridade: Essencial · UC-15 · RF-17

Critérios de aceite:

- Incluo tamanhos numéricos ou em letras, por exemplo de 32 a 56 para vestidos e P, M e G para bolsas.
- Um tamanho não se repete na grade do mesmo tipo.
- Reordeno os tamanhos, e essa ordem vale nas listas e nos filtros.
- Um tamanho desativado sai das opções, mas as peças que já o usam não mudam.

### US-14 · Cadastrar categorias

**Como** administrador da loja, **quero** cadastrar categorias como Madrinha e Formatura, **para** classificar os itens pela ocasião de uso.

Prioridade: Essencial · UC-16 · RF-19

Critérios de aceite:

- Cadastro, edito e desativo categorias.
- O nome não se repete dentro da loja.
- Uma categoria desativada some das listas, mas os itens continuam ligados a ela.

### US-15 · Cadastrar etiquetas

**Como** administrador da loja, **quero** cadastrar etiquetas numa tela própria, **para** padronizar os atributos usados na busca, como "Longo" ou "Renda".

Prioridade: Essencial · UC-17 · RF-20

Critérios de aceite:

- O sistema não aceita uma etiqueta igual a outra ignorando acento e maiúscula: "Longo" e "longo" são a mesma.
- Ao renomear uma etiqueta, o novo nome aparece em todos os itens que a usam.
- Uma etiqueta em uso não pode ser excluída; a mensagem mostra quantos itens a usam.

### US-16 · Relacionar etiquetas a um item

**Como** usuário da loja, **quero** escolher uma ou mais etiquetas para um item, **para** que ele apareça quando alguém filtrar por elas.

Prioridade: Essencial · UC-10 · RF-21

Critérios de aceite:

- No cadastro e na edição do item, escolho etiquetas cadastradas, com campo de busca.
- Um item aceita várias etiquetas.
- Removo uma etiqueta do item sem afetar a etiqueta nem os outros itens.

---

## Épico 5 — Catálogo

### US-17 · Cadastrar item

**Como** usuário da loja, **quero** cadastrar um item do catálogo, **para** substituir o caderno por um registro organizado e pesquisável.

Prioridade: Essencial · UC-08 · RF-22

Critérios de aceite:

- Tipo e nome são obrigatórios; cor, marca, descrição e categorias são opcionais.
- O preço do aluguel é informado em reais e não aceita valor negativo.
- Um item pode ter várias categorias.
- Ao salvar, abre a ficha do item, pronta para receber fotos e peças.

### US-18 · Editar e desativar item

**Como** usuário da loja, **quero** editar e desativar itens, **para** manter o catálogo atualizado sem perder o histórico.

Prioridade: Essencial · UC-08 · RF-23, RF-24

Critérios de aceite:

- Edito todos os dados do item.
- Se o item já tem peças, o tipo fica bloqueado para edição.
- Um item desativado não aparece na busca padrão, mas pode ser encontrado com o filtro de inativos e reativado.

### US-19 · Enviar e organizar fotos

**Como** usuário da loja, **quero** tirar e enviar fotos do item pelo celular, **para** mostrar o vestido às clientes sem precisar ir até a arara.

Prioridade: Essencial · UC-09 · RF-25, RF-26

Critérios de aceite:

- Envio várias fotos de uma vez, pela câmera ou pela galeria do celular.
- As fotos são reduzidas antes do envio, sem perda visível de qualidade.
- A primeira foto vira capa automaticamente; posso escolher outra capa e reordenar as fotos.
- Ao excluir uma foto, o sistema pede confirmação e remove também o arquivo.

### US-20 · Cadastrar peças

**Como** usuário da loja, **quero** cadastrar as peças de um item com o tamanho de cada uma, **para** saber exatamente quantas unidades tenho e de quais tamanhos.

Prioridade: Essencial · UC-11 · RF-27, RF-28

Critérios de aceite:

- Escolho o tamanho na grade do tipo do item; o tamanho é obrigatório quando o tipo usa tamanho.
- Cada peça recebe um código automático (ex.: V-0012), exibido na tela e nunca repetido na loja.
- Posso criar várias peças de uma vez marcando os tamanhos: marcar 38, 40 e 42 cria três peças.

### US-21 · Editar e desativar peça

**Como** usuário da loja, **quero** atualizar os dados de uma peça e desativá-la, **para** registrar ajustes e tirar de uso peças danificadas ou vendidas.

Prioridade: Essencial · UC-11 · RF-29, RF-30

Critérios de aceite:

- Edito tamanho, situação (disponível ou indisponível), anotações de ajuste e observações.
- Uma peça desativada sai das buscas, mas o histórico dela é mantido.
- Posso reativar uma peça desativada.

### US-22 · Ver a ficha do item

**Como** usuário da loja, **quero** ver tudo sobre um item numa tela só, **para** responder rapidamente à cliente se temos o vestido no tamanho dela.

Prioridade: Essencial · UC-07 · RF-31

Critérios de aceite:

- A ficha mostra as fotos (capa primeiro), os dados, as categorias e as etiquetas.
- As peças aparecem agrupadas por tamanho, com código e situação.
- A ficha funciona bem no celular.

---

## Épico 6 — Importação, busca e compartilhamento

### US-23 · Importar itens e peças por planilha

**Como** administrador da loja, **quero** importar o acervo de uma planilha de uma só vez, **para** não precisar cadastrar centenas de peças uma a uma.

Prioridade: Essencial · UC-18 · RF-32, RF-33

Critérios de aceite:

- Baixo um modelo de planilha com as colunas esperadas e um exemplo preenchido.
- Ao enviar a planilha, o sistema valida tudo antes de gravar e lista os erros por linha, por exemplo "Linha 12: o tamanho 60 não existe na grade de Vestido".
- Enquanto houver erro, nada é gravado.
- Sem erros, o sistema grava tudo e mostra um resumo com quantos itens e peças foram criados.
- As fotos são adicionadas depois, na ficha de cada item.

### US-24 · Buscar e filtrar itens

**Como** usuário da loja, **quero** buscar e filtrar o catálogo, **para** encontrar em segundos o que a cliente pediu, como "azul longo para madrinha, tamanho 40".

Prioridade: Essencial · UC-05 · RF-34, RF-35, RF-37

Critérios de aceite:

- A busca por texto procura em nome, cor, marca e etiquetas, sem diferenciar acentos e maiúsculas.
- Combino filtros de tipo, categoria, uma ou mais etiquetas, situação da peça e tamanho, exato ou em faixa ("do 38 ao 44").
- A lista mostra a foto de capa, tem paginação e ordena por nome ou data de cadastro.
- O resultado aparece em até 1 segundo.

### US-25 · Encontrar peça pelo código

**Como** usuário da loja, **quero** digitar o código de uma peça, **para** abrir o cadastro dela direto, sem navegar pelo catálogo.

Prioridade: Essencial · UC-06 · RF-36

Critérios de aceite:

- O campo aceita o código em maiúsculas ou minúsculas.
- Abre a ficha do item com a peça destacada.
- Se o código não existir, aparece "Peça não encontrada".

### US-26 · Compartilhar fotos pelo WhatsApp

**Como** usuário da loja, **quero** enviar as fotos de um item pelo WhatsApp, **para** mostrar opções à cliente antes de ela vir à loja.

Prioridade: Desejável · UC-12 · RF-38

Critérios de aceite:

- Na ficha do item, seleciono as fotos e toco em "Compartilhar".
- Abre o menu de compartilhamento do celular, de onde escolho o WhatsApp e o contato.
- As fotos chegam como imagens, não como links.
