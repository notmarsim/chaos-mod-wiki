# Edição dos sprites dos arcos

Os sete tiers ativos usam PNGs RGBA de **32×32** em `src/main/resources/assets/chaosmod/textures/item/`.

Para cada tier (`unstable`, `stable`, `duality`, `darklight`, `nova`, `zenith`, `chaos`):

- `<tier>_bow.png`: parado, sem flecha.
- `<tier>_bow_pulling_0.png`: início da puxada.
- `<tier>_bow_pulling_1.png`: puxada intermediária.
- `<tier>_bow_pulling_2.png`: puxada completa; também usada durante o disparo contínuo Chaos.

Edite os PNGs diretamente no Aseprite, LibreSprite, GIMP ou outro editor com transparência. Mantenha o canvas 32×32 e a posição do cabo alinhada entre os quatro estágios. Os modelos usam a camada `layer0`, sem coloração por código. No ambiente de desenvolvimento, F3+T recarrega os recursos.

O datagen gera os JSONs, mas não escreve nessas imagens. A base `models/item/tier_bow_sprite.json` contém as poses vanilla; não possui cubos. O Minecraft extruda a silhueta do PNG como nos itens vanilla. Os modelos cúbicos antigos são usados somente pelos arcos Void/Stellar legados, já retirados da progressão.

A folha de origem está em `tools/art/bows/bow-atlas.png`. `tools/ImportBowSpriteAtlas.py` faz apenas recorte e redução por nearest-neighbor, autorizados pelo usuário. Execute-o somente se quiser reimportar a folha: ele sobrescreve os 28 PNGs. Não é executado pelo datagen nem pelo build.

Arte criada pela ferramenta integrada de geração de imagens. Prompt arquivado junto à folha em `tools/art/bows/prompt.txt`. Nenhuma habilidade, receita ou estatística foi alterada nesta migração.
