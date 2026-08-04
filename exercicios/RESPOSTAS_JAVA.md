# Respostas - Exercícios Java

> Use depois de tentar. As respostas são referências compactas.

## Fácil

### 1
```java
public class OlaMundo {
    public static void main(String[] args) {
        System.out.println("Ola, mundo!");
    }
}
```

### 2
```java
Scanner sc = new Scanner(System.in);
String nome = sc.nextLine();
int idade = sc.nextInt();
System.out.println(nome + " - " + idade);
sc.close();
```

### 3 a 15
- 3: leia `double a`, `double b`; imprima operações.
- 4: `if (idade >= 18)`.
- 5: `double media = (n1 + n2) / 2`.
- 6: `static int somar(int a, int b) { return a + b; }`.
- 7: `static boolean ehPar(int n) { return n % 2 == 0; }`.
- 8: `for (int i = 1; i <= 10; i++)`.
- 9: `while (contador >= 1)`.
- 10: `class Cliente { String nome; String email; }`.
- 11: `Cliente c = new Cliente(); c.nome = "Ana";`.
- 12: `class Produto { String nome; double preco; int quantidade; }`.
- 13: `List<String> nomes = new ArrayList<>();`.
- 14: `Map<Integer,String> produtos = new HashMap<>();`.
- 15: envolva divisão em `try { } catch (ArithmeticException e) { }`.

## Médio

### 1 Conta
```java
public class Conta {
    private double saldo;
    public void depositar(double valor) {
        if (valor <= 0) throw new IllegalArgumentException("Valor invalido");
        saldo += valor;
    }
    public void sacar(double valor) {
        if (valor <= 0) throw new IllegalArgumentException("Valor invalido");
        if (valor > saldo) throw new IllegalStateException("Saldo insuficiente");
        saldo -= valor;
    }
    public double getSaldo() { return saldo; }
}
```

### 2 a 15
- 2: `while(opcao != 4)` lendo opções com `Scanner` e chamando métodos de `Conta`.
- 3: valide no construtor ou setter: `if (preco < 0) throw`.
- 4: `List<Contato> contatos = new ArrayList<>();`.
- 5: percorra lista e compare `contato.getNome().equalsIgnoreCase(nome)`.
- 6: `Map<Integer, Produto> estoque` com código como chave.
- 7: `class SaldoInsuficienteException extends RuntimeException`.
- 8: `interface Pagamento { void pagar(BigDecimal valor); }`.
- 9: `List<Pagamento>` e chame `pagar` em cada item.
- 10: `produtos.stream().filter(p -> p.getPreco() > 100).toList()`.
- 11: `Optional<Cliente> buscarPorCpf(String cpf)`.
- 12: classe `final`, campo `private final String valor`, sem setter.
- 13: `return n <= 1 ? 1 : n * fatorial(n - 1);`.
- 14: `return n == 0 ? 0 : n + soma(n - 1);`.
- 15: crie cenários no `main`: depósito válido, saque válido, saque inválido.

## Difícil

1. Biblioteca: classes `Livro`, `Usuario`, `Emprestimo`; um `BibliotecaService` controla regras.
2. Banco: `Conta` abstrata; subclasses sobrescrevem regras; `Cliente` possui contas.
3. Estoque: `ProdutoRepositoryEmMemoria` com `Map<Integer, Produto>`; `EstoqueService` concentra regras.
4. Pedidos: `Pedido` possui `List<ItemPedido>`; total é soma dos subtotais.
5. Strategy: `Pagamento` como interface; `Pix`, `Cartao`, `Boleto` como implementações.
6. Factory: método `criar(String tipo)` retorna estratégia correta.
7. Facade: `CheckoutFacade.finalizar()` chama validação, estoque, pagamento, notificação.
8. Builder: `Pedido.Builder().cliente(c).adicionarItem(i).build()`.
9. `equals/hashCode`: use `Objects.equals(cpf, other.cpf)` e `Objects.hash(cpf)`.
10. Login: `Map<String, Usuario>`; compare senha armazenada.
11. Streams: use `count`, `average`, `max`, `filter`.
12. Polimorfismo: `Funcionario.calcularBonus()` sobrescrito por subclasses.
13. Recursão categorias: categoria tem `List<Categoria> filhas`; percorra chamando o método nas filhas.
14. Carrinho: `Map<Produto, Integer>` ou lista de itens.
15. SRP: separe entidade, serviço e repositório em memória.
