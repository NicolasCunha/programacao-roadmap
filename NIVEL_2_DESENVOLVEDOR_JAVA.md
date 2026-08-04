# Nível 2 - Desenvolvedor Java

Neste nível, o estudante migra da lógica em VisuAlg para Java. A lógica continua a mesma, mas a sintaxe, a organização e os conceitos de execução ficam mais próximos do mercado.

---

## Índice de progresso

- [ ] [Objetivo do nível](#objetivo-do-nível)
- [ ] [Uso responsável de IA](#uso-responsável-de-ia)
- [ ] [Ponte entre VisuAlg e Java](#ponte-entre-visualg-e-java)
- [ ] [JVM, JDK, JRE e compilação](#jvm-jdk-jre-e-compilação)
- [ ] [Stack, heap e memória](#stack-heap-e-memória)
- [ ] [Primeiro programa em Java](#primeiro-programa-em-java)
- [ ] [Tipos, variáveis e entrada de dados](#tipos-variáveis-e-entrada-de-dados)
- [ ] [Condicionais, repetições e métodos](#condicionais-repetições-e-métodos)
- [ ] [Classes, objetos e encapsulamento](#classes-objetos-e-encapsulamento)
- [ ] [Orientação a objetos](#orientação-a-objetos)
- [ ] [Collections e exceções](#collections-e-exceções)
- [ ] [Lambdas, Streams e Optional](#lambdas-streams-e-optional)
- [ ] [Recursão em Java](#recursão-em-java)
- [ ] [SOLID](#solid)
- [ ] [Checklist de conclusão](#checklist-de-conclusão)

---

## Objetivo do nível

Ao final deste nível, o estudante deve conseguir escrever programas Java, entender classes e objetos, aplicar orientação a objetos, usar coleções, tratar exceções e entender conceitos básicos de execução e memória.

---

## Uso responsável de IA

Não use IA para gerar código completo nesta fase. Java exige prática manual para fixação de:

- Chaves `{}`.
- Ponto e vírgula.
- Nomes de classes e métodos.
- Tipos.
- Mensagens de erro.
- Estrutura de projetos.

Use IA apenas para explicar dúvidas pontuais ou revisar uma tentativa que você já fez.

---

## Ponte entre VisuAlg e Java

VisuAlg:

```visualg
se idade >= 18 entao
   escreval("Maior de idade")
senao
   escreval("Menor de idade")
fimse
```

Java:

```java
if (idade >= 18) {
    System.out.println("Maior de idade");
} else {
    System.out.println("Menor de idade");
}
```

A lógica é a mesma. A sintaxe muda.

---

## JVM, JDK, JRE e compilação

### JDK

JDK significa Java Development Kit. É o pacote usado por quem desenvolve em Java. Ele contém ferramentas como o compilador `javac`.

### JRE

JRE significa Java Runtime Environment. É o ambiente necessário para executar aplicações Java.

### JVM

JVM significa Java Virtual Machine. Ela executa o bytecode Java.

Fluxo simplificado:

```text
Arquivo .java -> compilador javac -> arquivo .class -> JVM -> programa executando
```

Exemplo:

```bash
javac OlaMundo.java
java OlaMundo
```

### Bytecode

Bytecode é o resultado da compilação do Java. Ele não é código de máquina direto, mas um formato entendido pela JVM.

---

## Stack, heap e memória

Para começar, pense na memória Java em duas áreas importantes: **stack** e **heap**.

### Stack, ou pilha

A stack guarda informações temporárias das chamadas de métodos.

Exemplo:

```java
public class ExemploStack {
    public static void main(String[] args) {
        int resultado = somar(2, 3);
        System.out.println(resultado);
    }

    static int somar(int a, int b) {
        return a + b;
    }
}
```

Quando `main` chama `somar`, a JVM cria uma nova área na pilha para aquela chamada. Quando `somar` termina, essa área é removida.

### Heap

A heap guarda objetos criados com `new`.

```java
Cliente cliente = new Cliente();
```

A variável `cliente` fica como referência, enquanto o objeto criado fica na heap.

### Referência

Uma referência aponta para um objeto.

```java
Cliente a = new Cliente();
Cliente b = a;
```

Nesse exemplo, `a` e `b` apontam para o mesmo objeto.

### Garbage Collector

O Garbage Collector remove objetos da heap que não são mais acessíveis.

O estudante não precisa controlar isso manualmente no começo, mas deve entender que objetos podem deixar de ser usados e a JVM pode limpar essa memória.

### Stack overflow

Stack overflow pode acontecer quando há chamadas de método demais, geralmente por recursão sem parada.

---

## Primeiro programa em Java

```java
public class OlaMundo {
    public static void main(String[] args) {
        System.out.println("Ola, mundo!");
    }
}
```

Explicação:

- `class`: define uma classe.
- `main`: ponto de entrada.
- `{}`: delimitam blocos.
- `System.out.println`: imprime na tela.
- `;`: finaliza comandos.

---

## Tipos, variáveis e entrada de dados

```java
int idade = 25;
double preco = 49.90;
String nome = "Maria";
boolean ativo = true;
```

Entrada:

```java
import java.util.Scanner;

public class Cadastro {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.print("Nome: ");
        String nome = scanner.nextLine();

        System.out.print("Idade: ");
        int idade = scanner.nextInt();

        System.out.println(nome + " - " + idade);
        scanner.close();
    }
}
```

---

## Condicionais, repetições e métodos

```java
if (idade >= 18) {
    System.out.println("Maior de idade");
} else {
    System.out.println("Menor de idade");
}
```

```java
for (int i = 1; i <= 10; i++) {
    System.out.println(i);
}
```

```java
static int somar(int a, int b) {
    return a + b;
}
```

---

## Classes, objetos e encapsulamento

```java
class Conta {
    private double saldo;

    public void depositar(double valor) {
        if (valor <= 0) {
            throw new IllegalArgumentException("Valor deve ser positivo");
        }
        saldo += valor;
    }

    public double getSaldo() {
        return saldo;
    }
}
```

Encapsulamento protege os dados e força alterações por regras controladas.

---

## Orientação a objetos

- Abstração: escolher o que importa.
- Encapsulamento: proteger dados.
- Herança: reaproveitar características.
- Polimorfismo: tratar objetos diferentes por um mesmo contrato.
- Composição: montar objetos a partir de outros objetos.

Exemplo de polimorfismo:

```java
interface Pagamento {
    void pagar(double valor);
}

class PagamentoPix implements Pagamento {
    public void pagar(double valor) {
        System.out.println("Pix: " + valor);
    }
}
```

---

## Collections e exceções

List:

```java
List<String> nomes = new ArrayList<>();
nomes.add("Ana");
```

Map:

```java
Map<Integer, String> produtos = new HashMap<>();
produtos.put(1, "Mouse");
```

Exceção:

```java
if (valor > saldo) {
    throw new IllegalStateException("Saldo insuficiente");
}
```

---

## Lambdas, Streams e Optional

```java
nomes.forEach(nome -> System.out.println(nome));
```

```java
List<String> filtrados = nomes.stream()
    .filter(nome -> nome.startsWith("A"))
    .toList();
```

```java
Optional<String> valor = Optional.ofNullable(buscarNome());
```

---

## Recursão em Java

Recursão é quando um método chama a si mesmo.

Ela precisa de:

1. Caso base.
2. Chamada recursiva com problema menor.

Exemplo: fatorial.

```java
public class FatorialRecursivo {
    public static void main(String[] args) {
        System.out.println(fatorial(5));
    }

    static int fatorial(int n) {
        if (n <= 1) {
            return 1;
        }

        return n * fatorial(n - 1);
    }
}
```

Fluxo:

```text
fatorial(5)
5 * fatorial(4)
5 * 4 * fatorial(3)
5 * 4 * 3 * fatorial(2)
5 * 4 * 3 * 2 * fatorial(1)
```

Cuidado: se não houver caso base, o método chama a si mesmo até causar `StackOverflowError`.

---

## SOLID

SOLID ajuda a criar código mais fácil de manter.

- SRP: uma classe deve ter uma responsabilidade principal.
- OCP: aberto para extensão, fechado para modificação.
- LSP: subclasses devem respeitar o contrato da classe base.
- ISP: interfaces pequenas e específicas.
- DIP: depender de abstrações.

---

## Checklist de conclusão

- [ ] Sei explicar JDK, JRE, JVM e bytecode.
- [ ] Entendo stack, heap, referência e Garbage Collector em nível inicial.
- [ ] Fiz o Olá Mundo em Java.
- [ ] Sei usar tipos básicos.
- [ ] Sei usar `Scanner`.
- [ ] Sei usar condições e repetições.
- [ ] Sei criar métodos.
- [ ] Sei criar classes e objetos.
- [ ] Entendo encapsulamento.
- [ ] Entendo recursão básica.
- [ ] Sei usar `List`, `Set` e `Map`.
- [ ] Entendo exceções.
