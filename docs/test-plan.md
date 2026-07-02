# Test Plan

## Objetivo

Validar o funcionamento do firmware do controle de portão eletrônico, verificando se todos os estados, eventos e ações ocorrem conforme especificado.

---

## Estratégia de Testes

Os testes serão realizados em etapas:

1. Testes unitários dos módulos.
2. Testes da máquina de estados.
3. Testes de integração.
4. Testes funcionais no simulador Wokwi.
5. Testes em hardware real.

---

## Casos de Teste

Serão validados, entre outros:

* Inicialização do sistema.
* Abertura do portão.
* Fechamento do portão.
* Interrupção durante o movimento.
* Inversão de sentido.
* Timeout.
* Estado de erro.
* Retorno ao funcionamento normal.

---

## Critérios de Aceitação

Cada caso de teste será considerado aprovado quando:

* o estado final estiver correto;
* a ação executada corresponder à especificação;
* nenhuma transição inválida ocorrer;
* o comportamento observado corresponder ao definido na máquina de estados.
