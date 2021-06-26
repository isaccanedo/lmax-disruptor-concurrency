## Simultaneidade com LMAX Disruptor - Uma Introdução

# 1. Visão Geral
Este artigo apresenta o LMAX Disruptor e fala sobre como ele ajuda a alcançar a simultaneidade de software com baixa latência. Também veremos um uso básico da biblioteca Disruptor.

# 2. O que é um disruptor?
Disruptor é uma biblioteca Java de código aberto escrita por LMAX. É uma estrutura de programação concorrente para o processamento de um grande número de transações, com baixa latência (e sem as complexidades do código concorrente). A otimização do desempenho é alcançada por um design de software que explora a eficiência do hardware subjacente.

### 2.1. Simpatia Mecânica
Vamos começar com o conceito central de simpatia mecânica - trata-se de entender como o hardware subjacente opera e programar da maneira que melhor funciona com esse hardware.

Por exemplo, vamos ver como a organização da CPU e da memória pode afetar o desempenho do software. A CPU possui várias camadas de cache entre ela e a memória principal. Quando a CPU está realizando uma operação, ela primeiro procura os dados em L1, depois em L2, em L3 e, por fim, na memória principal. Quanto mais longe, mais tempo demorará a operação.

Se a mesma operação for executada em um dado várias vezes (por exemplo, um contador de loop), faz sentido carregar esses dados em um local muito próximo da CPU.

Alguns números indicativos para o custo de perdas de cache:

<img src="cpu.png"
     
### 2.2. Por que não filas
As implementações de fila tendem a ter contenção de gravação nas variáveis ​​de início, fim e tamanho. Normalmente, as filas estão sempre quase cheias ou quase vazias devido às diferenças de ritmo entre consumidores e produtores. Eles muito raramente operam em um meio-termo equilibrado, onde a taxa de produção e consumo são equilibradas.

Para lidar com a contenção de gravação, uma fila geralmente usa bloqueios, o que pode causar uma mudança de contexto para o kernel. Quando isso acontece, o processador envolvido provavelmente perderá os dados em seus caches.

Para obter o melhor comportamento de cache, o design deve ter apenas um núcleo de gravação em qualquer local da memória (vários leitores são adequados, pois os processadores geralmente usam links especiais de alta velocidade entre seus caches). As filas falham no princípio de um gravador.

Se duas threads separadas estão gravando em dois valores diferentes, cada núcleo invalida a linha de cache do outro (os dados são transferidos entre a memória principal e o cache em blocos de tamanho fixo, chamados de linhas de cache). Essa é uma contenção de gravação entre os dois threads, embora eles estejam gravando em duas variáveis diferentes. Isso é chamado de falso compartilhamento, porque toda vez que a cabeça é acessada, a cauda também é acessada e vice-versa. 
     
### 2.3. Como funciona o disruptor
 
