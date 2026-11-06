# Multiblock Viewer

O **Multiblock Viewer** permite planejar estruturas e projetar um guia de construção no mundo. Fabrique-o no **tier Void**, forneça FE e clique com o botão direito para abrir a biblioteca. No sobrevivência, a construção é manual. Jogadores no criativo podem ativar explicitamente a construção automática.

## Energia

| Propriedade | Padrão |
| --- | ---: |
| Buffer | 256.000 FE |
| Entrada máxima | 2.048 FE/t |
| Espera | 4 FE/t |
| Prévia 3D em uso | 8 FE/t |
| Projeção holográfica | 32 FE/t |

Aplica-se somente o consumo do estado de maior prioridade. Uma projeção custa **32 FE/t no total**, mesmo com a prévia aberta. O Viewer físico alimenta o holograma; o local da construção não precisa de cabos de energia.

Sem energia, a interface básica continua acessível, mas a prévia 3D, o holograma e o HUD de construção são suspensos. Os projetos mantêm posição, orientação e configurações. Quebrar ou descarregar o Viewer de origem interrompe a projeção. Nenhum chunk é carregado à força.

## Biblioteca e prévia

Pesquise pelo nome da máquina ou filtre por tier. Selecione uma máquina para examinar os modelos reais dos blocos, materiais, interfaces obrigatórias e substituições estruturais. **Voltar** preserva o filtro e a posição da lista.

Um **+** após o tier identifica uma família com upgrades. Essas famílias também aparecem nos filtros dos tiers superiores suportados; selecioná-las aplica o tier filtrado ao plano inicial.

Arraste para girar a prévia, use a roda do mouse para aproximar e **Centralizar** para restaurar a câmera. Os filtros permitem ocultar casings, vidros, portas e outros componentes, inclusive tipos individuais de bloco. A transparência ajuda a visualizar o interior. Passe o mouse por um bloco para consultar nome, ID, coordenada relativa e substituições permitidas.

Os controles de camada exibem todas as camadas, somente a selecionada ou todas até a seleção. Prévias grandes usam uma amostra limitada da estrutura; selecione camadas individuais para examiná-las melhor. Os orçamentos de renderização e validação limitam o trabalho realizado, sem impor um novo tamanho máximo às máquinas.

A biblioteca inclui:

| Família | Configuração |
| --- | --- |
| Stellar Liquefier | Estrutura fixa |
| Stellar Reactor | Estrutura fixa |
| Industrial Refinery | Dimensões retangulares e tier de processamento |
| Industrial Extractor | Dimensões retangulares e tier de processamento |
| Stellar Energy Matrix | Dimensões retangulares e tier dos componentes |
| Event Horizon Forge | Estrutura fixa e interfaces permitidas na montagem |
| Matter Inversion Chamber | Geometria fixa e interfaces flexíveis na carcaça |
| Darklight Plasma Generator | Geometria fixa e posições autorizadas de interfaces |
| Nova Reactor | Tamanho, quantidade, espaçamento e deslocamento das colunas de combustível |
| Nova Waste Reprocessor | Alturas independentes das torres de resíduos e pó |
| Gravitational Compression | Estrutura fixa e interfaces externas flexíveis |
| Zenith Accelerator | Estrutura de 49 × 5 × 49; colunas flexíveis de três blocos para interfaces |
| Reality Devourer | Saídas nas partes planas das faces dos energy housings |
| Reality Gateway | Moldura e Terminal |

O Viewer não libera receitas nem contorna as regras de tier, construção ou operação das máquinas.

## Construção livre e planejada

**Construção livre** é o padrão. As posições roxas são flexíveis: qualquer substituição aceita pela máquina é válida. A distribuição sugerida de casings, vidros e portas serve como guia. As quantidades obrigatórias de interfaces são verificadas separadamente; preencher todas as posições com casing não resolve a falta de uma porta de energia.

Use **Construção planejada** para definir uma disposição específica. Inspecione uma posição flexível e alterne entre suas opções antes de iniciar. A projeção salva passa a exigir essa escolha. Estruturas fixas, arestas e componentes internos continuam seguindo as regras de cada máquina.

## Construção no mundo

1. Selecione **Construir**. A interface fecha e uma projeção provisória acompanha o bloco apontado.
2. Gire a projeção e confirme a posição. Cancelar encerra o posicionamento sem colocar nada.
3. Fabrique e coloque os blocos reais. O guia acompanha posições corretas, ausentes, incorretas e descarregadas.
4. Use as camadas e o HUD de materiais restantes. O avanço automático pode selecionar a próxima camada após concluir a atual.
5. **Estrutura completa** só aparece após o validador real também aceitar a montagem. A formação e a operação continuam sendo responsabilidade da máquina.

O holograma usa verde para blocos corretos, azul para ausentes, vermelho para incorretos e roxo para indicar posições flexíveis. Posições descarregadas são desconhecidas: não contam como ar nem como concluídas. A projeção não cria blocos físicos ou entidades.

Os atalhos podem ser alterados nas configurações de controles do Minecraft:

| Ação | Padrão |
| --- | --- |
| Confirmar / cancelar posicionamento | Alt + Enter / Alt + Backspace |
| Girar projeção | Alt + R |
| Alternar modo de camadas | Alt + L |
| Próxima camada / anterior | Alt + Seta para cima / Seta para baixo |
| Alternar filtro de ausentes/incorretos | Alt + V |
| Mostrar/ocultar blocos corretos | Alt + C |
| Minimizar / ocultar HUD | Alt + M / Alt + H |
| Avanço automático de camada | Alt + J |
| Pausar / parar projeto | Alt + P / Alt + Delete |
| Reabrir Viewer | Alt + B |
| Rolar materiais restantes | Alt + roda do mouse |

## Projetos salvos e servidores

Cada jogador possui seus próprios projetos, com um ativo por vez. A página **Projetos** permite retomar ou excluir construções. Fechar a interface, desconectar ou reiniciar o servidor não apaga o projeto. Um projeto parado pode ser retomado em um Viewer energizado.

O servidor controla a validade e o consumo de FE. No sobrevivência, os comandos descrevem apenas um guia: não criam blocos nem alteram inventários das máquinas. Os projetos podem apontar para outra dimensão carregada, mas nem a origem nem o local de construção mantêm chunks carregados automaticamente.

## Construção automática no criativo

No criativo, ative **Construção automática** na prévia, clique em **Construir**, posicione a projeção e confirme. Também é possível usar **Alt + G** durante o posicionamento ou no projeto ativo para alternar/iniciar/cancelar a construção. A tecla pode ser alterada nos controles.

O servidor verifica o modo criativo durante toda a operação. Primeiro confere o local e depois coloca os blocos gradualmente, evitando concentrar o trabalho em um tick. Os chunks precisam estar carregados. Inventários existentes e terrenos que obstruem a montagem são preservados, e cancelamentos de proteção de outros mods são respeitados. O guia informa obstruções ou áreas descarregadas em vez de limpá-las. Após a colocação dos cabos, suas conexões são configuradas.

A construção para se o proprietário sair do criativo, desconectar, cancelar ou se o Viewer de origem for descarregado. O plano salvo permanece após reiniciar, mas a colocação automática nunca recomeça sozinha. A exigência normal de energia do Viewer continua valendo.

Energia, orçamentos de trabalho, cache, distância de renderização e limites de projetos ficam em `chaosmod-multiblock-viewer.toml`. Um aviso de limite de cache pede aumentar o orçamento técnico para projetos enormes; isso não altera as dimensões legais do multibloco.
