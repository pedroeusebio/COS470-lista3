# Sistemas Distruídos
## Terceira lista de Exercícios

##### Para ler:

1. Slide 10 a 13 aula 17.
2. Aula 19.
3. 

#### Questão 1: Cite e explique dois desafios que precisam ser resolvidos na implementação de RPC.

1. Representação de dados possivelmente diferentes. (Little Endian, Big Endian). A representação de dados diferentes pode acarretar uma codificação diferente em cada maquina, podendo levar a erros na execução ou diferenciacão dos dados esperados.
2. Processos que chama função e que executa funcao estao em maquinas diferentes. Dessa forma, o espaco de enderecamento é diferente, alem do sistema operacional e do hardware.
3. Redes e máquinas podem falhar durante a chamada de função.

#### Questão 2: Explique todos os passos envolvidos em uma chamada RPC assíncrona.

1. O cliente faz uma chamada de um procedimento remoto e aguarda a resposta do servidor(espera bloqueante).
2. O pedido chega ao servidor, que envia uma mensagem de aceitação de pedido.
3. O cliente recebe a confirmação do servidor e continua o processamento normalmente.
4. O servidor faz o processamento correspondente a solicitação, quando termina, retorna os resultados para o cliente.
5. A execução do cliente é interrompida com a chegada da mensagem do servidor. O cliente uma mensagem de confirmação de recebimento dos resultados.

#### Questão 3: Qual o problema inerente a sistemas distribuidos que relógios sincronizados ajudam a resolver ? Dê um exemplo do problema.

O problema que a sincronização de relógios ajuda a resolver é sincronização de eventos. Idealmente, a sincronização de relogio ajudaria a descobrir a ordem cronologica dos eventos.
Um exemplo bem comum é a edição de um arquivo por duas pessoas em computadores diferentes. A primeira pessoa edita um arquivo e salva no Dropbox, o seu relogio local está adiantado, dessa forma o Dropbox reconhece como ultima edição e salva as alterações. A segunda pessoa edita depois da primeira, porém sua edição não será salva pois seu relógio local esta atrasado e o Dropbox reconhece que seu evento de edição ocorreu antes da primeira pessoa.

#### Questão 4: Considere o problema de manter a hora sincronizada entre dois relógios. Cite e explique os dois aspecto fundamentais que dificultam a sincronização.

A primeira grande dificuldade na sincronização de relógio é conhecido como *Clock Drift*. Esse problema está relacionado a oscilação do relógio, nenhum oscilador é fundamentalmente igual ao outro, dessa forma, as frenquências de oscilação são ligeiramente diferentes( mesmo que sejam de mesmo material, fatores externos como temperatura pode alterar a frenquência de oscilação) os pode acarretar em uma diferenciação de tempo.
O segundo problema é como ajustar a hora do computador de forma que os todos os computadores envolvidos marquem a mesma hora. É necessário que haja uma sincronização entre eles e que o método usado consiga lidar com atrasos.

#### Questão 5: Explique como funciona o mecanismo básico (visto em aula) para sincronização de relógios quando um computador deseja usar o relógio de outro como referência. Utilize os horários dos relógios e mostre como o relógio deve ser acertado.

O Algoritmo utilizado para sincronização de relógios para máquinas na mesma rede local consiste na utilização de um servidor para a aplicação do algoritmo.

1. Inicialmente o servidor informa sua hora para máquinas clientes.
2. As máquinas clientes por sua vez, respondem com a diferença com seus relógios.
3. O servidor faz a média dos valores recebidos (remove *outleirs*).
4. Servidor transmite a cada cliente ajuste necessário.
5. Clientes fazem ajustes em seus relógios.

#### Questão 6: Explique sucintamente como funciona o Network Time Protocol (NTP).

*Network Time Protocol* é um serviço de sincronização de relógios para a Internet, utilizando UTC como referência. Ele é composto por uma hierarquia de servidores e modelado como cliente/servidor.
Funcionamento:

1. O cliente periodicamente solicita hora a três ou mais servidores.
2. O cliente faz uma estimativa de tempo em relação ao tempo de solicitação e o tempo de resposta.
3. O cliente faz a media entre as três horas estimadas anteriormente.
4. **A hora não é setada diretamente**, ele utiliza a média das horas para atualizar a taxa de progressão do seu relógio de forma que sempre haja uma progressão da hora e nunca "volte no tempo".

#### Questão 7: Considere a figura abaixo com quatro processos e alguns eventos: Assumindo que os valores de relógio lógico inicialmente são zero:

1. (a e r), (h e b).
2. (a e m), (h e m).
3. b||k, b||n, b→u, k||n, k||u, n||u
4. ![](https://i.imgur.com/nYVCcIr.png)
5. ˜
6. ˜
7. ˜
8. ˜

#### Questão 8: Considere a figura acima com três processos e alguns eventos indicados: Utilize o algoritmo para ordenação total de eventos (globally ordered multicast) para definir a ordem em que os eventos indicados serão processados. Mostre o progresso do algoritmo, indicando como suas filas locais mudam com as mensagens e eventos.

Definição dos passos do algoritmo : 

1. L(a) = 1.1, L(b) = 1.3 .
2. Inicio da fila em P1:[a], P2:[] e P3:[b].
3. Depois de a: P1:[a], P2:[a] e P3:[a,b].
4. Depois de b: P1:[a,b], P2:[a,b] e P3:[a,b].
5. Depois de c: P1:[a,b,c], P2:[a,b,c] e P3:[a,b,c].
6. Depois de e: P1:[a,b,c,e], P2:[a,b,c,e] e P3:[a,b,c,e].
7. Depois de d: P1:[a,b,c,e,d], P2:[a,b,c,e,d] e P3:[a,b,c,e,d].

#### Questão 9: Cite e explique uma vantagem e uma desvantagem do algoritmo de exclusão mútua centralizado.

O algoritmo centralizado de exclusão mútua, tem como principio um coordenador que será responsável por coordenar acesso a região crítica, utilizando mensagens para a comunicação e fila para armazenar os pedidos dos processos.
Temos também os processos que fazem a solicitação para entrada na região critica e quando saem, avisam ao coordenador.
Dessa maneira temos como : 

- Vantagem: O número de mensagens para cada pedido de acesso é pequeno(apenas 3: *request*, *grant* e *realease*). Dessa maneira, torna o processo mais eficiente.
- Desvantagem: Ponto único de falha, visto que se o coordenador falhar, não haverá mais coordenação para acesso a região crítica.

#### Questão 10: No algoritmo de exclusão mútua em anel, qual a vantagem de um nó conhecer seus dois próximos vizinhos no anel, ao invés de apenas o próximo? Seria vantajoso conhecer mais de dois vizinhos?

A vantagem de um nó conhecer seus dois próximos vizinhos no anel é que se um de seus vizinho falhar ou desconectar, ainda é possível conectar-se no anel a partir do outro vizinho.
Conhecer mais dois vizinhos seria vantajoso, visto que se mais de dois nós falharem, ainda é possivel reconectar.


#### Questão 11: Em um sistema transacional, o que é ACID? Explique também seu significado.