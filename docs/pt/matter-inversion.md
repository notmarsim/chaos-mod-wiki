# Matter Inversion Chamber

A versão final é um multibloco Darklight **fechado de 13×13×13**, baseado no NBT atual enviado pelo usuário. Produz Antimatter com tanques Stellar e Darklight independentes e o conjunto central de inversão 3×3×3 fixo no centro geométrico.

## Geometria e orientação

As coordenadas locais são **X/Z=-6 até +6 e Y=0 até 12**. O Core fica em **(0,6,0)**. Na referência, os tanques ficam em X=-4 e X=+4, Z=0 e vão de **Y=1 até Y=11**. Suas colunas centrais ficam vazias; o líquido renderizado não utiliza blocos reais de fluido.

As **140 posições de arestas e vértices exigem Inversion Casing**. As **726 posições internas das faces** aceitam mistura livre de Containment Glass, Inversion Casing e terminais/ports válidos. As seis faces, inclusive teto e piso, podem receber componentes. É necessário ter pelo menos um terminal e uma porta de cada tipo. As paredes dos tanques, o núcleo e os emissores seguem a geometria do NBT.

A máquina identifica a orientação da estrutura e mantém a associação física dos tanques após formar. A face externa escolhida para o terminal não troca qual tanque contém cada líquido.

## Materiais e legenda da referência

A tabela e os diagramas representam o exemplo enviado, não uma exigência de material externo ou rota fixa de conduits. O exemplo tem **1.093 blocos sólidos**. Os materiais das faces, a quantidade de portas e o comprimento das rotas podem variar.

| Símbolo | Bloco na referência | Quantidade |
| --- | --- | ---: |
| `#` | Inversion Casing | 382 |
| `R` | Tank Reinforced Casing | 104 |
| `G` | Containment Glass | 551 |
| `M` | Inversion Matrix Casing | 26 |
| `C` | Matter Inversion Core | 1 |
| `E` | Inversion Emitter | 4 |
| `S` | Emitter Support | 8 |
| `F` | Inversion Fluid Conduit | 12 |
| `T` | Matter Inversion Terminal | 1 |
| `L` | Stellar Fluid Input Port | 1 |
| `D` | Darklight Fluid Input Port | 1 |
| `P` | Energy Input Port | 1 |
| `O` | Antimatter Output Port | 1 |

## Construção e redes de fluido

1. Faça uma casca fechada de 13×13×13. Arestas e vértices são Inversion Casing; preencha o interior das faces com vidro, casing ou componentes funcionais.
2. Reproduza os tanques 3×11×3 em X=-5 até -3 e X=+3 até +5, Z=-1 até +1, Y=1 até 11. Preserve os cantos e perímetros superior/inferior de casing, as laterais de vidro e as colunas centrais vazias. Os dois tanques ficam separados.
3. Monte o conjunto central 3×3×3 em X/Z=-1 até +1, Y=5 até 7: um Core em (0,6,0), cercado por 26 Matrix Casings. Mantenha os quatro emissores e os oito suportes nas posições dos diagramas.
4. Coloque pelo menos um Terminal, Stellar Input, Darklight Input, Energy Input e Antimatter Output no interior de qualquer face externa. Múltiplas portas compartilham o mesmo armazenamento. As entradas de fluido ainda precisam de um caminho interno válido.
5. Faça rotas Stellar e Darklight independentes. Cada entrada precisa chegar ao seu tanque; cada tanque também precisa alimentar seu encaixe central da Matrix. Trechos separados podem se ligar logicamente pelo mesmo tanque, como na referência. As linhas de fluidos diferentes nunca podem compartilhar um conduit ou encostar por uma face.

Os braços dos conduits conectam-se a outros conduits, à porta de fluido, aos quatro pontos centrais laterais de cada tanque em Y=1,6,11 e aos dois encaixes centrais da Matrix existentes no NBT. O encaixe central do tanque pode continuar sendo vidro. Não há conexão ao piso/casing externo, cantos dos tanques, outros vidros, suportes, emissores, terminal, porta de energia ou saída. A identidade dos fluidos é validada pela rede real, não pelos braços antigos salvos no NBT.

As rotas de entrada do NBT são exemplos e podem ser refeitas para acompanhar portas movidas. Conduits adicionais podem ocupar posições internas livres, mas não as colunas de líquido dos tanques nem componentes obrigatórios. As demais posições internas continuam sendo ar.

## Camadas Y da referência

As colunas vão de **X=-6 até +6**; as linhas vão de **Z=-6 até +6**, de cima para baixo. A última linha é a frente da referência. Vidro/casing/controles das faces e rotas de conduits podem mudar seguindo as regras; os componentes internos fixos não.

### Y=0

```text
#############
#############
#############
#############
#############
#############
#############
#############
#############
#############
#############
#############
#############
```

### Y=1

```text
#GGGGGGGGGGG#
G...........G
G...........G
G...........G
G...........G
GRRR.....RRRG
GR.R.....R.RG
GRRR.....RRRG
G.F.......F.G
G.F.......F.G
G.F.......F.G
G.FF.....FF.G
#TGLGPGOGDGG#
```

### Y=2

```text
#GGGGGGGGGGG#
G...........G
G...........G
G...........G
G...........G
GRGR.....RGRG
GG.G.SES.G.GG
GRGR.....RGRG
G...........G
G...........G
G...........G
G...........G
#GGGGGGGGGGG#
```

### Y=3

```text
#GGGGGGGGGGG#
G...........G
G...........G
G...........G
G...........G
GRGR.....RGRG
GG.G.....G.GG
GRGR.....RGRG
G...........G
G...........G
G...........G
G...........G
#GGGGGGGGGGG#
```

### Y=4

```text
#GGGGGGGGGGG#
G...........G
G...........G
G...........G
G...........G
GRGR.....RGRG
GG.G.....G.GG
GRGR.....RGRG
G...........G
G...........G
G...........G
G...........G
#GGGGGGGGGGG#
```

### Y=5

```text
#GGGGGGGGGGG#
G...........G
G...........G
G...........G
G...........G
GRGR.MMM.RGRG
GG.G.MMM.G.GG
GRGR.MMM.RGRG
G...........G
G...........G
G...........G
G...........G
#GGGGGGGGGGG#
```

### Y=6

```text
#GGGGGGGGGGG#
G...........G
G....SES....G
G...........G
G...........G
GRGR.MMM.RGRG
GG.GFMCMFG.GG
GRGR.MMM.RGRG
G...........G
G...........G
G....SES....G
G...........G
#GGGGGGGGGGG#
```

### Y=7

```text
#GGGGGGGGGGG#
G...........G
G...........G
G...........G
G...........G
GRGR.MMM.RGRG
GG.G.MMM.G.GG
GRGR.MMM.RGRG
G...........G
G...........G
G...........G
G...........G
#GGGGGGGGGGG#
```

### Y=8

```text
#GGGGGGGGGGG#
G...........G
G...........G
G...........G
G...........G
GRGR.....RGRG
GG.G.....G.GG
GRGR.....RGRG
G...........G
G...........G
G...........G
G...........G
#GGGGGGGGGGG#
```

### Y=9

```text
#GGGGGGGGGGG#
G...........G
G...........G
G...........G
G...........G
GRGR.....RGRG
GG.G.....G.GG
GRGR.....RGRG
G...........G
G...........G
G...........G
G...........G
#GGGGGGGGGGG#
```

### Y=10

```text
#GGGGGGGGGGG#
G...........G
G...........G
G...........G
G...........G
GRGR.....RGRG
GG.G.SES.G.GG
GRGR.....RGRG
G...........G
G...........G
G...........G
G...........G
#GGGGGGGGGGG#
```

### Y=11

```text
#GGGGGGGGGGG#
G...........G
G...........G
G...........G
G...........G
GRRR.....RRRG
GR.R.....R.RG
GRRR.....RRRG
G...........G
G...........G
G...........G
G...........G
#GGGGGGGGGGG#
```

### Y=12

```text
#############
#############
#############
#############
#############
#############
#############
#############
#############
#############
#############
#############
#############
```

## Produção e automação

Cada operação concluída usa **1.000 mB Stellar Liquid + 500 mB Darklight Liquid → 1 Antimatter**. Os líquidos permanecem nos tanques durante o processamento e são consumidos juntos na conclusão.

| Modo | Tempo | Consumo | FE total |
| --- | ---: | ---: | ---: |
| Safe | 45 s | 300 kFE/t | 270 MFE |
| Balanced | 30 s | 600 kFE/t | 360 MFE |
| Overdriven | 15 s | 1,6 MFE/t | 480 MFE |

Produza os dois líquidos no Stellar Liquefier existente e abasteça suas portas correspondentes. Cada tanque armazena 128.000 mB de forma independente. O buffer guarda 2 GFE, a entrada compartilhada aceita 4 MFE/t e a saída possui nove slots.

Escolha a intensidade e pressione **Start** para produção contínua. **Stop** interrompe o consumo, preserva o progresso já pago e permite trocar a intensidade antes de retomar. A troca fica bloqueada enquanto a máquina está ligada. Falta de FE ou líquidos, saída cheia e chunks necessários descarregados suspendem o processamento com segurança. A FE gasta não é reembolsada.

As portas de fluido rejeitam o líquido errado e qualquer extração. A porta de Antimatter permite somente retirar a saída. Cabos do Chaos e outras automações Forge de fluidos/itens podem utilizá-las. O Core preserva tanques, energia, inventário e progresso quando quebrado normalmente. O Darklight Liquid sustenta o processo; não há ingrediente adicional de estabilização.

O JEI mostra todas as receitas de construção e as três intensidades. Os valores ficam no `serverconfig/chaosmod-matter-inversion.toml` do mundo; um ciclo ativo mantém seus custos originais até terminar. A GUI mostra erros estruturais com coordenadas, progresso da validação e estado dos emissores. A máquina não força o carregamento de chunks.

Um operador pode usar `/place template chaosmod:matter_inversion_chamber` como referência de montagem completa. Ela usa as coordenadas importadas da referência e não é gerada naturalmente. Os projetos editáveis do Blockbench MCP ficam em `art/models/matter_inversion/`.
