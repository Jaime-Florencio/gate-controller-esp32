# System Model — Gate Controller ESP32

## Objetivo

Este documento define o modelo conceitual utilizado pelo firmware.

O objetivo é estabelecer um vocabulário único para todos os módulos do sistema, garantindo que Hardware, Events, StateMachine, GateController e Main utilizem as mesmas definições de estados, eventos e ações.

Este documento serve como base para a especificação das APIs e para a implementação do firmware.

---

# State (Estados)

Um estado representa a condição atual do sistema.

O firmware pode assumir apenas um estado por vez.

Estados definidos:

| Estado   | Descrição                                                        |
| -------- | ---------------------------------------------------------------- |
| FECHADO  | O portão encontra-se totalmente fechado.                         |
| ABRINDO  | O motor está abrindo o portão.                                   |
| ABERTO   | O portão encontra-se totalmente aberto.                          |
| FECHANDO | O motor está fechando o portão.                                  |
| PARADO   | Movimento interrompido antes do fim de curso.                    |
| ERRO     | Estado de falha provocado por timeout ou outra condição crítica. |

---

# Event (Eventos)

Um evento representa um fato ocorrido durante a execução do sistema.

Eventos são gerados pelo módulo Events e consumidos pela StateMachine.

Eventos definidos:

| Evento            | Descrição                                                  |
| ----------------- | ---------------------------------------------------------- |
| NO_EVENT          | Nenhum evento detectado durante o ciclo.                   |
| START             | Solicitação para iniciar ou alterar o movimento do portão. |
| STOP              | Solicitação para interromper o movimento.                  |
| FIM_CURSO_ABERTO  | Sensor indica que o portão chegou totalmente aberto.       |
| FIM_CURSO_FECHADO | Sensor indica que o portão chegou totalmente fechado.      |
| TIMEOUT           | Tempo máximo de abertura ou fechamento excedido.           |
| ERRO              | Indicação de condição crítica detectada pelo sistema.      |

---

# Action (Ações)

Uma ação representa uma operação física que deverá ser executada após uma decisão da StateMachine.

As ações são executadas pelo GateController.

Ações definidas:

| Ação             | Descrição                                              |
| ---------------- | ------------------------------------------------------ |
| NO_ACTION        | Nenhuma ação necessária.                               |
| ABRIR_PORTAO     | Acionar o motor no sentido de abertura.                |
| FECHAR_PORTAO    | Acionar o motor no sentido de fechamento.              |
| PARAR_MOTOR      | Desligar o motor imediatamente.                        |
| ENTRAR_MODO_ERRO | Executar procedimentos relacionados ao estado de erro. |

---

# Relação entre os elementos

O funcionamento do firmware segue o seguinte fluxo:

Evento
↓

StateMachine

↓

Novo Estado

↓

Ação

↓

GateController

↓

Hardware

Cada evento recebido pela máquina de estados pode resultar em:

* permanência no estado atual;
* mudança para um novo estado;
* geração de uma ação física.

---

# Observações

* Apenas um estado pode estar ativo por vez.
* Um ciclo do firmware pode gerar apenas um evento.
* Um evento pode ou não provocar mudança de estado.
* Uma mudança de estado pode ou não gerar uma ação física.
* O módulo Hardware não conhece estados nem eventos.
* O módulo GateController executa ações, mas não toma decisões.
* A StateMachine decide estados e ações, mas não acessa diretamente o hardware.
