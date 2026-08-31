# Ticket: 03 — Pay-out `FAILED` sem motivo do provedor

## 1. Entendimento do problema

- **Fatos relatados ou observados:** seis pagamentos foram criados por `POST /api/v1/payment`, aprovados inicialmente e depois emitiram `OPENPIX:MOVEMENT_FAILED`. Os campos de erro recebidos pelo cliente estavam vazios.
- **Impacto:** seis destinatários podem não ter recebido R$ 3.278,00 cada; não se pode afirmar que os valores foram debitados, devolvidos ou que a causa foi limite, chave inválida ou indisponibilidade bancária.
- **Ainda não confirmado:** estado final por pagamento, tentativa enviada ao provedor, resposta recebida, conta de origem, chave de destino e eventual reversão.

## 2. Informações necessárias

| Informação | Origem | Finalidade | Forma segura de obtenção |
| --- | --- | --- | --- |
| Horário, ambiente e `request-id` de cada criação/aprovação | Cliente e logs autorizados | Correlacionar a trilha completa | Canal autenticado, sem AppID |
| Estado, tentativas, resposta do provedor e movimentação financeira por `correlationID` | Consulta interna autorizada | Determinar se houve envio, recusa ou reversão | Registrar somente metadados |
| Chaves de destino mascaradas e confirmação de crédito pelos recebedores | Cliente | Investigar seletividade sem expor dados | Nunca solicitar dados bancários completos |

## 3. Plano de investigação

1. Consultar cada `correlationID` e comparar criação, aprovação, envio ao provedor, webhook e estado financeiro final.
2. Verificar resposta bruta, códigos internos e disponibilidade do provedor no intervalo; campo vazio não deve ser traduzido em causa para o cliente.
3. Separar os seis casos por evidência: não enviado, recusado, em reversão ou pendente de confirmação; confirmar qualquer reprocessamento antes de iniciar outro pagamento.
4. Se houver padrão sem motivo em múltiplas operações, abrir investigação de observabilidade/integração do provedor com evidências correlacionadas.

## 4. Hipóteses priorizadas

### Hipótese 1 — A falha ocorreu após a aprovação, mas o motivo do provedor não foi propagado

- **Por que é plausível:** `APPROVED` precedeu `MOVEMENT_FAILED`, e os campos de motivo vieram vazios.
- **Como confirmar ou descartar:** confrontar a resposta do provedor e a linha do tempo interna de cada pagamento.
- **Ação se confirmada:** registrar defeito de mapeamento/observabilidade e informar ao cliente somente o estado final confirmado.

### Hipótese 2 — Há uma condição comum no provedor ou na conta de origem

- **Por que é plausível:** seis falhas no mesmo fluxo sugerem, mas não provam, uma causa compartilhada.
- **Como confirmar ou descartar:** comparar conta, janela temporal, chaves mascaradas, códigos e demais pagamentos no período.
- **Ação se confirmada:** escalar ao responsável aplicável e definir recuperação sem duplicar créditos.

### Hipótese 3 — As falhas são independentes por destino ou regra transacional

- **Por que é plausível:** os campos vazios não excluem recusas distintas.
- **Como confirmar ou descartar:** comparar cada resposta do provedor e a confirmação de crédito dos recebedores.
- **Ação se confirmada:** orientar o tratamento individual de cada pagamento validado.

## 5. Resposta ao cliente

Olá, time PayoutFlow.

Entendemos o impacto de ficar sem motivação para seis pagamentos. `APPROVED` seguido de `MOVEMENT_FAILED` confirma que a análise precisa continuar, mas os campos vazios não permitem classificar com segurança como limite, chave inválida ou indisponibilidade bancária.

Vamos correlacionar cada `correlationID` com as tentativas e o estado financeiro final. Para complementar, enviem horário com fuso, ambiente e qualquer `request-id` das chamadas, além da confirmação de recebimento pelos destinatários quando disponível, sempre sem credenciais ou dados bancários completos. Pedimos que não reenviem os pagamentos até confirmarmos o estado de cada um, para evitar crédito duplicado.

Atenciosamente,

Equipe de Suporte Woovi

## 6. Follow-ups internos

| Ação ou registro | Área/responsável sugerido | Prioridade | Critério de conclusão |
| --- | --- | --- | --- |
| Correlacionar os seis pagamentos e suas movimentações | Suporte técnico | Alta | Estado final documentado por `correlationID` |
| Investigar ausência de motivo do provedor, se reproduzível | Engenharia de pagamentos | Alta | Causa ou limitação registrada |
| Avaliar recuperação/reversão sem duplicidade | Operações de pagamentos | Alta enquanto houver saldo incerto | Destinatários e cliente atualizados |

## 7. Fontes e suposições

### Fontes consultadas

- [Woovi Developers — API Reference](https://developers.woovi.com/api-redoc) — consultada em 31/08/2026.
- [Ticket 03 oficial](https://github.com/woovibr/jobs/blob/main/cs-tech-challenges/03-payout-failed-sem-motivo.md) — consultado em 31/08/2026.
- [Fontes e regras do repositório](../referencias/fontes.md) — consultada em 31/08/2026.

### Suposições a validar

- `APPROVED` e `FAILED` pertencem à mesma linha de pagamento; validar por correlação interna.
- Nenhum pagamento deve ser reenviado antes de confirmar seu resultado financeiro.
