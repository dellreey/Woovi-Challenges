# Ticket: 06 — Reembolso Pix no extrato sem identificadores

## 1. Entendimento do problema

- **Fatos relatados ou observados:** a CreditFin vê em abril um lançamento de R$ 24,91 rotulado `Reembolso Pix`, sem nome, CPF, `endToEndId`, `correlationID` ou comentário. O cliente conhece o caso por contato externo, mas não consegue conciliar sistematicamente linhas semelhantes.
- **Impacto:** conciliação manual e recorrência de contato; não há indício de valor incorreto ou falha de reembolso.
- **Ainda não confirmado:** origem interna da linha, disponibilidade dos identificadores em exportação/API, escopo dos demais lançamentos e política de exposição de dados no extrato.

## 2. Informações necessárias

| Informação | Origem | Finalidade | Forma segura de obtenção |
| --- | --- | --- | --- |
| Período, conta e captura sanitizada do extrato | Cliente | Reproduzir a visualização | Canal autenticado |
| `endToEndId` original e contrato de domínio | Cliente | Correlacionar o caso citado | Não registrar dados da cliente final |
| ID interno, status do reembolso e dados da exportação/API | Consulta interna autorizada | Identificar campos disponíveis e restrições | Acesso autorizado |

## 3. Plano de investigação

1. Correlacionar a linha de R$ 24,91 por data, valor, conta e `endToEndId` original; confirmar o reembolso sem expor dados da contraparte além do necessário.
2. Comparar painel, exportação e APIs disponíveis para verificar se o identificador existe em outro artefato suportado.
3. Avaliar com produto/compliance se o extrato pode exibir identificador pseudônimo, referência de reembolso ou export seguro sem ampliar dados pessoais.
4. Registrar melhoria de conciliação se a limitação for confirmada; não prometer alteração de layout ou prazo.

## 4. Hipóteses priorizadas

### Hipótese 1 — O extrato é uma visão resumida e os identificadores existem em outra trilha autorizada

- **Como confirmar ou descartar:** comparar linha, detalhe de reembolso e exportações/APIs.
- **Ação se confirmada:** orientar o fluxo de conciliação disponível e documentá-lo.

### Hipótese 2 — O produto não expõe identificadores suficientes para conciliação

- **Como confirmar ou descartar:** validar contrato de dados com produto e consultar casos equivalentes.
- **Ação se confirmada:** abrir melhoria com impacto, campos mínimos e avaliação de privacidade.

### Hipótese 3 — A linha está associada incorretamente

- **Como confirmar ou descartar:** conferir valor, horário e estado do reembolso por correlação interna.
- **Ação se confirmada:** corrigir o dado e investigar alcance.

## 5. Resposta ao cliente

Olá, time CreditFin.

Entendemos que um rótulo genérico não é suficiente para a conciliação. Vamos correlacionar a linha de R$ 24,91 usando o `endToEndId` original e verificar quais referências estão disponíveis nos detalhes, exportação ou API. Enviem o período/conta e o identificador pelo canal autenticado; não é necessário enviar dados pessoais da cliente final.

Também vamos registrar a necessidade de uma referência de conciliação no extrato, caso a limitação seja confirmada. Retornaremos com o caminho disponível e o estado da melhoria, sem prometer alteração de produto antes da avaliação.

Atenciosamente,

Equipe de Suporte Woovi

## 6. Follow-ups internos

| Ação ou registro | Área/responsável sugerido | Prioridade | Critério de conclusão |
| --- | --- | --- | --- |
| Correlacionar lançamento e reembolso citado | Suporte técnico | Média | Origem confirmada ao cliente |
| Mapear campos de painel, API e exportação | Produto/Suporte | Média | Alternativa documentada ou lacuna confirmada |
| Abrir melhoria de conciliação, se aplicável | Produto/Compliance | Média | Requisito e avaliação registrados |

## 7. Fontes e suposições

### Fontes consultadas

- [Woovi Developers — API Reference](https://developers.woovi.com/api-redoc) — consultada em 31/08/2026.
- [Ticket 06 oficial](https://github.com/woovibr/jobs/blob/main/cs-tech-challenges/06-reembolso-extrato-sem-identificacao.md) — consultado em 31/08/2026.
- [LGPD na ANPD](https://www.gov.br/anpd/pt-br/assuntos/legislacao/lei-geral-de-protecao-de-dados-lgpd) — consultada em 31/08/2026.

### Suposições a validar

- O `endToEndId` informado se relaciona ao lançamento citado.
- Não há outro artefato já disponível que resolva a conciliação em lote.
