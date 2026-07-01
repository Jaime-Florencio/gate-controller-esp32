# Máquina de Estados

## Estados

| Estado | Descrição |
|---|---|
| FECHADO | Portão totalmente fechado |
| ABRINDO | Portão em movimento de abertura |
| ABERTO | Portão totalmente aberto |
| FECHANDO | Portão em movimento de fechamento |
| PARADO | Portão parado em posição intermediária |
| ERRO | Sistema em condição de falha |

## Eventos

| Evento | Descrição |
|---|---|
| START | Comando do usuário |
| STOP | Comando de parada |
| FIM_CURSO_ABERTO | Sensor de fim de curso aberto acionado |
| FIM_CURSO_FECHADO | Sensor de fim de curso fechado acionado |
| TIMEOUT | Tempo máximo de movimento excedido |
| ERRO | Falha detectada |

## Regras principais

- FECHADO + START → ABRINDO
- ABERTO + START → FECHANDO
- ABRINDO + START → PARADO
- FECHANDO + START → PARADO
- ABRINDO + STOP → PARADO
- FECHANDO + STOP → PARADO
- PARADO + START → inverte o último sentido
- PARADO + STOP → permanece PARADO
- TIMEOUT durante movimento → ERRO

## Tabela de Transição

| Estado Atual | Evento | Próximo Estado | Ação Esperada |
|---|---|---|---|
| FECHADO | START | ABRINDO | Iniciar abertura |
| FECHADO | STOP | FECHADO | Nenhuma ação |
| FECHADO | FIM_CURSO_FECHADO | FECHADO | Garantir motor parado |
| FECHADO | FIM_CURSO_ABERTO | ERRO | Condição inconsistente |
| FECHADO | TIMEOUT | FECHADO | Ignorar |
| FECHADO | ERRO | ERRO | Entrar em falha |
| ABRINDO | START | PARADO | Parar motor e registrar último sentido |
| ABRINDO | STOP | PARADO | Parar motor e registrar último sentido |
| ABRINDO | FIM_CURSO_ABERTO | ABERTO | Parar motor |
| ABRINDO | FIM_CURSO_FECHADO | ERRO | Condição inconsistente |
| ABRINDO | TIMEOUT | ERRO | Parar motor e sinalizar falha |
| ABRINDO | ERRO | ERRO | Parar motor e sinalizar falha |
| ABERTO | START | FECHANDO | Iniciar fechamento |
| ABERTO | STOP | ABERTO | Nenhuma ação |
| ABERTO | FIM_CURSO_ABERTO | ABERTO | Garantir motor parado |
| ABERTO | FIM_CURSO_FECHADO | ERRO | Condição inconsistente |
| ABERTO | TIMEOUT | ABERTO | Ignorar |
| ABERTO | ERRO | ERRO | Entrar em falha |
| FECHANDO | START | PARADO | Parar motor e registrar último sentido |
| FECHANDO | STOP | PARADO | Parar motor e registrar último sentido |
| FECHANDO | FIM_CURSO_FECHADO | FECHADO | Parar motor |
| FECHANDO | FIM_CURSO_ABERTO | ERRO | Condição inconsistente |
| FECHANDO | TIMEOUT | ERRO | Parar motor e sinalizar falha |
| FECHANDO | ERRO | ERRO | Parar motor e sinalizar falha |
| PARADO | START | Depende do último sentido | Inverter movimento |
| PARADO | STOP | PARADO | Manter motor parado |
| PARADO | FIM_CURSO_ABERTO | ABERTO | Atualizar estado |
| PARADO | FIM_CURSO_FECHADO | FECHADO | Atualizar estado |
| PARADO | TIMEOUT | PARADO | Ignorar |
| PARADO | ERRO | ERRO | Entrar em falha |
| ERRO | START | ERRO | Ignorar até reset futuro |
| ERRO | STOP | ERRO | Manter sistema seguro |
| ERRO | FIM_CURSO_ABERTO | ERRO | Ignorar |
| ERRO | FIM_CURSO_FECHADO | ERRO | Ignorar |
| ERRO | TIMEOUT | ERRO | Ignorar |
| ERRO | ERRO | ERRO | Manter falha |