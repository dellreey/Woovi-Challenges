# Ticket: 11 — Split de cobrança diferente do payload enviado

## 1. Entendimento do problema

- **Fatos relatados ou observados:** a NexoPay enviou cobrança de R$ 5,00 com um split `SPLIT_SUB_ACCOUNT` de R$ 5,10, mas a resposta trouxe split de R$ 0,50 e outro `SPLIT_PARTNER` de R$ 1,00 não informado. A cobrança ficou `ACTIVE`.
- **Impacto:** risco de divisão incorreta se a cobrança for paga; a resposta não prova, por si só, qual regra produziu os valores ou se a configuração persistida é a esperada.
- **Ainda não confirmado:** payload efetivamente recebido, configurações de split/partner da conta, semântica de valor acima do total e estado final dos splits no momento do pagamento.

## 2. Informações necessárias

| Informação | Origem | Finalidade | Forma segura de obtenção |
| --- | --- | --- | --- |
| Request/response completos sanitizados, hora e ambiente | Cliente | Correlacionar a criação | Sem AppID ou segredos |
| Configurações de partner, subconta e regras de split | Consulta interna autorizada | Identificar regras aplicadas | Acesso autorizado |
| Status de pagamento da cobrança e dados persistidos | Consulta interna autorizada | Prevenir repasse incorreto | Não exibir contas completas |

## 3. Plano de investigação

1. Verificar o payload bruto recebido, resposta, `correlationID` e configuração vigente da conta/partner no momento da criação.
2. Validar a regra para soma de splits maior que a cobrança e como valores são normalizados, rejeitados ou complementados; não presumir que R$ 5,10 é aceito.
3. Confirmar por que existe `SPLIT_PARTNER` e se ele decorre de configuração contratada, regra de plataforma ou comportamento inesperado.
4. Antes do pagamento, avaliar cancelamento/substituição segura da cobrança somente após confirmar o estado e a política aplicável; reproduzir em Sandbox com dados fictícios se autorizado.

## 4. Hipóteses priorizadas

### Hipótese 1 — Regra configurada de partner adicionou o `SPLIT_PARTNER`

- **Como confirmar ou descartar:** consultar configuração e histórico da conta.
- **Ação se confirmada:** explicar a regra e revisar a configuração com o cliente autorizado.

### Hipótese 2 — O valor de split foi normalizado/rejeitado por regra de validação

- **Como confirmar ou descartar:** reproduzir payload equivalente e conferir logs de validação.
- **Ação se confirmada:** orientar payload válido e documentar a regra.

### Hipótese 3 — O serviço persistiu divisão diferente da solicitada

- **Como confirmar ou descartar:** comparar payload bruto, configuração e entidade persistida.
- **Ação se confirmada:** abrir defeito e proteger a cobrança contra liquidação indevida conforme processo.

## 5. Resposta ao cliente

Olá, time NexoPay.

Entendemos a preocupação: a divisão retornada não corresponde, à primeira vista, ao JSON enviado. Como a cobrança está `ACTIVE`, vamos primeiro conferir a configuração de partner/subconta e a regra de validação aplicada antes de ela ser paga. Não recomendamos criar outra cobrança nem assumir que o split será corrigido automaticamente.

Enviem pelo canal autenticado o request/response sanitizados, horário e ambiente. Vamos correlacionar o `correlationID`, validar por que houve um `SPLIT_PARTNER` e como o valor acima do total da cobrança foi tratado. Se for necessário substituir a cobrança, retornaremos com o procedimento seguro após confirmar o estado persistido.

Atenciosamente,

Equipe de Suporte Woovi

## 6. Follow-ups internos

| Ação ou registro | Área/responsável sugerido | Prioridade | Critério de conclusão |
| --- | --- | --- | --- |
| Correlacionar payload e configuração de split | Suporte técnico/Engenharia | Alta | Regra aplicada identificada |
| Proteger cobrança se houver risco de repasse indevido | Operações de pagamentos | Alta | Estado da cobrança e ação segura definidos |
| Corrigir documentação ou serviço, se divergente | Produto/Engenharia | Média/alta | Causa e orientação registradas |

## 7. Fontes e suposições

### Fontes consultadas

- [Woovi Developers — API Reference](https://developers.woovi.com/api-redoc) — consultada em 31/08/2026.
- [Ticket 11 oficial](https://github.com/woovibr/jobs/blob/main/cs-tech-challenges/11-charge-request-invalido.md) — consultado em 31/08/2026.

### Suposições a validar

- A cobrança ainda não foi paga e pode ser analisada antes de qualquer repasse.
- O `SPLIT_PARTNER` não foi adicionado conscientemente por configuração conhecida da NexoPay.
