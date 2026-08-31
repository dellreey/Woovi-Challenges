# Ticket: 08 — `correlationID` em crédito e débito de subcontas

## 1. Entendimento do problema

- **Fatos relatados ou observados:** a FreelaHub envia `correlationID` em créditos, débitos e transferências de subcontas, mas o extrato de subconta não o retorna; a listagem de transações externas não contém transferências internas e outros caminhos tentados falharam.
- **Impacto:** auditoria pós-incidente e idempotência do lado cliente ficam prejudicadas, embora não haja saldo parado relatado.
- **Ainda não confirmado:** se o `correlationID` é aceito/persistido nessas operações, se há endpoint/documentação privada aplicável e quais identificadores retornados podem servir de ponte para logs autorizados.

## 2. Informações necessárias

| Informação | Origem | Finalidade | Forma segura de obtenção |
| --- | --- | --- | --- |
| Requisições/respostas sanitizadas e versão da API | Cliente | Confirmar comportamento do contrato | Nunca AppID ou payload financeiro completo |
| IDs retornados, horários e subcontas mascaradas | Cliente | Correlacionar extrato e operação | Canal autenticado |
| Confirmação interna de persistência/consulta do campo | Produto/engenharia autorizados | Determinar suporte real | Consulta interna |

## 3. Plano de investigação

1. Conferir na especificação o contrato dos endpoints de crédito/débito e do extrato, registrando quais campos são de fato retornados.
2. Reproduzir uma operação isolada em ambiente autorizado com `correlationID` exclusivo e comparar resposta, extrato e logs internos.
3. Se não houver endpoint público que exponha a correlação, oferecer reconciliação assistida por IDs, horário, valor e `operationType`, dentro do acesso autorizado.
4. Registrar melhoria de produto: persistência e consulta de idempotency/correlation key para operações internas, sem prometer uma rota inexistente.

## 4. Hipóteses priorizadas

### Hipótese 1 — O `correlationID` não é exposto pelo extrato de subconta

- **Como confirmar ou descartar:** comparar contrato, reprodução e dados internos autorizados.
- **Ação se confirmada:** comunicar a limitação e fornecer alternativa de correlação suportada.

### Hipótese 2 — O campo não é persistido nessas operações

- **Como confirmar ou descartar:** verificar a trilha interna de uma operação de teste.
- **Ação se confirmada:** abrir melhoria/defeito de contrato e recomendar ledger cliente-side.

### Hipótese 3 — O cliente consulta um recurso inadequado para transferência interna

- **Como confirmar ou descartar:** mapear o tipo de operação e os recursos documentados.
- **Ação se confirmada:** orientar o recurso suportado, se existir.

## 5. Resposta ao cliente

Olá, time FreelaHub.

Entendemos a necessidade de trilha forense e não vamos afirmar que existe uma rota pública para esse `correlationID` sem validar o contrato específico de crédito e débito de subcontas. Pelos campos que vocês listaram, o extrato pode não expor essa referência diretamente.

Vamos reproduzir uma operação controlada e verificar se o valor é persistido e se há uma consulta suportada. Para os casos já ocorridos, enviem IDs retornados, horários, valores e `operationType` por canal autenticado; com isso podemos correlacionar a trilha autorizada. Como prática de resiliência, mantenham no seu ledger a associação entre o `correlationID` de domínio, o ID retornado e a resposta da operação, para idempotência e auditoria independente.

Atenciosamente,

Equipe de Suporte Woovi

## 6. Follow-ups internos

| Ação ou registro | Área/responsável sugerido | Prioridade | Critério de conclusão |
| --- | --- | --- | --- |
| Validar persistência e exposição do campo | Engenharia BaaS/Suporte | Média/alta | Contrato confirmado |
| Correlacionar casos já existentes | Suporte técnico | Média | Relatório assistido entregue |
| Avaliar melhoria de auditoria/idempotência | Produto/Engenharia | Média | Requisito priorizado ou limitação documentada |

## 7. Fontes e suposições

### Fontes consultadas

- [Woovi Developers — API Reference](https://developers.woovi.com/api-redoc) — consultada em 31/08/2026.
- [Ticket 08 oficial](https://github.com/woovibr/jobs/blob/main/cs-tech-challenges/08-correlationid-credit-debit.md) — consultado em 31/08/2026.

### Suposições a validar

- Os endpoints citados são os disponíveis à aplicação da FreelaHub.
- Não existe endpoint público documentado que já exponha a correlação de operações internas.
