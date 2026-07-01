# Arquitetura do Sistema

## Visão Geral

O projeto utiliza uma arquitetura modular em camadas para separar responsabilidades entre lógica de aplicação, geração de eventos, máquina de estados, controle do portão e acesso ao hardware.

O objetivo é evitar que a lógica principal dependa diretamente de GPIOs, sensores ou detalhes físicos do motor.

---

## Arquitetura em Camadas

```text
main.cpp
   |
   v
Events
   |
   v
StateMachine
   |
   v
GateController
   |
   v
Hardware