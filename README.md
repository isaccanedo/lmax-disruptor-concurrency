## Simultaneidade com LMAX Disruptor - Uma Introdução

# 1. Visão Geral
Este artigo apresenta o LMAX Disruptor e fala sobre como ele ajuda a alcançar a simultaneidade de software com baixa latência. Também veremos um uso básico da biblioteca Disruptor.

# 2. O que é um disruptor?
Disruptor é uma biblioteca Java de código aberto escrita por LMAX. É uma estrutura de programação concorrente para o processamento de um grande número de transações, com baixa latência (e sem as complexidades do código concorrente). A otimização do desempenho é alcançada por um design de software que explora a eficiência do hardware subjacente.

### 2.1. Simpatia Mecânica
Vamos começar com o conceito central de simpatia mecânica - trata-se de entender como o hardware subjacente opera e programar da maneira que melhor funciona com esse hardware.

Por exemplo, vamos ver como a organização da CPU e da memória pode afetar o desempenho do software. A CPU possui várias camadas de cache entre ela e a memória principal. Quando a CPU está realizando uma operação, ela primeiro procura os dados em L1, depois em L2, em L3 e, por fim, na memória principal. Quanto mais longe, mais tempo demorará a operação.

Se a mesma operação for executada em um dado várias vezes (por exemplo, um contador de loop), faz sentido carregar esses dados em um local muito próximo da CPU.