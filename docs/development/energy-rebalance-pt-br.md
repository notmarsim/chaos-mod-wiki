> **Atualização do Nova Reactor:** as regras de montagem interna, termodinâmica e UI abaixo foram substituídas pelo [guia modular](nova-reactor-modular-pt-br.md). As receitas e as outras máquinas continuam conforme este relatório.

# Rebalanceamento energético e Nova Reactor escalável

## Geração

Valores antes da calibração, que permanece aplicada às mesmas máquinas.

| Fonte | Produção | Buffer | Exportação nominal |
|---|---:|---:|---:|
| Basic Coal Generator | 32.000 FE por Coal/Charcoal | 128.000 FE | 2.000 FE |
| Void Coal Generator | 256.000 FE por Shadow Infused Coal | 4.000.000 FE | 16.000 FE |
| Stellar Generator | 64.000 FE/t | 20.000.000 FE | 64.000 FE |
| Darklight Generator | até 1.000.000 FE/t | 100.000.000 FE | 1.000.000 FE |
| Nova Reactor | 16–640 milhões FE/t de pico | 10–400 bilhões FE | 10 bilhões FE |
| Chaos Cube | 10 quadrilhões FE/t, inalterado | 9 quintilhões FE, inalterado | inalterada |

Basic/Void conservam o consumo passivo de 1/2 FE por tick. Stellar continua exigindo dia, céu aberto e ausência de chuva/trovão. Darklight usa `floor(1.000.000 × ((15 − luz)/15)²)`, com luz limitada a 0–15.

## Nova Reactor: construção

O controller pode ficar em qualquer posição interna de uma das seis faces externas, sem ocupar arestas ou cantos; não precisa estar centralizado. O Reactor Core permanece no centro geométrico. Preencha todas as faces, arestas e cantos com Machine Casing. Nas faces, substitua casings por ports conforme necessário; os ports não podem ocupar arestas dos reatores ampliados. A disposição antiga 3×3×3, inclusive seus ports na camada intermediária, continua aceita.

| Tamanho externo | Pico FE/t | Fuel Rods para o pico | Coolant Channels para refrigeração completa | Control Rods mínimos para o pico |
|---|---:|---:|---:|---:|
| 3×3×3 | 16.000.000 | integrado ao Core | integrado ao Core | integrado ao Core |
| 5×5×5 | 48.000.000 | 3 | 3 | 1 |
| 7×7×7 | 112.000.000 | 7 | 7 | 1 |
| 9×9×9 | 224.000.000 | 14 | 14 | 2 |
| 11×11×11 | 400.000.000 | 25 | 25 | 4 |
| 13×13×13 | 640.000.000 | 40 | 40 | 5 |

Nos tamanhos maiores, coloque os novos componentes em posições livres do interior, sem substituir o Core central. A primeira implementação conta os componentes; não exige colunas contíguas nem circuitos de fluido. Cada Fuel Rod suporta uma unidade de potência; cada Control Rod suporta oito. Um canal refrigera uma unidade em potência máxima. Componentes excedentes não ultrapassam o limite do tamanho.

Pelo menos um componente de cada tipo é obrigatório nos reatores maiores. Com menos componentes que os da tabela, o reator pode formar, mas sua potência ou refrigeração fica limitada. Shells incompletos, interiores incompatíveis e múltiplos controllers na mesma estrutura são rejeitados. Ports são associados apenas a uma estrutura validada.

Receitas na Void Station, conforme as imagens fornecidas:

| Componente | Machine Casing | Nova Bar | Nova Capacitor | Nova Circuit | Nova Rod |
|---|---:|---:|---:|---:|---:|
| Control Rod | 20 | 10 | 4 | 1 | 0 |
| Fuel Rod | 20 | 10 | 0 | 0 | 5 |
| Coolant Channel | 20 | 12 | 3 | 0 | 0 |

As três receitas ocupam um retângulo de 5×7 na grade 9×9, com moldura de casings. Receitas, loot, tags, blockstates e modelos de inventário são mantidos pelo datagen. As malhas ficam em `src/main/resources/assets/chaosmod/models/block/machines/nova_*.json` e podem ser importadas no Blockbench.

## Guia de montagem dos tamanhos maiores

| Exterior | Interior livre | Posição do Core (contando a partir de 1) | Blocos da casca | Casings usando 1 controller + 3 ports |
|---|---|---|---:|---:|
| 5×5×5 | 3×3×3 | (3, 3, 3) | 98 | 94 |
| 7×7×7 | 5×5×5 | (4, 4, 4) | 218 | 214 |
| 9×9×9 | 7×7×7 | (5, 5, 5) | 386 | 382 |
| 11×11×11 | 9×9×9 | (6, 6, 6) | 602 | 598 |
| 13×13×13 | 11×11×11 | (7, 7, 7) | 866 | 862 |

1. Faça um cubo oco com paredes, piso e teto de uma camada de Machine Casing. As dimensões da tabela são externas.
2. Deixe uma abertura temporária para entrar e coloque exatamente um Nova Reactor Core no centro geométrico.
3. Distribua Fuel Rods, Coolant Channels e Control Rods pelo interior usando as quantidades da tabela de potência. Não precisam estar conectados nem em colunas específicas. O restante deve ficar vazio; não use blocos decorativos no interior.
4. Substitua casings da casca por um controller e os ports. Eles podem estar nas seis faces, fora do centro, mas não em arestas ou cantos. Apenas um controller é permitido. Feche toda abertura restante.
5. Recomendação: um Item Port em INPUT recebe Black Hole (combustível) e Nova Bar (coolant); outro Item Port em OUTPUT retira Exotic Dust; um Energy Port conecta à rede de energia. Use o Configurator no Item Port para alternar INPUT/OUTPUT.
6. Abra o controller, confirme a estrutura formada, abasteça combustível/coolant, mantenha a saída de waste livre e ajuste a potência. O valor da tabela é um pico térmico: ter todos os canais e combustível não elimina o risco de sobreaquecimento.

Exemplo de face externa 5×5 (C = casing, X = controller ou port):

```text
CCCCC
CXXXC
CXXXC
CXXXC
CCCCC
```

X indica posições permitidas, não a obrigação de preenchê-las com ports. Toda posição não usada por controller/port continua sendo casing. A quarta imagem enviada pelo usuário é uma posição válida de controller.

## Operação, calor e persistência

A interface existente ganhou um painel com tamanho, estado de formação, geração atual/máxima, potência configurada/efetiva, eficiência térmica, temperaturas, combustível, coolant, waste e risco térmico. Os botões `−`/`+` ajustam a potência em passos de 5%, entre 0 e 100%, com validação no servidor.

A unidade de potência representa 16 milhões FE/t no pico, 20 unidades de combustível/t e 10 de waste/t. O consumo de coolant é `(10 + floor(coreHeat/500)) × unidades ativas`. Combustível, waste e coolant crescem proporcionalmente à potência usada. Buffers internos crescem conforme o tamanho, até 40× os buffers antigos.

A geração depende da temperatura da carcaça, waste, calibração e potência dos componentes. A temperatura ideal continua 25.000 K. Refrigeração parcial reduz o resfriamento; sem refrigeração suficiente, a temperatura sobe até a região de meltdown. A massa térmica acompanha os componentes produtivos: reatores maiores não recebem calor gratuitamente nem obtêm eficiência energética superior apenas pelo tamanho.

Desligar ou selecionar 0% interrompe a queima. O calor residual continua produzindo, com redução gradual da potência residual. A probabilidade de meltdown continua ligada ao calor real: começa em 45.000 K, chegando a 5% por tick em 50.000 K. A transformação do terreno e explosão existentes foram mantidas.

Estruturas usam snapshots em cache. Mudanças normais de construção invalidam imediatamente; checagens incrementais de até 128 posições/tick cobrem alterações por comandos/outros mods sem varrer o reator inteiro a cada tick. Estruturas inválidas são procuradas novamente a cada 40 ticks. Nenhuma validação força carregamento de chunks. Um reator deve permanecer carregado para funcionar.

NBT antigo é aceito com tamanho 3 e potência 100%. Configuração, temperaturas, recursos e energia continuam persistidos. Desmontar/recolocar ou reduzir o tamanho preserva a energia armazenada: temporariamente o buffer pode permanecer acima da capacidade nominal da estrutura menor. Itens dos slots são dropados apenas uma vez, sem também serem copiados no NBT do controller.

## Zenith Accelerator — atualização solicitada posteriormente

Zenith Bar e Zenith consomem **a mesma quantidade de itens por ciclo**. O bônus da barra é **15× em FE e condensate**, aplicado também às cargas intermediárias.

| Material | Recuperação a 100% | Condensate a 100% | Ganho líquido por ciclo de 380 ticks |
|---|---:|---:|---:|
| Zenith | 8.000.000.000 FE | 32 | 4.960.000.000 FE |
| Zenith Bar | 120.000.000.000 FE | 480 | 116.960.000.000 FE |

Consumo ativo: 8.000.000 FE/t; ciclo de referência: 3.040.000.000 FE. Material durante aceleração: `0,1 + 0,1 × carga` itens/t para ambos; permanece o acumulador fracionário atual. Tempos, movimento, carga, validação do anel, condição de colisão e armazenamento de 100 trilhões FE do Rod foram preservados. A entrega de condensate continua dividida em stacks, e a energia respeita o espaço disponível. O pedido mais recente substitui os valores anteriores de 24 bilhões FE/80 condensate para barras.

## Infraestrutura

Entradas de refiners/extractors agora têm margem de 6× o consumo base: suficiente para `3^1,5` de Speed e a penalidade máxima de 12% da calibração. Fórmulas dos módulos, Module Power, três slots, consumo base, tempos e buffers permanecem iguais.

| Tier | Entrada Refiner FE/chamada | Entrada Extractor FE/chamada |
|---|---:|---:|
| Basic | 3.000 | 12.000 |
| Void | 24.000 | 96.000 |
| Stellar | 192.000 | — |
| Darklight | 1.536.000 | 6.000.000 |
| Nova | 12.000.000 | 48.000.000 |
| Zenith | 96.000.000 | 384.000.000 |
| Chaotic | 768.000.000 | 3.072.000.000 |

A Basic Battery sobe de 10.000 para 12.000 FE por transferência para atender o Basic Extractor. Nenhuma capacidade de bateria foi alterada.

Cabos mantêm a capacidade por segmento. A rede pode encaminhar energia diretamente da fonte ao consumidor, com orçamento agregado de 12× sua capacidade por tick, sem exigir que toda a energia passe primeiro pelo buffer. Prioridades e configuração de lados são mantidas. Simulações reservam os limites dos consumidores/fontes durante a passagem, evitando multiplicar transferências por repetir o loop. FE inesperadamente recusado por um consumidor é retido, nunca descartado.

As transferências internas usam `long`; a API Forge externa continua recebendo partes limitadas a `Integer.MAX_VALUE`. O limite por chamada de um mod externo ainda depende daquele mod. Valores negativos são rejeitados, somas de capacidade de redes são saturadas e leitura de energia persistida é limitada ao armazenamento.

## Arquivos principais

- `energy/ProgressionEnergy.java`: constantes, curva Darklight, entradas e Accelerator.
- `energy/BigEnergyStorage.java`, `energy/network/EnergyNetwork.java`: segurança numérica e transporte.
- `block/entity/*GeneratorEntity.java`: Basic, Void, Stellar e Darklight.
- `block/entity/*RefinerEntity.java`, `AbstractExtractorEntity.java`, `BasicBatteryEntity.java`: entradas.
- `block/entity/NovaReactorControllerEntity.java`, `NovaReactorPortEntity.java`: operação, recursos e ports.
- Novos `block/entity/NovaReactorStructure.java` e `NovaReactorUpdates.java`: validação/cache/invalidação.
- `screen/NovaReactorMenu.java`, `NovaReactorScreen.java`, `machine/MachineMetric.java`: interface, sincronização de inteiros completos e controle por percentuais.
- `block/ModBlocks.java`, `item/ModItems.java`, `ModCreativeModTabs.java`, `block/custom/NovaReactorControllerBlock.java`: registros e persistência ao quebrar.
- `datagen/ModRecipeProvider.java`, `ModBlockStateProvider.java`, `ModBlockTagGenerator.java`, `datagen/loot/ModBlockLootTables.java`: dados regeneráveis.
- `assets/chaosmod/lang/en_us.json`, `pt_br.json`: nomes dos componentes.
- Novas malhas `nova_fuel_rod.json`, `nova_coolant_channel.json`, `nova_control_rod.json`.
- `machine/EnergyRebalanceGameTests.java`, `tools/PreviewNovaReactorModels.java`: testes e prévia.

## Validação

Resultado final: **120 GameTests obrigatórios passaram**, `runData` e `build` concluídos. O verificador do Accelerator passou em **10.393 checagens** de layout, orientação, ciclos, retomada/interrupção e geometria. Os três modelos novos passaram no parser nativo do Minecraft e na validação de UVs, texturas e referências geradas.

Os resultados finais das execuções ficam em `build/energy-rebalance-tests.log`, `build/energy-rebalance-datagen.log` e `build/energy-rebalance-build.log`. A prévia validada fica em `build/reports/machines/nova-internals.png`.

A inspeção visual foi feita por renderização das malhas reais e texturas. Não substitui uma sessão interativa com shaders/resource packs. A explosão de meltdown de raio 45 não foi disparada nos testes, para não interferir nas outras estruturas; a lógica destrutiva existente não foi alterada. Integrações externas específicas e dois clientes reais simultâneos não foram executados nesta sessão.
