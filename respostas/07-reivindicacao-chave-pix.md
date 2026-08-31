# Ticket: 07 — Reivindicação de chave Pix não solicitada

## 1. Entendimento do problema

- **Fatos relatados ou observados:** a Pizzaria do Bairro recebeu aviso da Woovi e do banco antigo sobre reivindicação de um e-mail corporativo como chave Pix. A titular afirma não ter iniciado o pedido.
- **Impacto:** possível perda de controle da chave e risco de engenharia social; não há evidência de invasão, origem do pedido ou prazo restante.
- **Ainda não confirmado:** se a mensagem é oficial, status da reivindicação, PSP solicitante e se a cliente ainda controla o e-mail/chave.

## 2. Informações necessárias

| Informação | Origem | Finalidade | Forma segura de obtenção |
| --- | --- | --- | --- |
| Capturas da mensagem e horário | Cliente | Validar origem e status | Não clicar em links enviados por terceiros |
| Chave mascarada, conta e status da reivindicação | Consulta interna autorizada | Confirmar evento no DICT/PSP | Acesso autenticado |
| Confirmação de controle do e-mail corporativo | Cliente | Avaliar risco de conta comprometida | Sem senhas ou códigos 2FA |

## 3. Plano de investigação

1. Confirmar, por canal autenticado e ferramentas autorizadas, se existe reivindicação para a chave e seu estado; nunca pedir código, senha ou aprovação por WhatsApp.
2. Orientar a cliente a não confirmar a transferência se não a solicitou e a acessar o banco antigo pelo app/site oficial para cancelar ou contestar conforme as opções exibidas.
3. Verificar acesso ao e-mail corporativo e atividades suspeitas; se houver indício, orientar proteção da conta e escalar risco de fraude.
4. Registrar a orientação e acompanhar somente o status confirmado, sem prometer cancelamento fora do fluxo do PSP/DICT.

## 4. Hipóteses priorizadas

### Hipótese 1 — É uma reivindicação legítima iniciada por terceiro

- **Como confirmar ou descartar:** validar status e PSP pelos registros autorizados.
- **Ação se confirmada:** orientar cancelamento/contestação pelo canal oficial antes de qualquer confirmação.

### Hipótese 2 — A mensagem é tentativa de phishing

- **Como confirmar ou descartar:** comparar remetente, links e evento interno; a cliente não deve interagir com links não verificados.
- **Ação se confirmada:** orientar bloqueio, preservação da evidência e reporte de fraude.

### Hipótese 3 — Há comprometimento do e-mail corporativo

- **Como confirmar ou descartar:** a cliente verifica acessos e recuperação pelo provedor de e-mail; suporte não solicita credenciais.
- **Ação se confirmada:** priorizar recuperação do e-mail e proteção das contas financeiras.

## 5. Resposta ao cliente

Olá! Entendemos a preocupação. Não confirme nenhuma solicitação de transferência de chave que você não iniciou e não envie códigos, senhas ou dados por mensagem.

Vamos confirmar pelo canal seguro se há uma reivindicação vinculada à sua chave. Enquanto isso, acesse o aplicativo ou site oficial do seu banco antigo — digitando o endereço ou abrindo o app por conta própria — e procure a notificação da chave para cancelar ou contestar, caso essa opção esteja disponível. Também recomendamos verificar se o e-mail corporativo continua sob seu controle e trocar a senha diretamente no provedor se houver qualquer atividade suspeita.

Envie apenas as capturas e o horário da notificação pelo canal autenticado; não inclua senhas, códigos de validação ou dados bancários.

Atenciosamente,

Equipe de Suporte Woovi

## 6. Follow-ups internos

| Ação ou registro | Área/responsável sugerido | Prioridade | Critério de conclusão |
| --- | --- | --- | --- |
| Validar status e autenticidade do evento | Suporte técnico/Operações Pix | Alta | Status confirmado sem expor dados |
| Escalar indício de fraude, se houver | Segurança/Fraude | Alta | Caso registrado e orientação enviada |
| Registrar resultado da reivindicação | Suporte técnico | Média | Cliente atualizada com estado confirmado |

## 7. Fontes e suposições

### Fontes consultadas

- [Banco Central — Pix](https://www.bcb.gov.br/estabilidadefinanceira/pix) — consultada em 31/08/2026.
- [Ticket 07 oficial](https://github.com/woovibr/jobs/blob/main/cs-tech-challenges/07-reivindicacao-chave-pix.md) — consultado em 31/08/2026.
- [Fontes e regras do repositório](../referencias/fontes.md) — consultada em 31/08/2026.

### Suposições a validar

- Os avisos recebidos correspondem a uma reivindicação real e não a phishing.
- A cliente possui acesso ao banco antigo e ao e-mail corporativo.
