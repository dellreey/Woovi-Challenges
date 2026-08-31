# Ticket: 10 — Saldo travado em subcontas BaaS

## 1. Entendimento do problema

- **Fatos relatados ou observados:** cerca de 14 subcontas mostram saldo positivo no extrato, mas saque retorna `INSUFFICIENT_BALANCE`; a diferença total entre disponível e extrato é estimada em R$ 280 mil e a operação está parada.
- **Impacto:** crítico, com clientes finais afetados. Não há confirmação sobre bloqueios, reservas, liquidações pendentes, limites, status de conta ou mudança recente.
- **Ainda não confirmado:** o que compõe saldo disponível, se há restrição de compliance/risco, e se transferência à conta master é permitida/autorizada.

## 2. Informações necessárias

| Informação | Origem | Finalidade | Forma segura de obtenção |
| --- | --- | --- | --- |
| Lista de subcontas mascaradas, horários, valores e request-ids | Cliente | Delimitar escopo | Canal autenticado |
| Saldos contábil/disponível/bloqueado e eventos pendentes | Consulta interna autorizada | Identificar a diferença | Acesso BaaS autorizado |
| Status de KYC, limites, bloqueios e alterações recentes | Operações/risco autorizados | Verificar restrições | Sem exposição ao cliente de dados internos |
| Autorização e elegibilidade para transferência master | Cliente e operações | Evitar movimentação indevida | Processo formal aplicável |

## 3. Plano de investigação

1. Abrir incidente operacional e correlacionar as 14 subcontas, separando saldo contábil, disponível e bloqueado por categoria.
2. Examinar falhas de saque, limites, liquidações, reservas e restrições de risco/compliance por conta, sem assumir uma causa pelo `INSUFFICIENT_BALANCE`.
3. Comparar o período afetado com mudanças de produto/configuração e outras contas para definir alcance.
4. Avaliar transferência à master somente após confirmar propriedade, saldo disponível, elegibilidade e aprovações; não mover fundos como paliativo sem trilha autorizada.

## 4. Hipóteses priorizadas

### Hipótese 1 — Parte do saldo está bloqueada ou em liquidação

- **Como confirmar ou descartar:** consultar detalhamento de saldo e eventos pendentes.
- **Ação se confirmada:** explicar a categoria confirmada e tratar a liberação conforme processo aplicável.

### Hipótese 2 — Há restrição de conta, limite ou regra operacional

- **Como confirmar ou descartar:** revisar status, KYC, limites e retorno completo de saque.
- **Ação se confirmada:** encaminhar ao responsável, sem divulgar regra sensível.

### Hipótese 3 — Há regressão de cálculo/exposição de saldo

- **Como confirmar ou descartar:** reproduzir e comparar contas/períodos afetados.
- **Ação se confirmada:** incidentar, mitigar e reconciliar o impacto confirmado.

## 5. Resposta ao cliente

Olá, time WhitePay BaaS.

Reconhecemos a criticidade e já tratamos a diferença entre saldo exibido e disponível como investigação prioritária. O retorno `INSUFFICIENT_BALANCE` não identifica sozinho a causa; precisamos separar saldos bloqueados, pendentes e disponíveis por subconta antes de movimentar valores.

Enviem pelo canal autenticado a lista das subcontas afetadas, horários das tentativas e request-ids. Vamos correlacionar os saldos e o status das contas. Não transferiremos valores para a conta master sem confirmar elegibilidade e aprovações, pois isso poderia criar risco operacional e de conciliação. Atualizaremos vocês com o escopo confirmado e a ação segura aplicável.

Atenciosamente,

Equipe de Suporte Woovi

## 6. Follow-ups internos

| Ação ou registro | Área/responsável sugerido | Prioridade | Critério de conclusão |
| --- | --- | --- | --- |
| Abrir incidente e reconciliar saldos por subconta | Operações BaaS | Crítica | Diferença categorizada por conta |
| Verificar restrições/limites e falhas de saque | Risco/Compliance/Engenharia | Crítica | Causa ou limitação confirmada |
| Avaliar movimentação à master se elegível | Operações autorizadas | Alta | Aprovação e trilha de movimentação registradas |

## 7. Fontes e suposições

### Fontes consultadas

- [Woovi Developers — API Reference](https://developers.woovi.com/api-redoc) — consultada em 31/08/2026.
- [Ticket 10 oficial](https://github.com/woovibr/jobs/blob/main/cs-tech-challenges/10-baas-saldo-travado-subcontas.md) — consultado em 31/08/2026.

### Suposições a validar

- As 14 contas pertencem ao mesmo cliente e estão autorizadas para consulta.
- A estimativa de R$ 280 mil usa o mesmo corte temporal de extrato e disponível.
