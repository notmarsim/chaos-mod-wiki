# Auditoria da Chaos Anvil antes da migração para Chaos Forge

Auditoria feita em 6 de novembro de 2026, sobre o código e os recursos locais. Este documento registra o comportamento anterior para que a substituição pelo multibloco não altere silenciosamente os custos, os ingredientes, o NBT ou o ritmo das receitas.

## Comportamento real encontrado

A Chaos Anvil é um **crafting instantâneo por clique**, e não uma máquina com progresso temporizado. `ChaosAnvilMenu.forgeItem` verifica o servidor, a validade do menu, o controle da máquina, a receita selecionada, os materiais e o slot de saída. Quando as condições são atendidas, consome **5.000.000.000.000.000.000 FE** e todos os ingredientes, coloca o resultado no slot 0 e toca `ANVIL_USE` com pitch 0,1, na mesma ação do servidor.

Não existe campo de duração, trabalho por tick, FE/t de processamento, receita ativa ou operação pendente. O tick da BlockEntity apenas sincroniza energia a cada 20 ticks. Não há processamento paralelo, repetição automática, Stability, estabilizador ou slots de módulos.

Os ingredientes são consumidos dos **36 slots principais do inventário do jogador**, incluindo a hotbar. Armadura e offhand não entram na alocação. Cada requisito pode usar várias stacks, respeitando `Ingredient.test`, e o plano evita contar o mesmo item duas vezes. A alocação acontece antes de qualquer consumo; um requisito incompleto não causa consumo parcial.

**Implicação para as três fases:** preservar o tempo real atual significa manter a conclusão atômica instantânea. As fases podem orientar os efeitos visuais sem adiar a entrega nem bloquear o próximo craft. Acrescentar, por exemplo, 100 ou 200 ticks por receita mudaria o balanceamento e depende de aprovação explícita. Não há duração antiga a dividir entre três intervalos positivos.

## Receitas herdadas — 11 receitas

Todas são emitidas pelo datagen `ModMachineRecipeData.anvil`, com os IDs abaixo e serializer `chaosmod:chaos_anvil`. Cada resultado é uma unidade, sem NBT de resultado explícito nos JSONs atuais. Todas exigem o mesmo custo de **5 × 10¹⁸ FE**, definido na BlockEntity, e têm conclusão instantânea.

| ID/result (`chaosmod:`) | Chaos Bars | Chaos Orbs | Chaotic Cores | Reality Cores Chaos completos | Extra |
| --- | ---: | ---: | ---: | ---: | --- |
| `chaos_axe` | 70 | 30 | 35 | 7 | — |
| `chaos_boots` | 50 | 30 | 35 | 5 | — |
| `chaos_bow` | 25 | 15 | 30 | 3 | 9 × `minecraft:string` |
| `chaos_chestplate` | 110 | 70 | 50 | 11 | — |
| `chaos_helmet` | 80 | 50 | 40 | 8 | — |
| `chaos_hoe` | 60 | 30 | 35 | 5 | — |
| `chaos_leggings` | 100 | 65 | 55 | 10 | — |
| `chaos_pickaxe` | 65 | 35 | 40 | 6 | — |
| `chaos_shield` | 50 | 25 | 35 | 5 | — |
| `chaos_shovel` | 50 | 25 | 30 | 5 | — |
| `chaos_sword` | 50 | 30 | 45 | 5 | — |

Os nomes históricos dos campos JSON são potencialmente enganosos: `cores` contém **Chaos Orb** e `stars` contém **Chaotic Core**. A migração deve preservar essa associação.

O Reality Core usa o ingrediente Forge **strict NBT** `forge:nbt`, com:

```snbt
{RealityCoreTier:8,RealityCoreCount:3,RealityCoreCompleted:1b,CustomModelData:1008}
```

Não basta conferir apenas `RealityCoreTier`, nem aceitar qualquer Reality Core ou um core incompleto. O `StrictNBTIngredient` do Forge 1.20.1 compara item, dano e share tags exatas. O `Ingredient` existente deve continuar fazendo a validação.

O serializer de resultado usa `ShapedRecipe.itemStackFromJson`, que no Forge delega a `CraftingHelper.getItemStack(..., true, true)` e aceita NBT. `assemble` devolve `output.copy()` e não transfere arbitrariamente o NBT dos ingredientes para o resultado. A nova Forge deve preservar esse comportamento, inclusive para datapacks que adicionem resultados com NBT.

## Contrato de receita e inventário

`ChaosAnvilRecipe.requirements()` é a API adequada para obter os quatro requisitos obrigatórios e o quinto opcional. `extraCount` aceita de 1 a 2.304 no JSON; os requisitos são validados pela alocação entre 0 e 2.304 antes do craft.

**Não usar `ChaosAnvilRecipe.matches` como validador final da nova Forge.** Esse método exige cada requisito inteiro em um único slot, enquanto o funcionamento real usa alocação entre stacks do inventário do jogador. Isso impediria, por exemplo, as 110 barras e 70 orbs do peitoral em slots de limite 64. O método antigo não é usado pelo caminho real de forjamento.

A BlockEntity antiga possui `ItemStackHandler(4)`, mas o menu só mostra o **slot 0 como saída**, com `mayPlace=false`. O crafting exige que esse slot esteja totalmente vazio; nem resultados iguais são empilhados automaticamente. Os slots 1–3 não participam do crafting. A GUI adiciona primeiro 36 slots do jogador e depois a saída (índice de menu 36); shift-click só move essa saída para o jogador.

O inventário compartilhado novo precisa substituir a antiga capacidade útil de 36 slots de ingredientes, aceitar distribuição em múltiplas stacks e continuar disponibilizando uma única operação por solicitação. Uma implementação com cinco slots limitados a 64 itens não executaria todas as receitas existentes. Resultado e ingredientes precisam usar o mesmo armazenamento visto pela GUI e pelas portas, sem consumir recursos de um jogador arbitrário durante automação.

O menu antigo ordena receitas por ID; botões 0–N selecionam uma receita e botão 100 forja. A seleção é um `DataSlot` do menu, reiniciado ao abrir. Não há uma seleção persistida na máquina.

## Energia, controle e persistência

| Informação | Comportamento antigo |
| --- | --- |
| Buffer da BlockEntity | 9.000.000.000.000.000.000 FE |
| Limite nominal de recebimento | 9.000.000.000.000.000.000 FE por chamada; sem `withReceiveBudget()` agregado |
| Extração energética | 0 |
| Face energética | `DOWN` ou consulta sem lado; outras faces não expõem a capability bruta |
| Custo por craft | 5.000.000.000.000.000.000 FE, único débito atômico |
| Armazenamento | `BigEnergyStorage`; capacidade anterior ainda cabe em `long` |
| NBT salvo | `inventory` e `energyData`, além do NBT/persistent data padrão de BlockEntity |
| Slot de saída ocupado | Nenhum ingrediente ou FE consumido |
| Energia insuficiente | Nenhum ingrediente ou FE consumido; nenhuma operação começa |
| Craft pendente após salvar | Inexistente: o craft antigo é instantâneo |

`MachineControl.tick` permite impedir o craft por Enabled/redstone/regras de controle. A Anvil é incluída no suporte genérico `IEnergyBE` e recebe estado de calibração, mas o custo instantâneo não chama os multiplicadores de energia ou trabalho da calibração. Não criar um desconto, penalidade ou tempo novo por reinterpretar esse estado.

O BlockItem antigo é um `EnergyBlockItem` fire-resistant, limite de stack 1, com capacidade **5 × 10¹⁸ FE** — diferente do buffer de **9 × 10¹⁸ FE** da BlockEntity. O item usa `energyData` no NBT e não tem extração de FE. Essa diferença não deve truncar os dados de uma BlockEntity existente durante migração.

### Problema antigo de drops relevante para migração

`ChaosAnvilBlock.getDrops` tem um ramo copiado da Chaos Cube: testa `ChaosCubeEntity` e cria um item Chaos Cube. Esse ramo não corresponde a uma `ChaosAnvilEntity`, logo uma Anvil normal segue a loot table `dropSelf`, sem salvar seu `energyData` no item. Ao remover o bloco, o inventário é derrubado separadamente por `onRemove`.

A migração não deve confiar em quebrar o bloco antigo para recuperar energia. Deve ler diretamente o NBT completo da BlockEntity existente e preservar seu inventário, energia e dados de controle em um caminho de migração explícito. Os quatro slots podem conter itens inseridos por mods externos, mesmo que o crafting só use o slot 0.

## Automações e integrações antigas

- A capability `IItemHandler` expõe os quatro slots em todas as faces e não faz filtragem própria; o wrapper `MachineSideAccess` aplica os modos de entrada/saída configurados.
- O **Item Transporter do próprio mod bloqueia explicitamente `ChaosAnvilEntity`** em `ItemNetworkManager.isForbiddenMachine`. Expor uma capability na nova porta é necessário, mas essa blacklist não deve incluir a nova Forge.
- Mods externos que consultem diretamente as capabilities podem inserir e extrair dos slots antigos; tais itens não são usados pelo caminho de crafting manual.
- JEI registra `ChaosAnvilCategory`, UID/RecipeType de interface `chaosmod:chaos_anvil`, todas as receitas do mesmo `ChaosAnvilRecipe.Type.INSTANCE`, ícone do bloco antigo e título `block.chaosmod.chaos_anvil`.
- A categoria JEI mostra os quatro requisitos, o quinto opcional e o resultado; as quantidades são desenhadas em texto. Não há transferência JEI específica para esse menu no plugin atual.
- O click area JEI antigo é registrado para `ChaosAnvilScreen` em `(178,82,21,14)`.
- Jade usa o provider energético genérico `IEnergyBE`; não existe um provider de processamento/estrutura exclusivo da Anvil.
- O Multiblock Viewer não possui definição da Chaos Anvil. A Chaos Forge precisa de uma definição nova para seu template completo.
- O controle genérico da máquina e seu comparator continuam associados ao bloco antigo, sem detalhes próprios de Stability ou operação em progresso.

## Registros e recursos encontrados

| Área | Referência anterior |
| --- | --- |
| Block | `ModBlocks.CHAOS_ANVIL`, registro `chaosmod:chaos_anvil`, classe `ChaosAnvilBlock` |
| BlockItem | `ModItems.CHAOS_ANVIL`, registro `chaosmod:chaos_anvil` |
| BlockEntity | `ModBlockEntities.CHAOS_ANVIL_BE`, registro `chaosmod:chaos_anvil_be`, classe `ChaosAnvilEntity` |
| Menu | `ModMenuTypes.CHAOS_ANVIL_MENU`, registro `chaosmod:chaos_anvil_menu`, classe `ChaosAnvilMenu` |
| Screen | `ChaosAnvilScreen`, registrada em `ClientModEvents` |
| Serializer | `ModRecipes.CHAOS_ANVIL_SERIALIZER`, registro `chaosmod:chaos_anvil` |
| RecipeType em runtime | `ChaosAnvilRecipe.Type.INSTANCE`, ID `chaos_anvil` |
| Receita da própria máquina | `ModRecipeProvider`, ID `chaosmod:chaos_anvil_void_station` |
| Receitas de equipamento | `ModMachineRecipeData.anvil`, 11 JSONs gerados |
| Aba criativa | `ModCreativeModTabs`, entrada explícita da Anvil |
| Loot | `ModBlockLootTables.dropSelf`, JSON gerado `loot_tables/blocks/chaos_anvil.json` |
| Tag | `mineable/pickaxe`, entrada no `ModBlockTagGenerator` |
| Blockstate | `ModBlockStateProvider.machineWithItem`, JSON gerado com quatro orientações horizontais |
| Item model | `ModItemModelProvider`, parent `chaosmod:block/machines/chaos_anvil` |
| Modelos fonte | `models/block/machines/chaos_anvil.json` e `models/block/chaos_anvil.json` |
| Texturas fonte | `textures/block/chaos_anvil.png`, `chaos_anvil_top.png`, GUI `chaos_anvil_gui.png`; modelo de máquina usa também a família `machines/chaos_*` |
| Tradução ativa | `en_us.json`: nome do bloco e título/descrição do avanço; não havia chaves equivalentes de Anvil em `pt_br.json` |
| Avanço | ID `chaosmod:chaos_anvil`, pai do tier Chaos, critério `has_chaos_anvil`, recompensa 1.000 XP |
| Teste existente | `EquipmentRevisionGameTests.recipesAndAtomicAllocation`: ingredientes exatos do arco, falta de string, tier incorreto e round-trip de rede |

O bloco antigo usa `HORIZONTAL_FACING`, apontando para o jogador ao colocar, é não oclusivo e abre o menu ao interagir. Não há requisito de estrutura, sol, dimensão, tier do jogador ou dificuldade no caminho de craft; a progressão é definida pelos materiais, Reality Cores e FE.

## Receita antiga para fabricar a própria Anvil

Void Station 9×9, saída 1 Anvil, com **36 Chaos Bars, 14 Chaos Capacitors e 7 Chaos Circuits**. Layout canônico, usando `B` = Bar, `P` = Capacitor, `C` = Circuit e `.` = vazio:

```text
BBBBBBBBB
BPPPPPPPB
BPPPPPPPB
BBBBCBBBB
...BCB...
...BCB...
..BBCBB..
..BCCCB..
..BBBBB..
```

Essa receita é um ponto de progressão existente. Sua substituição pela receita do Forge Terminal deve ser explícita no datagen; simplesmente manter a produção da Anvil antiga preservaria uma máquina obsoleta disponível ao jogador.

## Estratégia recomendada para a implementação

1. **Reutilizar `ChaosAnvilRecipe` e seu serializer/ID.** O nome interno legado é um formato de dados e não uma segunda máquina. Isso preserva os 11 IDs, os datapacks, os ingredientes NBT e a rede sem duplicar o sistema de receitas. A categoria JEI pode mudar de título/ícone para Chaos Forge mantendo o UID interno, ou registrar um UID visual novo com uma única lista de receitas.
2. **Extrair/reutilizar a alocação sobre `IItemHandler`.** Usar `requirements()` e snapshots das stacks, agregando múltiplos slots e consumindo somente depois de validar saída, FE, estrutura e todos os ingredientes. Não usar o `matches` legado de slots fixos.
3. **Preservar a transação instantânea e o custo total fixo de 5 × 10¹⁸ FE.** Os três estágios podem ser estados visuais, sem criar uma duração de produção nova. Se a implementação desejar duração real, obter aprovação antes de alterar essa regra.
4. **Um controlador e portas delegadas.** Terminal mantém o inventário compartilhado, buffer FE exato, seleção e estado. Portas não guardam cópias de inventário/energia. Capability retida de uma porta inválida ou descarregada precisa rejeitar operações.
5. **Desativar o caminho antigo de craft.** Remover a receita de fabricação, entrada criativa, ingrediente JEI, screen/click area e título de máquina obsoleta. Atualizar o avanço mantendo ID/critério para o progresso salvo.
6. **Compatibilidade sem perda.** Remover o registro antigo somente com migração explícita de blocos, itens e dados brutos de BlockEntity, conforme o mecanismo abaixo. Não apagar registros e esperar que o mundo descarte silenciosamente blocos, inventários ou FE.
7. **Evitar conversão shapeless que perca NBT.** Uma conversão de Anvil carregada para Terminal precisa copiar a energia e os itens. É preferível um migrador de BlockEntity/BlockItem, validado em servidor, com um estado inequívoco que impeça reimportação dupla.
8. **Não considerar referências de compatibilidade como máquinas ativas.** Nomes internos no serializer e registros de leitura de saves podem permanecer com comentários claros; gameplay, JEI, Viewer, tradução e progressão devem apresentar somente a Chaos Forge.

Esta auditoria não modificou classes de produção nem os JSONs das receitas e não executou Gradle. Os valores foram conferidos tanto no datagen quanto nos 11 JSONs gerados existentes.

## Viabilidade da remoção real dos registros antigos

A análise do código-fonte local do Forge **1.20.1-47.1.3** confirmou um caminho que não exige manter a máquina antiga registrada:

- `MissingMappingsEvent` é publicado no Forge event bus. Os mappings de `ForgeRegistries.Keys.BLOCKS` e `ITEMS` permitem `remap` para o novo Forge Terminal. O Forge instala aliases e usa o novo objeto ao ler os nomes antigos do mundo.
- O registry de **BlockEntityType desabilita saving** em `GameData`. Portanto, não basta escrever um handler `MissingMappingsEvent` para `chaos_anvil_be`: esse ID não será migrado por um snapshot desse registry.
- `ChunkSerializer.read` publica `ChunkDataEvent.Load` antes de executar o callback `postLoadChunk` que instancia as BlockEntities. O callback guarda a mesma lista `block_entities` do NBT. É possível reescrever **apenas o campo `id`** de `chaosmod:chaos_anvil_be` para `chaosmod:chaos_forge` antes de `BlockEntity.loadStatic` consultar o registry.
- O evento é assíncrono. O migrador só percorre os CompoundTags daquela lista; não consulta o mundo, não acessa outras posições e não carrega chunks. Chunks efetivamente alterados são marcados para salvar.
- O controlador novo carrega os campos antigos `inventory` e `energyData`: slot antigo 0 vai para a saída 36; slots antigos 1–3 vão para entradas 0–2. O formato novo usa `Items` e `Energy`, impedindo reimportação dupla ao reabrir.
- Também é tratado o layout histórico `Level.TileEntities`. Outros IDs e todos os campos restantes são preservados exatamente.
- MenuTypes não são estado persistido do mundo; o menu antigo não precisa de alias. O serializer `chaos_anvil` continua como formato de dados das receitas, sem registrar uma segunda máquina.

Esse caminho é implementado em `ForgeLegacyMigration`, com verificações em `ForgeMigrationGameTests`: mutação exclusiva do ID, idempotência, preservação de dados de outros mods, carregamento pelo loader vanilla no novo tipo e round-trip do inventário/FE após salvar. Os testes precisam ser executados pelo integrador principal após concluir o controlador.
