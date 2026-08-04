# Nível 2 - Desenvolvedor Java

## Índice

- [ ] [Uso responsável de IA](#uso-responsável-de-ia)
- [ ] [JVM, JDK, JRE e bytecode](#jvm-jdk-jre-e-bytecode)
- [ ] [Stack, heap, referência e Garbage Collector](#stack-heap-referência-e-garbage-collector)
- [ ] [Sintaxe base](#sintaxe-base)
- [ ] [Orientação a objetos](#orientação-a-objetos)
- [ ] [Collections, exceções e streams](#collections-exceções-e-streams)
- [ ] [Recursão em Java](#recursão-em-java)
- [ ] [SOLID](#solid)

## Uso responsável de IA

Não use IA para gerar classes prontas. Escreva manualmente para memorizar sintaxe, chaves, ponto e vírgula, tipos e mensagens de erro.

## JVM, JDK, JRE e bytecode

- JDK: kit para desenvolver, com compilador `javac`.
- JRE: ambiente para executar aplicações Java.
- JVM: máquina virtual que executa bytecode.
- Bytecode: arquivo `.class` gerado a partir do `.java`.

```text
.java -> javac -> .class -> JVM
```

## Stack, heap, referência e Garbage Collector

- Stack: pilha de chamadas de métodos e variáveis locais.
- Heap: área onde ficam objetos criados com `new`.
- Referência: variável que aponta para um objeto na heap.
- Garbage Collector: remove objetos que não podem mais ser acessados.

```java
Cliente a = new Cliente();
Cliente b = a; // a e b apontam para o mesmo objeto
```

Recursão sem parada pode causar `StackOverflowError`.

## Sintaxe base

```java
public class OlaMundo {
    public static void main(String[] args) {
        System.out.println("Ola, mundo!");
    }
}
```

```java
int idade = 20;
double preco = 10.50;
String nome = "Ana";
boolean ativo = true;
```

## Orientação a objetos

- Classe: molde.
- Objeto: instância.
- Método: comportamento.
- Encapsulamento: proteger dados.
- Herança: reaproveitar características.
- Polimorfismo: tratar implementações diferentes pelo mesmo contrato.
- Composição: montar objetos com outros objetos.

```java
class Conta {
    private double saldo;
    public void depositar(double valor) { saldo += valor; }
    public double getSaldo() { return saldo; }
}
```

## Collections, exceções e streams

```java
List<String> nomes = new ArrayList<>();
Set<String> cpfs = new HashSet<>();
Map<Integer, String> produtos = new HashMap<>();
```

```java
if (valor > saldo) {
    throw new IllegalStateException("Saldo insuficiente");
}
```

```java
List<String> filtrados = nomes.stream()
    .filter(n -> n.startsWith("A"))
    .toList();
```

## Recursão em Java

```java
static int fatorial(int n) {
    if (n <= 1) return 1;
    return n * fatorial(n - 1);
}
```

## SOLID

- SRP: responsabilidade única.
- OCP: aberto para extensão, fechado para modificação.
- LSP: subclasses devem respeitar o contrato da classe base.
- ISP: interfaces pequenas e específicas.
- DIP: dependa de abstrações.

Exemplo SRP: não deixe `Pedido` calcular total, salvar no banco e enviar email. Separe em `Pedido`, `CalculadoraPedido`, `PedidoRepository` e `ServicoEmail`.
