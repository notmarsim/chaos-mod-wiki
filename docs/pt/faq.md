# Dúvidas e comandos

[English](../en/faq.md)

## Minha máquina tem energia, mas não trabalha

Confira a receita de entrada, espaço na saída, estabilidade, modo de operação e redstone. Em AUTO, **todas** as regras habilitadas precisam permitir o funcionamento. Uma Source ausente da rede carregada também pode bloquear. O [guia do Controller](controller.md) explica cada condição.

## As regras de Energy e Stability parecem se contradizer

Elas são independentes e continuam ativas simultaneamente. Stability normalmente para em valores baixos e retoma em altos; uma regra Energy comum faz o contrário. O acelerador usa uma regra de reserva de energia. Confira a métrica, fonte e limites, não apenas a regra visível no editor.

## Por que a barra de energia do acelerador não é a porcentagem da colisão?

A barra mostra FE armazenada. A carga do feixe é separada. Para avançar, são necessários estrutura 7×7 válida, material e FE suficiente. Veja o [guia do acelerador](accelerator.md).

## Por que não consigo fechar uma Breach?

Destrua todos os Reality Anchors vivos e então segure o uso com Stable Orb a até seis blocos do centro. Os Anchors aparecem após uma pequena espera e precisam de chunks ativos; não surgem apenas à noite.

## Fechar uma Rift restaura o terreno ou remove os mobs?

Não. O fechamento interrompe os spawns adicionais daquela Rift e remove o centro. O terreno alterado vira uma Scar e as criaturas existentes permanecem.

## Posso restaurar uma Reality usando Flux Crystals?

Não. Flux Crystal serve para manutenção de máquinas. A estabilidade da Reality é uma reserva finita de exploração. Ao chegar a zero, começa o [colapso](realities.md).

## Por que um portal já visitado ainda demora?

Destinos novos e salvos exigem aproximadamente quatro segundos de contato. Após chegar, saia da área do portal para liberar a proteção de retorno antes de entrar novamente.

## Por que o terreno novo é diferente do antigo?

Atualizações do gerador afetam chunks novos. Terreno existente e construções não são regenerados ao atualizar o mod.

## Comandos do Criativo

Os comandos abaixo exigem que o jogador esteja **no modo Criativo**. Eles não são comandos para Survival nem para o console do servidor.

```text
/realityrift create <tier> <tamanho>
/realityrift list
```

Tiers: `unstable`, `stable`, `void`, `stellar`, `duality`, `darklight`, `nova`, `zenith`, `chaos`.

Tamanhos: `minor`, `reality`, `major`, `breach`.

A criação ocorre na sua posição atual no **Overworld**, respeita o limite de eventos ativos e exige espaço dentro da borda do mundo para os Anchors de uma Breach.

Exemplos:

```text
/realityrift create unstable minor
/realityrift create stellar reality
/realityrift create void major
/realityrift create nova breach
```

Esses comandos criam eventos reais, com alterações no terreno e criaturas. Use um mundo Criativo separado para conhecê-los. Criar Stable e Chaos por comando não significa que esses tiers apareçam naturalmente.
