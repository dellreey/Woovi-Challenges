# Ticket: 02 — Pix Automático trimestral some entre painel e API

## 1. Entendimento do problema

- **Fatos relatados ou observados:** a Helix SaaS consegue selecionar a frequência **Trimestral** no painel e concluir o fluxo sem erro. Ao tentar replicar a assinatura pela API, enviou `frequency: "QUARTERLY"` e recebeu `400` com `invalid enum value`. O cliente informa que o material consultado pela API listava somente `WEEKLY`, `MONTHLY`, `SEMIANNUALLY` e `ANNUALLY`.
- **Impacto:** a integração de Pix Automático está bloqueada para o ciclo trimestral; o cliente tem prazo declarado para concluí-la até sexta-feira. Não há evidência de mandato, cobrança ou valor em risco.
- **Escopo e cronologia:** há aparente divergência entre painel, documentação vista pelo cliente e validação da API. Não foram fornecidos corpo completo, rota, ambiente, horário, `correlationID`, `request-id` ou resposta integral da chamada que falhou.
- **Ainda não confirmado:** não está confirmado que a requisição usou `type: "PIX_RECURRING"`, a rota e o ambiente corretos, que o `400` veio do serviço atual, ou que o enum documentado no momento da chamada realmente excluía `QUARTERLY`.

## 2. Informações necessárias

| Informação | Origem | Finalidade | Forma segura de obtenção |
| --- | --- | --- | --- |
| Corpo sanitizado da requisição, incluindo `frequency`, `type` e `correlationID` | Cliente | Confirmar o valor enviado e o contexto da assinatura | Chat autenticado; nunca solicitar AppID ou token |
| Rota, ambiente, horário com fuso, status e cabeçalhos de correlação da resposta | Cliente; consulta interna autorizada | Correlacionar a validação que gerou o `400` | Captura sanitizada ou logs de integração |
| Capturas da opção do painel e da página de documentação consultada, com data/URL | Cliente | Distinguir diferença de interface, documentação desatualizada ou contrato de API | Sem credenciais ou dados de pagadores |
| Registro interno da requisição e da versão/validação aplicada no período | Consulta interna autorizada | Verificar se houve comportamento regressivo ou regra específica | Acesso interno autorizado |
| Resultado de reprodução controlada com payload mínimo e `QUARTERLY` | Suporte técnico; ambiente de teste autorizado | Separar erro de integração de divergência do serviço | Usar somente credenciais e dados de Sandbox |

## 3. Plano de investigação

1. **Correlacionar o `400` informado** — localizar a chamada por horário e `correlationID`, conferindo rota, ambiente, `type`, valor literal de `frequency` e mensagem de validação; se o valor não for `QUARTERLY` em uma assinatura `PIX_RECURRING`, orientar a correção do payload; se for, seguir para a reprodução.
2. **Conferir o contrato publicado e a configuração do painel** — comparar os prints do cliente com a referência oficial vigente e com a configuração salva pelo painel; `QUARTERLY` corresponde a trimestral na referência atual para Pix Automático. Se os materiais diferirem, preservar URLs, versão/data e impacto para correção documental.
3. **Reproduzir de modo controlado** — em ambiente autorizado, criar uma assinatura mínima `PIX_RECURRING` com `frequency: "QUARTERLY"` e um novo `correlationID`; se aceitar, comparar o payload e o ambiente com os da Helix; se repetir o `400`, registrar request-id, resposta e hora para a engenharia.
4. **Definir a comunicação e a mitigação** — se a API aceitar a reprodução, entregar ao cliente o payload corrigido e orientar novo teste idempotente; se rejeitar o enum documentado, abrir defeito de contrato/validação e informar apenas o estado confirmado, sem sugerir que o painel manual seja uma solução permanente para a integração.

## 4. Hipóteses priorizadas

### Hipótese 1 — A chamada da Helix não correspondeu ao contrato aplicável ao Pix Automático

- **Por que é plausível:** faltam corpo completo, `type`, rota e ambiente; o campo pode ter sido enviado em outro local, com transformação pelo cliente HTTP, ou em fluxo que não é `PIX_RECURRING`.
- **Como confirmar ou descartar:** comparar a requisição sanitizada e o registro interno com um payload mínimo aceito para `PIX_RECURRING` e `QUARTERLY`.
- **Ação se confirmada:** fornecer o ajuste específico de payload/rota e pedir nova chamada com um `correlationID` inédito.

### Hipótese 2 — Houve divergência entre a documentação consultada e o contrato disponível naquele momento

- **Por que é plausível:** o cliente relata uma lista de enums sem `QUARTERLY`, enquanto a referência atual descreve `QUARTERLY` como trimestral para Pix Automático.
- **Como confirmar ou descartar:** registrar URL, data, versão/captura da documentação e comparar com a especificação publicada e o histórico de alteração acessível internamente.
- **Ação se confirmada:** corrigir ou versionar a documentação, comunicar o enum correto e publicar exemplo de `PIX_RECURRING` com frequência trimestral.

### Hipótese 3 — A validação da API rejeita indevidamente um enum suportado pelo painel

- **Por que é plausível:** se a chamada sanitizada contiver `type: "PIX_RECURRING"` e `frequency: "QUARTERLY"` no ambiente correto, o `400` conflita com o contrato atual e com a opção exibida no painel.
- **Como confirmar ou descartar:** reproduzir o mesmo cenário em ambiente autorizado e correlacionar request-id, deployment/versão e resposta de validação.
- **Ação se confirmada:** abrir defeito de alta prioridade para a equipe responsável pela API/Pix Automático, anexando somente metadados e payloads sanitizados; acompanhar correção ou orientação oficial antes de prometer uma data ao cliente.

## 5. Resposta ao cliente

Olá, time Helix SaaS.

Entendemos que a divergência entre painel e API bloqueia a entrega da integração trimestral. O valor esperado para uma recorrência trimestral no Pix Automático é `QUARTERLY`, desde que a assinatura use `type: "PIX_RECURRING"`; a referência atual lista essa frequência como trimestral.

Como vocês receberam `400`, vamos correlacionar a chamada antes de concluir a causa. Podem enviar, pelo canal autenticado, o corpo sanitizado da requisição (sem AppID/token), a rota e o ambiente usados, o horário com fuso, o `correlationID` e a resposta completa com qualquer identificador de requisição? Também ajudam as duas capturas citadas, com a URL e a data em que a documentação foi consultada.

Com esses dados, vamos comparar a validação da chamada com a configuração do painel e retornar o próximo passo confirmado. Não recomendamos substituir a integração por operação manual enquanto essa divergência não estiver esclarecida.

Atenciosamente,

Equipe de Suporte Woovi

## 6. Follow-ups internos

| Ação ou registro | Área/responsável sugerido | Prioridade | Critério de conclusão |
| --- | --- | --- | --- |
| Correlacionar a requisição relatada e reproduzir `QUARTERLY` em ambiente autorizado | Suporte técnico | Alta para o bloqueio de integração | Payload, ambiente e resultado documentados com metadados de correlação |
| Corrigir ou versionar a documentação se o material consultado estiver defasado | Documentação de desenvolvedores | Média | Referência e exemplo mostram os enums aplicáveis a `PIX_RECURRING` |
| Abrir defeito de contrato se a API rejeitar `QUARTERLY` no cenário suportado | Engenharia de API / Pix Automático | Alta se reproduzível | Causa, correção ou orientação oficial registrada e cliente atualizado |

## 7. Fontes e suposições

### Fontes consultadas

- [Woovi Developers — Como criar um Pix Automático](https://developers.woovi.com/docs/pix-automatic/pix-automatic-how-to-create) — consultada em 31/08/2026. Descreve `QUARTERLY` como frequência trimestral aceita para `PIX_RECURRING` e a rota `POST /api/v1/subscriptions`.
- [Woovi Developers — API Reference](https://developers.woovi.com/api-redoc) — consultada em 31/08/2026. Lista `QUARTERLY` entre as frequências e restringe Pix Automático às frequências admitidas no produto.
- [Ticket 02 — Pix Automático trimestral some entre painel e API](https://github.com/woovibr/jobs/blob/main/cs-tech-challenges/02-pix-automatico-trimestral.md) — consultado em 31/08/2026.
- [Fontes e regras de pesquisa do repositório](../referencias/fontes.md) — consultada em 31/08/2026.

### Suposições a validar

- A assinatura relatada pretende usar `type: "PIX_RECURRING"`; confirmar pelo payload sanitizado.
- O `400` foi emitido pela API da Woovi no ambiente e na rota suportados, e não por validação intermediária do cliente; confirmar por logs e identificadores de correlação.
- A opção trimestral do painel e o enum `QUARTERLY` se referem ao mesmo produto/conta; confirmar pela configuração persistida e pelo ambiente.
