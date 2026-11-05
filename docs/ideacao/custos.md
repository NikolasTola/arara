# Custos

> Atualizado em 04/11/2026 · SaaS de gestão para lojas de aluguel de vestidos
> Versão do documento: 1.0.0

Na fase piloto, com uma loja, a v1 custa cerca de **R$ 3,62 por mês (R$ 44 por ano)**: R$ 40 por ano do domínio e alguns centavos de tráfego da API para o Brasil. Todo o resto cabe nos planos gratuitos com folga. O primeiro custo relevante é o banco de dados, quando o sistema crescer para algumas lojas. Este documento cobre só a infraestrutura; o tempo de desenvolvimento não entra.

## Sumário

1. [Premissas](#1-premissas)
2. [Custo da fase piloto](#2-custo-da-fase-piloto)
3. [Projeção com mais lojas](#3-projeção-com-mais-lojas)
4. [Pontos de atenção](#4-pontos-de-atenção)
5. [Fontes](#5-fontes)

---

## 1. Premissas

Os cálculos são estimativas a partir do porte da loja piloto; os valores reais só aparecem com o sistema em uso.

| Item | Premissa |
| --- | --- |
| Lojas | 1 (loja piloto) |
| Usuários | 1 a 3 |
| Acervo | 300 itens, cerca de 350 peças |
| Fotos | 3 por item, cerca de 300 KB cada (WebP, até 1.600 px) |
| Uso | Loja aberta 10 horas por dia, 26 dias por mês |
| Requisições à API | 30.000 por mês (cerca de 1.150 por dia de uso) |
| Tempo por requisição | 200 ms, com 1 vCPU e 512 MiB de memória |
| Inicializações a frio | 30 por dia, de 5 segundos cada |
| Tamanho médio de resposta da API | 10 KB |
| Banco ativo | 8 horas por dia, no tamanho mínimo (0,25 CU) |
| Backup | 1 por dia, cerca de 10 MB, guardado por 30 dias |
| Câmbio | US$ 1 = R$ 5,46 (cotação de 04/11/2026) |

---

## 2. Custo da fase piloto

Com uma loja, o total é de **R$ 3,62 por mês**. O único serviço que cobra pelo uso é o tráfego da API para o Brasil, que não tem franquia gratuita: R$ 0,29 por mês.

| Serviço | Uso estimado no mês | Limite gratuito | Uso do limite | Custo por mês |
| --- | --- | --- | --- | --- |
| Cloud Run — CPU | 9.900 vCPU-s (30.000 × 0,2 s + 3.900 s de inicializações) | 180.000 vCPU-s | 5,5% | R$ 0,00 |
| Cloud Run — memória | 4.950 GiB-s (9.900 s × 0,5 GiB) | 360.000 GiB-s | 1,4% | R$ 0,00 |
| Cloud Run — requisições | 30.000 | 2 milhões | 1,5% | R$ 0,00 |
| Tráfego da API para o Brasil | 0,28 GiB (30.000 × 10 KB) × US$ 0,19 | Sem franquia para a América do Sul | — | R$ 0,29 |
| Neon — processamento | 60 CU-h (0,25 CU × 8 h × 30 dias) | 100 CU-h | 60% | R$ 0,00 |
| Neon — armazenamento | Menos de 0,1 GB | 1 GB | Menos de 10% | R$ 0,00 |
| R2 — armazenamento | 0,6 GB (270 MB de fotos + 300 MB de backups) | 10 GB | 6% | R$ 0,00 |
| R2 — operações | Cerca de 1.000 gravações e 20.000 leituras | 1 milhão de gravações e 10 milhões de leituras | Menos de 1% | R$ 0,00 |
| Artifact Registry | 0,3 GB (2 versões da imagem, cerca de 150 MB cada) | 0,5 GB | 60% | R$ 0,00 |
| Cloudflare Pages | Site estático do frontend | Uso comercial e banda sem custo | — | R$ 0,00 |
| GitHub Actions | 530 min (100 execuções × 5 min + 30 backups × 1 min), se o repositório for privado | 2.000 min | 27% | R$ 0,00 |
| Sentry | Erros do frontend e do backend | 5.000 erros | — | R$ 0,00 |
| Resend | E-mails de confirmação (RF-09) | 3.000 e-mails | Menos de 1% | R$ 0,00 |
| Domínio .com.br | R$ 40 por ano, no Registro.br | — | — | R$ 3,33 |
| **Total** | | | | **R$ 3,62** |

No ano, são cerca de R$ 44: R$ 40 do domínio, pagos de uma vez, e R$ 3,48 de tráfego.

---

## 3. Projeção com mais lojas

O banco é o primeiro limite gratuito a estourar e responde por mais de 90% do custo a partir de 10 lojas. Mesmo assim, o custo por loja cai com a escala: R$ 11 por mês com 10 lojas e menos de R$ 9 com 30.

Premissas: cada loja usa o sistema como a piloto. Com 10 lojas, o banco precisa de 0,5 CU por 12 horas ao dia; com 30, de 1 CU por 14 horas ao dia, já no plano pago do Neon (US$ 0,106 por CU-hora e US$ 0,35 por GB-mês).

| Serviço | 1 loja | 10 lojas | 30 lojas |
| --- | --- | --- | --- |
| Cloud Run (CPU, memória, requisições) | R$ 0,00 | R$ 0,00 | R$ 0,49 |
| Tráfego da API para o Brasil | R$ 0,29 | R$ 2,89 | R$ 8,74 |
| Neon (processamento e armazenamento) | R$ 0,00 | R$ 105,16 | R$ 245,97 |
| Cloudflare R2 | R$ 0,00 | R$ 0,00 | R$ 0,05 |
| Domínio | R$ 3,33 | R$ 3,33 | R$ 3,33 |
| **Total por mês** | **R$ 3,62** | **R$ 111,38** | **R$ 258,58** |
| Custo por loja | R$ 3,62 | R$ 11,14 | R$ 8,62 |

Cálculo do banco:

- 10 lojas: 180 CU-h (0,5 × 12 × 30) × US$ 0,106 + 0,5 GB × US$ 0,35 = US$ 19,26
- 30 lojas: 420 CU-h (1 × 14 × 30) × US$ 0,106 + 1,5 GB × US$ 0,35 = US$ 45,05

O plano gratuito do Neon cobre até 100 CU-h por mês, o que equivale a cerca de 13 horas de banco ativo por dia no tamanho mínimo. A saída do plano gratuito deve acontecer entre 3 e 10 lojas, conforme os horários de uso se espalharem pelo dia. Com 30 lojas, o Sentry e o GitHub Actions também podem passar dos limites gratuitos; eles não entram na tabela.

---

## 4. Pontos de atenção

- **A restrição R-07 não é cumprida à risca.** O tráfego da API para o Brasil não tem franquia gratuita no Google Cloud, então há um custo de centavos além do domínio. Vale ajustar o texto da restrição para "custo quase zero".
- **A região pode ser revista.** A tabela de preços do Cloud Run mostra franquia gratuita também para regiões da segunda faixa de preço (128.571 vCPU-s por mês). Antes do primeiro deploy, confirme se São Paulo (`southamerica-east1`) tem essa franquia; se tiver, a restrição R-08 pode cair e o sistema fica mais rápido para a loja.
- **O cartão fica cadastrado no Google Cloud.** Configure um alerta de orçamento de R$ 5 por mês para saber na hora se algum uso sair do previsto (RNF-28).

Ações para manter o custo baixo:

1. Limitar o tamanho máximo do banco no Neon a 0,25 CU enquanto houver uma só loja.
2. Criar uma política de limpeza no Artifact Registry que mantenha só as 2 imagens mais recentes.
3. Ativar compressão (gzip ou brotli) nas respostas da API, para reduzir o tráfego pago.
4. Servir as fotos direto do R2, que não cobra tráfego de saída, sem passar pela API.
5. Acompanhar no console do Neon, todo mês, quantas CU-horas foram usadas. É o primeiro limite que vai estourar.

---

## 5. Fontes

Preços consultados em 04/11/2026.

- [Planos do Neon](https://neon.com/docs/introduction/plans): plano gratuito com 100 CU-h e 1 GB por projeto; plano Launch a US$ 0,106 por CU-hora e US$ 0,35 por GB-mês.
- [Preços do Cloud Run](https://cloud.google.com/run/pricing): franquia de 180.000 vCPU-s, 360.000 GiB-s e 2 milhões de requisições; US$ 0,000024 por vCPU-s acima disso.
- [Preços de rede do Google Cloud](https://cloud.google.com/vpc/network-pricing): US$ 0,19 por GiB de tráfego para a América do Sul, sem franquia.
- [Preços do Cloudflare R2](https://developers.cloudflare.com/r2/pricing/index.md): 10 GB, 1 milhão de gravações e 10 milhões de leituras grátis; US$ 0,015 por GB-mês acima disso; saída gratuita.
- [Preços do Artifact Registry](https://cloud.google.com/artifact-registry/pricing?hl=pt-BR): 0,5 GiB grátis; cerca de US$ 0,10 por GiB-mês acima disso.
- [Limites do Cloudflare Pages](https://developers.cloudflare.com/pages/platform/limits): plano gratuito sem proibição de uso comercial.
- [Plano gratuito do GitHub Actions](https://cicdcalculator.com/github-actions-free-tier): 2.000 minutos por mês em repositórios privados; ilimitado em públicos.
- [Plano gratuito do Sentry](https://costbench.com/software/developer-tools/sentry/free-plan/): 5.000 erros por mês.
- [Preços do Resend](https://resend.com/pricing.md): 3.000 e-mails por mês grátis.
- [Preço do domínio .com.br](https://www.homehost.com.br/blog/dominio/registro-de-dominio-br/): R$ 40 por ano no Registro.br.
- [Cotação do dólar](https://www.meelion.com/indicadores-financeiros/dolar-hoje/): R$ 5,46 em 04/11/2026.
