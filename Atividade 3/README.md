# Semana 3 - Sinais analógicos e digitais

## Links
- Wokwi: https://wokwi.com/projects/481593523157395457
- Planilha: https://docs.google.com/spreadsheets/d/1NNtWgG6NgmsHvQUFLNMYZOiKPxiI8vcvIhcQkmhpI5A/edit?usp=sharing

## Tabela de E/S

| Tag | Endereço | Tipo | Faixa | Função |
|---|---|---|---|---|
| BTN | D2 | DI | HIGH / LOW | Comando do operador. Nesta atividade só é lido e mostrado no Serial (0 = solto, 1 = apertado) |
| POT | A0 | AI | 0 a 1023 | Faz o papel do sensor de temperatura na simulação |
| LED_ALARME | D8 | DO | OFF / ON | Acende quando a temperatura chega em PERIGO (85% ou mais) |
| pct | memória | REAL | 0 a 100 % | Leitura convertida em percentual, usada para decidir o estado |

## Casos de teste

### Aquisição (Wokwi)

| Caso | Entrada | Esperado | Obtido | Situação |
|---|---|---|---|---|
| A1 | Botão solto, pot no mínimo | botão 0; bruto perto de 0 | botão 0; bruto 0; NORMAL | OK |
| A2 | Botão apertado, pot no mínimo | botão 1; bruto perto de 0 | botão 1; bruto 0; NORMAL | OK |
| A3 | Botão solto, pot no meio | botão 0; bruto perto de 512 | botão 0; bruto 500 (48,88%); NORMAL | OK |
| A4 | Botão solto, pot no máximo | botão 0; bruto 1023; PERIGO | botão 0; bruto 1023 (100%); PERIGO | OK |

### Limites dos estados (planilha, aba Fronteira)

| Percentual | Esperado | Obtido | Situação |
|---|---|---|---|
| -1 | FALHA | FALHA | OK |
| 0 | NORMAL | NORMAL | OK |
| 69,99 | NORMAL | NORMAL | OK |
| 70 | ATENÇÃO | ATENÇÃO | OK |
| 84,99 | ATENÇÃO | ATENÇÃO | OK |
| 85 | PERIGO | PERIGO | OK |
| 100 | PERIGO | PERIGO | OK |
| 100,01 | FALHA | FALHA | OK |

O potenciômetro nunca sai da faixa de 0 a 1023, então os casos de FALHA foram testados só na planilha.

## Gráfico

O gráfico mostra a temperatura de cada uma das 10 amostras que coletei girando o potenciômetro aos poucos. A temperatura vai subindo de 0 °C até 92,57 °C. Até a amostra 7 (64,03 °C) fica em NORMAL, nas amostras 8 e 9 (72,63 °C e 80,74 °C) entra em ATENÇÃO, e na amostra 10 (92,57 °C) chega em PERIGO e o LED acende.

## Conclusão

O circuito leu o botão e o potenciômetro certinho, e a conversão do valor bruto para percentual e °C deu os mesmos números no Arduino e na planilha. Os estados mudaram nos limites certos (70% e 85%) e a FALHA foi testada antes dos outros estados. Tudo isso foi feito em simulação, então não testa coisas do mundo real, como ruído elétrico, cabo comprido, precisão de um sensor de verdade ou a corrente real de um transmissor 4-20 mA.
