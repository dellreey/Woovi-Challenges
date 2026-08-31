# Ticket: 04 — Taxa de cobrança Pix acima do contrato

## 1. Entendimento do problema

- **Fatos relatados ou observados:** a MicroPay relata contrato com R$ 0,80 por cobrança paga, mínimo de R$ 0,50 e plano percentual de 0,80%. Informa R$ 4.327,00 de taxa para R$ 312.000,00 no último mês, aproximadamente 1,39%.
- **Impacto:** risco comercial e necessidade de conciliação; não há amostra de fatura, contrato vigente, quantidade de cobranças pagas, impostos, estornos, tarifas adicionais ou período exato.
- **Ainda não confirmado:** qual regra contratual vigia, como mínimo e percentual se combinam, e quais lançamentos compõem o total informado.

## 2. Informações necessárias

| Informação | Origem | Finalidade | Forma segura de obtenção |
| --- | --- | --- | --- |
| Contrato/aditivo vigente e data de vigência | Comercial/financeiro autorizado | Identificar regra aplicável | Consulta interna; não anexar contrato completo |
| Fatura/extrato itemizado e período | Cliente e financeiro interno | Reconciliar cada componente | Canal autenticado |
| Quantidade de cobranças pagas, valores, estornos e tarifas | Consulta interna autorizada | Recalcular a cobrança | Export sanitizado e mínimo necessário |
| Identificadores de 3 a 5 lançamentos questionados | Cliente | Fazer auditoria amostral conjunta | IDs, sem dados de pagador |

## 3. Plano de investigação

1. Confirmar plano, aditivos, impostos e vigência antes de interpretar “mínimo” ou percentual.
2. Reconciliar a fatura por categoria: cobranças pagas, regra percentual/fixa, mínimos, estornos, ajustes, serviços adicionais e tributos, quando aplicáveis.
3. Comparar uma amostra de lançamentos com as transações correspondentes e calcular a tarifa pela regra contratual confirmada.
4. Se houver diferença material não explicada, preservar a memória de cálculo e encaminhar à equipe de faturamento/comercial para correção ou esclarecimento formal.

## 4. Hipóteses priorizadas

### Hipótese 1 — O total inclui componentes além da tarifa percentual informada

- **Por que é plausível:** o relato mistura valor fixo, mínimo e percentual, sem a composição da fatura.
- **Como confirmar ou descartar:** itemizar cada lançamento contra a regra vigente.
- **Ação se confirmada:** explicar a composição com memória de cálculo auditável.

### Hipótese 2 — O plano/aplicação contratual está divergente

- **Por que é plausível:** a taxa efetiva relatada é superior a 0,80%, mas não há evidência da tabela aplicada.
- **Como confirmar ou descartar:** comparar cadastro de faturamento, contrato e fatura no mesmo período.
- **Ação se confirmada:** corrigir o cadastro/fatura conforme o processo aprovado e registrar a causa.

### Hipótese 3 — Há lançamento ou conciliação incorreta

- **Por que é plausível:** sem amostra e detalhamento não se exclui duplicidade, período errado ou classificação inadequada.
- **Como confirmar ou descartar:** auditar lançamentos amostrais e totalizadores.
- **Ação se confirmada:** envolver faturamento para revisão e comunicação formal.

## 5. Resposta ao cliente

Olá, time MicroPay.

Entendemos a preocupação e a necessidade de uma conciliação clara. Pelo total informado, a taxa efetiva relatada é de cerca de 1,39%, mas ainda não é possível concluir a causa sem separar os itens da fatura e confirmar qual regra contratual estava vigente no período.

Vamos revisar contrato/aditivos, cadastro de faturamento e o detalhamento das tarifas. Para acelerar, enviem o período exato e de três a cinco IDs de lançamentos questionados pelo canal autenticado. Retornaremos com uma memória de cálculo por categoria; se identificarmos cobrança fora da regra vigente, seguiremos o processo interno de correção aplicável.

Atenciosamente,

Equipe de Suporte Woovi

## 6. Follow-ups internos

| Ação ou registro | Área/responsável sugerido | Prioridade | Critério de conclusão |
| --- | --- | --- | --- |
| Validar contrato e tabela vigente | Comercial/Faturamento | Alta | Regra aplicável documentada |
| Reconciliar fatura e amostra de transações | Financeiro | Alta | Memória de cálculo auditável |
| Corrigir divergência, se confirmada | Faturamento/Comercial | Alta | Ajuste ou justificativa formal registrada |

## 7. Fontes e suposições

### Fontes consultadas

- [Ticket 04 oficial](https://github.com/woovibr/jobs/blob/main/cs-tech-challenges/04-cobranca-acima-do-contrato.md) — consultado em 31/08/2026.
- [Fontes e regras do repositório](../referencias/fontes.md) — consultada em 31/08/2026.

### Suposições a validar

- Os R$ 4.327,00 e R$ 312.000,00 são do mesmo período e escopo.
- A redação contratual citada está completa e vigente.
