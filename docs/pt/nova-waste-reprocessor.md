# Nova Waste Reprocessor

O Reprocessador converte **10.000 mB de Nova Waste em 8 Exotic Dust**. O ciclo padrão dura **200 ticks (10 segundos)**, consumindo **320.000 FE/t**, totalizando **64 MFE**. Os valores são configuráveis em `chaosmod-nova-waste-reprocessor.toml`.

## Montagem

O exemplo fornecido mede **3 x 15 x 13**, com duas torres ocas conectadas a uma câmara central **3 x 3 x 3**. A instalação aceita rotações horizontais e permite inverter as funções das torres trocando suas portas e os tipos de cabos.

O núcleo central fica na coordenada original **(1, 1, 6)**. As bases das torres ocupam **X=0..2, Z=0..2** e **X=0..2, Z=10..12**. A câmara central ocupa **X=0..2, Y=0..2, Z=5..7**. Preserve essas distâncias horizontais.

- Cada torre usa anéis ocos de revestimento **3 x 3**, mantendo o centro vazio em todas as camadas.
- Coloque um único **Tower Cap** sobre a coluna central vazia. Os oito blocos vizinhos na camada do topo ficam como ar.
- Cada torre deve ter pelo menos **quatro blocos de altura incluindo o topo**, com três anéis abaixo dele. As alturas podem ser diferentes, chegando até o limite de construção do mundo.
- As portas ocupam o centro das faces laterais, excluindo os anéis inferior/superior e todos os cantos.
- Use portas de fluido na torre de resíduos e portas de itens na torre de pó. Todas as portas de uma torre compartilham seu armazenamento.
- Na segunda camada da base, coloque uma porta voltada à câmara central.

A câmara possui um **Reprocessing Core** no centro. Os centros das duas faces opostas voltadas às torres são obrigatoriamente a entrada de fluido e a saída de itens. Nas outras quatro faces coloque exatamente **um Terminal**, **uma a três portas de energia**, preenchendo os demais centros com revestimento. Arestas e vértices permanecem revestimento.

Conecte os caminhos obrigatórios usando cabos existentes de qualquer tier:

```text
Saída da torre de resíduos -> Cabo de fluido -> Cabo de fluido -> Entrada da câmara
Saída de itens da câmara -> Transportador -> Transportador -> Entrada da torre de pó
```

Configure os cabos para **puxar da origem** e **enviar ao destino**, mantendo abertas as conexões entre cabos. O tier determina a velocidade real de transporte. Apenas colocar os cabos fisicamente não basta: a conexão deve estar funcional. O Reprocessador não teleporta recursos entre os inventários.

Operadores podem usar `/place template chaosmod:nova_waste_reprocessor` para obter um exemplo limpo, sem recursos ou energia salvos.

## Armazenamento e operação

Cada camada de anel adiciona, por padrão, **1.000.000 mB** à torre de resíduos ou **65.536 itens** à torre de pó. As capacidades são independentes. Os saldos usam contadores persistentes de 64 bits; as interfaces Forge expõem quantidades adequadas para cada transferência.

Forneça FE, alimente as portas externas da torre de resíduos, configure os caminhos e clique em **Iniciar**. A câmara recebe resíduos por sua porta dedicada, paga FE durante o progresso e finaliza cada lote de forma atômica. O pó concluído aguarda na saída da câmara até ser levado pelos transportadores à torre de pó. Use portas externas dessa torre para extrair.

Falta de energia, recursos, capacidade, chunks ou integridade estrutural suspende a operação com segurança. **Parar preserva o progresso pago.** Os resíduos só são consumidos quando o resultado completo pode ser preservado. Reduzir uma torre não apaga excedentes: inserção e produção aguardam espaço. Quebrar o núcleo preserva saldos, energia, pó pendente e progresso no item dropado, evitando milhões de entidades no chão.

O terminal oferece abas de visão geral, armazenamento e diagnóstico, com capacidades, alturas, progresso, energia, taxas de transporte e posição dos erros. A validação incremental usa limites configuráveis de trabalho por tick e não força o carregamento de chunks.

## Exportação do Nova Reactor

Instale a **Nova Reactor Waste Fluid Port** em uma face externa válida. No terminal do reator selecione **External Extraction**. O mesmo reservatório passa a disponibilizar Nova Waste como fluido, sem convertê-lo internamente. Conecte cabos de fluido à torre de resíduos. Vários reatores podem alimentá-la. Trocar o modo não recria nem duplica resíduos.

O JEI mostra a conversão e as receitas dos componentes. As capabilities Forge permitem automação pelas portas externas com outros mods.
