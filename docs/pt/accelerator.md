# Zenith Accelerator

[English](../en/accelerator.md)

O Zenith Accelerator atual é um multibloco de **49 × 5 × 49**. Ele usa Zenith Bars para gerar FE e Reality Condensate. Forneça energia externa para a ignição e mantenha combustível e espaço na saída. Valores energéticos e duração das fases ficam em `chaosmod-zenith-accelerator.toml`.

## Construção

Selecione **Zenith Accelerator** no [Multiblock Viewer](multiblock-viewer.md) para consultar o molde completo por camadas e os materiais. A estrutura usa Casing, Foundation, Acceleration Coils, Containment Coils/Pylons, Beam Guides e Reaction Core próprios. Mantenha livres os canais de feixe indicados.

Terminal, Fuel Injector e Energy Output Ports ocupam as quatro colunas de interfaces em X/Z **(7,7), (41,7), (7,41), (41,41)**, nos níveis **Y1–Y3** relativos à origem. As posições antigas de terminal/injector são casing. Instale exatamente **um Terminal**, pelo menos **um Fuel Injector** e pelo menos **quatro Energy Output Ports**. Portas adicionais compartilham o limite agregado existente.

## Operação

Forneça FE e Zenith Bars pelo Fuel Injector ou use o inventário do terminal. Zenith Bars comprimidas são aceitas pelo sistema existente de matéria comprimida. O Injector também expõe a saída de Reality Condensate para transporte de itens.

Clique em **Iniciar**. A máquina passa por Ignição, Aceleração e Convergência antes de Gerar. Escolha a potência e acompanhe energia, combustível, saída e diagnósticos da estrutura. **Parar** inicia seu desligamento/resfriamento. Saída bloqueada ou recursos insuficientes impedem a continuação da produção.

O anel, os feixes e o objeto central são efeitos visuais. Suas posições acompanham os canais reais e a rotação da máquina.

## Componentes antigos

O antigo **Accelerator Input, Center Accelerator Rod, segmento reto e curva** foram retirados da aba criativa, da lista de ingredientes do JEI e das receitas de produção. Seus registros continuam legíveis para preservar mundos e inventários existentes.

| Componente antigo | Conversão por crafting de mão única |
| --- | --- |
| Center Accelerator Rod | Zenith Reaction Core |
| Accelerator Input | Zenith Accelerator Terminal |
| Segmento reto | Zenith Acceleration Coil |
| Curva | Zenith Accelerator Foundation |

Blocos antigos salvos continuam compatíveis; novas instalações devem usar o multibloco atual.
