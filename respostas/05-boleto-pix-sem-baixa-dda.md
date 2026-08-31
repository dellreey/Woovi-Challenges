# Ticket: 05 — Boleto pago via Pix QR continua aberto no DDA

## 1. Entendimento do problema

- **Fatos relatados ou observados:** cinco boletos emitidos pela Woovi foram pagos via QR Pix e constam `COMPLETED` no painel. No DDA dos pagadores ainda aparecem em aberto; um cliente realizou novo pagamento para a mesma cobrança.
- **Impacto:** há pelo menos um risco de pagamento em duplicidade e confusão para pagadores. Não estão confirmados os IDs dos boletos, identificadores Pix, banco apresentante, origem do segundo pagamento ou status de liquidação do boleto no arranjo de DDA.
- **Ainda não confirmado:** se o QR e o boleto são a mesma cobrança, se os dois valores foram recebidos e se a exibição do DDA depende de atualização do banco do pagador.

## 2. Informações necessárias

| Informação | Origem | Finalidade | Forma segura de obtenção |
| --- | --- | --- | --- |
| IDs das cinco cobranças e horários de pagamento com fuso | Cliente/consulta autorizada | Correlacionar boleto, Pix e status | Canal autenticado |
| Comprovantes sanitizados, `endToEndId` e identificador do boleto duplicado | Cliente | Confirmar dois pagamentos e origem | Ocultar dados pessoais e saldos |
| Status interno de cada cobrança, boleto e liquidação | Consulta interna autorizada | Separar pagamento Pix, boleto e baixa | Acesso autorizado |
| Banco do pagador e evidência de DDA em aberto | Cliente | Encaminhar investigação de exibição quando aplicável | Sem credenciais bancárias |

## 3. Plano de investigação

1. Correlacionar cada cobrança com QR, boleto, transação Pix e status interno; confirmar se o segundo pagamento pertence à mesma obrigação.
2. Para o caso duplicado, verificar recebimento, estado de crédito e elegibilidade de devolução antes de qualquer reembolso; não solicitar nova ação ao pagador.
3. Verificar a natureza do boleto e os eventos de liquidação disponíveis; comparar com a informação apresentada no DDA do banco do pagador.
4. Se o pagamento estiver confirmado e a pendência for apenas a visualização bancária, fornecer evidência ao cliente e encaminhar o caso pelo canal apropriado, sem prometer prazo ou alteração direta no DDA.

## 4. Hipóteses priorizadas

### Hipótese 1 — O DDA do banco pagador ainda não refletiu a liquidação exibida pela Woovi

- **Por que é plausível:** o painel marca pagamento concluído, enquanto a divergência está no aplicativo de terceiros.
- **Como confirmar ou descartar:** correlacionar boleto e pagamento com o status disponível ao arranjo e a evidência do banco.
- **Ação se confirmada:** orientar o pagador a não pagar novamente e encaminhar evidências ao banco responsável pela exibição.

### Hipótese 2 — O QR Pix e o boleto possuem trilhas de baixa distintas

- **Por que é plausível:** o pagamento via QR pode ter sido confirmado sem que a informação exibida pelo DDA acompanhe o mesmo momento.
- **Como confirmar ou descartar:** revisar tipo de cobrança, identificadores e eventos de liquidação.
- **Ação se confirmada:** explicar a diferença confirmada e registrar oportunidade de documentação, se houver.

### Hipótese 3 — Houve efetivamente pagamento duplicado

- **Por que é plausível:** o cliente relata dois comprovantes para a mesma cobrança.
- **Como confirmar ou descartar:** validar ambos os recebimentos, valores e identificadores únicos.
- **Ação se confirmada:** seguir o processo de devolução aplicável, confirmando o destino seguro antes do reembolso.

## 5. Resposta ao cliente

Olá, time Padaria Central.

Entendemos a preocupação, principalmente pelo possível pagamento em duplicidade. O status `COMPLETED` confirma que precisamos correlacionar as transações, mas não basta para concluir por que o DDA continua exibindo o boleto em aberto no banco do pagador.

Por favor, enviem pelo canal autenticado os IDs das cinco cobranças, os horários e os comprovantes sanitizados dos dois pagamentos do caso duplicado. Vamos confirmar a relação entre boleto, QR Pix e cada transação antes de orientar qualquer devolução. Enquanto isso, orientem o pagador a não realizar novo pagamento para essa cobrança.

Após confirmar os dois recebimentos, retornaremos com o próximo passo seguro para a devolução, se aplicável, e com a orientação baseada no status de liquidação identificado.

Atenciosamente,

Equipe de Suporte Woovi

## 6. Follow-ups internos

| Ação ou registro | Área/responsável sugerido | Prioridade | Critério de conclusão |
| --- | --- | --- | --- |
| Correlacionar os cinco boletos e pagamentos Pix | Suporte técnico | Alta pelo risco de duplicidade | Estado documentado por cobrança |
| Tratar devolução se dois recebimentos forem confirmados | Operações financeiras | Alta | Destino e devolução confirmados |
| Investigar diferença de exibição no DDA | Suporte/Parceiro de boleto | Média | Evidência e orientação registradas |

## 7. Fontes e suposições

### Fontes consultadas

- [Woovi Developers — API Reference](https://developers.woovi.com/api-redoc) — consultada em 31/08/2026.
- [Banco Central — Pix](https://www.bcb.gov.br/estabilidadefinanceira/pix) — consultada em 31/08/2026.
- [Ticket 05 oficial](https://github.com/woovibr/jobs/blob/main/cs-tech-challenges/05-boleto-pix-sem-baixa-dda.md) — consultado em 31/08/2026.

### Suposições a validar

- Os dois comprovantes se referem à mesma cobrança e representam dois recebimentos distintos.
- O DDA observado pertence ao banco do pagador e não a uma tela em cache ou a outro boleto.
