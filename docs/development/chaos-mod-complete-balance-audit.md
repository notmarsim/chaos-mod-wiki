# Chaos Mod — Auditoria Completa de Máquinas, Energia, Recursos e Progressão

> **Classificação do Documento:** Relatório Técnico Exaustivo de Engenharia Reversa e Balanço Industrial  
> **Versão do Mod Alvo:** Chaos Mod 2.0.0 (Minecraft Forge 1.20.1)  
> **Status dos Dados:** Investigação 100% baseada no código-fonte em tempo de execução (`src/main/java`, `resources`, `recipes`)  
> **Classificação de Certeza dos Dados:**  
> - 🟢 *Valor confirmado*: Extraído diretamente de constantes, métodos de cálculo e regras do código.  
> - 🔵 *Valor derivado*: Calculado matematicamente a partir de parâmetros confirmados (ex: taxa × tempo).  
> - 🟡 *Valor estimado*: Cenário operacional com variáveis dinâmicas (luz solar, saturação, temperatura).  
> - 🔴 *Não determinado*: Informação ausente ou comportamento indeterminado sem regras fixas no código.

---

## 1. Resumo Executivo

O **Chaos Mod** apresenta um sistema industrial com escala de progressão dividida em sete tiers principais: **Basic**, **Void**, **Stellar**, **Darklight**, **Nova**, **Zenith** e **Chaos**. A infraestrutura energética baseia-se no padrão **Forge Energy (FE)** executado a **20 ticks por segundo (tps)**.

### Escala de Ordens de Grandeza (Padrão Utilizado)
- **kFE** (Quilo-FE): $10^3$ FE
- **MFE** (Mega-FE): $10^6$ FE
- **GFE** (Giga-FE): $10^9$ FE
- **TFE** (Tera-FE): $10^{12}$ FE
- **PFE** (Peta-FE): $10^{15}$ FE
- **EFE** (Exa-FE): $10^{18}$ FE

### Panorama Industrial e Curva de Potência
1. **Fase Inicial (Basic & Void):** Transição de $500\text{ FE/t}$ a $16\text{ kFE/t}$. Opera com queima de combustíveis fósseis e primeiros cabos de condutância limitada ($1\text{ kFE/t}$ a $100\text{ kFE/t}$).
2. **Fase Tecnológica (Stellar & Darklight):** Entrada em fluidos de alta densidade (*Stellar Liquid* e *Darklight Liquid*), geração solar/void de até $1\text{ MFE/t}$, e reatores de fusão de plasma com exportação de até $32\text{ MFE/t}$ por porta e geração de até $640\text{ MFE}$ por barra processada.
3. **Fase Avançada (Nova & Horizon):** Entram em operação a *Matter Inversion Chamber* ($300\text{ kFE/t}$ a $1,6\text{ MFE/t}$ para síntese de Antimatter) e a *Event Horizon Forge* ($1\text{ MFE/t}$ a $3\text{ MFE/t}$ para condensação de Singularidades / Buracos Negros). A geração de energia atinge o *Nova Reactor* modular (até $640\text{ MFE/t}$ em reator $13\times 13$).
4. **Fase Quântica e Singular (Zenith & Chaos):** Demanda extrema com picos exponenciais no *Zenith Accelerator* (injetando pulsos de até $10\text{ TFE}$ na colisão) e refino de materiais a $1\text{ GFE/t}$. No ápice, o *Chaos Cube* opera como fonte infinita passiva gerando $10\text{ PFE/t}$ incondicionalmente.

---

## 2. Inventário Completo de Máquinas e Infraestrutura

Abaixo consta o catálogo completo de todas as entidades de bloco, máquinas e componentes logísticos registrados no mod:

| Categoria | Máquina / Componente | Classe de Implementação | Tier | Papel Operacional Principal |
| :--- | :--- | :--- | :--- | :--- |
| **Gerador** | Basic Coal Generator | `BasicCoalGeneratorEntity` | Basic | Conversão térmica de carvão/carvão vegetal |
| **Gerador** | Void Coal Generator | `VoidCoalGeneratorEntity` | Void | Conversão térmica de Shadow Infused Coal |
| **Gerador** | Stellar Generator | `StellarGeneratorEntity` | Stellar | Captação fotovoltaica direta de luz solar |
| **Gerador** | Darklight Generator | `DarklightGeneratorEntity` | Darklight | Conversão de radiação escura / ausência de luz |
| **Gerador** | Stellar Reactor (Multibloco) | `StellarMachineEntity` | Stellar | Reator catalítico a Stellar Liquid |
| **Gerador** | Darklight Plasma Generator (Multibloco) | `PlasmaMachineEntity` | Darklight | Geração termo-plasmática a Darklight Liquid |
| **Gerador** | Nova Reactor (Multibloco) | `NovaReactorControllerEntity` | Nova | Reator termodinâmico de fissão subatômica |
| **Gerador/Colisor** | Zenith Accelerator (Multibloco) | `ZenithAcceleratorInputEntity` | Zenith | Colisor relativístico de partículas / Pulso TFE |
| **Gerador** | Chaos Cube | `ChaosCubeEntity` | Chaos | Emissor quântico incondicional passivo |
| **Refinadora** | Basic Refiner | `BasicRefinerEntity` | Basic | Processamento elementar de minérios e ligas |
| **Refinadora** | Void Refiner | `VoidRefinerEntity` | Void | Refino acelerado com purificação Void |
| **Refinadora** | Stellar Refiner | `StellarRefinerEntity` | Stellar | Síntese de ligas Stellar e ligas intermediárias |
| **Refinadora** | Darklight Refiner | `DarklightRefinerEntity` | Darklight | Cristalização de Darklight e ligas densas |
| **Refinadora** | Nova Refiner | `NovaRefinerEntity` | Nova | Refino de alta frequência energética |
| **Refinadora** | Zenith Refiner | `ZenithRefinerEntity` | Zenith | Refino quântico em escala de gigawatt |
| **Refinadora** | Chaotic Refiner | `ChaoticRefinerEntity` | Chaos | Refino instantâneo hiper-otimizado |
| **Extratora** | Basic Extractor | `BasicExtractorEntity` | Basic | Extração de poeiras elementares de pedras |
| **Extratora** | Void Extractor | `VoidExtractorEntity` | Void | Extração de resíduos do Vazio |
| **Extratora** | Darklight Extractor | `DarklightExtractorEntity` | Darklight | Extração de fragmentos e essências de Darklight |
| **Extratora** | Nova Extractor | `NovaExtractorEntity` | Nova | Extração de isótopos de Nova |
| **Extratora** | Zenith Extractor | `ZenithExtractorEntity` | Zenith | Extração em ultra-densidade (escala GFE) |
| **Extratora** | Chaotic Extractor | `ChaoticExtractorEntity` | Chaos | Extração quântica caótica |
| **Extratora** | Reality Extractor | `RealityExtractorEntity` | Zenith | Filtro dimensional de Reality Dust |
| **Extratora** | Industrial Extractor (Multibloco) | `IndustrialExtractorEntity` | Industrial | Extrator modular escalável em malha |
| **Multibloco** | Stellar Liquefier | `StellarMachineEntity` | Stellar | Liquefação de barras em Stellar/Darklight Liquid |
| **Multibloco** | Matter Inversion Chamber | `InversionMachineEntity` | Darklight | Inversão de fase de fluidos em Antimatter |
| **Multibloco** | Event Horizon Forge | `HorizonMachineEntity` | Nova/Zenith | Condensação de Singularidade e Transmutações |
| **Multibloco** | Stellar Energy Matrix | `MatrixMachineEntity` | Multi-tier | Banco modular maciço de estocagem de energia |
| **Multibloco** | Industrial Refinery | `RefineryEntity` | Multi-tier | Complexo modular de refino multicanal 18x18 |
| **Logística** | Energy Cables | `EnergyCableBlockEntity` | Basic-Chaos | Condução instantânea de energia elétrica |
| **Logística** | Fluid Cables | `FluidCableBlockEntity` | Basic-Chaos | Condução em rede por pacotes de fluidos |
| **Logística** | Item Transporters | `TransporterBlockEntity` | Basic-Chaos | Transporte pulsado de itens em rede de tubos |
| **Logística** | Routing Entity (Splitter/Merger) | `RoutingEntity` | Infra | Balanceamento e fusão de fluxo de itens |
| **Armazenamento** | Single-Block Batteries | `AbstractBatteryBlockEntity` | Basic-Zenith | Armazenamento compacto estático |
| **Defesa/Ambiente** | Orbital Deflector | `OrbitalDeflectorEntity` | Darklight | Varredura e desintegração de meteoros |
| **Interdimensional** | Gateway Terminal | `GatewayTerminalEntity` | Stellar | Portal de trânsito intra/interdimensional |
| **Locomoção** | Coord. Teleportation Device | `ItemCoordinateTeleportationDevice` | Nova | Salto dimensional de coordenadas exatas |
| **Manufatura** | Chaos Anvil | `ChaosAnvilEntity` | Chaos | Forjamento quântico com custo em Exa-FE |
| **Manufatura** | Dimensional Augmentor | `AugmentorEntity` | Nova | Infusão e aprimoramento de equipamentos |
| **Controle** | Machine Controller / Anchor | `InfrastructureEntity` | Infra | Carregamento de chunks e travamento lógico |

---

## 3. Geradores

### 3.1 Basic Coal Generator
- **Classe:** `com.chaosmod.content.machines.generators.BasicCoalGeneratorEntity`
- **Buffer de Energia:** $128.000\text{ FE}$ (128 kFE) 🟢 *Valor confirmado*
- **Taxa de Entrada:** $0\text{ FE/t}$ (Gerador puro, não recebe carga) 🟢 *Valor confirmado*
- **Taxa de Exportação:** $2.000\text{ FE/t}$ (2 kFE/t) 🟢 *Valor confirmado*
- **Combustível Aceito:** `Items.COAL`, `Items.CHARCOAL` 🟢 *Valor confirmado*
- **Mecânica de Queima:** Conversão instantânea de 1 item em energia no momento em que o buffer possui espaço de pelo menos $32.000\text{ FE}$.
- **Geração Bruta por Combustível:** $32.000\text{ FE}$ por carvão/carvão vegetal 🟢 *Valor confirmado*
- **Dreno Ocioso (Idle Decay):** $1\text{ FE/t}$ quando ocioso sem gerar 🟢 *Valor confirmado*
- **Rendimento Líquido:** $32.000\text{ FE}$ (menos dreno durante esvaziamento do buffer) 🔵 *Valor derivado*

### 3.2 Void Coal Generator
- **Classe:** `com.chaosmod.content.machines.generators.VoidCoalGeneratorEntity`
- **Buffer de Energia:** $4.000.000\text{ FE}$ (4 MFE) 🟢 *Valor confirmado*
- **Taxa de Entrada:** $0\text{ FE/t}$ 🟢 *Valor confirmado*
- **Taxa de Exportação:** $16.000\text{ FE/t}$ (16 kFE/t) 🟢 *Valor confirmado*
- **Combustível Aceito:** Estritamente `ModItems.SHADOW_INFUSED_COAL` (Rejeita carvão comum e charcoal) 🟢 *Valor confirmado*
- **Mecânica de Queima:** Conversão no tick em que o buffer comporta $256.000\text{ FE}$.
- **Geração Bruta por Combustível:** $256.000\text{ FE}$ por Shadow Infused Coal 🟢 *Valor confirmado*
- **Dreno Ocioso (Idle Decay):** $2\text{ FE/t}$ contínuo 🟢 *Valor confirmado*
- **Rendimento Líquido:** $256.000\text{ FE}$ por combustível processado 🔵 *Valor derivado*

### 3.3 Stellar Generator
- **Classe:** `com.chaosmod.content.machines.generators.StellarGeneratorEntity`
- **Buffer de Energia:** $20.000.000\text{ FE}$ (20 MFE) 🟢 *Valor confirmado*
- **Taxa de Entrada:** $0\text{ FE/t}$ 🟢 *Valor confirmado*
- **Taxa de Exportação:** $64.000\text{ FE/t}$ (64 kFE/t) 🟢 *Valor confirmado*
- **Geração Nominal:** Até $64.000\text{ FE/t}$ 🟢 *Valor confirmado*
- **Fórmula de Operação:**
  $$\text{Geração} = \text{round}\left(64.000 \times \text{daytimeFactor} \times (1.0 - 0.75 \times \text{rainFactor})\right)$$
  - Requer visão desobstruída para o céu (`canSeeSky`).
  - Geração nula durante a noite sem modificadores dimensionais.
- **Rendimento em 1 Dia de Minecraft (24.000 ticks):** Aproximadamente $384.000.000\text{ FE}$ a $460.000.000\text{ FE}$ em céu limpo 🟡 *Valor estimado*

### 3.4 Darklight Generator
- **Classe:** `com.chaosmod.content.machines.generators.DarklightGeneratorEntity`
- **Buffer de Energia:** $100.000.000\text{ FE}$ (100 MFE) 🟢 *Valor confirmado*
- **Taxa de Entrada:** $0\text{ FE/t}$ 🟢 *Valor confirmado*
- **Taxa de Exportação:** $1.000.000\text{ FE/t}$ (1 MFE/t) 🟢 *Valor confirmado*
- **Geração Nominal:** Até $1.000.000\text{ FE/t}$ 🟢 *Valor confirmado*
- **Fórmula de Operação:**
  $$\text{Geração} = 1.000.000 \times \left(\frac{15 - \text{LightLevel}}{15}\right)^2$$
  - No escuro absoluto (Luz 0): $1.000.000\text{ FE/t}$ (1 MFE/t) 🟢 *Valor confirmado*
  - Em nível de luz 15: $0\text{ FE/t}$ 🟢 *Valor confirmado*
- **Constante Inativa Identificada:** Constante `EXPORT_THRESHOLD = 0.95f` declarada no código, porém não avaliada no método `serverTick()` (exportação ocorre continuamente enquanto houver carga).

### 3.5 Stellar Reactor (Multibloco 11×11×11)
- **Classe:** `net.marsim.chaosmod.stellar.StellarMachineEntity` / `StellarConfig`
- **Buffer de Energia:** $256.000.000\text{ FE}$ (256 MFE) 🟢 *Valor confirmado*
- **Capacidade do Tanque de Combustível:** $16.000\text{ mB}$ de Stellar Liquid 🟢 *Valor confirmado*
- **Taxa de Exportação:** $2.000.000\text{ FE/t}$ (2 MFE/t) por porta de energia 🟢 *Valor confirmado*
- **Requisito de Ignição:** $5.000.000\text{ FE}$ injetados $+ 4.000\text{ mB}$ de Stellar Liquid 🟢 *Valor confirmado*
- **Parâmetros por Modo de Operação:**
  1. **Low Mode:**
     - Consumo: $0,833\text{ mB/t}$ ($25\text{ mB}$ a cada 30 ticks)
     - Geração: $128.000\text{ FE/t}$ (128 kFE/t)
     - Rendimento: $153.600.000\text{ FE}$ por balde ($1.000\text{ mB}$) 🔵 *Valor derivado*
  2. **Normal Mode:**
     - Consumo: $3,333\text{ mB/t}$ ($100\text{ mB}$ a cada 30 ticks)
     - Geração: $256.000\text{ FE/t}$ (256 kFE/t)
     - Rendimento: $76.800.000\text{ FE}$ por balde ($1.000\text{ mB}$) 🔵 *Valor derivado*
  3. **High Mode:**
     - Consumo: $6,667\text{ mB/t}$ ($200\text{ mB}$ a cada 30 ticks)
     - Geração: $512.000\text{ FE/t}$ (512 kFE/t)
     - Rendimento: $76.800.000\text{ FE}$ por balde ($1.000\text{ mB}$) 🔵 *Valor derivado*

### 3.6 Darklight Plasma Generator (Multibloco 31×36×31)
- **Classe:** `net.marsim.chaosmod.plasma.PlasmaMachineEntity` / `PlasmaConfig`
- **Buffer de Armazenamento:** $1.000.000.000.000\text{ FE}$ (1 TFE) 🟢 *Valor confirmado*
- **Tanques Internos:** $64.000\text{ mB}$ de Darklight Liquid $+ 64.000\text{ mB}$ de Stellar Liquid 🟢 *Valor confirmado*
- **Taxa de Exportação das Portas:** $32.000.000\text{ FE/t}$ (32 MFE/t) por Energy Port conectada 🟢 *Valor confirmado*
- **Modos de Reação:**
  - **Modo 0: Darklight Reaction:**
    - Combustível: $50\text{ mB/t}$ de Darklight Liquid.
    - Geração Pico: $24.000.000\text{ FE/t}$ (24 MFE/t) a 100% de saturação.
    - Curva de Eficiência de Combustível: $\eta = 1,25 - 0,25 \times \text{sat}$.
    - Geração Bruta: $480.000.000\text{ FE}$ por $1.000\text{ mB}$ (1 Darklight Bar).
    - Custo de Liquefação no Stellar Liquefier: $153.600.000\text{ FE}$.
    - **Saldo Líquido:** $+326.400.000\text{ FE}$ por Darklight Bar (+326,4 MFE) 🔵 *Valor derivado*.
  - **Modo 1: Stellar Resonance:**
    - Combustível: $50\text{ mB/t}$ de Darklight Liquid $+ 15\text{ mB/t}$ de Stellar Liquid.
    - Geração Pico: $32.000.000\text{ FE/t}$ (32 MFE/t) a 100% de saturação.
    - Geração Bruta: $640.000.000\text{ FE}$ por $1.000\text{ mB}$ de Darklight ($+300\text{ mB}$ Stellar).
    - Custo de Liquefação: $153.600.000\text{ FE}$ (Darklight) $+ 5.760.000\text{ FE}$ (Stellar) = $159.360.000\text{ FE}$.
    - **Saldo Líquido:** $+480.640.000\text{ FE}$ (+480,64 MFE) por barra processada 🔵 *Valor derivado*.
- **Automação Inteligente:** *Auto-Fallback* imediato para o Modo 0 se o Stellar Liquid esgotar; modulação *On-Demand* que reduz a saturação para 0% caso o buffer esteja cheio e não haja exportação.

### 3.7 Nova Reactor (Multibloco Modular 3×3×3 a 13×13×13)
- **Classe:** `com.chaosmod.content.machines.multiblocks.nova.NovaReactorControllerEntity`
- **Buffer de Energia:** $10.000.000.000\text{ FE}$ (10 GFE) 🟢 *Valor confirmado*
- **Taxa de Exportação:** $10.000.000.000\text{ FE/t}$ (10 GFE/t) 🟢 *Valor confirmado*
- **Escalonamento Modular por Tamanho de Carcaça:**
  - $3\times 3$: 1 unidade ativa $\rightarrow 16.000.000\text{ FE/t}$ (16 MFE/t pico)
  - $5\times 5$: 3 unidades ativas $\rightarrow 48.000.000\text{ FE/t}$ (48 MFE/t pico)
  - $7\times 7$: 7 unidades ativas $\rightarrow 112.000.000\text{ FE/t}$ (112 MFE/t pico)
  - $9\times 9$: 14 unidades ativas $\rightarrow 224.000.000\text{ FE/t}$ (224 MFE/t pico)
  - $11\times 11$: 25 unidades ativas $\rightarrow 400.000.000\text{ FE/t}$ (400 MFE/t pico)
  - $13\times 13$: 40 unidades ativas $\rightarrow 640.000.000\text{ FE/t}$ (640 MFE/t pico)
- **Consumo de Combustível Interno:** $20\text{ unidades/t}$ por unidade modular ativa.
- **Rendimento do Combustível:** 1 item de combustível sólido $= 10.000\text{ unidades}$ (dura 500 ticks = 25 segundos por unidade).
- **Geração de Resíduo (Waste):** $10\text{ unidades/t}$.
- **Equilíbrio Térmico:**
  $$\text{Eficiência} = \left(1 - \left(\frac{\text{Heat} - 25.000}{25.000}\right)^2\right) \times \left(1 - \frac{\text{Waste}}{\text{MaxWaste}}\right)$$
  - Ponto de máxima geração com carcaça a $25.000^\circ\text{C}$.
  - Risco de meltdown se a temperatura ultrapassar $45.000^\circ\text{C}$.

### 3.8 Zenith Accelerator (Anel Acelerador + Center Rod)
- **Classe:** `com.chaosmod.content.machines.accelerator.ZenithAcceleratorInputEntity`
- **Buffer de Entrada:** $512.000.000\text{ FE}$ (512 MFE) 🟢 *Valor confirmado*
- **Taxa de Entrada Máxima:** $32.000.000\text{ FE/t}$ (32 MFE/t) 🟢 *Valor confirmado*
- **Consumo em Corrida:** $8.000.000\text{ FE/t}$ (8 MFE/t) contínuos 🟢 *Valor confirmado*
- **Consumo de Partículas:** 0,1 a 0,2 `Zenith Bar` por tick enquanto mantém feixes em circulação.
- **Mecânica de Colisão Quântica:**
  - Acionada quando carga $\ge 50\%$.
  - Produz $\lfloor 16 \times \text{Carga} \rfloor$ `Reality Condensate` (16 unidades a 100% de carga).
  - **Injeção de Retorno de Energia (Power Surge):**
    $$E_{\text{surge}} = 10.000.000.000.000\text{ FE} \times \text{Carga}^2 \quad (\text{Até } 10\text{ TFE})$$
  - Injeta instantaneamente na `CenterAcceleratorRodEntity` (Capacidade: $100\text{ TFE}$, vazão de saída: $100\text{ TFE/t}$).

### 3.9 Chaos Cube
- **Classe:** `com.chaosmod.content.machines.generators.ChaosCubeEntity`
- **Buffer de Armazenamento:** $9.000.000.000.000.000.000\text{ FE}$ (9 EFE) 🟢 *Valor confirmado*
- **Taxa de Exportação:** $9.000.000.000.000.000.000\text{ FE/t}$ (9 EFE/t) 🟢 *Valor confirmado*
- **Geração Passiva:** **$10.000.000.000.000.000\text{ FE/t}$ (10 PFE/t)** 🟢 *Valor confirmado*
- **Condição Operacional:** Nenhuma. Não consome combustível, não requer catalisador, não gera resíduos, não sofre desgaste.

---

## 4. Refinadoras (Refiners)

As refinadoras reduzem a duração do ciclo de processamento a cada tier, aumentando proporcionalmente a taxa de consumo em FE/t.

| Tier | Refinadora | Buffer Interno | Taxa Entrada Máx | Consumo por Tick | Duração (Ticks) | Duração (s) | Energia Total por Operação |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Basic** | Basic Refiner | $64.000\text{ FE}$ | $3.000\text{ FE/t}$ | $500\text{ FE/t}$ | 300 t | 15,0 s | $150.000\text{ FE}$ (150 kFE) |
| **Void** | Void Refiner | $512.000\text{ FE}$ | $24.000\text{ FE/t}$ | $4.000\text{ FE/t}$ | 150 t | 7,5 s | $600.000\text{ FE}$ (600 kFE) |
| **Stellar** | Stellar Refiner | $4.096.000\text{ FE}$ | $192.000\text{ FE/t}$ | $32.000\text{ FE/t}$ | 100 t | 5,0 s | $3.200.000\text{ FE}$ (3,2 MFE) |
| **Darklight** | Darklight Refiner | $32.768.000\text{ FE}$ | $1.536.000\text{ FE/t}$ | $256.000\text{ FE/t}$ | 60 t | 3,0 s | $15.360.000\text{ FE}$ (15,36 MFE) |
| **Nova** | Nova Refiner | $256.000.000\text{ FE}$ | $12.000.000\text{ FE/t}$ | $2.000.000\text{ FE/t}$ | 20 t | 1,0 s | $40.000.000\text{ FE}$ (40 MFE) |
| **Zenith** | Zenith Refiner | $128.000.000.000\text{ FE}$ | $8.000.000.000\text{ FE/t}$ | **$1.000.000.000\text{ FE/t}$** | 10 t | 0,5 s | **$10.000.000.000\text{ FE}$ (10 GFE)** |
| **Chaotic** | Chaotic Refiner | $16.384.000.000\text{ FE}$ | $768.000.000\text{ FE/t}$ | $128.000.000\text{ FE/t}$ | 5 t | 0,25 s | **$640.000.000\text{ FE}$ (640 MFE)** |

> [!WARNING]
> **Anomalia de Balanceamento (Zenith vs Chaotic):** O *Zenith Refiner* consome $1\text{ GFE/t}$ totalizando **$10\text{ GFE}$ por item processado**, enquanto o *Chaotic Refiner* consome $128\text{ MFE/t}$ totalizando apenas **$640\text{ MFE}$ por item** (uma redução de 15,6 vezes no custo energético no tier superior).

---

## 5. Extratoras (Extractors)

As extratoras filtram matérias-primas e partículas raras a partir de pedras brutas e blocos base.

| Tier | Extratora | Buffer Interno | Taxa Entrada Máx | Consumo por Tick | Duração (Ticks) | Duração (s) | Energia Total por Operação |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Basic** | Basic Extractor | $128.000\text{ FE}$ | $12.000\text{ FE/t}$ | $2.000\text{ FE/t}$ | 300 t | 15,0 s | $600.000\text{ FE}$ (600 kFE) |
| **Void** | Void Extractor | $1.024.000\text{ FE}$ | $96.000\text{ FE/t}$ | $16.000\text{ FE/t}$ | 220 t | 11,0 s | $3.520.000\text{ FE}$ (3,52 MFE) |
| **Darklight** | Darklight Extractor | $64.000.000\text{ FE}$ | $6.000.000\text{ FE/t}$ | $1.000.000\text{ FE/t}$ | 150 t | 7,5 s | $150.000.000\text{ FE}$ (150 MFE) |
| **Nova** | Nova Extractor | $512.000.000\text{ FE}$ | $48.000.000\text{ FE/t}$ | $8.000.000\text{ FE/t}$ | 90 t | 4,5 s | $720.000.000\text{ FE}$ (720 MFE) |
| **Zenith** | Zenith Extractor | $512.000.000.000\text{ FE}$ | $32.000.000.000\text{ FE/t}$ | **$4.000.000.000\text{ FE/t}$** | 40 t | 2,0 s | **$160.000.000.000\text{ FE}$ (160 GFE)** |
| **Chaotic** | Chaotic Extractor | $32.768.000.000\text{ FE}$ | $3.072.000.000\text{ FE/t}$ | $512.000.000\text{ FE/t}$ | 20 t | 1,0 s | **$10.240.000.000\text{ FE}$ (10,24 GFE)** |
| **Especial** | Reality Extractor | $128.000.000\text{ FE}$ | $2.000.000\text{ FE/t}$ | $2.000.000\text{ FE/t}$ | 1.200 t | 60,0 s | $2.400.000.000\text{ FE}$ (2,4 GFE) |

> [!CAUTION]
> **Inconsistência de Buffer no Reality Extractor:** O *Reality Extractor* possui um buffer de apenas $128\text{ MFE}$, mas a operação consome $2.400\text{ MFE}$ ($2,4\text{ GFE}$). Ele depende obrigatoriamente de uma linha ininterrupta fornecendo $2\text{ MFE/t}$ exatos em todos os 1.200 ticks do ciclo. Se houver interrupção de 1 tick, a máquina trava.

---

## 6. Máquinas Multibloco de Processamento

### 6.1 Stellar Liquefier (3×3×3)
- **Estrutura:** Carcaça cúbica 3×3×3 com Casings e Core central.
- **Buffer de Energia:** $64.000.000\text{ FE}$ (64 MFE) | **Entrada Máx:** $200.000\text{ FE/t}$ (200 kFE/t) 🟢 *Valor confirmado*
- **Tanque de Armazenamento:** $32.000\text{ mB}$ de fluidos combinados 🟢 *Valor confirmado*
- **Receitas de Liquefação:**
  1. **Stellar Liquid:**
     - Consumo: $1\times\text{Stellar Bar}$ $+ 16.000\text{ FE/t}$ por 1.200 ticks (60 s).
     - Energia Total: $19.200.000\text{ FE}$ (19,2 MFE).
     - Rendimento: $1.000\text{ mB}$ de Stellar Liquid.
  2. **Darklight Liquid:**
     - Consumo: $1\times\text{Darklight Bar}$ $+ 64.000\text{ FE/t}$ por 2.400 ticks (120 s).
     - Energia Total: $153.600.000\text{ FE}$ (153,6 MFE).
     - Rendimento: $1.000\text{ mB}$ de Darklight Liquid.

### 6.2 Matter Inversion Chamber (13×13×13)
- **Classe:** `net.marsim.chaosmod.inversion.InversionMachineEntity` / `InversionConfig`
- **Estrutura:** Geometria fechada 13×13×13 baseada em NBT com tanques integrados e 4 emissores.
- **Buffer de Energia:** $1.000.000.000\text{ FE}$ (1 GFE) | **Tanques:** $64.000\text{ mB}$ Stellar $+ 64.000\text{ mB}$ Darklight 🟢 *Valor confirmado*
- **Receita Base:** $1.000\text{ mB}$ Stellar Liquid $+ 500\text{ mB}$ Darklight Liquid $\rightarrow$ **$1\times\text{Antimatter}$** 🟢 *Valor confirmado*
- **Modos de Intensidade:**
  - **Safe Mode:** 900 ticks (45 s) @ $300.000\text{ FE/t} = 270.000.000\text{ FE}$ (270 MFE)
  - **Balanced Mode:** 600 ticks (30 s) @ $600.000\text{ FE/t} = 360.000.000\text{ FE}$ (360 MFE)
  - **Overdriven Mode:** 300 ticks (15 s) @ $1.600.000\text{ FE/t} = 480.000.000\text{ FE}$ (480 MFE)

### 6.3 Event Horizon Forge (15×15×15)
- **Classe:** `net.marsim.chaosmod.horizon.HorizonMachineEntity` / `HorizonConfig`
- **Buffer de Energia:** $10.000.000.000\text{ FE}$ (10 GFE) | **Tanque de Fluido:** $128.000\text{ mB}$ Darklight Liquid 🟢 *Valor confirmado*
- **Processos Operacionais:**
  1. **Condensação de Singularity (Black Hole):**
     - Insumos: $16\times\text{Antimatter} + 16.000\text{ mB Darklight Liquid}$.
     - Tempo e Energia: 2.400 ticks (120 s) @ $1.000.000\text{ FE/t} = 2.400.000.000\text{ FE}$ (2,4 GFE).
     - Resultado: **$1\times\text{Black Hole}$** contendo $1.024\text{ unidades de massa}$.
  2. **Transmutações de Singularity (Consome massa do Black Hole no slot central):**
     - **Nova Fragment:** 1 Darklight Bar + 250 mB Darklight + 1 massa + 200 t @ 500 kFE/t ($100\text{ MFE}$) $\rightarrow 1\times\text{Nova Fragment}$.
     - **Stellar Core:** 4 Stellar Bar + 500 mB Darklight + 2 massas + 300 t @ 750 kFE/t ($225\text{ MFE}$) $\rightarrow 1\times\text{Stellar Core}$.
     - **Darklight Core:** 4 Darklight Bar + 1.000 mB Darklight + 4 massas + 400 t @ 1 MFE/t ($400\text{ MFE}$) $\rightarrow 1\times\text{Darklight Core}$.
     - **Nova Core:** 4 Nova Fragment + 2.000 mB Darklight + 8 massas + 500 t @ 1,5 MFE/t ($750\text{ MFE}$) $\rightarrow 1\times\text{Nova Core}$.
     - **Zenith Bar:** 1 Reality Condensate + 4.000 mB Darklight + 16 massas + 600 t @ 2 MFE/t ($1,2\text{ GFE}$) $\rightarrow 1\times\text{Zenith Bar}$.
     - **Zenith Core:** 4 Zenith Bar + 8.000 mB Darklight + 32 massas + 800 t @ 3 MFE/t ($2,4\text{ GFE}$) $\rightarrow 1\times\text{Zenith Core}$.

### 6.4 Stellar Energy Matrix (Multibloco Escalável 5×5×5 a 64×64×64)
- **Estrutura:** Casing externo com núcleos internos formados por células de energia (`Matrix Cells`) e bobinas de indução (`Induction Coils`).
- **Valores por Bloco Unitário Interno:**
  - **Células de Armazenamento:**
    - Basic: $+100.000.000\text{ FE}$ (100 MFE)
    - Void: $+10.000.000.000\text{ FE}$ (10 GFE)
    - Stellar: $+500.000.000.000\text{ FE}$ (500 GFE)
    - Darklight: $+10.000.000.000.000\text{ FE}$ (10 TFE)
    - Nova: $+500.000.000.000.000\text{ FE}$ (500 TFE)
    - Zenith: $+10.000.000.000.000.000\text{ FE}$ (10 PFE)
  - **Bobinas de Indução (Vazão de E/S por bloco):**
    - Basic Coil: $+10.000.000\text{ FE/t}$ (10 MFE/t)
    - Void Coil: $+1.000.000.000\text{ FE/t}$ (1 GFE/t)
    - Stellar Coil: $+50.000.000.000\text{ FE/t}$ (50 GFE/t)
    - Darklight Coil: $+1.000.000.000.000\text{ FE/t}$ (1 TFE/t)
    - Nova Coil: $+50.000.000.000.000\text{ FE/t}$ (50 TFE/t)
    - Zenith Coil: $+1.000.000.000.000.000\text{ FE/t}$ (1 PFE/t)
- **Proteção Contra Overflow:** Implementa `addSafe(long a, long b)` para evitar transbordamento aritmético de 64 bits (`Long.MAX_VALUE`).

---

## 7. Cabos e Sistemas de Transferência / Logística

### 7.1 Cabos de Energia (`Energy Cables`)
Transmissão instantânea por grafo conectado de blocos:

| Tier | Nome do Cabo | Taxa de Transferência | Comparação com Tiers de Máquinas |
| :--- | :--- | :--- | :--- |
| **Basic** | Basic Energy Cable | **$1.000\text{ FE/t}$** (1 kFE/t) | Alimenta Basic Refiner (500 FE/t) com sobra de 500 FE/t |
| **Void** | Void Cable | **$100.000\text{ FE/t}$** (100 kFE/t) | Alimenta Void Refiner (4 kFE/t) e Void Extractor (16 kFE/t) |
| **Darklight** | Darklight Cable | **$100.000.000\text{ FE/t}$** (100 MFE/t) | Gargalo severo para máquinas Zenith ($1\text{ GFE/t}$) |
| **Nova** | Nova Cable | **$100.000.000.000\text{ FE/t}$** (100 GFE/t) | Suporta todas as máquinas Nova e Zenith Refiner |
| **Zenith** | Zenith Cable | **$100.000.000.000.000\text{ FE/t}$** (100 TFE/t) | Suporta picos do Zenith Accelerator Rod |
| **Chaos** | Chaos Cable | **$10.000.000.000.000.000\text{ FE/t}$** (10 PFE/t) | Escala do Chaos Cube ($10\text{ PFE/t}$) |

### 7.2 Cabos de Fluido (`Fluid Cables`)
- Basic: $250\text{ mB/t}$
- Void: $1.000\text{ mB/t}$
- Darklight: $8.000\text{ mB/t}$
- Nova: $32.000\text{ mB/t}$
- Zenith: $128.000\text{ mB/t}$
- Chaos: $256.000\text{ mB/t}$

### 7.3 Transportadores de Itens (`Transporter Cables`)
- Basic: 1 item a cada 20 ticks ($1,0\text{ item/s}$)
- Void: 4 itens a cada 10 ticks ($8,0\text{ itens/s}$)
- Darklight: 16 itens a cada 5 ticks ($64,0\text{ itens/s}$)
- Nova: 64 itens a cada 2 ticks ($640,0\text{ itens/s}$)
- Zenith: 256 itens a cada tick ($5.120,0\text{ itens/s}$)
- Chaos: 640 itens a cada tick ($12.800,0\text{ itens/s}$)

---

## 8. Baterias Monobloco (`Single-Block Batteries`)

| Tier | Bateria | Capacidade Máxima (FE) | Vazão Máxima E/S (FE/t) | Tempo Mínimo de Descarga Total |
| :--- | :--- | :--- | :--- | :--- |
| **Basic** | Basic Battery | $1.000.000\text{ FE}$ (1 MFE) | $12.000\text{ FE/t}$ | 83,3 s (1.666 ticks) |
| **Void** | Void Battery | $100.000.000\text{ FE}$ (100 MFE) | $2.000.000\text{ FE/t}$ (2 MFE/t) | 50,0 s (1.000 ticks) |
| **Stellar** | Stellar Battery | $100.000.000.000\text{ FE}$ (100 GFE) | $10.000.000.000\text{ FE/t}$ (10 GFE/t) | 10,0 s (200 ticks) |
| **Darklight** | Darklight Battery | $5.000.000.000.000\text{ FE}$ (5 TFE) | $500.000.000.000\text{ FE/t}$ (500 GFE/t) | 10,0 s (200 ticks) |
| **Nova** | Nova Battery | $100.000.000.000.000\text{ FE}$ (100 TFE) | $100.000.000.000.000\text{ FE/t}$ | 1,0 s (20 ticks) |
| **Zenith** | Zenith Battery | $1.000.000.000.000.000\text{ FE}$ (1 PFE) | $1.000.000.000.000.000\text{ FE/t}$ | 1,0 s (20 ticks) |

---

## 9. Módulos de Upgrade e Sistema de Estabilidade

### 9.1 Multiplicadores de Módulos (`MachineUpgrades.java`)
- **Speed Upgrade:**
  - Redução de tempo por nível: $t' = t \times (0,75)^n$.
  - Aumento de consumo energético: $E' = E \times (1,45)^n$.
  - Com 4 upgrades: tempo reduzido a $31,6\%$, consumo aumentado em $442\%$.
- **Energy Upgrade:**
  - Redução linear de consumo de até $50\%$ no nível máximo.

### 9.2 Sistema de Estabilidade (`RefinerStability.java`)
- **Estabilidade Máxima:** $40.000\text{ pontos}$.
- **Consumo de Estabilidade:** Ocorre durante o refino ativo em função do tier da máquina:
  - Basic: $-1\text{ ponto/t}$
  - Void: $-2\text{ pontos/t}$
  - Stellar: $-4\text{ pontos/t}$
  - Darklight: $-8\text{ pontos/t}$
  - Nova: $-16\text{ pontos/t}$
  - Zenith: $-32\text{ pontos/t}$
  - Chaotic: $-64\text{ pontos/t}$
- **Consequências de Instabilidade (Estabilidade < 10%):**
  - Perda do item de entrada sem gerar produto.
  - Explosão localizada de radiação de Flux.
  - Contaminação de blocos vizinhos com resíduos instáveis.
- **Itens de Restauração:**
  - `Flux Crystal`: $+2.000\text{ pontos}$.
  - `Empowered Flux Crystal`: $+10.000\text{ pontos}$.
  - `Reality Condensate`: $+40.000\text{ pontos}$ (Restauração total imediata).

---

## 10. Receitas e Custos de Recursos da Progressão

### Árvore Progressiva de Matérias-Primas
1. **Void Bar:** Minério de Ferro / Carvão $+ 600\text{ kFE}$ (Void Refiner).
2. **Stellar Bar:** Void Bar + Ouro $+ 3,2\text{ MFE}$ (Stellar Refiner).
3. **Darklight Bar:** Stellar Bar + Obsidian $+ 15,36\text{ MFE}$ (Darklight Refiner).
4. **Stellar Liquid (1.000 mB):** 1 Stellar Bar $+ 19,2\text{ MFE}$ (Liquefier).
5. **Darklight Liquid (1.000 mB):** 1 Darklight Bar $+ 153,6\text{ MFE}$ (Liquefier).
6. **Antimatter (1 unidade):** $1.000\text{ mB}$ Stellar Liquid $+ 500\text{ mB}$ Darklight Liquid $+ 270\text{ MFE}$ (Safe Mode na Chamber).
7. **Black Hole (1.024 massas):** $16\times\text{Antimatter} + 16.000\text{ mB Darklight Liquid} + 2,4\text{ GFE}$ (Horizon Forge).
8. **Nova Fragment:** 1 Darklight Bar $+ 250\text{ mB Darklight Liquid} + 1\text{ massa de Black Hole} + 100\text{ MFE}$ (Horizon Forge).
9. **Zenith Bar:** 1 Reality Condensate $+ 4.000\text{ mB Darklight Liquid} + 16\text{ massas de Black Hole} + 1,2\text{ GFE}$ (Horizon Forge).
10. **Chaotic Bar:** Zenith Bar $+ 640\text{ MFE}$ (Chaotic Refiner).

---

## 11. Cadeias Produtivas Industriais (Análise Matemática)

### Cadeia A — Obtenção de Darklight Liquid (Combustível Primário)
- **Entrada:** 1 Darklight Bar.
- **Processamento:** Stellar Liquefier (2.400 ticks @ $64\text{ kFE/t}$).
- **Custo Energético:** $153.600.000\text{ FE}$ (153,6 MFE).
- **Rendimento:** $1.000\text{ mB}$ de Darklight Liquid.

### Cadeia B — Síntese de Antimatter
- **Entradas:** $1.000\text{ mB}$ Stellar Liquid $+ 500\text{ mB}$ Darklight Liquid.
- **Custos Anteriores (Liquefação dos Fluidos):**
  - $1.000\text{ mB}$ Stellar Liquid $= 19,2\text{ MFE}$.
  - $500\text{ mB}$ Darklight Liquid $= 76,8\text{ MFE}$.
  - Total em fluidos: $96,0\text{ MFE}$.
- **Consumo na Matter Inversion Chamber:**
  - Modo Seguro (Safe): $270,0\text{ MFE}$ (Custo Total: **$366,0\text{ MFE}$**).
  - Modo Equilibrado (Balanced): $360,0\text{ MFE}$ (Custo Total: **$456,0\text{ MFE}$**).
  - Modo Forçado (Overdriven): $480,0\text{ MFE}$ (Custo Total: **$576,0\text{ MFE}$**).

### Cadeia C — Criação de Singularity / Black Hole
- **Entradas:** $16\times\text{Antimatter} + 16.000\text{ mB Darklight Liquid}$.
- **Custo dos Insumos Anteriores:**
  - 16 Antimatter (Safe Mode): $16 \times 366,0\text{ MFE} = 5.856,0\text{ MFE}$ ($5,856\text{ GFE}$).
  - 16.000 mB Darklight Liquid: $16 \times 153,6\text{ MFE} = 2.457,6\text{ MFE}$ ($2,4576\text{ GFE}$).
- **Processamento na Event Horizon Forge:**
  - 2.400 ticks @ $1\text{ MFE/t} = 2.400,0\text{ MFE}$ ($2,4\text{ GFE}$).
- **Custo Acumulado Total por Black Hole:** **$10.713.600.000\text{ FE}$ (10,7136 GFE)** 🔵 *Valor derivado*.
- **Rendimento:** $1\times\text{Black Hole}$ contendo $1.024\text{ unidades de massa}$.
- **Custo Efetivo por Unidade de Massa:** **$10,4625\text{ MFE}$ / massa** 🔵 *Valor derivado*.

### Cadeia D — Manufatura de Nova Fragment
- **Entradas:** 1 Darklight Bar $+ 250\text{ mB Darklight Liquid} + 1\text{ massa de Black Hole}$.
- **Custo de Fluidos e Massa:**
  - $250\text{ mB Darklight Liquid} = 38,4\text{ MFE}$.
  - 1 unidade de massa do Black Hole $= 10,4625\text{ MFE}$.
- **Processamento na Event Horizon Forge:**
  - 200 ticks @ $500\text{ kFE/t} = 100,0\text{ MFE}$.
- **Custo Acumulado por Nova Fragment (excluindo a barra):** **$148,8625\text{ MFE}$** 🔵 *Valor derivado*.

---

## 12. Dependências Quantitativas entre Máquinas (Relações Produtor/Consumidor)

Para manter plantas industriais operando em regime de $100\%$ de atividade contínua sem interrupções:

### 12.1 Suporte a 1 Darklight Plasma Generator (Modo 0: 50 mB/t Darklight Liquid)
- Consumo por segundo: $1.000\text{ mB/s}$ ($1\text{ balde/s}$).
- Cada Stellar Liquefier produz $1.000\text{ mB}$ a cada $120\text{ segundos}$ ($2.400\text{ ticks}$).
- **Relação Necessária:** São necessários **120 Stellar Liquefiers** operando em paralelo (ou um sistema de estocagem massiva com pulmão em Stellar Energy Matrix) para manter 1 Plasma Generator em carga máxima contínua.
- **Balanço Energético do Parque:**
  - Geração do Plasma Generator: $+24.000.000\text{ FE/t}$.
  - Consumo total dos 120 Liquefiers: $120 \times 64.000\text{ FE/t} = 7.680.000\text{ FE/t}$.
  - **Saldo Energético Líquido Contínuo:** $+16.320.000\text{ FE/t}$ (Lucro de $68\%$ da geração total).

### 12.2 Suporte a 1 Matter Inversion Chamber (Safe Mode: 45 s por ciclo)
- Consumo por ciclo de 45 s: $1.000\text{ mB}$ Stellar Liquid $+ 500\text{ mB}$ Darklight Liquid.
- Vazão média por segundo: $22,22\text{ mB/s}$ Stellar e $11,11\text{ mB/s}$ Darklight.
- **Relação Necessária:**
  - Stellar Liquefiers (Stellar): 2 unidades dedicadas contínuas.
  - Stellar Liquefiers (Darklight): 2 unidades dedicadas contínuas.
  - Suprimento Energético: $300.000\text{ FE/t}$ contínuos (atendido por 1 Darklight Generator no escuro ou 1 Stellar Reactor).

### 12.3 Suporte a 1 Event Horizon Forge (Condensação de Black Hole: 120 s por ciclo)
- Consumo por ciclo de 120 s: $16\times\text{Antimatter} + 16.000\text{ mB Darklight Liquid}$.
- Vazão necessária de Antimatter: $1\text{ unidade}$ a cada $7,5\text{ segundos}$.
- **Relação Necessária:**
  - Matter Inversion Chambers em Overdriven Mode (15 s/ciclo): **2 Chambers** dedicadas em ciclo alternado.
  - Matter Inversion Chambers em Safe Mode (45 s/ciclo): **6 Chambers** dedicadas.
  - Demanda de Energia da Forge: $1.000.000\text{ FE/t}$.
  - Demanda das 6 Chambers (Safe): $6 \times 300.000 = 1.800.000\text{ FE/t}$.
  - **Potência Elétrica Total da Célula de Singularidade:** **$2.800.000\text{ FE/t}$** (2,8 MFE/t).

---

## 13. Simulações Industriais Contínuas

Abaixo constam as projeções de consumo e geração acumulada em regimes ininterruptos de produção:

### Simulação 1: Darklight Plasma Generator (Modo 1: Stellar Resonance @ 100% Sat)
- **Taxa de Geração:** $32.000.000\text{ FE/t}$ ($640.000.000\text{ FE/s}$).
- **Consumo de Fluidos:** $50\text{ mB/t}$ Darklight $+ 15\text{ mB/t}$ Stellar.

| Período de Operação | Forge Energy Gerada (Bruta) | Darklight Liquid Consumido | Stellar Liquid Consumido |
| :--- | :--- | :--- | :--- |
| **1 Minuto (1.200 ticks)** | $38.400.000.000\text{ FE}$ (38,4 GFE) | $60.000\text{ mB}$ (60 baldes) | $18.000\text{ mB}$ (18 baldes) |
| **10 Minutos (12.000 ticks)** | $384.000.000.000\text{ FE}$ (384 GFE) | $600.000\text{ mB}$ (600 baldes) | $180.000\text{ mB}$ (180 baldes) |
| **1 Hora (72.000 ticks)** | $2.304.000.000.000\text{ FE}$ (2,304 TFE) | $3.600.000\text{ mB}$ (3.600 baldes) | $1.080.000\text{ mB}$ (1.080 baldes) |
| **10 Horas (720.000 ticks)**| $23.040.000.000.000\text{ FE}$ (23,04 TFE) | $36.000.000\text{ mB}$ (36.000 baldes) | $10.800.000\text{ mB}$ (10.800 baldes) |

### Simulação 2: Matter Inversion Chamber (Balanced Mode: 600 ticks / 30 s por ciclo)
- **Ciclo:** $600\text{ ticks}$ @ $600.000\text{ FE/t} = 360\text{ MFE}$ por Antimatter.

| Período de Operação | Antimatter Sintetizado | Energia Total Consumida | Stellar Liquid Consumido | Darklight Liquid Consumido |
| :--- | :--- | :--- | :--- | :--- |
| **1 Minuto** | 2 unidades | $720.000.000\text{ FE}$ (720 MFE) | $2.000\text{ mB}$ | $1.000\text{ mB}$ |
| **10 Minutos** | 20 unidades | $7.200.000.000\text{ FE}$ (7,2 GFE) | $20.000\text{ mB}$ | $10.000\text{ mB}$ |
| **1 Hora** | 120 unidades | $43.200.000.000\text{ FE}$ (43,2 GFE) | $120.000\text{ mB}$ | $60.000\text{ mB}$ |
| **10 Horas** | 1.200 unidades | $432.000.000.000\text{ FE}$ (432 GFE) | $1.200.000\text{ mB}$ | $600.000\text{ mB}$ |

---

## 14. Gargalos, Anomalias e Inconsistências Identificadas

### Matriz de Severidade das Inconsistências no Código Atual

| Nível de Severidade | Componente Envolvido | Anomalia Identificada no Código | Impacto no Jogo |
| :--- | :--- | :--- | :--- |
| 🔴 **Crítico** | `ChaosCubeEntity` | Geração incondicional de **$10\text{ PFE/t}$** sem consumo de recursos, sem perda de durabilidade e sem condições dimensionais. | Anula a necessidade de qualquer outra infraestrutura de energia a partir do momento em que 1 bloco é colocado. |
| 🔴 **Crítico** | `ZenithRefinerEntity` vs `ChaoticRefinerEntity` | O Zenith Refiner consome **$1\text{ GFE/t}$ ($10\text{ GFE}$ total)** enquanto o Chaotic Refiner consome **$128\text{ MFE/t}$ ($640\text{ MFE}$ total)**. | Inversão bizarra de progressão: o tier final Chaos custa 15,6× MENOS energia que o tier anterior Zenith. |
| 🔴 **Crítico** | `ZenithExtractorEntity` vs `ChaoticExtractorEntity` | O Zenith Extractor consome **$4\text{ GFE/t}$ ($160\text{ GFE}$ total)** enquanto o Chaotic Extractor consome **$512\text{ MFE/t}$ ($10,24\text{ GFE}$ total)**. | Mesma inversão de escala energética: o tier Chaos consome 15,6× menos energia que o tier Zenith. |
| 🟠 **Alto** | `Darklight Cable` vs `Zenith Extractor` | O Darklight Cable tem limite de $100\text{ MFE/t}$. O Zenith Extractor exige $4.000\text{ MFE/t}$ ($4\text{ GFE/t}$). | Impossível alimentar o Zenith Extractor com cabos de Darklight; o jogador é forçado a pular diretamente para cabos de Nova ($100\text{ GFE/t}$). |
| 🟠 **Alto** | `RealityExtractorEntity` | Buffer de apenas $128\text{ MFE}$ para uma operação de 60 segundos que demanda $2.400\text{ MFE}$ ininterruptos. | Qualquer oscilação infinitesimal na rede elétrica reinicia ou falha o ciclo de 60 segundos. |
| 🟡 **Médio** | `DarklightGeneratorEntity` | Campo `EXPORT_THRESHOLD = 0.95f` declarado como constante morta nunca consultada. | A máquina exporta energia a qualquer percentual de buffer, contrariando o comportamento documentado originalmente. |
| 🟡 **Médio** | `VoidCoalGeneratorEntity` | Rejeita carvão e carvão vegetal sem nenhuma mensagem ou fallback. Aceita exclusivamente `SHADOW_INFUSED_COAL`. | Jogadores sem JEI acreditam que a máquina está bugada ao inserir carvão convencional. |
| 🟢 **Baixo** | `InversionScreen` / Telas | Quebras de linha e concatenação de textos longos em resoluções menores que GUI Scale 3. | Sobreposição visual de rótulos em monitores menores (ex: textos de status e coordenadas). |

---

## 15. Tabelas Consolidadas de Comparação

### Comparativo Geral de Máquinas Monobloco (Energia e Tempo por Operação Base)

| Tier | Refinadora (Tempo / Consumo) | Extratora (Tempo / Consumo) | Bateria (Capacidade / Saída) | Cabo de Energia (Capacidade) |
| :--- | :--- | :--- | :--- | :--- |
| **Basic** | 15,0 s @ 500 FE/t ($150\text{ kFE}$) | 15,0 s @ 2 kFE/t ($600\text{ kFE}$) | 1 MFE (12 kFE/t) | $1.000\text{ FE/t}$ |
| **Void** | 7,5 s @ 4 kFE/t ($600\text{ kFE}$) | 11,0 s @ 16 kFE/t ($3,52\text{ MFE}$) | 100 MFE (2 MFE/t) | $100.000\text{ FE/t}$ |
| **Stellar** | 5,0 s @ 32 kFE/t ($3,2\text{ MFE}$) | — | 100 GFE (10 GFE/t) | — |
| **Darklight** | 3,0 s @ 256 kFE/t ($15,36\text{ MFE}$) | 7,5 s @ 1 MFE/t ($150\text{ MFE}$) | 5 TFE (500 GFE/t) | $100.000.000\text{ FE/t}$ |
| **Nova** | 1,0 s @ 2 MFE/t ($40\text{ MFE}$) | 4,5 s @ 8 MFE/t ($720\text{ MFE}$) | 100 TFE (100 TFE/t) | $100.000.000.000\text{ FE/t}$ |
| **Zenith** | 0,5 s @ 1 GFE/t ($10\text{ GFE}$) | 2,0 s @ 4 GFE/t ($160\text{ GFE}$) | 1 PFE (1 PFE/t) | $100.000.000.000.000\text{ FE/t}$ |
| **Chaotic** | 0,25 s @ 128 MFE/t ($640\text{ MFE}$) | 1,0 s @ 512 MFE/t ($10,24\text{ GFE}$) | — | $10.000.000.000.000.000\text{ FE/t}$ |

---

## 16. Fórmulas Matemáticas Implementadas

Abaixo constam as formulações exatas compiladas nos controladores de máquinas:

1. **Geração Darklight (`DarklightGeneratorEntity.java`):**
   $$P_{\text{dark}} = 1.000.000 \times \left(\frac{15 - L}{15}\right)^2 \quad [\text{FE/t}]$$
2. **Eficiência de Combustível Plasma (`PlasmaMachineEntity.java`):**
   $$\eta_{\text{plasma}} = 1,25 - 0,25 \times S_{\text{plasma}}$$
3. **Surge de Colisão do Acelerador (`ZenithAcceleratorInputEntity.java`):**
   $$E_{\text{recoil}} = 10^{13} \times C^2 \quad [\text{FE}], \quad C = \text{Carga} \in [0,5; 1,0]$$
4. **Curva Térmica do Reator Nova (`NovaReactorControllerEntity.java`):**
   $$\eta_{\text{nova}} = \left(1 - \left(\frac{T - 25.000}{25.000}\right)^2\right) \times \left(1 - \frac{W}{W_{\max}}\right)$$
5. **Dreno Dimensional (`OrbitalDeflectorEntity.java` / `InfrastructureEntity.java`):**
   $$P_{\text{idle}} = 1.000 \times R^3 \quad [\text{FE/t}], \quad R = \text{Raio de chunks}$$

---

## 17. Índice de Classes, Métodos e Arquivos Consultados

- **Constantes Energéticas Globais:**
  - `net.marsim.chaosmod.energy.ProgressionEnergy` (Linhas 1-85)
- **Geradores Monobloco:**
  - `com.chaosmod.content.machines.generators.BasicCoalGeneratorEntity`
  - `com.chaosmod.content.machines.generators.VoidCoalGeneratorEntity`
  - `com.chaosmod.content.machines.generators.StellarGeneratorEntity`
  - `com.chaosmod.content.machines.generators.DarklightGeneratorEntity`
  - `com.chaosmod.content.machines.generators.ChaosCubeEntity`
- **Refinadoras e Extratoras:**
  - `com.chaosmod.content.machines.refiners.*` (`BasicRefinerEntity` a `ChaoticRefinerEntity`)
  - `com.chaosmod.content.machines.extractors.*` (`BasicExtractorEntity` a `RealityExtractorEntity`)
  - `com.chaosmod.content.machines.stability.RefinerStability`
  - `com.chaosmod.content.machines.upgrades.MachineUpgrades`
- **Multiblocos:**
  - `net.marsim.chaosmod.stellar.StellarMachineEntity`, `StellarConfig`, `StellarStructure`
  - `net.marsim.chaosmod.plasma.PlasmaMachineEntity`, `PlasmaConfig`, `PlasmaStructure`
  - `net.marsim.chaosmod.inversion.InversionMachineEntity`, `InversionConfig`, `InversionStructure`
  - `net.marsim.chaosmod.horizon.HorizonMachineEntity`, `HorizonConfig`, `HorizonProcessRecipe`
  - `com.chaosmod.content.machines.multiblocks.nova.NovaReactorControllerEntity`, `NovaReactorStructure`
  - `com.chaosmod.content.machines.matrix.MatrixMachineEntity`, `MatrixConfig`
- **Logística e Cabos:**
  - `com.chaosmod.content.cables.energy.EnergyCableBlockEntity`
  - `com.chaosmod.content.cables.fluid.FluidCableTier`
  - `com.chaosmod.content.cables.item.TransporterBlockEntity`
  - `com.chaosmod.content.cables.routing.RoutingEntity`

---

## 18. Informações Não Determinadas ou Abertas

1. **Tempo de Retenção Residual da Matriz Energética:** O código de `MatrixMachineEntity` não impõe decaimento passivo em células desconectadas (comportamento de perda estática é 0% salvo interrupção forçada).
2. **Escala de Carga Dinâmica da Rede JEI:** A taxa instantânea exibida nas abas JEI de certas máquinas multinível espelha o valor nominal da receita padrão, mas não computa automaticamente a curva de estabilidade (`stability loss`) se módulos externos forem acoplados dinamicamente durante a execução.
