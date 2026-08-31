# Ticket: 09 — Cliente perdeu acesso ao 2FA

## 1. Entendimento do problema

- **Fatos relatados ou observados:** a administradora trocou de celular, perdeu o autenticador e não acessa a conta; relata saldo de R$ 47.000 e necessidade de pagar fornecedor no dia seguinte.
- **Impacto:** acesso administrativo e operação financeira bloqueados. O CNPJ e e-mail enviados não bastam, por si só, para redefinir 2FA.
- **Ainda não confirmado:** identidade, papel administrativo, dispositivos/sessões existentes, contatos autorizados e procedimento interno aplicável.

## 2. Informações necessárias

| Informação | Origem | Finalidade | Forma segura de obtenção |
| --- | --- | --- | --- |
| E-mail cadastrado e empresa, já no chamado autenticado | Cliente | Localizar conta | Não solicitar senha, código ou seed |
| Evidências de titularidade e autorização exigidas internamente | Cliente/Equipe de identidade | Validar recuperação | Canal seguro definido pelo produto |
| Sessões, dispositivos e eventos recentes | Consulta interna autorizada | Avaliar risco de comprometimento | Acesso restrito |

## 3. Plano de investigação

1. Iniciar o fluxo oficial de recuperação por canal seguro e validar a identidade/autoridade antes de qualquer alteração.
2. Revisar sinais de acesso suspeito e suspender/escalar conforme o procedimento aplicável; perda de aparelho não prova fraude, mas exige cautela.
3. Após validação, executar somente o reset permitido, exigir novo 2FA e registrar a ação auditável.
4. Confirmar a necessidade operacional de pagamento, sem usar isso para encurtar controles de identidade.

## 4. Hipóteses priorizadas

### Hipótese 1 — Perda legítima do autenticador após troca de aparelho

- **Como confirmar ou descartar:** concluir validação de identidade e ausência de sinais de risco relevantes.
- **Ação se confirmada:** seguir recuperação oficial e reconfigurar 2FA.

### Hipótese 2 — Tentativa de tomada de conta

- **Como confirmar ou descartar:** revisar atividade, contato e documentos conforme processo interno.
- **Ação se confirmada:** bloquear recuperação, escalar segurança e orientar a cliente pelo canal seguro.

### Hipótese 3 — Há outro administrador autorizado disponível

- **Como confirmar ou descartar:** verificar papéis e regras da conta.
- **Ação se confirmada:** orientar o caminho autorizado, sem compartilhar acesso.

## 5. Resposta ao cliente

Olá. Entendemos a urgência e vamos ajudar pelo processo seguro de recuperação. Para proteger sua conta e o saldo, não conseguimos remover o 2FA apenas por e-mail, CNPJ ou urgência de pagamento.

Vamos enviar as instruções do canal de verificação aplicável à sua conta. Não envie senha, códigos de seis dígitos, chave do autenticador ou documentos por este chat. Depois da validação, orientaremos a reconfiguração do autenticador e confirmaremos o acesso de forma segura.

Atenciosamente,

Equipe de Suporte Woovi

## 6. Follow-ups internos

| Ação ou registro | Área/responsável sugerido | Prioridade | Critério de conclusão |
| --- | --- | --- | --- |
| Iniciar recuperação com verificação reforçada | Suporte/Identidade | Alta | Identidade e autorização confirmadas |
| Revisar sinais de risco | Segurança | Alta | Risco avaliado e registrado |
| Registrar reset e novo fator, se aprovado | Suporte | Alta | Ação auditável concluída |

## 7. Fontes e suposições

### Fontes consultadas

- [Ticket 09 oficial](https://github.com/woovibr/jobs/blob/main/cs-tech-challenges/09-cliente-perdeu-2fa.md) — consultado em 31/08/2026.
- [Fontes e regras do repositório](../referencias/fontes.md) — consultada em 31/08/2026.

### Suposições a validar

- A solicitante tem autorização administrativa para a conta.
- Existe procedimento interno de recuperação que atende esse tipo de conta.
