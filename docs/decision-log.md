# Decision Log — Gate Controller ESP32

Este documento registra as principais decisões de engenharia tomadas durante o desenvolvimento do firmware do controle de portão eletrônico com ESP32.

O objetivo é manter rastreável o motivo das escolhas arquiteturais, facilitando manutenção, revisão técnica e evolução futura do projeto.

---

## DEC-001 — Utilizar arquitetura em camadas

**Decisão:**
O firmware será organizado em camadas/módulos com responsabilidades separadas.

**Motivo:**
Evitar que leitura de hardware, lógica de estados, regras de operação e acionamento físico fiquem concentrados em um único arquivo ou função.

**Consequência:**
O sistema fica mais fácil de testar, manter e expandir.

---

## DEC-002 — Criar o módulo Hardware

**Decisão:**
Criar uma camada dedicada para acesso direto ao ESP32.

**Motivo:**
Isolar detalhes como GPIO, sensores, botão e acionamento do motor.

**Consequência:**
Outros módulos não precisam conhecer pinos, níveis lógicos ou detalhes elétricos.

---

## DEC-003 — Criar o módulo Events

**Decisão:**
Criar um módulo responsável por transformar leituras físicas em eventos lógicos.

**Motivo:**
O botão e os sensores são sinais elétricos. O sistema precisa traduzi-los para eventos como START, STOP, FIM_CURSO_ABERTO e FIM_CURSO_FECHADO.

**Consequência:**
A máquina de estados recebe eventos limpos, sem precisar interpretar GPIO diretamente.

---

## DEC-004 — Criar o módulo StateMachine

**Decisão:**
Criar uma máquina de estados para controlar o comportamento lógico do portão.

**Motivo:**
O portão possui estados bem definidos e regras de transição explícitas.

**Consequência:**
As regras de funcionamento ficam centralizadas, previsíveis e documentáveis.

---

## DEC-005 — Criar o módulo GateController

**Decisão:**
Criar um módulo intermediário entre StateMachine e Hardware.

**Motivo:**
A StateMachine deve decidir o que deve acontecer, mas não deve saber como acionar fisicamente o motor.

**Consequência:**
O GateController traduz ações lógicas como ABRIR_PORTAO, FECHAR_PORTAO e PARAR_MOTOR em comandos físicos para o Hardware.

---

## DEC-006 — StateMachine não acessa GPIO

**Decisão:**
A StateMachine não terá acesso direto ao Hardware.

**Motivo:**
Misturar regra de negócio com controle de GPIO aumentaria o acoplamento e dificultaria testes.

**Consequência:**
A máquina de estados pode ser testada sem depender do ESP32 ou do Wokwi.

---

## DEC-007 — Hardware não altera estados

**Decisão:**
O módulo Hardware não pode modificar o estado do sistema.

**Motivo:**
Hardware deve apenas ler sinais e executar comandos elétricos.

**Consequência:**
As regras do portão permanecem centralizadas na StateMachine.

---

## DEC-008 — Timeout controlado pela StateMachine

**Decisão:**
O controle de timeout será responsabilidade da StateMachine.

**Motivo:**
O timeout depende do tempo de permanência em estados como ABRINDO ou FECHANDO.

**Consequência:**
A StateMachine registra quando entrou em determinado estado e detecta quando o tempo máximo foi excedido.

---

## DEC-009 — Usar NO_EVENT quando nada acontecer

**Decisão:**
O módulo Events deve retornar NO_EVENT quando nenhuma entrada relevante for detectada.

**Motivo:**
O loop principal executa continuamente, mas na maior parte do tempo nenhum novo evento ocorre.

**Consequência:**
NO_EVENT não causa transição de estado nem ação física.

---

## DEC-010 — Separar estado lógico de ação física

**Decisão:**
A StateMachine deve retornar tanto o novo estado quanto a ação lógica necessária.

**Motivo:**
Estado e ação não são a mesma coisa. Um estado representa a condição lógica do sistema; uma ação representa algo que deve ser executado.

**Consequência:**
Exemplo: FECHADO + START gera estado ABRINDO e ação ABRIR_PORTAO.
