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
---
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

### Explicação
`List` é baseado em um array dinâmico, o que significa que quando você insere um elemento no meio (como no índice 1), todos os elementos subsequentes precisam ser deslocados para abrir espaço para o novo elemento. Isso resulta em uma operação de tempo O(n) no pior caso.

`LinkedList`, por outro lado, é uma estrutura de dados composta por nós que apontam para o próximo (e possivelmente o anterior) nó. Quando você insere um elemento após o primeiro nó, você simplesmente ajusta os ponteiros dos nós envolvidos, o que é uma operação de tempo O(1). Portanto, `LinkedList` é muito mais eficiente para inserções no meio da lista em comparação com `List`.

---
## Struct x Class

### Struct

1. Vantagens:
   2. Performance: Alocação na stack (geralmente mais rápida). 
   3. Cópia por valor: Isolação de dados, evitando efeitos colaterais. 
   4. Menor overhead: Menos metadados e indireção. 
5. Desvantagens:
   6. Sem herança: Não pode reutilizar código via herança. 
   7. Cópia por valor: Pode ser ineficiente para tipos grandes. 
   8. Limitações: Não pode ter construtor sem parâmetros (antes do C# 10). 
9. Quando usar:
   10. Tipos de dados pequenos e imutáveis (ex: Point, Color, Vector).
   Quando a performance é crítica e a cópia por valor não é um problema.
   Tipos que representam um único valor lógico (ex: enum, bool). 
### Class

1. Vantagens:
   2. Herança: Reutilização de código e polimorfismo. 
   3. Cópia por referência: Eficiente para tipos grandes, evita cópias desnecessárias. 
   4. Flexibilidade: Mais recursos e maior expressividade. 
5. Desvantagens:
   6. Performance: Alocação no heap (geralmente mais lenta). 
   7. Cópia por referência: Possibilidade de efeitos colaterais indesejados. 
   8. Maior overhead: Mais metadados e indireção. 
9. Quando usar:
   10. Tipos de dados complexos com estado mutável.
   Quando a herança e o polimorfismo são necessários.
   Tipos que representam entidades com identidade (ex: Customer, Product).

### Em resumo

Escolha struct quando: Você precisa de performance, os dados são pequenos e imutáveis, e a herança não é necessária.
Escolha class quando: Você precisa de herança, os dados são complexos e mutáveis, e a performance não é tão crítica.