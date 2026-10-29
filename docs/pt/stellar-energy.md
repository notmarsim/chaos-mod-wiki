# Stellar Liquefier e Stellar Reactor

A cadeia é **Barra Stellar → Líquido Stellar → sol artificial → FE**. Todos os materiais de construção pertencem ao tier Stellar ou a tiers anteriores. O Stellar Generator continua sendo a opção compacta de geração diurna sem combustível.

## Produção padrão

| Máquina / injeção | Entrada ou combustível | Tempo | Energia |
|---|---|---|---|
| Liquefier | 1 Barra Stellar → 1.000 mB | 1.200 ticks / 60 s | Consome 16.000 FE/t |
| Reactor Low | 1.000 mB | 1.200 ticks / 60 s | Gera 128.000 FE/t |
| Reactor Normal | 1.000 mB | 300 ticks / 15 s | Gera 256.000 FE/t |
| Reactor High | 1.000 mB | 150 ticks / 7,5 s | Gera 512.000 FE/t |

Valores em operação estável a 20 TPS. Um, quatro ou oito Liquefiers sustentam Low, Normal e High, respectivamente. A ignição forma o sol e aumenta a geração gradualmente. O combustível fracionário é pago antes de gerar energia e persiste ao salvar. Com o buffer de saída cheio, o consumo de combustível pausa.

Cada barra custa 19,2 MFE para liquefazer. O Reactor produz 153,6 MFE por balde em Low ou 76,8 MFE em Normal/High, antes dos custos de liquefação e ignição. A expansão horizontal aumenta a potência com uma escolha de eficiência.

## Liquefier 3×3×3

Use 21 revestimentos do Liquefier Stellar, 1 Controller, 1 Liquefaction Core e uma porta de cada tipo: entrada de itens, saída de fluido, entrada de energia e estabilizadores.

1. Monte uma estrutura fechada de 3×3×3, com revestimentos nas arestas e quinas.
2. Coloque o Liquefaction Core no bloco central.
3. Coloque o Controller no centro de uma face vertical, voltado para fora.
4. Coloque as quatro portas nos centros de outras quatro faces. O centro restante é revestimento.
5. Forneça Barras Stellar, FE e Flux Crystals. Abra o Controller e clique em Iniciar.

O tanque armazena 32.000 mB e o buffer energético armazena 64 MFE por padrão. A máquina processa uma barra por vez. Falta de energia, tanque cheio ou chunks necessários descarregados preservam o progresso e a barra reservada. Extraia o fluido pela porta de saída usando tubos compatíveis ou um balde vazio.

## Reactor 11×11×11

A estrutura possui uma carcaça fechada, um Reactor Core central, seis Field Emitters e doze Containment Coils. O restante do interior deve ser ar. O Controller fica no centro de uma face vertical, voltado para fora.

As coordenadas abaixo vão de **0 a 10**, a partir da quina inferior noroeste: X para leste, Y para cima e Z para sul.

| Componente | Coordenada |
|---|---|
| Controller voltado para norte | (5, 5, 0) |
| Entrada de fluido | (0, 5, 5) |
| Saída de energia | (10, 5, 5) |
| Porta de estabilizadores | (5, 5, 10) |
| Reactor Core | (5, 5, 5) |

Todas as arestas e quinas usam Reactor Casing. As demais posições externas podem usar Reactor Casing ou Stellar Reinforced Glass, exceto Controller e portas. Um exemplo com faces envidraçadas usa **116 revestimentos, 482 vidros, 1 Controller e 3 portas**.

Coloque Field Emitters em (1,5,5), (9,5,5), (5,1,5), (5,9,5), (5,5,1) e (5,5,9).

As doze Containment Coils ficam em:

- Plano XY: (1,1,5), (1,9,5), (9,1,5), (9,9,5).
- Plano XZ: (1,5,1), (1,5,9), (9,5,1), (9,5,9).
- Plano YZ: (5,1,1), (5,1,9), (5,9,1), (5,9,9).

É possível girar esse arranjo para outras orientações. O sol permanece centralizado e varia de aproximadamente 2 a 5 blocos de diâmetro conforme a injeção. A renderização não cria blocos no mundo.

## Ignição e automação

Insira Líquido Stellar pela porta de entrada de fluido. Forneça a energia de ignição **ao Controller** e coloque Flux Crystals pela porta de estabilizadores ou pela GUI. Inicie com pelo menos **4.000 mB**, **5 MFE** e um estabilizador compatível disponível. Os 4.000 mB são uma quantidade mínima armazenada, não um custo extra de combustível.

A GUI seleciona Low/Normal/High e mostra geração real em FE/t, consumo de fluido, armazenamento, contenção e estabilidade. A porta de saída energética funciona com os cabos do Chaos e outros sistemas Forge FE. As portas de fluido usam IFluidHandler para tanques e tubos compatíveis. Os cabos de fluidos do Chaos conectam essas portas: configure Puxar junto à saída do Liquefier e Enviar junto à entrada do Reactor.

Ambas as máquinas reutilizam a estabilidade Stellar e os thresholds automáticos dos cristais. Maior injeção aumenta a perda de estabilidade. Em 0%, ocorre a falha normal de máquinas. Progresso, tanques, energia e combustível fracionário persistem ao salvar. Chunks necessários descarregados suspendem a operação sem forçar carregamento.

O botão **?** da GUI resume a construção. O JEI mostra os craftings, a liquefação e os três modos de combustível. Reactor Core e Controller são fabricados na Void Station; os demais componentes usam a crafting table.

## Configuração

Os valores ficam em `serverconfig/chaosmod-stellar-energy.toml` de cada mundo/servidor: tempo, rendimento e consumo da liquefação, tanques, buffers, ignição, formação, exportação e combustível/geração/estabilidade de cada modo. Configure capacidades suficientes para comportar as receitas e a ignição.
