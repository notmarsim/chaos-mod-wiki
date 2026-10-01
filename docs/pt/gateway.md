# Gateway

[English](../en/gateway.md)

O Gateway permite viajar para as Realities. As Rifts apresentam criaturas dimensionais no começo do jogo; um Gateway alimentado permite escolher e revisitar destinos.

## Monte a moldura

Construa um **contorno vertical 5×5** com **interior vazio 3×3**. Coloque o **Gateway Terminal** no centro da base: são **15 Gateway Frame Blocks e um Terminal**.

```text
F F F F F
F . . . F
F . . . F
F . . . F
F F T F F
```
![Gateway](../assets/gateway.png)

A orientação do Terminal define o plano do portal. Mantenha o interior livre e forneça FE ao Terminal. As receitas do Terminal e dos Frames usam a grade grande da Void Station; veja os [desenhos exatos](recipes.md).

## Busque, salve e abra destinos

Um Terminal de ida montado e alimentado busca destinos automaticamente a cada **200 ticks elegíveis** (normalmente dez segundos), com **95% de chance de sucesso** por tentativa. Cada tentativa custa **1 milhão de FE**, inclusive quando falha. Terminais de retorno não fazem buscas. O Terminal suporta até **dez Realities salvas**. Confira tier, tipo de terreno e qualidade dos recursos antes de abrir uma conexão.

O Terminal possui **buffer de 512 milhões de FE** e recebe até **16 milhões de FE por transferência**. A abertura e a manutenção variam pelo tier:

| Destino | Abertura FE | Manutenção FE/t |
|---|---:|---:|
| Unstable | 1.000.000 | 1.000 |
| Stable | 2.000.000 | 2.000 |
| Void | 4.000.000 | 4.000 |
| Stellar | 8.000.000 | 8.000 |
| Duality | 16.000.000 | 16.000 |
| Darklight | 32.000.000 | 32.000 |
| Nova | 64.000.000 | 64.000 |
| Zenith | 128.000.000 | 128.000 |
| Chaos | 256.000.000 | 256.000 |

## Viaje e retorne

Permaneça no portal por aproximadamente **quatro segundos** para viajar, inclusive em destinos já visitados. Sair antes do fim interrompe a carga. A animação indica o progresso; no modo Chaos, o efeito é vermelho.

Após chegar, afaste-se do portal antes de tentar outra viagem. A proteção de chegada impede o retorno imediato enquanto você permanece na área. A geração do destino procura chão de apoio e espaço para sair da moldura, mas confira o entorno antes de se afastar.

## Desbloqueie o modo Chaos

![Chaos](../assets/chaos_gateway.png)

Rotas normais excluem Chaos. Complete a [progressão dos Reality Cores](extractor.md), fabrique um **Chaotic Catalyst** e clique com o botão direito agachado no Terminal usando o item para ativar o modo Chaos.

!!! warning "Prepare a saída"
    Confira a [estabilidade](realities.md) do destino, leve suprimentos e lembre onde está o portal de retorno. Uma Reality colapsada é perdida permanentemente.
