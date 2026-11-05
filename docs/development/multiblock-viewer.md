# Multiblock Viewer e fechamento dos modelos — 2026-11-05

## Implementação

- Novo bloco Void `chaosmod:multiblock_viewer`, com BlockEntity, menu, tela, modelo editável do Blockbench, loot, receita, tags e traduções EN/PT.
- Energia compartilhada entre faces: 256.000 FE de buffer, entrada de 2.048 FE/t e estados de consumo exclusivo de 4/8/32 FE/t. A projeção usa energia somente do Viewer físico.
- Biblioteca das 13 famílias de multiblocos: Liquefier, Stellar Reactor, Refinery, Industrial Extractor, Energy Matrix, Event Horizon Forge, Matter Inversion, Plasma Generator, Nova Reactor, Waste Reprocessor, Gravitational Compression, Zenith Accelerator e Gateway.
- Prévia com os modelos reais dos blocos, giro, zoom, câmera centralizada, transparência, filtros por categoria/bloco, inspeção, alternativas e três modos de camadas.
- Materiais fixos, posições flexíveis e interfaces obrigatórias separados. Ícones de componentes preservam o tier. Contagens iguais de itens com orientações diferentes são agrupadas.
- Construção livre e planejada; posicionamento por mira, rotação e confirmação sem colocar blocos ou consumir materiais.
- Holograma com cores de estado, HUD de progresso/materiais, camadas, avanço automático, pausa, parada e reabertura remota autorizada. Atalhos configuráveis usam Alt por padrão.
- Projetos individuais em SavedData, um ativo por jogador, preservando parâmetros, posição, dimensão, rotação e preferências. A identidade do Viewer impede uma substituição silenciosa da fonte.
- Validação incremental e cache compacto; chunks descarregados permanecem desconhecidos. A conclusão depende também do validador real da máquina, executado em modo de auditoria sem alterações no mundo.
- Planos grandes geram células sob demanda e agregam materiais, sem alocar o volume inteiro. Trabalho, cache, distância, desenho e projetos salvos têm orçamentos técnicos configuráveis.

## Modelos e compatibilidade

As casings do Nova Reactor e Reprocessor foram refeitas como blocos sólidos de armadura metálica, usando detalhes Nova. Os IDs existentes foram preservados, inclusive `nova_reactor_chamber`, cujo nome exibido continua sendo Nova Reactor Casing.

Carcaças e painéis de serviço foram fechados nas famílias Nova, Reprocessor, Compression, Event Horizon, Inversion, Industrial, Stellar e Matrix. Pequenos espaços nos encontros das molduras foram eliminados. A oclusão foi ajustada para modelos recuados não apagarem faces vizinhas necessárias. Vidros, conduits, hastes e componentes internos intencionalmente abertos mantêm sua função visual.

Stellar e Plasma receberam rotação/espelhamento de estados. A validação Stellar preserva a montagem antiga alinhada aos eixos e aceita também a montagem inteira relativa ao controller, sem aceitar uma mistura inválida das duas formas. As auditorias do Viewer não reposicionam portas, não atualizam luz, não alteram conexões e não formam máquinas automaticamente.

Não foi alterada a versão 2.0.0, e não foram removidos registros de blocos existentes. Receita, loot, tags e referências do novo Viewer são gerados por `runData`. O protocolo de rede passa a 4.21 devido aos novos pacotes.

## Arquivos principais

| Área | Arquivos |
| --- | --- |
| Registro e energia | `viewer/ViewerContent.java`, `ViewerBlock.java`, `ViewerEntity.java`, `ViewerConfig.java`, `ViewerMenu.java` |
| Projetos e rede | `ViewerProject.java`, `ViewerProjects.java`, `ViewerActionPacket.java`, `ViewerStatePacket.java` |
| Regras e acompanhamento | `viewer/definition/`, `ViewerRules.java`, `ViewerScan.java` |
| Cliente | `viewer/client/ViewerScreen.java`, `ViewerPreview.java`, `ViewerProjection.java`, `ViewerHologram.java`, `ViewerClientState.java`, `ViewerClientEvents.java`, `ViewerModels.java`, `ViewerRenderer.java` |
| Testes | `ViewerGameTests.java`, `definition/DefinitionGameTests.java` |
| Integrações | `ChaosMod.java`, `ClientModEvents.java`, `ModMessages.java`, `ModCreativeModTabs.java`, providers de datagen e validadores estruturais das máquinas |
| Modelagem | `tools/blockbench/`, modelos JSON e texturas em `assets/chaosmod/`, projetos editáveis em `art/models/` |
| Guias | `docs/en/multiblock-viewer.md`, `docs/pt/multiblock-viewer.md`, navegação em `mkdocs.yml` |

## Verificação reproduzível

Resultado final: **29/29 testes novos do Viewer aprovados**. A suíte completa executou **346 testes**, com **344 aprovados** e as duas falhas antigas de compressão descritas abaixo. As 13 famílias foram verificadas nas quatro rotações, incluindo leitura sem alterações no mundo e compatibilidade da montagem Stellar antiga.

O datagen e o MkDocs em modo estrito concluíram. A auditoria geométrica aprovou **48/48 modelos opacos**, e o parser nativo do Minecraft leu esses 48 modelos. A câmera do cliente passou em **15.680 verificações** de cantos projetados, órbitas e escalas de GUI; também foram conferidas **178 traduções por idioma** e as dimensões dos painéis.

O `build --offline` terminou com sucesso e gerou `build/libs/chaosmod-2.0.0.jar`. O JAR foi conferido quanto às classes novas, materiais Void da receita, preservação de energia/identidade no loot, traduções e presença das texturas dos 48 modelos. A biblioteca classifica Compression como Nova e Gateway como Zenith, conforme seus craftings; famílias com upgrades também aparecem nos filtros dos tiers suportados.

```powershell
./gradlew.bat runData --offline
./gradlew.bat runGameTestServer -PgameTestDirectory=build/viewer-gametest-clean --offline
./gradlew.bat build --offline
.venv-docs/Scripts/python.exe tools/audit_multiblock_shells.py
.venv-docs/Scripts/python.exe tools/qa/check_viewer_client.py --java-home <JDK-17>
.venv-docs/Scripts/mkdocs.exe build --strict
```

Execute datagen e testes em invocações separadas para que recursos novos sejam copiados antes de iniciar o servidor de testes.

Resultados detalhados da execução ficam nos logs `build/viewer-datagen.log`, `build/viewer-tests-release.log` e `build/viewer-build.log`. O relatório de cobertura dos modelos fica em `build/reports/multiblock-viewer/shell-coverage.json`; a prévia dos arquivos exportados, em `shell-models.png`. A verificação matemática do cliente fica em `build/reports/viewer-client-qa/report.txt`.

## Limites da verificação

- A conferência visual dentro do Minecraft foi mantida para depois, conforme solicitado anteriormente. A prévia local valida os arquivos exportados; os testes matemáticos não substituem renderização OpenGL em jogo.
- Prévias enormes usam amostragem com modelos reais e limite de desenho. Camadas individuais permitem examinar a construção com mais detalhe. O limite técnico de cache pode ser aumentado para projetos extraordinariamente grandes.
- Estimativas térmicas que exigem montar o grafo completo são omitidas para reatores enormes; contagens e estimativas agregadas de armazenamento e geração continuam disponíveis.
- A suíte geral contém dois testes antigos de compressão incompatíveis com a lógica atual dos reservatórios: `paidAtomicCompressionAndPartialDecompression` e `canonicalItemsVirtualOverflowAndPersistence`. O primeiro ainda seleciona o modo 1 como descompressão, mas esse modo agora é recarga; o segundo exige o formato antigo de NBT. Esses testes e a lógica de reservatórios não foram alterados nesta tarefa. Seus resultados devem ser distinguidos dos testes novos do Viewer.
