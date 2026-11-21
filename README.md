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