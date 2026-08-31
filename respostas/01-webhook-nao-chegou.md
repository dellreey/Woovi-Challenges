# Ticket: 01 — Webhook não chegou

## 1. Entendimento do problema

- **Fatos relatados ou observados:** a Acme Gateway (volume aproximado de R$ 2 milhões/mês) informa três cobranças `COMPLETED` sem o evento `OPENPIX:TRANSACTION_RECEIVED` após mais de 40 minutos: `0a1b2c3d4e5f60718293a4b5c6d7e8f9`, `1f2e3d4c5b6a7980a1b2c3d4e5f60718` e `9988776655443322ffeeddccbbaa1100`. O endpoint estava online e recebia outras cobranças. O reenvio manual da primeira cobrança chegou ao endpoint.
- **Impacto:** o crédito do cliente final está atrasado. O volume justifica prioridade alta, mas o valor das três operações e a abrangência além delas não foram confirmados.
- **Escopo e cronologia:** o relato se limita a três cobranças e ao evento esperado. Faltam horários exatos com fuso, ambiente, configuração vigente e resultado das tentativas originais.
- **Ainda não confirmado:** não está confirmado que existe incidente, que houve tentativa original de entrega para cada ID ou que outras contas/eventos foram afetados.

## 2. Informações necessárias

| Informação | Origem | Finalidade | Forma segura de obtenção |
| --- | --- | --- | --- |
| Horário de conclusão de cada cobrança, com fuso, e ambiente | Cliente; consulta interna autorizada | Correlacionar cobrança, evento e entrega | Canal de suporte autenticado |
| URL mascarada, evento e ativação da configuração vigente | Cliente; consulta interna autorizada | Conferir o destino e a assinatura aplicáveis | Nunca enviar AppID, token ou cabeçalho de autorização |
| Horário, destino e resultado das tentativas originais e do reenvio | Consulta interna autorizada | Distinguir ausência de tentativa, falha e atraso | Registrar somente metadados necessários |
| Logs sanitizados do endpoint: timestamp, código HTTP e ID de requisição | Cliente | Confrontar a visão do destino com as tentativas | Sem payload, credenciais ou dados pessoais |
| Recebimento posterior das segunda e terceira cobranças | Cliente | Medir persistência e escopo | Atualização no chamado autenticado |

Configuração, registros das cobranças e tentativas devem ser consultados internamente conforme acesso autorizado; logs do endpoint e efeito no sistema dependem da Acme.

## 3. Plano de investigação

1. **Correlacionar operação e evento** — consultar os três IDs e horários, confirmando a conclusão e o evento aplicável; se não corresponderem a `OPENPIX:TRANSACTION_RECEIVED`, explicar a divergência com a evidência; se corresponderem, seguir para as entregas.
2. **Examinar tentativas originais e reenvio** — por ID, comparar existência, horário, destino e resultado das tentativas com o reenvio bem-sucedido da primeira; se houver erro HTTP ou timeout, cruzar com logs sanitizados do cliente; se não houver tentativa, preservar a evidência e investigar geração/roteamento.
3. **Validar a configuração no momento dos eventos** — verificar URL, evento, ativação, conta e ambiente vigentes; se forem divergentes, corrigir somente após validação do cliente e testar de forma controlada; se forem aplicáveis, manter a análise na geração/entrega.
4. **Delimitar o escopo** — procurar ocorrências correlatas no mesmo intervalo e evento, sem inferir abrangência pelo volume; abrir fluxo de incidente apenas se a evidência atender aos critérios internos de falha sistêmica ou impacto além do caso.

## 4. Hipóteses priorizadas

### Hipótese 1 — As tentativas originais falharam ou atrasaram, e o reenvio manual contornou somente a primeira ocorrência

- **Por que é plausível:** as três cobranças foram relatadas como concluídas sem evento por mais de 40 minutos, enquanto o reenvio da primeira chegou. Isso não identifica a causa.
- **Como confirmar ou descartar:** comparar tentativa original e reenvio por ID, horário, destino e resultado.
- **Ação se confirmada:** recuperar os casos pelo procedimento interno aplicável e investigar a causa; avaliar incidente conforme o escopo confirmado.

### Hipótese 2 — O endpoint rejeitou, expirou ou não processou as tentativas originais

- **Por que é plausível:** endpoint online e recebendo outros eventos reduzem a probabilidade de indisponibilidade contínua, mas não excluem falha transitória, resposta não `2xx`, bloqueio seletivo ou erro de processamento.
- **Como confirmar ou descartar:** confrontar códigos HTTP, tempo de resposta e identificadores das tentativas com logs sanitizados de proxy e aplicação no horário exato.
- **Ação se confirmada:** a Acme corrige a condição do endpoint e valida o recebimento; suporte verifica a recuperação aplicável sem prometer reprocessamento.

### Hipótese 3 — A configuração no momento das cobranças diferia daquela usada no reenvio

- **Por que é plausível:** o êxito do reenvio demonstra que o endpoint recebeu aquela requisição, mas não prova que URL, evento, ativação, conta ou ambiente eram idênticos antes.
- **Como confirmar ou descartar:** revisar o histórico de configuração e compará-lo com o destino e a assinatura do reenvio.
- **Ação se confirmada:** corrigir a configuração aprovada pelo cliente, validar uma entrega controlada e recuperar operações com proteção contra crédito duplicado.

## 5. Resposta ao cliente

Olá, time Acme Gateway.

Entendemos o impacto: o crédito dos clientes finais está atrasado. Registramos três cobranças concluídas sem o evento esperado por mais de 40 minutos e que o reenvio manual da primeira chegou ao endpoint; isso, por si só, ainda não permite concluir que há um incidente.

Estamos correlacionando cada cobrança, a configuração vigente e as tentativas de entrega originais. Para complementar a análise, pedimos os horários de conclusão com fuso, a confirmação da URL mascarada e do ambiente, além de logs sanitizados do endpoint no intervalo (horário, código HTTP e identificador de requisição, se houver), sem payloads ou credenciais.

Também pedimos a confirmação sobre o recebimento posterior da segunda e terceira cobranças. Retornaremos assim que a correlação indicar o escopo confirmado e o próximo passo seguro para os créditos pendentes.

Atenciosamente,

Equipe de Suporte Woovi

## 6. Follow-ups internos

| Ação ou registro | Área/responsável sugerido | Prioridade | Critério de conclusão |
| --- | --- | --- | --- |
| Correlacionar os três IDs, tentativas originais e reenvio | Suporte técnico | Alta, pelo crédito pendente | Estado de evento/entrega documentado por ID e cliente atualizado |
| Investigar geração/roteamento se houver evento aplicável sem tentativa original | Engenharia de webhooks | Alta enquanto houver créditos pendentes | Causa ou limitação confirmada, mitigação registrada e recuperação definida |
| Investigar o endpoint com a Acme se houver falha de entrega | Suporte técnico e equipe da Acme | Alta enquanto houver créditos pendentes | Evidência correlacionada e endpoint validado |

Não há elementos suficientes para declarar incidente. O reenvio bem-sucedido é evidência útil, mas não determina causa ou escopo.

## 7. Fontes e suposições

### Fontes consultadas

- [Woovi Developers — API Reference](https://developers.woovi.com/api-redoc) — consultada em 31/08/2026. A referência descreve `OPENPIX:TRANSACTION_RECEIVED` e que resposta `2xx` dentro do tempo de entrega reconhece o recebimento; outros resultados são objeto de nova tentativa.
- [Woovi Developers — Criando um webhook para interceptar um Pix via API](https://developers.woovi.com/en/docs/webhook/api/webhook-api) — consultada em 31/08/2026. Confirma os elementos configuráveis: URL, evento e ativação.
- [Fontes e regras de pesquisa do repositório](../referencias/fontes.md) — consultada em 31/08/2026.

### Suposições a validar

- Os três IDs pertencem à conta e ao ambiente da Acme; validar por consulta autorizada.
- O estado `COMPLETED` e a expectativa de `OPENPIX:TRANSACTION_RECEIVED` pertencem ao mesmo fluxo; validar por operação, sem inferir apenas pelos nomes.
- O reenvio manual usou a mesma configuração relevante às tentativas originais; validar com horários, destino e histórico de configuração.
