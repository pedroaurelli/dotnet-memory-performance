# .NET: gerenciamento de memória para otimização de performance

## Stack:
Armazena tipos de valores primitivos como int, bool, double, byte, etc.
É uma estrutura de "pilha de dados" para armazenar esses tipos de valores.

## Heap:
Armazena tipos mais complexos, como objetos (instâncias de classes).
Quando você cria um novo objeto (ex: Usuario usuario = new Usuario()), os valores desse objeto são armazenados na heap.

## LOH (Large Object Heap):
É uma área na memória heap reservada para objetos grandes, com tamanho igual ou superior a 85 kb.

## Referências:
Quando você cria um objeto, a referência (ponteiro) para a localização desse objeto na heap é armazenada na stack.
Múltiplas referências podem apontar para o mesmo objeto na heap.

## List x LinkedList
### Teste 1. Adicionando elementos no final
```c#
ChavesDeAcesso = new List<Guid>(new Guid[10]);
Stopwatch stopwatch = new Stopwatch();
stopwatch.Start();
for (int i = 0; i < 100000; i++)
{
    ChavesDeAcesso.Add(Guid.NewGuid());
}
stopwatch.Stop();
Console.WriteLine($"Tempo total List em ms: {stopwatch.Elapsed.TotalMilliseconds}");
// Tempo total List em ms: 92,7232

ChavesDeAcessoLinkedList = new LinkedList<Guid>();
Stopwatch stopwatchLinkedList = new Stopwatch();
ChavesDeAcessoLinkedList.AddFirst(Guid.NewGuid());
stopwatchLinkedList.Start();
for (int i = 0; i < 100000; i++)
{
    ChavesDeAcessoLinkedList.AddLast(Guid.NewGuid());
}
stopwatchLinkedList.Stop();
Console.WriteLine($"Tempo total LinkedList em ms: {stopwatchLinkedList.Elapsed.TotalMilliseconds}");
// Tempo total LinkedList em ms: 108,527

```
### Teste 2. Adicionado elementos no index 1
```c#
ChavesDeAcesso = new List<Guid>(new Guid[10]);
Stopwatch stopwatch = new Stopwatch();
stopwatch.Start();
for (int i = 0; i < 100000; i++)
{
    ChavesDeAcesso.Insert(1, Guid.NewGuid());
}
stopwatch.Stop();
Console.WriteLine($"Tempo total List em ms: {stopwatch.Elapsed.TotalMilliseconds}");
// Tempo total List em ms: 2065,8273

ChavesDeAcessoLinkedList = new LinkedList<Guid>();
Stopwatch stopwatchLinkedList = new Stopwatch();
ChavesDeAcessoLinkedList.AddFirst(Guid.NewGuid());
stopwatchLinkedList.Start();
for (int i = 0; i < 100000; i++)
{
    ChavesDeAcessoLinkedList.AddAfter(ChavesDeAcessoLinkedList.First, Guid.NewGuid());
}
stopwatchLinkedList.Stop();
Console.WriteLine($"Tempo total LinkedList em ms: {stopwatchLinkedList.Elapsed.TotalMilliseconds}");
// Tempo total LinkedList em ms: 101,7579
```

### Exmplicação
`List` é baseado em um array dinâmico, o que significa que quando você insere um elemento no meio (como no índice 1), todos os elementos subsequentes precisam ser deslocados para abrir espaço para o novo elemento. Isso resulta em uma operação de tempo O(n) no pior caso.

`LinkedList`, por outro lado, é uma estrutura de dados composta por nós que apontam para o próximo (e possivelmente o anterior) nó. Quando você insere um elemento após o primeiro nó, você simplesmente ajusta os ponteiros dos nós envolvidos, o que é uma operação de tempo O(1). Portanto, `LinkedList` é muito mais eficiente para inserções no meio da lista em comparação com `List`.
