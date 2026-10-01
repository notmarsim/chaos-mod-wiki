# Nova Reactor

[English](../en/reactor.md)

O Nova Reactor transforma combustível em calor e calor em FE. Temperatura, resfriamento e remoção de resíduos fazem parte da operação.

## Monte a estrutura 3×3×3

O topo e a base são camadas sólidas 3×3 de **Machine Casing**. A camada intermediária possui um **Nova Reactor Core** no centro, um **Nova Reactor Controller** no meio de uma face externa e casing ao redor. O core deve ficar no centro.

```text
Topo / base        Meio, exemplo
C C C              C E C
C C C              I R I
C C C              C T C

C = Machine Casing     R = Reactor Core
T = Controller         E = Energy Port
I = Item Port

Item Port e Energy Port podem se permutar
```
![Nova Reactor Layer 1](../assets/nova_reactor_l1.png)
![Nova Reactor Layer 2](../assets/nova_reactor_l2.png)
![Nova Reactor Layer 3](../assets/nova_reactor_l3.png)

A camada intermediária aceita no máximo **uma energy port** e **duas item ports**. Use a porta de energia para FE e portas de itens de entrada/saída para abastecimento e remoção de resíduos. O exemplo utiliza duas portas de itens, uma para cada direção.

## Forneça combustível e refrigerante

| Recurso | Função | Valor interno |
|---|---|---:|
| Blackhole | Combustível | 10.000 unidades por item |
| Nova Bar | Refrigerante | 5.000 unidades por item |
| Exotic Dust | Resíduo extraído | Remove 5.000 unidades de waste por item |

Uma item port em modo de entrada recebe os materiais; em modo de saída, disponibiliza resíduos. Combustível e refrigerante comportam até 1.000.000 de unidades cada; waste comporta 100.000. Durante a queima, são consumidas 20 unidades de combustível/t e produzidas 10 unidades de waste/t. O consumo de refrigerante aumenta com o calor do core.

## Controle a temperatura

O pico é **4.000.000 FE/t**, com temperatura ideal de **25.000 K**. A geração também depende do estado e da calibração do reator. O buffer armazena **10 bilhões de FE**.

Use [regras do Controller](controller.md) para limitar Core Heat, Casing Heat e Waste, além de exigir reservas de Fuel e Coolant. Um planejamento inicial é parar a queima antes da faixa perigosa e retomar somente após resfriar. Acompanhe as duas temperaturas; a quantidade de FE armazenada não é um indicador de segurança.

!!! danger "Meltdown"
    A partir de **45.000 K no core**, o reator pode sofrer um meltdown destrutivo. Não use esse valor como limite normal de desligamento. Interromper a queima não elimina imediatamente o calor acumulado.

O resfriamento e a geração pelo calor residual continuam quando o controle pausa a reação. Mantenha a saída de energia e a extração de resíduos disponíveis e confira o abastecimento antes de deixar a instalação funcionando.
