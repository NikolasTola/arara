# Requisitos — próximas versões

> Atualizado em 04/11/2026 · SaaS de gestão para lojas de aluguel de vestidos

Requisitos que ficam fora da v1 (login e catálogo). O modelo de dados da v1 já está preparado para recebê-los sem retrabalho; cada versão entra como um módulo novo, ligado por loja.

## Sumário

1. [v2 — Reservas](#v2--reservas)
2. [v3 — Financeiro](#v3--financeiro)
3. [Futuro — evolução do SaaS](#futuro--evolução-do-saas)

---

## v2 — Reservas

A v2 resolve a dor principal da loja: impedir que a mesma peça seja alugada duas vezes no mesmo período e saber quando ela volta.

| ID | Requisito |
| --- | --- |
| RF-39 | Cadastrar clientes com dados de contato. |
| RF-40 | Reservar peças para um período, bloqueando conflito de datas e respeitando o intervalo para lavagem entre aluguéis. |
| RF-41 | Ver a agenda de provas, retiradas e devoluções. |
| RF-42 | Acompanhar a situação da peça no ciclo do aluguel: em prova, alugada, na lavanderia, na costureira. |
| RF-43 | Consultar o histórico de aluguéis por peça e por cliente. |

**Impacto no modelo de dados:** novas tabelas `clientes`, `reservas` e `reserva_pecas`; `pecas.situacao` ganha os estados do ciclo do aluguel.

---

## v3 — Financeiro

A v3 registra o dinheiro de cada reserva e mostra o resultado da loja por período.

| ID | Requisito |
| --- | --- |
| RF-44 | Registrar sinal, caução, pagamentos e multas da reserva. |
| RF-45 | Gerar relatórios financeiros por período. |

**Impacto no modelo de dados:** novas tabelas ligadas a `reservas`; nenhuma tabela da v1 muda.

---

## Futuro — evolução do SaaS

Entram quando houver mais de uma loja usando o sistema.

| ID | Requisito |
| --- | --- |
| RF-46 | Definir planos do SaaS e cobrar a assinatura das lojas. |
| RF-47 | Permitir que uma loja nova se cadastre sozinha (self-service). |
| RF-48 | Permitir que cada loja crie campos próprios nos itens. |

**Impacto no modelo de dados:** uma tabela `planos` ligada a `modulos`; `tenant_modulos` continua servindo para exceções por loja.
