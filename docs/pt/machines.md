# Máquinas, módulos e estabilidade

[English](../en/machines.md)

Refiners processam receitas de itens; extratores de recursos possuem uma progressão de máquinas separada. O **Reality Extractor** é outro equipamento: ele coleta amostras de núcleos dimensionais e possui um [guia próprio](extractor.md).

## Três tipos de módulos

Máquinas compatíveis possuem **três slots compartilhados de upgrade**. Cada slot recebe um módulo. Módulos idênticos podem ocupar slots diferentes e suas potências são somadas, até o limite normal de cobertura. Os itens continuam não empilháveis; também é possível misturar tipos e tiers.

| Família | Efeito com cobertura completa |
|---|---|
| Stability | 75% menos perda de estabilidade |
| Speed | Até 4× a velocidade de trabalho |
| Energy | 75% menos FE/t |

A cobertura é a soma da potência daquela família dividida pela demanda do tier da máquina, limitada a 100%. Potência e demanda por tier: **Basic 1, Void 2, Stellar 2, Darklight 4, Nova 8, Zenith 16 e Chaotic 32**. Por exemplo, um Basic Energy Module numa máquina Darklight fornece 25% de cobertura, reduzindo o custo base de energia em 18,75%.

Os slots são compartilhados: preencher os três com velocidade não deixa espaço para energia ou estabilidade. Escolha um equilíbrio que sua geração e manutenção consigam sustentar.

## Mantenha a estabilidade

O slot de manutenção com Flux Crystal é separado dos upgrades. A recuperação por cristal diminui nos tiers superiores:

| Tier da máquina | Recuperação por Flux Crystal | Recuperação passiva, do vazio ao máximo |
|---|---:|---:|
| Basic | 25 pontos percentuais | 5 minutos |
| Void | 15 | 10 minutos |
| Stellar | 10 | 20 minutos |
| Darklight | 5 | 35 minutos |
| Nova | 2 | 60 minutos |
| Zenith | 1 | 90 minutos |
| Chaotic | 0,5 | 120 minutos |

Os tempos passivos pressupõem máquinas carregadas e 20 TPS. Pausar máquinas compatíveis não interrompe a recuperação passiva.

!!! danger "Stability em 0"
    Se a estabilidade chegar a zero, a máquina explode

**Próximo passo:** configure uma [regra para Stability](controller.md) e melhore a [calibração](controller.md) para reduzir penalidades de operação.
