# Evidência complementar — Ticket 01

## Objetivo

Validar, em ambiente Sandbox da Woovi e com a collection OpenPix no Postman, o fluxo técnico de entrega do webhook `OPENPIX:TRANSACTION_RECEIVED`.

## Fluxo executado

1. Configurar um webhook ativo para `OPENPIX:TRANSACTION_RECEIVED`.
2. Criar uma cobrança de teste de R$ 1,00 no Sandbox.
3. Simular o pagamento da cobrança no painel Sandbox.
4. Confirmar o recebimento do `POST` no endpoint temporário de teste, com resposta `200`.

## Evidências

| Etapa | Captura |
| --- | --- |
| Webhook configurado | [00-webhook-configurado.png](00-webhook-configurado.png) |
| Evento de teste recebido | [01-webhook-teste.png](01-webhook-teste.png) |
| Cobrança criada | [02-cobranca-criada.png](02-cobranca-criada.png) |
| Pagamento simulado | [03-pagamento-simulado.png](03-pagamento-simulado.png) |
| Evento final recebido | [04-webhook-recebido.png](04-webhook-recebido.png) |

## Limites da evidência

O teste comprova que o fluxo configurado funciona no Sandbox: cadastro do webhook, criação de cobrança, simulação de pagamento e recepção do evento. Ele não prova a causa de falhas em cobranças reais, não substitui a correlação de logs de entrega e não deve ser usado para inferir status de produção.

As capturas foram registradas sem credenciais; a chave HMAC e a URL temporária de recebimento foram ocultadas.
