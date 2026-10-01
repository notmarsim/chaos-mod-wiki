# Energia e automação

[English](../en/energy.md)

## Valores

Valores base, antes de calibração e upgrades:

| Tier | Refiner FE/t | Ciclo base do refiner | Extrator de recursos FE/t |
|---|---:|---:|---:|
| Basic | 500 | 300 ticks | 2.000 |
| Void | 4.000 | 150 ticks | 16.000 |
| Stellar | 32.000 | 100 ticks | — |
| Darklight | 256.000 | 60 ticks | 1.000.000 |
| Nova | 2.000.000 | 20 ticks | 8.000.000 |
| Zenith | 16.000.000 | 10 ticks | 64.000.000 |
| Chaotic | 128.000.000 | 5 ticks | 512.000.000 |

| Fonte | Saída base |
|---|---:|
| Basic Coal Generator | Exporta 250 FE/t |
| Void Coal Generator | Exporta 2.000 FE/t |
| Geração Stellar | Até 8.000 FE/t nas condições solares adequadas |
| Geração Darklight | Até 64.000 FE/t na escuridão |
| Nova Reactor | Até 4.000.000 FE/t na temperatura ideal |

Como referência, um Zenith Refiner sem modificadores exige o pico de quatro Nova Reactors. Um Zenith Extractor exige dezesseis. Considere condições imperfeitas e outras cargas, em vez de depender do máximo teórico de todos os geradores.

## Conecte a rede

Use cabos de energia para conectar geradores, armazenamento e consumidores. Cabos de transporte de itens movem inventários e têm outra função. As famílias atuais de cabos são Basic, Void, Darklight, Nova, Zenith e Chaos.

A interação agachado com o Configurator configura faces dos blocos compatíveis. Confira as conexões quando um bloco próximo não recebe energia. Um Machine Controller ligado à rede encontra máquinas compatíveis em chunks carregados; ele não carrega o mundo apenas para procurá-las.

Defina a prioridade dos consumidores como **HIGH**, **NORMAL** ou **LOW** no Controller. A rede atende primeiro as maiores prioridades e divide a energia dentro de cada prioridade. Transferências diretas entre blocos seguem a própria lógica.

## Mantenha uma área carregada

O **Dimensional Anchor** é um carregador de chunks alimentado por energia, diferente do Reality Anchor vivo das Breaches. Configure-o pelo Controller. Ele suporta áreas quadradas de **3×3 até 10×10 chunks**, consumindo `1.000 × lado³ FE/t`: de 27.000 FE/t em 3×3 a 1.000.000 FE/t em 10×10. Seu buffer armazena 256 milhões de FE.

Ele libera o carregamento quando fica sem energia, é desligado, removido ou está em uma Reality indisponível. Carregar chunks não impede o colapso de uma Reality.

**Próximo passo:** configure [regras automáticas independentes](controller.md) e a [manutenção das máquinas](machines.md).
