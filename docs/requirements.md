## Obejtivo# Requisitos do Sistema

## Projeto

**Nome:** Gate Controller ESP32

**Versão:** 0.1.0

**Autor:** Jaime Florencio da Silva Neto

**Objetivo**

Desenvolver um sistema embarcado para controle de um portão eletrônico utilizando ESP32, baseado em máquina de estados, programação orientada a eventos e arquitetura modular.

O principal objetivo do projeto é aplicar conceitos utilizados no desenvolvimento profissional de firmware, como abstração de hardware, desacoplamento entre módulos, temporização não bloqueante, documentação técnica e organização do código.

---

# Escopo

## O projeto contempla

- Controle de abertura e fechamento do portão;
- Máquina de estados finitos;
- Processamento orientado a eventos;
- Controle do motor através de uma camada de abstração de hardware;
- Sensores de fim de curso;
- Tratamento de timeout;
- Tratamento de estados de erro;
- Temporização baseada em `millis()`;
- Simulação completa utilizando Wokwi;
- Arquitetura modular em C++.

## Fora do escopo

Nesta primeira versão não serão implementados:

- Controle via Bluetooth;
- Controle via Wi-Fi;
- Controle remoto RF;
- Interface gráfica;
- Aplicativo para smartphone;
- Detecção de obstáculos;
- Controle de velocidade do motor.

---

# Requisitos Funcionais

## RF-01

O sistema deve permanecer inicialmente no estado **FECHADO**.

## RF-02

O sistema deve permitir iniciar a abertura do portão quando receber o evento START.

## RF-03

O sistema deve permitir iniciar o fechamento do portão quando apropriado.

## RF-04

O sistema deve interromper o movimento quando receber o evento STOP.

## RF-05

O sistema deve detectar o acionamento do sensor de fim de curso aberto.

## RF-06

O sistema deve detectar o acionamento do sensor de fim de curso fechado.

## RF-07

O sistema deve interromper o motor ao atingir qualquer fim de curso.

## RF-08

O sistema deve detectar timeout durante o movimento.

## RF-09

Ao detectar timeout o sistema deve entrar no estado ERRO.

## RF-10

O sistema deve permitir recuperar a operação após a resolução do erro.

---

# Requisitos Não Funcionais

## RNF-01

O firmware deve possuir arquitetura modular.

## RNF-02

A máquina de estados deve permanecer desacoplada da camada de hardware.

## RNF-03

Toda interação com GPIO deverá ocorrer exclusivamente através do módulo de Hardware.

## RNF-04

A lógica principal não deverá utilizar `delay()`.

## RNF-05

A temporização deverá utilizar `millis()`.

## RNF-06

Cada módulo deverá possuir responsabilidade única.

## RNF-07

O código deverá seguir boas práticas de legibilidade e organização.

## RNF-08

O projeto deverá ser compatível com PlatformIO.

## RNF-09

O projeto deverá ser compatível com Wokwi.

## RNF-10

Todo comportamento do sistema deverá estar documentado.

---

# Estados do Sistema

- FECHADO
- ABRINDO
- ABERTO
- FECHANDO
- PARADO
- ERRO

---

# Eventos

- START
- STOP
- FIM_CURSO_ABERTO
- FIM_CURSO_FECHADO
- TIMEOUT
- ERRO

---

# Premissas

- O motor será simulado no Wokwi.
- Os sensores de fim de curso serão simulados.
- Existe apenas um portão.
- O sistema executará um único ciclo de controle.
- O loop principal permanecerá em execução contínua.

---

# Restrições

- ESP32 DevKit
- Framework Arduino
- Linguagem C++
- PlatformIO
- Git
- GitHub
- Wokwi

---

# Critérios de Aceitação

O projeto será considerado concluído quando:

- Todos os estados puderem ser alcançados.
- Todas as transições ocorrerem corretamente.
- Nenhum `delay()` estiver presente na lógica principal.
- Toda interação com hardware ocorrer através da camada Hardware.
- O firmware possuir documentação completa.
- O projeto estiver publicado no GitHub.