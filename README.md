# Trabalho Prático: Processamento Concorrente de Imagens

<sub>Última atualização: 28/09/2025</sub>

## Sumário

- [Objetivos](#objetivos)
- [Tarefas](#tarefas)
  - [Implementação](#implementação)
  - [Experimentação](#experimentação)
  - [Relato](#relato)
- [Autoria e política de colaboração](#autoria-e-política-de-colaboração)
- [Entrega](#entrega)
- [Avaliação](#avaliação)
- [Dúvidas e informações](#dúvidas-e-informações)

## Objetivos

Os objetivos deste trabalho são colocar em prática a implementação de programas concorrentes utilizando *threads* e analisar o desempenho desses programas frente a cargas de trabalho variadas, bem como explorar as diferenças com relação a programas sequenciais.

## Tarefas

### Implementação

Este trabalho consiste essencialmente em implementar um programa que realiza o processamento de um conjunto de imagens, mais precisamente aplicando uma transformação para escala de cinza (*grayscale*) sobre essas imagens. O programa deverá considerar uma lista de URLs para imagens publicamente disponíveis na Web, para as quais deverá ser feito o *download* para o diretório local de trabalho para que sejam processadas. Ambas as tarefas deverão ser realizadas por *threads*, sendo um tipo de *thread* dedicado exclusivamente para realizar o *download* de imagem e outro para realizar o seu processamento, respeitando o [Princípio da Responsabilidade Única](https://giovannamoeller.medium.com/o-princípio-da-responsabilidade-única-single-responsibility-principle-srp-do-solid-21707ec45304) (SRP, do inglês *single-responsibility principle*). Para avaliar o desempenho do programa implementado, **apenas** o tempo de execução para realização da tarefa de processamento das imagens, a fim de minimizar a variabilidade decorrente da latência da rede na realização dos *downloads*. Cada *thread* deverá realizar o *download* e o processamento de **uma única** imagem, ou seja, para $n$ imagens, deverá haver $n$ *threads* para realizar o *download* e $n$ *threads* para o processamento.

O programa poderá ser implementado utilizando facilidades providas pelas linguagens de programação C++, Java **ou** Python, à escolha. Outra linguagem de programação diferente dessas três poderá ser utilizada contanto a proposta seja previamente validada com o docente. O desenvolvimento da solução deve de antemão visar à busca de desenvolvimento de software de qualidade, isto é, funcionando correta e eficientemente, exaustivamente testado, bem documentado e com tratamento adequado de eventuais exceções.

Os seguintes repositórios, todos disponíveis publicamente no GitHub, poderão ser utilizados como ponto de partida para a implementação do programa:

- [Versão em Java](https://github.com/evertonrsc/java-imageprocessing)
- [Versão em C++](https://github.com/evertonrsc/cpp-imageprocessing)
- [Versão em Python](https://github.com/evertonrsc/py-imageprocessing)

Cada um desses programas realiza o processamento de um conjunto de imagens para transformá-las para escala de cinza de forma **sequencial**, uma após uma. Dessa forma, a tarefa deste trabalho consistiria basicamente em modificar essas implementações para produzir uma versão concorrente utilizando *threads*.

Nos repositórios ora disponibilizados, a lista de URLs das imagens a serem processadas é gerada automaticamente por um modelo de Inteligência Artificial (IA) generativa, mais especificamente o [Google Gemini](https://ai.google.dev/gemini-api/docs/models) em sua versão 2.5 Flash-Lite por meio de sua [API](https://ai.google.dev/api). No entanto, é importante ressaltar que o seu uso **não é obrigatório**. Essa integração com um modelo de IA generativa foi realizada unicamente com o propósito de facilitar a tarefa de obter a lista de URLs das imagens, para que o esforço focasse apenas na implementação concorrente. Apesar de o objetivo primário não ser o aprendizado quanto ao uso programático de um modelo de IA generativa, este trabalho possibilita experenciar de forma relativamente simples como um modelo desse tipo pode ser utilizado na prática para facilitar a geração de casos e entradas de teste em projetos de *software* no mundo real.

### Experimentação

A tarefa de experimentação consiste em realizar sucessivas execuções do programa concorrente implementado considerando uma carga crescente de trabalho, com conjuntos de 10, 25, 50, 100, 250, 500, 1000 e 2500 imagens. Essa variação possibilitará explorar o que acontece quando um grande número de *threads* é criado e a capacidade do sistema operacional para lidar com elas de forma eficiente. De antemão, destaque-se que é possível que haja dificuldades para a execução de cenários com números maiores de imagens a serem processadas, em função dos limites impostos pelo sistema operacional, o que deverá ser devidamente registrado no relato (ver #relato). No entanto, esse comportamento é esperado e intencional para este trabalho, uma vez que será possível explorar de forma prática os limites da programação concorrente.

A versão sequencial do programa também deverá ser submetida à mesma carga de trabalho (incluindo a mensuração do tempo de execução) para possibilitar uma análise comparativa em relação à versão sequencial. Do ponto de vista quantitativo, essa análise pode ser feita determinando o ganho de desempenho (*speed-up*) eventualmente obtido com a versão concorrente. Esse cálculo pode ser realizado através da seguinte equação:

$$ S = \frac{T_s}{T_c} $$

sendo $S$ o *speed-up*, $T_s$ o tempo médio despendido pela versão sequencial e $T_c$ o tempo médio despendido pela versão concorrente. Caso o valor do *speed-up* seja superior a 1, tem-se que a versão concorrente é de fato melhor em termos de desempenho do que a sequencial.

**Observação:** É bem sabido que o início da execução de um programa implementado na linguagem de programação Java é afetado de forma relativamente prejudicial pelo carregamento de classes na memória realizado pela máquina virtual Java (JVM) antes da execução propriamente dita do programa, o que se chama *warm-up*. Com isso, apenas após esse processo de carregamento ter sido concluído é que se pode mensurar de forma confiável o desempenho do programa. Caso os programas objeto deste trabalho tenham sido implementados nessa linguagem de programação, a estratégia mais simples para uma medição confiável (uma vez que os programas em questão não possuem requisitos estritos de latência) é realizar algumas execuções do programa e as desconsiderar, justamente pelo fato de os tempos de execução observados serem certamente influenciados pelo tempo de *warm-up* da JVM.

### Relato

Uma vez realizadas as tarefas de implementação e de experimentação, deverá ser elaborado um relatório contendo, no mínimo, as seguintes seções:

1. **Introdução.** Descrever a implementação da versão concorrente do programa, incluindo desafios enfrentados nesse processo.
2. **Metodologia.** Apresentar a caracterização técnica do computador utilizado (processador, sistema operacional, quantidade de memória RAM), a linguagem de programação e a versão do compilador empregados, os cenários considerados, entre outras informações. Deverão também ser descritos qual o procedimento adotado para gerar os resultados, como a comparação entre as versões foi feita etc.
3. **Resultados.** Apresentar os resultados obtidos com relação aos tempos observados na execução das versões sequencial e concorrente na forma de gráfico de linha e tabelas.
4. **Conclusões.** Discutir os resultados na perspectiva do relacionamento entre o desempenho dos programas e a carga de trabalho, bem como uma análise do ganho de desempenho (*speed-up*).

## Autoria e política de colaboração

Este trabalho deverá necessariamente ser realizado em equipe composta de **até dois estudantes**, sendo importante, dentro do possível, dividir as tarefas igualmente entre os integrantes da equipe. Após a implementação das soluções para os problemas propostos, o arquivo [`author.md`](https://github.com/ufrn-concprog/image-processing/tree/master/author.md) presente no repositório deverá ser editado preenchendo as informações de identificação dos integrantes da equipe, na seção [Informações de Autoria](https://github.com/ufrn-concprog/image-processing/tree/master/author.md#identificação-de-autoria).

O trabalho em cooperação entre estudantes da turma é estimulado, sendo admissível a discussão de ideias e estratégias. Contudo, tal interação não deve ser entendida como permissão para utilização de (parte de) código-fonte de colegas, o que pode caracterizar situação de plágio. **Trabalhos copiados no todo ou em parte de outros colegas ou da Internet ou ainda gerados por ferramentas de IA serão anulados e receberão nota zero.**

## Entrega

O sistema de controle de versões [Git](https://git-scm.com) e o serviço de hospedagem de repositórios [GitHub](https://git-scm.com) serão utilizados para possibilitar a entrega da implementação realizada. Para possibilitar a associação de repositórios Git para cada equipe e reuni-los sob uma mesma infraestrutura, foi criada uma atividade (*assignment*) no GitHub Classroom. Cada integrante de equipe deverá acessar este [*link*](https://classroom.github.com/a/q7gvbCvu), aceitar o convite para ingressar no GitHub Classroom e finalmente seguir as instruções em tela para acessar a atividade e ingressar em uma equipe existente ou criar outra. Este [vídeo](https://youtu.be/ObaFRGp_Eko) demonstra como ocorre esse processo.

No momento de criação de uma equipe, o GitHub Classroom cria um repositório Git privado acessível unicamente pelos integrantes da equipe e pelo docente, sob a organização [`ufrn-concprog`](https://github.com/ufrn-concprog). A fim de garantir a boa manutenção do repositório, deve-se ainda configurar corretamente o arquivo `.gitignore` para desconsiderar arquivos que não devam ser versionados, a exemplo dos arquivos executáveis gerados a partir da compilação do código-fonte. Também não é necessário versionar os arquivos de saída resultantes do processo de ordenação.

A entrega deste trabalho deverá ser realizada até as **23:59 do dia 7 de outubro de 2025** no respectivo repositório Git da equipe. O relatório elaborado, preferencialmente em formato *Adobe Portable Format* (PDF), deverá ser enviado através da opção *Tarefas* da Turma Virtual do SIGAA, juntamente com, no campo *Comentários*, o endereço do repositório. **Um único membro da equipe deve realizar esse envio** e não serão aceitos envios por outros meios ou repositórios que não sejam os descritos nesta especificação.

## Avaliação

A avaliação do trabalho contabilizará nota de até 4,0 pontos na 1ª Unidade da disciplina. O trabalho será avaliado de acordo com os seguintes critérios:

- utilização correta dos conceitos de *threads*;
- corretude da execução do programa implementado;
- aplicação de boas práticas de programação, incluindo legibilidade, organização e documentação de código fonte;
- correta utilização do repositório Git, no qual deverá ser registrado todo o histórico da implementação por meio de *commits*, e;
- qualidade do relatório produzido.

## Dúvidas e informações

Caso haja qualquer dúvida, questionamento ou necessidade de informação adicional, é possível:

- enviar *e-mail* para o endereço <everton.cavalcante@ufrn.br>;
- enviar mensagem privada diretamente ao docente, utilizando o servidor Discord;
- enviar mensagem no canal de texto `#duvidas` no servidor Discord, ou;
- agendar encontros síncronos pelo canal de áudio `Fale com Prof. Everton` no servidor Discord.
