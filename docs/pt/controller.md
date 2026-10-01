# Machine Controller e calibração

[English](../en/controller.md)

## Qual ferramenta usar?

**Configurator:** clique com o botão direito numa máquina compatível para ajustar **Energy Frequency**, **Field Alignment** e **Phase Offset**. A eficiência exibida indica se o ajuste melhorou. Os alvos são próprios de cada máquina e ficam salvos nela. A eficiência varia de 85% a 100%; uma calibração ruim pode reduzir a velocidade e aumentar os custos de energia e estabilidade.

**Machine Controller:** conecte-o à rede de cabos de energia e abra seu painel para controlar modos de operação, regras, fontes, prioridades, grupos e saída de comparador. A frente acompanha a orientação de colocação. O Controller não calibra máquinas nem funciona como ponte entre cabos.

## Modos de operação

| Modo | Comportamento |
|---|---|
| ON | Habilita o trabalho sem aplicar limites automáticos; redstone continua valendo |
| OFF | Pausa o trabalho |
| AUTO | Todas as regras habilitadas precisam permitir a operação; redstone continua valendo |

Selecione a máquina, escolha uma métrica com **Edit**, ative **Rule: ON** e configure os limites inferior e superior. Ativar uma regra seleciona AUTO. Confirme valores digitados com Enter; os botões de ajuste também alteram os números.

### Exemplo: duas regras ao mesmo tempo

Configure **Stability 20/90**: parar quando a estabilidade cair até o limite inferior e retomar após recuperar até o superior. Depois selecione **Energy 10/95**: parar no limite superior de energia e retomar no inferior. As duas regras continuam salvas e ativas ao trocar o editor; qualquer regra bloqueada pausa a máquina.

Entre os limites, a decisão anterior é mantida. Esse intervalo evita que a máquina fique ligando e desligando rapidamente. Cuidado ao monitorar a energia da própria máquina: se ela parar cheia e nada consumir seu buffer, poderá nunca atingir o limite de retomada sem intervenção.

**Source** permite monitorar outra máquina ou bateria compatível na mesma rede. Uma fonte ausente ou desconectada pausa a automação correspondente. Grupos organizam a lista; não unem os buffers das máquinas.

## Redstone explicado

O sinal é lido **na máquina selecionada**, não no Controller.

| Opção | Quando permite trabalho | Exemplo |
|---|---|---|
| IGNORED | Independentemente do sinal | Operação automática sem alavanca |
| HIGH | Sinal maior que zero | Alavanca ligada = habilitada |
| LOW | Sem sinal | Alavanca ligada = pausada |
| PULSE | Por 20 ticks após passar de sem sinal para com sinal | Um botão libera o trabalho brevemente |

PULSE é uma janela de aproximadamente um segundo a 20 TPS, **não uma receita completa**. Manter o sinal não repete a janela; uma nova transição reinicia o prazo. OFF sempre bloqueia, e AUTO continua exigindo a liberação das regras.

## Regras do reator e do acelerador

| Máquina | Métrica | Controle |
|---|---|---|
| Nova Reactor | Core Heat / Casing Heat | Para acima da temperatura alta; retoma abaixo da baixa, em kelvin |
| Nova Reactor | Waste | Para com muito resíduo; retoma após extração |
| Nova Reactor | Fuel / Coolant | Para com pouca reserva; retoma após reposição |
| Accelerator | Energy | Protege a reserva: pausa com pouca energia e retoma após carregar |
| Accelerator | Material | Controla novos lotes pela quantidade de entrada; não interrompe o feixe já ativo |
| Accelerator | Release | Solicita colisão na carga configurada, entre 50% e 100% |
| Accelerator | Collector | Para quando o armazenamento da haste central está muito cheio |

Desligar o reator interrompe a queima, mas o resfriamento e a geração pelo calor residual continuam. O acelerador pode recarregar FE enquanto AUTO pausa o trabalho; OFF manual e bloqueio por redstone também bloqueiam o carregamento. A regra Release usa seu próprio feixe, não uma fonte remota.

O Controller encontra apenas partes carregadas da rede, com uma busca limitada de cabos. Veja [energia e carregamento de chunks](energy.md) se uma máquina distante desaparecer da lista.
