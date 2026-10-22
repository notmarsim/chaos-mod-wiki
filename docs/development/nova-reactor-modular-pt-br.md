# Nova Reactor — colunas, teto e preenchimento por coolant

Esta revisão substitui a montagem modular anterior. Os limites energéticos, receitas existentes e registros dos blocos foram preservados.

## Como construir

1. Monte um cubo completo de Machine Casing, o casing já utilizado pelo reator. Tamanhos permitidos: 3, 5, 7, 9, 11 e 13 blocos em cada eixo.
2. Coloque exatamente um Nova Reactor Core no centro absoluto X/Y/Z.
3. Use exatamente um Controller. Controller e Item/Energy Ports podem substituir casing em qualquer face, fora das arestas e cantos. O 3³ mantém a exceção antiga dos ports laterais.
4. A partir do 5³, construa colunas completas de Fuel Rod, do primeiro bloco acima do piso ao último abaixo do teto.
5. Na posição do teto diretamente acima de **cada** coluna, substitua o casing por um Control Rod. A associação é 1:1; controles internos, controles órfãos e colunas sem controle são inválidos.
6. Não coloque colunas vizinhas ao norte, sul, leste ou oeste. Diagonais são permitidas. A coordenada horizontal do Core não pode conter Fuel Rods.
7. Preencha **todo** o restante do interior com **Nova Reactor Coolant**, incluindo os espaços acima e abaixo do Core. Não pode sobrar ar.
8. Abasteça os Item Ports de entrada com Black Holes e Nova Bars. Configure ports de saída para retirar waste e Energy Ports para exportar FE.

Nova Reactor Coolant reutiliza o registro e a receita do antigo **Nova Coolant Channel** (`chaosmod:nova_coolant_channel`). Não foi introduzido um fluido novo. Esses blocos representam o preenchimento físico: dão massa térmica e permitem transportar calor. **Não substituem o estoque operacional de coolant**, que continua sendo abastecido com Nova Bars e consumido durante a refrigeração.

O 3³ conserva a unidade compacta integrada, sem colunas novas, controles no teto ou preenchimento adicional.

| Estrutura | Altura interna | Colunas para o teto | Fuel Rods em blocos | Controles no teto | Coolant físico nessa configuração | Limite | Buffer |
|---|---:|---:|---:|---:|---:|---:|---:|
| 3³ | 1 | Integrada | 0 extras | 0 extras | 0 extras | 16 MFE/t | 10 GFE |
| 5³ | 3 | 3 | 9 | 3 | 17 | 48 MFE/t | 30 GFE |
| 7³ | 5 | 7 | 35 | 7 | 89 | 112 MFE/t | 70 GFE |
| 9³ | 7 | 14 | 98 | 14 | 244 | 224 MFE/t | 140 GFE |
| 11³ | 9 | 25 | 225 | 25 | 503 | 400 MFE/t | 250 GFE |
| 13³ | 11 | 40 | 440 | 40 | 890 | 640 MFE/t | 400 GFE |

Os totais são exemplos para alcançar o teto nominal, não uma receita rígida. Uma coluna já pode formar um reator maior. Colunas extras são permitidas se respeitarem a separação, mas não aumentam o teto nem o consumo correspondente.

### Exemplo 5³

Camada interna na altura do Core, vista de cima:

```text
F C F
C O C
F C C
```

`F` = Fuel Rod, `C` = coolant físico, `O` = Core. Repita as posições F em todas as três camadas internas; nas camadas acima e abaixo do Core, substitua O por coolant. No teto coloque os três Control Rods exatamente sobre F, e casing nas demais posições livres de Controller/Ports.

Para 7³ a 13³, uma distribuição alternada como um tabuleiro é válida, sempre reservando a coordenada do Core. Também é possível deslocar ou reduzir os grupos de colunas. Um 9³ com 4 colunas tem 64 MFE/t instalados, com 10 tem 160 MFE/t, com 14 chega ao teto de 224 MFE/t; 18 colunas continuam limitadas a 224 MFE/t.

## Energia e recursos

```text
altura = tamanho - 2
volumeCoolant = altura³ - 1 - colunas × altura
potênciaInstalada = colunas × 16.000.000 FE/t
unidadesAtivas = min(colunas, unidadesEstruturais) × potênciaConfigurada / 100
fuel/t = 20 × unidadesAtivas
waste/t = 10 × unidadesAtivas
```

Cada Black Hole fornece 10.000 unidades de fuel; cada Nova Bar fornece 5.000 coolant operacional. A extração existente transforma 5.000 waste em Exotic Dust. Acumuladores fracionários persistidos evitam perdas de arredondamento em baixa potência. Eficiência térmica baixa **não** reduz a queima de combustível.

```text
eficiênciaTérmica = max(0, 1 - ((casingHeat - 25000) / 25000)²)
eficiênciaWaste = max(0, 1 - waste / capacidadeWaste)
FE/t = 16M × unidadesAtivas × eficiênciaTérmica × eficiênciaWaste
```

O coolant não multiplica diretamente os FE. Ele altera as temperaturas e, por consequência, a eficiência. O buffer limita a geração ao espaço disponível. O desligamento preserva a geração residual existente, com unidades residuais decaindo por 0,998 por tick.

## Simulação térmica e posicionamento

A validação guarda colunas, contatos, volume e coeficientes em cache. O tick utiliza dois reservatórios, núcleo e carcaça, com energia térmica = temperatura × massa. Não há simulação individual de cada bloco por tick.

As colunas que ultrapassam o teto não reagem. A seleção das colunas ativas segue uma ordem determinística por posição.

- **Contato:** contam-se as faces horizontais de cada bloco de Fuel Rod em contato com coolant. Colunas nas bordas têm menos faces de contato que colunas cercadas de coolant.
- **Distância:** cada bloco de coolant contribui com peso 1 até distância horizontal Manhattan 1 da coluna ativa mais próxima, 0,5 em distância 2, 0,25 em 3 e 0,125 em 4. Acima disso não contribui para circulação ativa, mas continua contribuindo para massa térmica.
- **Compartilhamento:** um bloco de coolant contribui uma única vez, mesmo próximo de várias colunas.
- **Concentração:** colunas próximas em distância horizontal 2 aumentam moderadamente o calor; colunas ortogonalmente adjacentes são estruturalmente proibidas.

```text
exposição = média das faces de contato / (4 × altura)
concentração = 1 + 0,025 × média de outras colunas ativas em distância 2
massa = max(1, unidadesEstruturais × (altura / 3)^0,6)
        × (1 + 0,6 × volumeCoolant / altura³)
capacidadeEspacial = min(2 × colunasAtivas, somaPesos / (altura × 2,5))
                    × (0,65 + 0,35 × exposição)
fatorTransferência = 0,75 + 0,25 × exposição
calorReação = 350 × unidadesAtivas × concentração
```

O volume aumenta a capacidade de armazenar calor. Já a remoção ativa é limitada por transferência, estoque operacional e circulação nos ports:

```text
capacidadeCirculação = min(capacidadeEspacial, 8 × ItemPortsDeEntrada)
fatorTemperatura = min(2, coreHeat / 25000)
consumoCoolant/t = (10 + coreHeat / 500) × capacidadeCirculação × fatorTemperatura
remoçãoAtiva = 250 × capacidadeCirculação × fatorTemperatura
```

Sem Item Port de entrada ou sem coolant operacional suficiente para o débito daquele tick, não há remoção ativa. Trocar um port entre entrada e saída atualiza sua participação sem reconstruir o grafo. Energy Ports não circulam coolant. O coolant físico não é consumido nem se transforma em fonte infinita de resfriamento. Muitos blocos podem retardar aquecimento e resfriamento, mas não impedem superaquecimento sem reposição.

Adiciona-se o calor da reação ao núcleo, remove-se o resfriamento ativo disponível, transfere-se calor do núcleo mais quente para a carcaça e aplica-se dissipação passiva. A transferência sai de um reservatório e entra no outro uma única vez.

```text
condutância = unidadesEstruturais × fatorTransferência
transferência = max(0, energiaNúcleo - energiaCarcaça) × 0,1 × condutância / massa
dissipação de cada reservatório = energiaReservatório × 0,002 × condutância / massa
```

Temperaturas ficam entre 0 e 50.000. A temperatura ideal permanece 25.000; refrigeração excessiva reduz naturalmente o rendimento ao manter a carcaça fria, sem penalidade energética arbitrária.

## Segurança e saves

SCRAM manual/automático permanece. A proteção automática começa em 42.000, pode ser ajustada entre 30.000 e 49.000 ou desligada. O reset exige núcleo e carcaça abaixo de 35.000. SCRAM corta fuel e calor novo, mas conserva calor e geração residuais. O resfriamento continua enquanto houver circulação e coolant.

A chance de meltdown existente permanece: zero abaixo de 45.000; de 0,1% até 5% por tick entre 45.000 e 50.000. Não há multiplicador por tamanho. A explosão e transformação de terreno existentes foram mantidas.

Registros, itens, receitas, FE, ports e dados de energia não foram removidos. Reatores compactos antigos continuam integrados. Reatores maiores já salvos como montados recebem o perfil de compatibilidade correspondente à versão anterior: contagem integrada antiga ou grafo espacial anterior. O perfil fica persistido até uma alteração estrutural; **ao reconstruir, passam a exigir teto controlado e interior preenchido**. A UI mostra um aviso de migração.

Novos saves usam `ThermodynamicsVersion = 2`. Energia, recursos, frações, temperaturas precisas e SCRAM são preservados. Remover Core invalida no próximo tick; mudanças normais de construção invalidam pelo evento. Checagem incremental detecta outras alterações em até uma passagem de 128 posições/tick, sem varredura completa contínua nem carregamento forçado de chunks. Um multiblock inválido não queima fuel nem gera energia. Energia já armazenada não é apagada na desmontagem.

## Interface e arquivos

As quatro abas foram preservadas. Structure mostra dimensões externas/internas, colunas, total de blocos Fuel Rod, controles, volume físico, coolant operacional e estado. Thermal mostra massa térmica, produção e remoção de calor. Overview mantém geração, potências e FE; Resources mantém estoques e taxas. Textos novos são em inglês via localização.

Arquivos alterados nesta revisão:

- `src/main/java/net/marsim/chaosmod/block/entity/NovaReactorStructure.java`: validação estrita e checagem do Core.
- `src/main/java/net/marsim/chaosmod/block/entity/NovaReactorLayout.java`: colunas, contatos, distância, volume e coeficientes.
- `src/main/java/net/marsim/chaosmod/block/entity/NovaReactorControllerEntity.java`: circulação, migração e novos campos sincronizados.
- `src/main/java/net/marsim/chaosmod/block/entity/NovaThermodynamics.java`: capacidade de circulação por port.
- `src/main/java/net/marsim/chaosmod/screen/NovaReactorScreen.java`: novos diagnósticos.
- `src/main/java/net/marsim/chaosmod/machine/EnergyRebalanceGameTests.java`: montagem dos cenários de teste.
- `src/main/java/net/marsim/chaosmod/machine/NovaThermalGameTests.java`: adaptação e migração.
- `src/main/resources/assets/chaosmod/lang/en_us.json` e `pt_br.json`: nome do coolant e textos.
- Este guia.

Novo: `src/main/java/net/marsim/chaosmod/machine/NovaFilledReactorGameTests.java`, com testes das novas regras.

## Validação e limitações

Resultado executado: **134 GameTests obrigatórios passaram**. O loop de quatro reatores 13³ por 400 ticks levou **52,80 ms** nesta máquina, sem incluir montagem, renderização ou rede real.

Os resultados desta revisão ficam em `build/reactor-filled-validation.log` e `build/reactor-filled-build.log`. A suíte cobre todos os tamanhos, Core central/duplicado, colunas completas, controle 1:1, adjacência, preenchimento, volumes, potência, consumo, frações, térmica, SCRAM, migração, remoção, sincronização e exportação FE. O benchmark usa quatro reatores 13³ carregados, com 400 ticks por reator e verificação de reutilização dos grafos.

O teste de meltdown cobre seus limiares e a curva de probabilidade; não executa a destruição de raio 45 na suíte compartilhada. Não houve sessão visual interativa ou teste com dois clientes reais. A simulação continua compacta, não resolve temperaturas individuais por bloco. A estrutura desmontada interrompe a simulação, conforme o comportamento anterior. O preenchimento é representado pelo bloco existente de coolant, não por um fluido Forge com balde ou escoamento.


## Indicadores e meltdown por tamanho

A aba Thermal mostra Passive Dissipation (perda natural dos dois reservatórios), Net Heat (variação líquida de calor armazenado por tick), Thermal Trend e Cooling Utilization. Stable significa variação entre -0,05 e +0,05 H/t. A utilização compara a remoção efetiva com o máximo espacial a 50.000 K, incluindo restrições de coolant e ports; não é um bônus de geração. Details mantém os indicadores complementares. Tooltips explicam as métricas.

O topo do Control Rod ocupa toda a largura do bloco, fechando as frestas entre ele e o teto. A área de transformação do meltdown é centrada no Core: raios de 15/21/27/33/39/45 blocos para tamanhos 3/5/7/9/11/13. A força da explosão acompanha a escala (`raio × 25 / 45`); a chance térmica não muda.

Nova, Zenith e Chaotic Extractors recebem antimatter com 4,5% de chance base por extração e quantidades fixas 5/15/32. Os pesos relativos dos demais drops são mantidos. Como nos outros materiais, filtros renormalizam as probabilidades entre os itens habilitados. JEI usa a mesma tabela e mostra automaticamente o novo resultado.


## Remoção de calibração

O Nova Reactor não usa mais calibração: dados antigos de Dial/Target são ignorados e não reduzem a geração. O painel de calibração e seus comandos não aceitam o reator. A integração com Machine Controller, redstone, SCRAM e regras de operação permanece. O atalho I/O foi removido da UI do reator; Shift + Configurator continua abrindo a configuração lateral existente.
