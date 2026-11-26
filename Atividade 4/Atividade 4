# Semana 4 - Sistemas discretos e lógica combinacional

## Links
- CircuitVerse: https://circuitverse.org/users/488823/projects/semana-4-logica-combinacional-99d9a21d-7971-4988-a2d4-7758fc34c97e/simulator/embed
- PLC Simulator Online (Ladder): https://app.plcsimulator.online/dFM90vVuVBx8ReBwQkZl


## Variáveis e convenção
Em todas as variáveis, 1 = verdadeiro/ativo e 0 = falso/inativo.

| Variável | Significado (quando vale 1) |
|---|---|
| S | botão de partida apertado |
| G | proteção da máquina fechada |
| E | emergência acionada |
| M | motor ligado |
| T | temperatura alta |
| P | pressão alta |
| MNT | manutenção ativa |
| CA / CB | comando do motor A / motor B |
| MA / MB | motor A / motor B ligado |

## Expressões usadas
- Partida: `M = S · G · ¬E`
- Alarme: `ALARME = (T + P) · ¬MNT`
- Intertravamento (prioridade do A): `MA = CA` e `MB = CB · ¬CA`
- Com emergência: `MA = CA · ¬E` e `MB = CB · ¬CA · ¬E`

## Testes

### Portas básicas
| Porta | Entradas | Esperado | Obtido | Situação |
|---|---|---|---|---|
| AND | A=1, B=1 | 1 | 1 | OK |
| AND | A=1, B=0 | 0 | 0 | OK |
| OR | A=1, B=0 | 1 | 1 | OK |
| OR | A=0, B=0 | 0 | 0 | OK |
| NOT | A=0 | 1 | 1 | OK |
| NOT | A=1 | 0 | 0 | OK |

### Partida: M = S · G · ¬E (CircuitVerse x Ladder)
| S | G | E | Esperado | CircuitVerse | Ladder | Situação |
|---|---|---|---|---|---|---|
| 0 | 0 | 0 | 0 | 0 | 0 | OK |
| 0 | 0 | 1 | 0 | 0 | 0 | OK |
| 0 | 1 | 0 | 0 | 0 | 0 | OK |
| 0 | 1 | 1 | 0 | 0 | 0 | OK |
| 1 | 0 | 0 | 0 | 0 | 0 | OK |
| 1 | 0 | 1 | 0 | 0 | 0 | OK |
| 1 | 1 | 0 | 1 | 1 | 1 | OK |
| 1 | 1 | 1 | 0 | 0 | 0 | OK |

Só uma das oito combinações liga o motor. As duas ferramentas deram o mesmo resultado em todas as linhas.

### Alarme: ALARME = (T + P) · ¬MNT
| T | P | MNT | Esperado | Obtido | Situação |
|---|---|---|---|---|---|
| 0 | 0 | 0 | 0 | 0 | OK |
| 0 | 0 | 1 | 0 | 0 | OK |
| 0 | 1 | 0 | 1 | 1 | OK |
| 0 | 1 | 1 | 0 | 0 | OK |
| 1 | 0 | 0 | 1 | 1 | OK |
| 1 | 0 | 1 | 0 | 0 | OK |
| 1 | 1 | 0 | 1 | 1 | OK |
| 1 | 1 | 1 | 0 | 0 | OK |

Com a manutenção ativa o alarme nunca toca, para não disparar à toa enquanto o técnico mexe na máquina.

### Intertravamento (prioridade do A)
| CA | CB | MA | MB | Situação |
|---|---|---|---|---|
| 0 | 0 | 0 | 0 | OK |
| 1 | 0 | 1 | 0 | OK |
| 0 | 1 | 0 | 1 | OK |
| 1 | 1 | 1 | 0 | OK |

Escolhi dar prioridade ao motor A: se os dois comandos forem apertados juntos, só o A liga. Em nenhum caso MA e MB ficaram ligados ao mesmo tempo (MA · MB = 0).

### Emergência geral
| CA | CB | E | MA | MB | Situação |
|---|---|---|---|---|---|
| 1 | 0 | 0 | 1 | 0 | OK |
| 0 | 1 | 0 | 0 | 1 | OK |
| 1 | 1 | 0 | 1 | 0 | OK |
| 0 | 1 | 1 | 0 | 0 | OK |
| 1 | 1 | 1 | 0 | 0 | OK |

Com E=1 os dois motores desligam, não importa quais comandos estejam apertados.

## Erro encontrado e corrigido
Na primeira montagem da emergência, o ¬E foi ligado só no motor A. No teste CA=0, CB=1, E=1 o motor B continuava ligado com a emergência acionada. Corrigi colocando o ¬E também no AND do motor B.

## Prints
![Portas básicas](01-portas-basicas.png)
![Partida ligada](02-partida-ligada.png)
![Partida com emergência](03-partida-emergencia.png)
![Alarme ligado](04-alarme-ligado.png)
![Alarme em manutenção](05-alarme-manutencao.png)
![Intertravamento](06-intertravamento-conflito.png)
![Emergência geral](07-emergencia-geral.png)
![Ladder ligado](08-ladder-ligado.png)
![Ladder com emergência](09-ladder-emergencia.png)

## Conclusão
As portas AND, OR e NOT funcionaram como esperado e todas as expressões bateram com as tabelas-verdade. A partida deu o mesmo resultado no CircuitVerse e no Ladder. O intertravamento nunca deixou os dois motores ligados juntos, e a emergência desligou tudo em todos os casos. Isso foi só simulação, então não garante segurança de verdade: numa máquina real a emergência precisa de hardware próprio (relé de segurança), e coisas como fio rompido, contato colado ou falha no CLP não aparecem aqui.
