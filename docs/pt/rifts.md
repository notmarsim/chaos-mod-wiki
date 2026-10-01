# Reality Rifts e Scars

[English](../en/rifts.md)

Uma Reality Rift é um vazamento temporário no **Overworld**. O centro colorido representa seu tier, o terreno próximo se transforma gradualmente e criaturas daquele tier ficam muito mais frequentes. Isso complementa o spawn natural de mobs tierizados já existente.

## Tiers e raridade

A seleção natural usa os pesos abaixo **depois** de o jogo decidir tentar criar uma Rift. Não são probabilidades por tick ou por chunk.

| Tier      | Participação na seleção natural |
|-----------|---:|
| Unstable  | 65% |
| Void      | 17% |
| Stellar   | 11% |
| Duality   | 4,5% |
| Darklight | 1,8% |
| Nova      | 0,6% |
| Zenith    | 0,1% |

**Stable e Chaos Rifts não aparecem naturalmente.** As tentativas também dependem de proximidade de jogadores, limite de Rifts ativas e distância entre centros. Portanto, não existe um intervalo garantido para encontrar uma.

## Tamanhos e influência no terreno

| Tamanho | Seleção natural | Raio-alvo | Duração inicial típica | Limite de blocos alterados |
|---|---:|---:|---:|---:|
| Minor Rift | 60% | 12 blocos | 3–5 dias de Minecraft | 256 |
| Reality Rift | 30% | 26 blocos | 6–9 dias | 1.400 |
| Major Rift | 10% | 44 blocos | 10–14 dias | 4.000 |
| Dimensional Breach | Por evolução | 96 blocos | Precisa ser selada | 18.000 |

Um dia de Minecraft tem 24.000 ticks, normalmente 20 minutos. Uma Rift que cresce inicia uma nova etapa. O efeito central aumenta bastante com o tamanho; estágios maiores também suportam mais criaturas.

## Ciclo de vida e evolução

A maioria segue:

```text
ACTIVE → WEAKENING → CLOSING → CLOSED
```

Uma verificação na metade da etapa possui **10% de chance de Minor → Reality**, **8% de Reality → Major** ou **5% de Major → Breach**. São verificações individuais por etapa, não sorteios repetidos a cada tick. O crescimento é precedido por aviso e espera:

> Rift instability increasing...

A transição final avisa sobre grande instabilidade dimensional e anuncia **DIMENSIONAL BREACH FORMED**. Consulte o [guia de Breaches](breaches.md) se isso ocorrer perto do seu farm.

## Feche uma Rift

Segure o botão de uso com um **Stable Orb** a até **seis blocos** do centro por **cinco segundos**. O progresso pausa se você se afastar ou soltar, mas não reinicia. Um orb é consumido ao concluir no Survival.

O fechamento remove a ruptura central e encerra seus spawns adicionais. **Não remove criaturas existentes nem restaura o terreno.** A área restante é uma **Reality Scar**. Scars antigas também podem aparecer raramente na geração, com terreno alterado e pequenos detalhes como cristais, vegetação morta ou ruínas.

## Recompensas

Fechamentos comuns fornecem uma quantidade aleatória de **Rift Residue** e uma pequena chance de **Rift Core**, aumentando por tier e tamanho. Use o índice de tier `t` (Unstable 0, Stable 1, Void 2, Stellar 3, Duality 4, Darklight 5, Nova 6, Zenith 7, Chaos 8) e tamanho `s` (Minor 0, Reality 1, Major 2):

- Faixa de residue: `1 + s` até `4 + 2 × (t + 1) × (s + 1)`.
- Chance de Rift Core: `0,5% + 0,3% × t + 0,8% × s`.

Uma Unstable Minor fornece 1–6 residue e tem 0,5% de chance de core; uma Zenith Major fornece 3–52 e tem 4,2%. As recompensas aparecem no centro quando seu chunk está processando entidades. Breaches possuem [recompensas maiores e garantidas](breaches.md).
