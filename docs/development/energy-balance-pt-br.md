> Auditoria anterior ao rebalanceamento. Para os valores atuais e o reator escalável, consulte [energy-rebalance-pt-br.md](energy-rebalance-pt-br.md).

# Auditoria energética — Chaos Mod

Valores lidos do código atual em 21/10/2026. Nenhum valor energético foi alterado nesta tarefa.

Referência: 20 ticks/s, calibração 100%, sem módulos, entrada disponível e saída livre. FE/s = FE/t × 20. Custos por operação não incluem pausas.

**Transferência:** limites do armazenamento são por chamada da API, não um orçamento global por tick. Exportadores próprios normalmente chamam uma vez por tick; múltiplos lados, slots ou mods podem alterar a vazão agregada. A API Forge padrão usa int, limitada a 2.147.483.647 FE/chamada. Caminhos internos long permitem os números maiores.

## Refiners

| Máquina | Buffer FE | Entrada FE/chamada | Consumo FE/t | FE/s | Ticks/operação | Segundos | FE/operação | Module Power |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| Basic Refiner | 64.000 | 2.000 | 500 | 10.000 | 300 | 15 | 150.000 | 3 |
| Void Refiner | 512.000 | 16.000 | 4.000 | 80.000 | 150 | 7.5 | 600.000 | 6 |
| Stellar Refiner | 4.096.000 | 128.000 | 32.000 | 640.000 | 100 | 5 | 3.200.000 | 9 |
| Darklight Refiner | 32.768.000 | 1.024.000 | 256.000 | 5.120.000 | 60 | 3 | 15.360.000 | 12 |
| Nova Refiner | 256.000.000 | 8.000.000 | 2.000.000 | 40.000.000 | 20 | 1 | 40.000.000 | 24 |
| Zenith Refiner | 2.048.000.000 | 64.000.000 | 16.000.000 | 320.000.000 | 10 | 0.5 | 160.000.000 | 48 |
| Chaotic Refiner | 16.384.000.000 | 512.000.000 | 128.000.000 | 2.560.000.000 | 5 | 0.25 | 640.000.000 | 96 |

Saída energética: 0. Fontes: ProgressionEnergy.java, *RefinerEntity.java, MachineUpgrades.java.

## Extractors

| Máquina | Buffer FE | Entrada FE/chamada | Consumo FE/t | FE/s | Ticks/operação | Segundos | FE/operação | Module Power |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| Basic Extractor | 128.000 | 8.000 | 2.000 | 40.000 | 300 | 15 | 600.000 | 3 |
| Void Extractor | 1.024.000 | 64.000 | 16.000 | 320.000 | 220 | 11 | 3.520.000 | 6 |
| Darklight Extractor | 64.000.000 | 4.000.000 | 1.000.000 | 20.000.000 | 150 | 7.5 | 150.000.000 | 12 |
| Nova Extractor | 512.000.000 | 32.000.000 | 8.000.000 | 160.000.000 | 90 | 4.5 | 720.000.000 | 24 |
| Zenith Extractor | 4.096.000.000 | 256.000.000 | 64.000.000 | 1.280.000.000 | 40 | 2 | 2.560.000.000 | 48 |
| Chaotic Extractor | 32.768.000.000 | 1.024.000.000 | 512.000.000 | 10.240.000.000 | 20 | 1 | 10.240.000.000 | 96 |
| Reality Extractor | 128.000.000 | 8.000.000 | 2.000.000 | 40.000.000 | 1.200 | 60 | 2.400.000.000 | Sem módulos |

Não existe Stellar Extractor registrado. Saída energética: 0. Extractors comuns sorteiam um lote permitido por operação, não uma unidade. Fontes: AbstractExtractorEntity.java, subclasses e RealityExtractorEntity.java.

## Geradores

| Máquina | Buffer FE | Geração-base | FE/s | Exportação própria nominal FE/t | Condição |
|---|---:|---|---:|---:|---|
| Basic Coal Generator | 128.000 | 8.000 FE/carvão | Variável | 250 | Coal/Charcoal; perde 1 FE/t enquanto há energia |
| Void Coal Generator | 4.000.000 | 64.000 FE/combustível | Variável | 2.000 | Shadow Infused Coal; perde 2 FE/t |
| Stellar Generator | 10.000.000 | 8.000 FE/t | 160.000 | 8.000 | Dia, sem chuva/trovão, luz do céu acima |
| Darklight Generator | 50.000.000 | 0–64.000 FE/t | 0–1.280.000 | 1.000.000 | min(64.000, (15 − luz local)² × 288) |
| Nova Reactor | 10.000.000.000 | 0–4.000.000 FE/t | 0–80.000.000 | 10.000.000.000 | Temperatura, waste e calibração |
| Chaos Cube | 9.000.000.000.000.000.000 | 10.000.000.000.000.000 FE/t | 200.000.000.000.000.000 | 9.000.000.000.000.000.000 | Sem combustível |

Os geradores de carvão convertem vários itens no mesmo tick até encher o buffer; não têm duração de queima fixa. Exportação e carga de itens são caminhos distintos. Basic/Void/Stellar/Darklight e reator multiplicam a produção pela calibração. Chaos Cube não multiplica sua geração pela calibração. Fontes: classes de cada gerador.

## Nova Reactor

| Parâmetro | Valor |
|---|---|
| Geração | floor(4.000.000 × max(0, 1 − ((casingHeat − 25.000)/25.000)²) × (1 − waste/100.000)), depois calibração |
| Condição | Estrutura válida, casingHeat > 100 e buffer com espaço |
| Pico | Carcaça em 25.000, waste 0 e calibração 100% |
| Combustível | Buffer interno 1.000.000; 10.000 unidades/item; consumo 20/t ligado |
| Duração/item | 500 ticks = 25 s de queima |
| Referência FE/item | Até 2.000.000.000 FE em 500 ticks mantidos no pico; não é rendimento garantido e não inclui calor residual |
| Waste | +10/t; buffer 100.000; 5.000 unidades por Exotic Dust |
| Coolant | Buffer 1.000.000; 5.000 unidades/item; gasto 10 + floor(coreHeat/500) por tick de queima |
| Aquecimento | +350 coreHeat/t sem coolant; +205/t com coolant suficiente |
| Dissipação | Núcleo/carcaça perdem max(1, floor(calor/500))/t; transferência de 10% inteiro da diferença positiva núcleo→carcaça |
| Meltdown | A partir de 45.000 coreHeat: chance por tick de 0,1% a 5% em 50.000 |
| Desligado | Para a queima; calor residual ainda gera energia |
| Ports | Compartilham o buffer do controller; não somar buffers |

Fonte: NovaReactorControllerEntity.java e NovaReactorPortEntity.java.

## Zenith Accelerator

| Parâmetro | Valor |
|---|---|
| Input: buffer | 512.000.000 FE |
| Input: entrada | 32.000.000 FE/chamada |
| Consumo ativo | 8.000.000 FE/t = 160.000.000 FE/s, incluindo carga e injeção |
| Input: saída | 0 |
| Center Accelerator Rod: buffer | 100.000.000.000.000 FE |
| Rod: entrada/extração da API | 100.000.000.000.000 FE/chamada |
| Rod: exportação própria | Solicita até 2×10¹⁸ FE por vizinho, limitada pelo conteúdo (≤10¹⁴), receptor e conexões |
| Colisão com Zenith | floor(250.000.000 × carga) FE recuperados |
| Colisão com Zenith Bar | floor(750.000.000 × carga) FE recuperados |
| Condensate com Zenith | floor(2^(10 × (carga − 0,5))); em 100%: 32 |
| Condensate com barra | floor(resultado-base × 2,5); em 100%: 80 |
| Carga mínima | 50% para liberar |
| Material/tick acelerando | Zenith: 0,1 + 0,1 × carga; barra: valor ÷ 11; acumulador fracionário |
| Carga inicial | 60 ticks, velocidade 0,01 |
| Aceleração | +0,005/t até 1,5; aguarda fim da volta de 24 blocos |
| Injeção | 3 blocos a 0,25/t = 12 ticks |

É um consumidor líquido de FE. Retorno na colisão é recuperação parcial no rod. Módulos de refiners e calibração não alteram seu custo fixo.

Simulação da lógica, ciclo completo sem pausas: **380 ticks (19 s), 3.040.000.000 FE consumidos**. Saldo consumido após retorno: 2.790.000.000 FE com Zenith ou 2.290.000.000 FE com barra. Material acumulado aproximado: 44.950 Zeniths ou 4.086 barras; itens efetivamente retirados são inteiros e o reset do ciclo limpa a fração restante (aproximadamente 44 Zeniths ou 4 barras retiradas no ciclo cheio).

Fontes: ZenithAcceleratorInputEntity.java, CenterAcceleratorRodEntity.java, ProgressionEnergy.java, AcceleratorCycle.java e AcceleratorLayout.java.

## Baterias

| Bateria | Capacidade FE | Entrada/extração FE/chamada | Consumo/geração |
|---|---:|---:|---|
| Basic | 1.000.000 | 10.000 | 0 |
| Void | 100.000.000 | 2.000.000 | 0 |
| Stellar | 100.000.000.000 | 10.000.000.000 | 0 |
| Darklight | 5.000.000.000.000 | 500.000.000.000 | 0 |
| Nova | 100.000.000.000.000 | 100.000.000.000.000 | 0 |
| Zenith | 1.000.000.000.000.000 | 1.000.000.000.000.000 | 0 |

A exportação das baterias tenta a taxa por lado de saída, podendo superar essa taxa agregada por tick com vários lados. Carga/descarga de itens usa int. Fonte: AbstractBatteryEntity.java e subclasses.

## Demais máquinas

| Máquina | Buffer FE | Entrada FE/chamada | Consumo |
|---|---:|---:|---|
| Gateway Terminal | 512.000.000 | 16.000.000 | Scan: 1.000.000 por tentativa a cada 2.400 ticks (120 s); portal abaixo |
| Chaotic Converter | 200.000 | 100.000 | 500 FE/t; 100 ticks (5 s); 50.000 FE/operação |
| Chaos Anvil | 9.000.000.000.000.000.000 | 9.000.000.000.000.000.000 | 5.000.000.000.000.000.000 FE/crafting (5 CE) |
| Dimensional Augmentor | 2.000.000.000 | 10.000.000 | min(buffer, 10.000 × 2^tier × nível-desejado²) por upgrade |
| Orbital Deflector | 1.000.000 | 100.000 | 100 FE/t ligado + 250.000 FE/interceptação |
| Void Station manual/automação | 0 | 0 | Sem FE |
| Machine Controller | 0 | 0 | Sem FE próprio |
| Dimensional Anchor | 256.000.000 | 16.000.000 | 1.000 × tamanho³ FE/t; tamanho 3–10: 27.000–1.000.000 FE/t (540.000–20.000.000 FE/s) |

Saída energética: 0. Augmentor: base 10.000 configurável; expansões I/II/III custam 250.000.000 / 1.000.000.000 / 1.750.000.000 FE. Fontes: classes homônimas, AugmentConfig.java e InfrastructureEntity.java.

## Gateway por afinidade

| Tier | Abrir FE | Manter FE/t | Manter FE/s |
|---|---:|---:|---:|
| Unstable | 1.000.000 | 1.000 | 20.000 |
| Stable | 2.000.000 | 2.000 | 40.000 |
| Void | 4.000.000 | 4.000 | 80.000 |
| Stellar | 8.000.000 | 8.000 | 160.000 |
| Duality | 16.000.000 | 16.000 | 320.000 |
| Darklight | 32.000.000 | 32.000 | 640.000 |
| Nova | 64.000.000 | 64.000 | 1.280.000 |
| Zenith | 128.000.000 | 128.000 | 2.560.000 |
| Chaos | 256.000.000 | 256.000 | 5.120.000 |

Realidades existentes guardam seus custos no save e podem conservar valores antigos. Fontes: RealityTier.java, RealityData.java.

## Cabos de energia

| Tier | Capacidade FE por segmento |
|---|---:|
| Basic | 1.000 |
| Void | 100.000 |
| Darklight | 100.000.000 |
| Nova | 100.000.000.000 |
| Zenith | 100.000.000.000.000 |
| Chaos | 10.000.000.000.000.000 |

Cabos contribuem para a capacidade da rede, sem geração. Transporters não consomem FE. Casings, curvas/blocos do acelerador e gateway frames não têm consumo/buffer independente.

## Efeito dos módulos e calibração

- S = min(1, power Speed/demanda); E = min(1, power Energy/demanda).
- Velocidade = 1 + 2S, até 3×.
- FE/t = ceil(ceil(base × (1 + 0,8 × (1 − calibração))) × (1 + 2S)^1,5 × (1 − 0,75E)).
- Progresso/tick = round((100 + round(200S)) × calibração); ticks/operação arredondam para cima.
- Calibração 85–100%: em 85%, +12% consumo/tick e −15% progresso nominal.
- Energy 100%: consumo 25% do original. Speed 100%: ~5,196× consumo/tick, ~1/3 da duração e ~1,732× FE/operação.
- Slots compartilham Module Power: não se pode maximizar Speed e Energy simultaneamente no orçamento atual.
- Stability não reduz FE diretamente. Estas fórmulas são dos refiners/extractors comuns. Reality Extractor também sofre o acréscimo energético da calibração e usa ritmo proporcional à eficiência, mas não recebe os três módulos.

## Pontos para buffs/nerfs

- Basic Refiner consome 500 FE/t contra 250 de exportação própria do gerador; Basic Extractor consome 2.000 FE/t.
- Void Refiner consome 4.000 FE/t contra 2.000 de exportação própria do gerador; Void Extractor consome 16.000 FE/t.
- Stellar Refiner exige 4 geradores Stellar no pico; Darklight Refiner exige 4 Darklight, e Darklight Extractor exige 16.
- Nova Refiner consome 2.000.000 FE/t; Nova Extractor e Accelerator usam 8.000.000 FE/t; um reator chega a 4.000.000 FE/t.
- Zenith Refiner usa 16.000.000 FE/t; Zenith Extractor usa 64.000.000 FE/t.
- Chaos Anvil custa 500 ticks (25 s) de geração exclusiva de um Chaos Cube, ignorando gargalos.
- Speed aumenta o consumo por tick sem aumentar a entrada da máquina.

Esta é uma auditoria estática do código; não uma medição de vazão com outros mods.
