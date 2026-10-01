# Realities e estabilidade dimensional

[English](../en/realities.md)

Cada Reality possui tier, perfil de terreno, qualidade de recursos e estabilidade persistente. Os biomas combinam blocos vanilla com blocos próprios do tier; a geração atual possui **18 blocos decorativos por tier** e **4–10 biomas por Reality**.

## Terreno e qualidade dos recursos

Os perfis incluem **Normal, Islands, Cavernous, Shattered, Abyss e Sky Islands**. Cavernous possui teto fechado; Abyss enfatiza poços verticais profundos e camadas inferiores cada vez mais hostis. Sky Islands contém porções de terreno suspensas sobre o vazio, com gravidade reduzida. Leve blocos e uma forma de lidar com quedas.

O scanner informa qualidade **DEPLETED, LOW, NORMAL, RICH ou ABUNDANT**. Isso altera a geração de minérios e depósitos exclusivos. Realities DEPLETED ainda podem ter criaturas e um core extraível.

Mudanças de geração valem para chunks novos. Terreno já explorado não é reescrito quando o gerador é alterado.

## A estabilidade é um recurso limitado

Realities começam com **100% de estabilidade**. Não há regeneração passiva, e permanecer parado dentro não consome estabilidade. Ações na Reality têm custos:

| Ação | Custo de estabilidade |
|---|----------------------:|
| Extrair uma amostra do core da Reality |                   50% |
| Remover um depósito exclusivo |                    5% |
| Minerar um bloco de minério |                    1% |
| Quebrar outro bloco comum |                  0,5% |
| Morte de um mob |                 0,25% |

Os efeitos de instabilidade aumentam ao passar por 75%, 50% e 25%. Combates contínuos ou grandes escavações podem esgotar uma região mesmo sem extrair o core.

## Colapso

!!! danger "Zero inicia um colapso de 30 segundos"
    Ao chegar a zero, começa uma **contagem de 600 ticks**. Saia pelo portal de retorno antes do fim. Jogadores que permanecerem morrem e dropam o inventário **mesmo com keepInventory ativado**. A região colapsada fica permanentemente indisponível.

A contagem é persistente e pode avançar enquanto o servidor está funcionando, mesmo com a região descarregada. Deslogar ou descarregar chunks não restaura a Reality. O portal de saída continua utilizável durante a contagem.

**Próximo passo:** planeje a [extração do core](extractor.md) antes de gastar a reserva de mineração.
