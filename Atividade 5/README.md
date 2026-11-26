# Semana 5 - Sistemas sequenciais e GRAFCET

## Objetivo
Modelar um semáforo veicular com GRAFCET e validar a sequência com testes de tempo.

## Convenções usadas
- Etapa inicial: quadrado com borda dupla
- Etapa: quadrado numerado
- Ação: retângulo ligado à etapa
- Transição: barra horizontal entre duas etapas
- Receptividade: texto ao lado da barra. `t(E0) ≥ 8 s` quer dizer "a etapa 0 está ativa há 8 segundos ou mais"

## Arquivos
- Fonte editável: `grafcet.drawio`
- Imagem: `semaforo.png`
- Testes: `casos_teste.csv`

## GRAFCET do semáforo
<img width="269" height="530" alt="grafcet drawio" src="https://github.com/user-attachments/assets/e410d7ad-00ab-40f4-b30f-34a8a5afdeaf" />


### Etapas e ações
| Etapa | Situação | Ação |
|---|---|---|
| E0 (inicial) | vermelho | VERMELHO = 1 |
| E1 | verde | VERDE = 1 |
| E2 | amarelo | AMARELO = 1 |

### Transições
| Transição | De → para | Receptividade |
|---|---|---|
| T0 | E0 → E1 | t(E0) ≥ 8 s |
| T1 | E1 → E2 | t(E1) ≥ 10 s |
| T2 | E2 → E0 | t(E2) ≥ 3 s |

## Testes
Testei cada tempo logo antes e exatamente no limite, porque é nesses pontos que aparece erro de comparação (usar > no lugar de ≥).

| Caso | Etapa atual | Tempo | Esperado | Obtido | Situação |
|---|---|---|---|---|---|
| S01 | E0 | 7,9 s | permanece E0 | permanece E0 | PASSA |
| S02 | E0 | 8,0 s | ativa E1 | ativa E1 | PASSA |
| S03 | E1 | 9,9 s | permanece E1 | permanece E1 | PASSA |
| S04 | E1 | 10,0 s | ativa E2 | ativa E2 | PASSA |
| S05 | E2 | 2,9 s | permanece E2 | permanece E2 | PASSA |
| S06 | E2 | 3,0 s | retorna E0 | retorna E0 | PASSA |

### Percurso manual de um ciclo
| Passo | Etapa ativa | Evento | Transição | Nova etapa |
|---|---|---|---|---|
| 0 | E0 | tempo < 8 s | nenhuma | E0 |
| 1 | E0 | tempo = 8 s | T0 | E1 |
| 2 | E1 | tempo = 10 s | T1 | E2 |
| 3 | E2 | tempo = 3 s | T2 | E0 |

Um ciclo completo dura 8 + 10 + 3 = 21 segundos.

## Decisões
- O semáforo começa no vermelho porque é a situação mais segura ao ligar o sistema.
- Cada etapa liga só uma cor, então nunca acendem duas cores ao mesmo tempo.
- O tempo de cada etapa começa a contar quando ela fica ativa.

## Limitações
O desenho e os testes foram feitos no papel (percurso manual), sem rodar em um CLP. Não foram modelados botão de pedestre, modo noturno (amarelo piscando) nem falha de lâmpada.
