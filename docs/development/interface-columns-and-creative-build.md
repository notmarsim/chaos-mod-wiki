# Interfaces flexíveis e construção criativa

## Zenith Accelerator

As posições antigas de terminal `(45,2,32)` e injectors `(45,2,16)`/`(46,2,16)` passam a ser casing. As quatro colunas em X/Z `(7,7)`, `(41,7)`, `(7,41)` e `(41,41)` aceitam interfaces nos níveis Y1, Y2 e Y3. O molde propõe terminal em `(7,1,7)`, um injector em `(7,3,7)` e quatro saídas em Y2.

Mantêm-se exatamente um terminal, pelo menos um injector e quatro saídas de energia. Mais saídas compartilham o limite agregado existente. A origem da estrutura é descoberta a partir do terminal móvel; o runtime deixa de presumir que seus inventários e saídas estejam em posições fixas.

Os testes identificaram ainda um problema anterior na produção de Reality Condensate: o processamento tentava inserir o resultado pelo handler externo que rejeita entradas no slot de saída. A produção agora grava o resultado internamente após verificar espaço e só então desconta o progresso correspondente. Capacidades e throughput não foram aumentados.

## Reality Devourer

A geometria do template determina os interiores planos das seis faces de cada Energy Output Housing. Arestas, vértices e células enterradas não aceitam portas. Há 308 posições geometricamente válidas na estrutura atual, sem teto numérico artificial. É necessária pelo menos uma saída global; cada banco continua com seu próprio orçamento, além do limite agregado da máquina.

As oito saídas antigas que ficavam em posições inadequadas foram substituídas por housing no molde. A referência passa a usar quatro saídas válidas, uma por banco. Nenhum bloco em mundos existentes é substituído automaticamente: montagens anteriores precisam ter suas interfaces ajustadas às novas regras. JSON, schematics e Viewer usam a mesma regra de aceitação.

## Viewer

O jogador no criativo pode habilitar construção automática antes de confirmar o Build. Alt+G permite alternar, iniciar ou cancelar; o servidor confere o modo de jogo, propriedade, distância, dimensão, proteção e chunks durante o trabalho. Há pré-validação e colocação incremental, com até 32 colocações por etapa e orçamento compartilhado pelo Viewer.

A construção não força chunks, não substitui inventários e não remove obstruções sólidas. Cancelamentos do evento Forge de colocação restauram a tentativa. O plano persiste, mas os jobs que alteram o mundo são transitórios e nunca reiniciam após reload. Cabos são configurados depois da montagem. O sobrevivência mantém o fluxo manual.

## Travamento anterior de configuração

O `latest.log` e o crash report de 05/11/2026, 00:47:47, mostram duas correções simultâneas de `chaosmod-multiblock-viewer.toml`, terminando em `WritingException`/bloqueio de arquivo no Windows antes da criação dos níveis. O consumo registrado era cerca de 280 MiB para um limite de 4 GiB; não foi falta de memória.

`StartupConfigSpec` mantém o watcher das três configurações novas fora da fase inicial de carregamento/correção/gravação. O gate abre no evento `ServerAboutToStart`, depois da gravação final do ConfigTracker. Valores, arquivos por mundo, correção de campos, cache e sincronização permanecem sob o controle do Forge. A configuração do usuário não é apagada nem redefinida.

O motivo do gate abranger também a gravação posterior ao bind está no fluxo do [ConfigFileTypeHandler](https://github.com/MinecraftForge/MinecraftForge/blob/1.20.x/fmlcore/src/main/java/net/minecraftforge/fml/config/ConfigFileTypeHandler.java) e do [ForgeConfigSpec](https://github.com/MinecraftForge/MinecraftForge/blob/1.20.x/src/main/java/net/minecraftforge/common/ForgeConfigSpec.java). A alteração se limita ao Viewer, Zenith Accelerator e Reality Devourer.

## Verificação

Os testes cobrem posições e rotações das interfaces, orçamento energético compartilhado, portas em faces/arestas, construção criativa e negativa em sobrevivência, inventários, proteção, conexões de cabos e ciclo de vida das configurações.

Logs desta revisão: `build/interface-autobuild-datagen.log`, `build/interface-autobuild-tests.log` e `build/interface-autobuild-build.log`. A conferência visual dentro do Minecraft permanece para depois; nenhum save do usuário foi aberto ou alterado pelas ferramentas.

Resultado final: **364/366 testes aprovados**, incluindo os 11 novos desta revisão, os dois ciclos de configuração, os testes funcionais das máquinas e as 14 definições do Viewer nas quatro rotações. As duas falhas restantes são os testes antigos de Compression já registrados no relatório anterior; esses testes e a lógica de reservatórios não foram alterados nesta tarefa. O log final completo é `build/interface-autobuild-tests-final.log`.

O datagen concluiu com sucesso. A verificação sem janela do cliente passou em **15.680 casos de câmera**, **207 chaves EN/PT** e dimensões dos painéis. A documentação foi compilada com MkDocs em modo estrito. O protocolo de rede passa a **4.22**, mantendo a versão do mod em 2.0.0.
