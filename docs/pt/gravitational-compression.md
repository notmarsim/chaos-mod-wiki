# Câmara de Compressão Gravitacional

A Câmara de Compressão Gravitacional do tier Nova transforma materiais simples em **Matéria Comprimida** e libera seu conteúdo através de inventários convencionais. AE2, Refined Storage e transporters armazenam o item comprimido normalmente, sem tratá-lo como seus ingredientes originais.

## Montagem

Siga o molde fixo de **13 × 11 × 13**. A referência limpa pode ser carregada por um Structure Block como `chaosmod:gravitational_compression_chamber`. Nenhum bloco de outros mods usado como molde é exigido.

- Fundação Y0: 168 Carcaças de Compressão Gravitacional e um Núcleo de Compressão em X6, Y0, Z6.
- Teto Y10: 169 carcaças.
- Quatro pilares de canto: 36 Estruturas Gravitacionais, de Y1 a Y9.
- Paredes: 396 Vidros Blindados Gravitacionais antes das portas e do terminal.
- Anéis: 16 Bobinas de Contenção em Y1 e mais 16 em Y7. Em cada anel, Z3/9 usa X5–7, Z4/8 usa X4/8 e Z5–7 usa X3/9.
- Colunas emissoras: X/Z4/4, 4/8, 8/4 e 8/8, de Y2 a Y6: 20 emissores.
- Todo o restante do interior deve ficar vazio. A singularidade visual em X6, Y5, Z6 continua sendo **ar**.

Substitua posições externas fora das arestas por exatamente um Terminal, pelo menos uma Porta de Entrada de Itens, uma de Saída e uma de Energia. Teto e fundação são permitidos. O Núcleo da fundação não pode ser substituído. As portas se orientam automaticamente para fora; todas compartilham os recursos do Núcleo.

A carcaça usa o ID exclusivo `gravitational_compression_casing`, preservando o bloco e a receita existentes da Event Horizon Forge.

## Compressão

Com a máquina parada, selecione **Compressão** e o nível I ou II. Insira materiais elegíveis pela porta ou pelo slot de depósito do terminal. O armazenamento virtual usa um contador de 64 bits por material, sem criar milhões de pilhas. O material selecionado tem preferência, mas outros materiais disponíveis também podem ser processados automaticamente.

| Nível | Unidades originais | Tempo | Consumo | Energia total |
| --- | ---: | ---: | ---: | ---: |
| I | 1.000.000 | 60 s | 800.000 FE/t | 960 MFE |
| II | 1.000.000.000 | 600 s | 8.000.000 FE/t | 96 GFE |

Os materiais só são consumidos quando o produto concluído pode ser armazenado com segurança. Falta de energia, estrutura incompleta, chunks descarregados e saída cheia suspendem o trabalho. Parar preserva o progresso pago; mudar modo, nível ou material enquanto parada cancela esse progresso e mantém os ingredientes.

## Descompressão parcial

Pare a máquina, escolha **Descompressão** e insira Matéria Comprimida pela entrada. A operação leva um quarto do tempo e metade do FE/t da compressão correspondente. Ao concluir, o conteúdo vira saldo virtual do material original. No máximo uma pilha por tick é liberada para os nove slots físicos de saída. Os transporters retiram qualquer quantidade e o restante continua virtual, sem entidades ou pilhas gigantes.

Trocar o modo não duplica saldos. O Núcleo dropado preserva materiais virtuais, produtos, estabilizadores, energia e progresso pago. Guarde esse item ao mover a máquina.

## Estabilidade e integração

A câmara usa Stability de máquina Nova e três slots exclusivos de estabilizadores. Flux Crystal, Empowered Flux Crystal, Reality Condensate e Creative Stabilizer seguem as regras existentes. A estabilidade em zero causa falha de contenção. Não existe crescimento ou aumento de massa de Black Holes; o processo inicial não exige esse catalisador.

Os materiais são definidos pela tag `chaosmod:compressible`, com exclusões em `chaosmod:compression_blacklist`. As categorias iniciais incluem barras, pepitas, gemas, pós, materiais brutos e blocos de armazenamento. Ferramentas, itens danificados, pilhas com dados individuais, baterias, tanques e inventários são rejeitados.

As portas de itens usam `IItemHandler` do Forge. As portas de energia compartilham entrada de 32 MFE/t e buffer de 20 GFE. Mais portas não aceleram o processo interno. Os dois níveis aparecem no JEI para todos os materiais elegíveis.

Os valores ficam centralizados em `chaosmod-compression.toml`. Alterar a quantidade dos níveis muda o significado de Matéria Comprimida existente; faça backup primeiro. Contagens usam inteiros de 64 bits e entradas excedentes são rejeitadas antes de causar overflow.

## Estruturas existentes e portas de itens configuráveis

Tanto o **Gravitational Casing** (`gravitational_casing`) de construções existentes quanto o **Gravitational Compression Casing** são válidos na base e no teto. A referência construída era rejeitada porque seus 337 casings anteriores não eram reconhecidos. As regras de frames, coils, emissores, paredes e espaços vazios continuam obrigatórias.

Fabrique a **Gravitational Item Port** e use o Configurator para selecionar entrada ou saída. O indicador frontal muda com o modo. As antigas portas de entrada/saída continuam aceitas e configuráveis; não é necessário substituí-las. Instale ao menos uma porta em cada modo.

Abrir o terminal solicita a validação no servidor. O diagnóstico informa os blocos esperado/encontrado e a posição mundial/local da primeira falha. Uma mensagem limitada é registrada no log por tentativa solicitada, sem repetir logs nas verificações automáticas.

## Consultando os materiais armazenados

Os ingredientes depositados são movidos para o inventário virtual do Core no próximo tick do servidor. Eles continuam visíveis na lista de **Materiais armazenados**: cada linha mostra as unidades originais e os itens comprimidos, com valores exatos por nível no tooltip. Os botões de página permitem consultar todos os tipos. Clique numa linha enquanto a máquina está parada para selecionar o material a processar. Cada jogador tem sua própria página; o material selecionado para processamento é uma configuração da máquina.

Um depósito manual abre a página do material depositado. Inserções por cabos preservam a página em consulta. Produtos físicos e estabilizadores continuam usando slots de inventário normais. Abrir um terminal já formado não exibe mais uma mensagem de sucesso da formação.
