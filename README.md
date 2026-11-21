# Nome: Daniel Santos da Silva
# Fernando Cruz de Oliveira
# Maravania de Paula Souza

# Estudo de Caso 2 - Problema Produtor/Consumidor (Buffer Não Sincronizado)

Repositório com os códigos da atividade prática de Sistemas Operacionais, baseado nos exemplos de concorrência em Java do livro *Java: Como Programar* (Deitel) e nos conceitos teóricos de sincronização de processos/threads (Carlos Maziero).

## Estrutura do Projeto

- `Buffer.java`: Interface que define os métodos básicos de escrita (`set`) e leitura (`get`) no buffer compartilhado.
- `UnsynchronizedBuffer.java`: Implementação simples do buffer usando uma variável inteira (`buffer = -1`) sem controle de acesso simultâneo.
- `Producer.java`: Thread responsável por gerar os valores de `1` a `4` em intervalos aleatórios de até 3 segundos e gravar no buffer.
- `Consumer.java`: Thread responsável por ler o buffer quatro vezes em intervalos aleatórios e calcular a soma total dos valores lidos.
- `SharedBufferTest.java`: Classe principal (`main`) que instancia o buffer compartilhado e inicia as threads `Producer` e `Consumer`.

## Como Compilar e Executar

No terminal, dentro da pasta onde estão os arquivos `.java`, execute:

```bash
javac *.java
java SharedBufferTest





Fundamentação Teórica e Análise da Execução
De acordo com o Capítulo 5 do Deitel, quando duas ou mais threads executam de forma concorrente e assíncrona acessando um mesmo dado compartilhado, o chaveamento de contexto pode ocorrer a qualquer instante. Como a manipulação da variável buffer é uma seção crítica e a classe UnsynchronizedBuffer não aplica exclusão mútua (acesso serializado de apenas uma thread por vez), ocorre corrupção e inconsistência nos dados.

Como o escalonador do sistema operacional alterna as threads de forma imprevisível, o resultado final da soma no consumidor (que deveria ser 1 + 2 + 3 + 4 = 10) muda a cada execução, reproduzindo os três cenários exemplificados na Figura 5.5 do livro:

O Consumer acorda antes do Producer escrever o primeiro valor e acaba lendo o estado inicial -1. Além disso, lê o valor 1 três vezes seguidas antes que o produtor atualize o buffer, totalizando 2 em vez de 10.

O Producer executa duas vezes seguidas (escreve 1 e escreve 2) antes de o Consumer ler, fazendo com que o valor 1 seja perdido. No final, o consumidor lê o 4 duas vezes e fecha com soma 13.

Mesmo sem bloqueios no código, os tempos aleatórios de sleep podem acabar alternando perfeitamente cada escrita e leitura (1, 2, 3, 4), resultando na soma esperada 10 — mas sem nenhuma garantia de que isso se repetirá na próxima execução.

Como resolver o problema:
Para garantir a comunicação segura entre o produtor e o consumidor sem perda de dados ou leitura de valores repetidos, o capítulo apresenta o uso de semáforos ou monitores em Java (synchronized, wait() e notifyAll()):

Um controle bloqueia o Producer caso o valor anterior ainda não tenha sido consumido.

Outro controle bloqueia o Consumer até que um novo valor seja efetivamente produzido no buffer.
