# Chaos Mod — Atualização de máquinas e criaturas

## Void Station e automação

- Adicionado **Void Automation Module**, responsável por habilitar a automação da Void Station.
- Adicionados **Blank Void Pattern** e **Void Recipe Pattern**, com desenhos próprios. As três novas texturas usam 64×64 pixels e transparência.
- Receitas válidas da grade manual 9×9 podem ser gravadas em um Blank Void Pattern usando **Encode Pattern**.
- Adicionados oito slots de patterns, seleção de receita e visualização dos ingredientes como ghost items.
- Adicionado inventário de entrada independente, com 81 slots divididos em três páginas, e saída exclusiva para produtos concluídos.
- Adicionados os modos **Selected** (uma fabricação por ativação), **Auto Repeat** (produção contínua) e **Auto Stock** (produção até a quantidade configurada na saída da própria máquina).
- A automação verifica todos os ingredientes, espaço na saída e recipientes devolvidos antes de consumir materiais.
- A entrada automática limita o recebimento por material à quantidade de uma receita, evitando que o primeiro ingrediente ocupe o buffer inteiro.
- Integração com transportadores por **Forge IItemHandler**, respeitando as configurações laterais.
- O módulo não armazena receitas: remover o módulo preserva os patterns.
- Patterns guardam o ID da receita; a fabricação sempre valida a receita 9×9 registrada no servidor.
- Configurações, inventários e seleção persistem após reiniciar o mundo. Máquinas com a configuração antiga migram a receita e os ingredientes para o novo sistema.
- Removidas as cópias de receitas da integração com o autocrafting do Create; a automação passa pela Void Station.
- Preservado o crafting manual e impedida a extração ou o drop da prévia de resultado como se fosse um produto já fabricado.
- Interface reorganizada em abas Manual e Automation, com escala para telas menores. Controles de automação e paginação aparecem somente na aba correspondente.
- Corrigidas sobreposições entre instruções, botões e o indicador **Input 1/3**. O acesso à configuração lateral usa **I/O**.

## Máquinas e eventos

- Configuração lateral de itens e energia independente, com indicação visual dos blocos adjacentes.
- Direita e esquerda seguem a perspectiva do jogador ao posicionar a máquina.
- Atualização das conexões de transporte quando a configuração lateral muda, preservando transferências de energia de alta capacidade.
- Calibração ampliada para cinco variáveis e tornada mais tolerante. Calibrações perfeitas antigas são preservadas na migração.
- Chance natural de meteoros reduzida em 80%.
- Orbital Deflector recebe inventário de 27 slots para armazenar os drops recuperados. O armazenamento é verificado antes da cobrança da energia de recuperação.

## Criaturas tierizadas

- Sistema compartilhado de tiers ampliado para as espécies vanilla elegíveis, preservando identidade, comportamento, equipamentos e dados de cada criatura.
- Villagers, wandering traders e Endermans ficam fora do sistema. Endermans tierizados antigos recuperam seus atributos vanilla, preservando a proporção de vida e nomes dados pelo jogador.
- Novas versões tierizadas de **Wither** e **Ender Dragon** ficam disponíveis somente por comando. Invocações vanilla continuam criando os chefes normais; entidades já salvas são preservadas.
- Vida, dano, velocidade, resistência a knockback e penetração parcial de armadura ajustados por tier, com limites.
- Balanceamento por espécie conforme as metas de golpes. Unstable, Stable, Void e Stellar usam espada Stable e armadura Netherite como referência; Duality em diante usa equipamento equivalente.
- Para Chaos, a referência passa a ser o **Chaos Axe** com armadura Chaos. A **Chaos Sword mantém dano efetivamente infinito**, tanto no ataque direto quanto na habilidade de Shift + botão direito, inclusive contra mobs Chaos.
- Referências consideram dificuldade Normal, 20 pontos de vida, ataques carregados sem críticos ou encantamentos e espada e todas as peças Zenith no nível 21. Habilidades e condições reais de combate podem alterar a quantidade de golpes.
- Rift Walker fica mais forte que o Zombie equivalente e mantém movimentação mais rápida. Reality Anchors também escalam com o tier.
- Adicionadas cores por afinidade e geometrias 3D às criaturas hostis, preservando animações vanilla, incluindo detalhes específicos de espécies e chefes.
- Loot vanilla preservado. Apenas criaturas hostis recebem loot de progressão; o sistema permite adicionar drops próprios para criaturas não hostis posteriormente.
- Corrigido o escalonamento dos ataques à distância, incluindo tridentes e o sonic boom do Warden.
- Armadura Chaos mantém proteção contra fontes comuns, mas permite combate contra criaturas Chaos.

### Habilidades por tier

- **Unstable:** chance de ataque adicional, com cooldown.
- **Stable:** maior resistência a knockback.
- **Void:** teleporte curto durante combate.
- **Stellar:** ataques aplicam fogo.
- **Duality:** alternância entre defesa e ataque.
- **Darklight:** redução temporária da eficiência da armadura.
- **Nova:** pulso de energia ao acumular muito dano, sem destruir o terreno.
- **Zenith:** regeneração limitada e resistência a efeitos negativos.
- **Chaos:** combinação de habilidades dos demais tiers.

## Comandos

- Adicionado comando exclusivo para jogadores no criativo:
  ```mcfunction
  /tiermob spawn minecraft:zombie void
  /tiermob spawn minecraft:wither nova ~ ~ ~
  /tiermob spawn minecraft:ender_dragon chaos ~ ~ ~
  /tiermob spawn chaosmod:rift_walker zenith ~ ~ ~
  ```
- Criaturas geradas pelo comando preservam o tier escolhido, mesmo dentro de uma Reality de outra afinidade.

## Compatibilidade

- IDs existentes e dados de mundos anteriores são preservados; novos dados possuem migração ou valores padrão.
- Cliente e servidor precisam usar a mesma versão atualizada do mod devido às mudanças de comunicação das máquinas.
- A integração utiliza as capacidades do Forge; a validação visual dentro do jogo e testes com cada modpack continuam necessários.

## Ajustes da versão 1.1.0

- Balanceamento Zenith usa espada e set completo no nível 21, incluindo os efeitos da armadura.
- Cada item Zenith com upgrades consome 64 Zenith Bars no total para ir do nível 0 ao 21. Upgrades posteriores continuam disponíveis; níveis existentes são preservados.
- Chaos Axe passa a ter 150 de dano base (162 com Strength IV do set Chaos). A vida dos mobs Chaos da tabela foi recalculada com essa referência.

## Proteção térmica, módulos e estabilidade

- Itens Darklight, Nova, Zenith e Chaos/Chaotic resistem a fogo e lava quando derrubados.
- Cada peça de armadura Darklight, Nova, Zenith ou Chaos reduz dano de fogo/lava em 25%. Quatro peças dão imunidade, inclusive misturando tiers.
- Módulos compartilham uma capacidade total equivalente a três módulos do tier da máquina. É possível repetir ou misturar tipos e usar módulos inferiores com efeito proporcional.
- O tooltip mostra a potência total instalada e a capacidade compartilhada. Instalações antigas são preservadas, com efeitos limitados à capacidade.
- Estabilidade deixa de se regenerar durante trabalho contínuo. Basic sem módulos perde aproximadamente 3% por minuto de produção contínua com calibração ideal e velocidade normal; em repouso, recupera estabilidade.
- Perdas fracionárias de estabilidade são persistidas, permitindo que módulos de Stability tenham efeito mesmo nas máquinas Basic.

## Breach Stability e Dimensional Guardian

- Aumentadas moderadamente as chances de crescimento das Rifts até Dimensional Breach.
- Breaches passam a ter estabilidade: Relatively Stable, Unstable Breach, Critical Instability e Dimensional Collapse.
- Quanto menor a estabilidade, maiores as ondas de criaturas e a intensidade da contaminação, mantendo limites de processamento.
- Adicionado Dimensional Guardian, exclusivo do colapso de uma Breach, com modelo 3D maior que um Iron Golem e nove afinidades compartilhando a mesma entidade.
- Cada afinidade possui comportamento tecnológico próprio, incluindo gravidade, plasma, escudos, descarga, ondas de choque e distorção espacial.
- Adicionados nove Dimensional Guardian Cores, com desenho próprio e cor por tier. São drops do boss e ainda não possuem receitas ou uso.
- Breaches antigas recebem 100% de estabilidade; tempo descarregado não causa colapso retroativo.


## Dimensional Augmentor e equipamentos

- Corrigida a bossbar do Dimensional Guardian permanecendo após a morte.
- Adicionado Dimensional Augmentor com 27 aprimoramentos permanentes por jogador, estatísticas, custos progressivos em Guardian Cores/FE e expansão Chaos até nível 25.
- Receita 9×9 com componentes Darklight na Void Station; traduções em português e inglês.
- Adicionados arcos e escudos dos nove tiers, modelos 3D e poses de uso com cores de cada tier.
- Escudos com 1,4× a durabilidade do peitoral de referência; equipamentos Chaos indestrutíveis e Darklight em diante resistentes a fogo/lava.
