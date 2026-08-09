# Respostas - Exercícios Java

> Use este arquivo apenas depois de tentar. As respostas são referências, não a única forma correta — nomes de variáveis e pequenos detalhes podem mudar sem problema. Alguns exemplos omitem `import`s óbvios (`java.util.*`) para focar no essencial.

## Nível Fácil — Estação Espacial

### 1 - Primeira transmissão

```java
public class Aurora {
    public static void main(String[] args) {
        System.out.println("Ola, mundo!");
    }
}
```

O nome do arquivo precisa ser `Aurora.java`, igual ao nome da classe pública.

### 2 - Cadastro de tripulante

```java
Scanner sc = new Scanner(System.in);
System.out.print("Nome: ");
String nome = sc.nextLine();
System.out.print("Idade: ");
int idade = sc.nextInt();
System.out.println(nome + " - " + idade + " anos");
sc.close();
```

### 3 - Consumo de combustível

```java
Scanner sc = new Scanner(System.in);
double a = sc.nextDouble();
double b = sc.nextDouble();
System.out.println("Soma: " + (a + b));
System.out.println("Subtracao: " + (a - b));
System.out.println("Multiplicacao: " + (a * b));
System.out.println("Divisao: " + (a / b));
```

`double`, não `int`, porque combustível é medido em litros com casas decimais.

### 4 - Cadete apto para EVA

```java
Scanner sc = new Scanner(System.in);
int idade = sc.nextInt();
if (idade >= 18) {
    System.out.println("Apto para EVA");
} else {
    System.out.println("Nao apto para EVA");
}
```

### 5 - Leituras de oxigênio

```java
Scanner sc = new Scanner(System.in);
double leitura1 = sc.nextDouble();
double leitura2 = sc.nextDouble();
double media = (leitura1 + leitura2) / 2;
System.out.println("Media: " + media);
```

### 6 - Módulo de acoplamento

```java
static int somar(int a, int b) {
    return a + b;
}
```

### 7 - Ala Leste ou Oeste

```java
static boolean ehPar(int numero) {
    return numero % 2 == 0;
}
```

### 8 - Órbitas por dia

```java
Scanner sc = new Scanner(System.in);
int numero = sc.nextInt();
for (int i = 1; i <= 10; i++) {
    System.out.println(numero + " x " + i + " = " + (numero * i));
}
```

### 9 - Contagem regressiva de lançamento

```java
int contador = 10;
while (contador >= 1) {
    System.out.println(contador);
    contador--;
}
System.out.println("Lancamento!");
```

### 10 - Classe Tripulante

```java
public class Tripulante {
    String nome;
    String email;
}
```

### 11 - Objeto Tripulante

```java
Tripulante t = new Tripulante();
t.nome = "Ana";
t.email = "ana@aurora.space";
System.out.println(t.nome + " - " + t.email);
```

### 12 - Classe Equipamento

```java
public class Equipamento {
    String nome;
    double preco;
    int quantidade;
}
```

### 13 - Tripulação na ponte de comando

```java
List<String> tripulantesDePlantao = new ArrayList<>();
tripulantesDePlantao.add("Ana");
tripulantesDePlantao.add("Bruno");

for (String nome : tripulantesDePlantao) {
    System.out.println(nome);
}
```

### 14 - Mapa de equipamentos por compartimento

```java
Map<Integer, String> equipamentos = new HashMap<>();
equipamentos.put(101, "Traje espacial");
equipamentos.put(102, "Kit de reparo");

System.out.println(equipamentos.get(101));
```

### 15 - Distribuição de oxigênio

```java
Scanner sc = new Scanner(System.in);
int oxigenioDisponivel = sc.nextInt();
int tripulantes = sc.nextInt();

try {
    int cota = oxigenioDisponivel / tripulantes;
    System.out.println("Cota por tripulante: " + cota);
} catch (ArithmeticException e) {
    System.out.println("Sensor de tripulacao com falha: nao ha tripulantes registrados");
}
```

`ArithmeticException` é lançada pelo Java automaticamente numa divisão inteira por zero — o `catch` intercepta antes que o programa quebre sem explicação.

---

## Nível Médio — Fintech BeeBank

### 1 - Classe Conta

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

    public double getSaldo() {
        return saldo;
    }
}
```

### 2 - App da BeeBank no terminal

```java
Scanner sc = new Scanner(System.in);
Conta conta = new Conta();
int opcao = -1;

while (opcao != 4) {
    System.out.println("1.Depositar 2.Sacar 3.Saldo 4.Sair");
    opcao = sc.nextInt();

    switch (opcao) {
        case 1 -> {
            System.out.print("Valor: ");
            conta.depositar(sc.nextDouble());
        }
        case 2 -> {
            System.out.print("Valor: ");
            conta.sacar(sc.nextDouble());
        }
        case 3 -> System.out.println("Saldo: " + conta.getSaldo());
        case 4 -> System.out.println("Encerrando...");
        default -> System.out.println("Opcao invalida");
    }
}
```

### 3 - Produto de investimento

```java
public class Produto {
    private String nome;
    private double preco;

    public Produto(String nome, double preco) {
        if (preco < 0) throw new IllegalArgumentException("Preco nao pode ser negativo");
        this.nome = nome;
        this.preco = preco;
    }

    public void setPreco(double preco) {
        if (preco < 0) throw new IllegalArgumentException("Preco nao pode ser negativo");
        this.preco = preco;
    }

    public double getPreco() {
        return preco;
    }
}
```

Validar tanto no construtor quanto em `setPreco` garante que a regra vale sempre, não só na criação do objeto.

### 4 - Beneficiários cadastrados

```java
public class Contato {
    String nome;
    String telefone;
    String email;
}
```

```java
List<Contato> beneficiarios = new ArrayList<>();
Contato c = new Contato();
c.nome = "Bruno";
c.telefone = "11999998888";
c.email = "bruno@email.com";
beneficiarios.add(c);
```

### 5 - Busca de beneficiário

```java
Contato buscarPorNome(List<Contato> lista, String nome) {
    for (Contato c : lista) {
        if (c.nome.equalsIgnoreCase(nome)) {
            return c;
        }
    }
    return null;
}
```

`equalsIgnoreCase` compara o conteúdo do texto, sem diferenciar maiúsculas de minúsculas — mais tolerante que exigir a digitação exata do nome salvo.

### 6 - Catálogo de produtos financeiros

```java
Map<Integer, Produto> catalogo = new HashMap<>();
catalogo.put(1, new Produto("CDB Flex", 100.0));
catalogo.put(2, new Produto("Tesouro BeeBank", 50.0));
```

### 7 - Saldo insuficiente

```java
public class SaldoInsuficienteException extends RuntimeException {
    public SaldoInsuficienteException(String mensagem) {
        super(mensagem);
    }
}
```

```java
public void sacar(double valor) {
    if (valor > saldo) {
        throw new SaldoInsuficienteException("Saldo insuficiente para sacar " + valor);
    }
    saldo -= valor;
}
```

### 8 - Formas de pagamento

```java
public interface Pagamento {
    void pagar(double valor);
}

public class PagamentoPix implements Pagamento {
    public void pagar(double valor) {
        System.out.println("Pagando " + valor + " via Pix");
    }
}

public class PagamentoCartao implements Pagamento {
    public void pagar(double valor) {
        System.out.println("Pagando " + valor + " via cartao");
    }
}
```

### 9 - Processando pagamentos, qualquer que seja o tipo

```java
List<Pagamento> pagamentos = List.of(new PagamentoPix(), new PagamentoCartao());
for (Pagamento p : pagamentos) {
    p.pagar(100.0);
}
```

O laço não sabe (nem precisa saber) se cada `Pagamento` é um Pix ou um cartão — polimorfismo em ação.

### 10 - Produtos premium

```java
List<Produto> premium = produtos.stream()
    .filter(p -> p.getPreco() > 100)
    .toList();
```

### 11 - Busca de cliente por CPF

```java
Optional<Cliente> buscarPorCpf(List<Cliente> clientes, String cpf) {
    for (Cliente c : clientes) {
        if (c.getCpf().equals(cpf)) {
            return Optional.of(c);
        }
    }
    return Optional.empty();
}
```

`Optional` deixa explícito, no tipo de retorno, que pode não existir cliente algum para aquele CPF — quem chama o método é forçado a lidar com essa possibilidade, em vez de arriscar um `NullPointerException` mais tarde.

### 12 - Classe imutável Cpf

```java
public final class Cpf {
    private final String valor;

    public Cpf(String valor) {
        if (valor == null || valor.isBlank()) {
            throw new IllegalArgumentException("CPF nao pode ser vazio");
        }
        this.valor = valor;
    }

    public String getValor() {
        return valor;
    }
}
```

Sem setter e com `final` no atributo: depois de criado, um `Cpf` nunca muda — qualquer "alteração" exigiria criar um `Cpf` novo.

### 13 - Simulação de senha

```java
static long fatorial(int n) {
    if (n <= 1) return 1;
    return n * fatorial(n - 1);
}
```

### 14 - Parcelas acumuladas

```java
static double somaParcelas(int n) {
    if (n == 0) return 0;
    return n + somaParcelas(n - 1);
}
```

### 15 - Testes manuais da Conta

```java
public class TesteConta {
    public static void main(String[] args) {
        Conta conta = new Conta();

        conta.depositar(100);
        System.out.println("Deposito valido - saldo: " + conta.getSaldo());

        conta.sacar(50);
        System.out.println("Saque valido - saldo: " + conta.getSaldo());

        try {
            conta.sacar(1000);
        } catch (IllegalStateException e) {
            System.out.println("Saque invalido capturado: " + e.getMessage());
        }

        try {
            conta.depositar(-10);
        } catch (IllegalArgumentException e) {
            System.out.println("Deposito invalido capturado: " + e.getMessage());
        }
    }
}
```

---

## Nível Difícil — Marketplace TechShop

### 1 - Laboratório de testes da TechShop

```java
public class Gadget {
    String nome;
    boolean disponivel = true;
}

public class Cliente {
    String nome;
}

public class Emprestimo {
    Gadget gadget;
    Cliente cliente;

    public void emprestar() {
        if (!gadget.disponivel) throw new IllegalStateException("Gadget indisponivel");
        gadget.disponivel = false;
    }

    public void devolver() {
        gadget.disponivel = true;
    }
}
```

### 2 - TechPay, a carteira digital da loja

```java
public class Cliente {
    String nome;
}

public abstract class Conta {
    protected double saldo;
    public abstract void sacar(double valor);
    public double getSaldo() { return saldo; }
}

public class ContaCorrente extends Conta {
    private double limite = 200;

    @Override
    public void sacar(double valor) {
        if (valor > saldo + limite) throw new IllegalStateException("Limite excedido");
        saldo -= valor;
    }
}

public class ContaPoupanca extends Conta {
    @Override
    public void sacar(double valor) {
        if (valor > saldo) throw new IllegalStateException("Saldo insuficiente");
        saldo -= valor;
    }
}
```

`ContaCorrente` permite saldo negativo até o limite; `ContaPoupanca` não permite de forma nenhuma — a mesma assinatura de método (`sacar`), comportamento diferente por subclasse.

### 3 - Estoque completo

```java
public class EstoqueService {
    private Map<Integer, Produto> produtos = new HashMap<>();
    private static final int ESTOQUE_MINIMO = 5;

    public void cadastrar(int codigo, Produto produto) {
        produtos.put(codigo, produto);
    }

    public void atualizarQuantidade(int codigo, int quantidade) {
        produtos.get(codigo).setQuantidade(quantidade);
    }

    public Produto buscar(int codigo) {
        return produtos.get(codigo);
    }

    public void remover(int codigo) {
        produtos.remove(codigo);
    }

    public List<Produto> abaixoDoMinimo() {
        return produtos.values().stream()
            .filter(p -> p.getQuantidade() < ESTOQUE_MINIMO)
            .toList();
    }
}
```

### 4 - Sistema de pedidos

```java
public class ItemPedido {
    Produto produto;
    int quantidade;

    public double getSubtotal() {
        return produto.getPreco() * quantidade;
    }
}

public class Pedido {
    Cliente cliente;
    List<ItemPedido> itens = new ArrayList<>();

    public double getTotal() {
        return itens.stream().mapToDouble(ItemPedido::getSubtotal).sum();
    }
}
```

### 5 - Formas de pagamento do checkout

```java
public interface EstrategiaPagamento {
    void pagar(double valor);
}

public class Pix implements EstrategiaPagamento {
    public void pagar(double valor) { System.out.println("Pix: " + valor); }
}

public class Cartao implements EstrategiaPagamento {
    public void pagar(double valor) { System.out.println("Cartao: " + valor); }
}

public class Boleto implements EstrategiaPagamento {
    public void pagar(double valor) { System.out.println("Boleto: " + valor); }
}
```

### 6 - Fábrica de formas de pagamento

```java
public class PagamentoFactory {
    public static EstrategiaPagamento criar(String tipo) {
        return switch (tipo) {
            case "PIX" -> new Pix();
            case "CARTAO" -> new Cartao();
            case "BOLETO" -> new Boleto();
            default -> throw new IllegalArgumentException("Tipo invalido: " + tipo);
        };
    }
}
```

### 7 - Fachada do checkout

```java
public class CheckoutFacade {
    public void finalizar(Pedido pedido, EstrategiaPagamento pagamento) {
        validarCarrinho(pedido);
        double total = pedido.getTotal();
        pagamento.pagar(total);
        baixarEstoque(pedido);
    }

    private void validarCarrinho(Pedido pedido) {
        if (pedido.itens.isEmpty()) throw new IllegalStateException("Carrinho vazio");
    }

    private void baixarEstoque(Pedido pedido) {
        for (ItemPedido item : pedido.itens) {
            item.produto.setQuantidade(item.produto.getQuantidade() - item.quantidade);
        }
    }
}
```

Quem chama `finalizar` não precisa saber que existem quatro passos internos — a Facade esconde essa orquestração atrás de um único método.

### 8 - Builder de Pedido

```java
public class Pedido {
    private Cliente cliente;
    private List<ItemPedido> itens = new ArrayList<>();

    public static class Builder {
        private Pedido pedido = new Pedido();

        public Builder cliente(Cliente cliente) {
            pedido.cliente = cliente;
            return this;
        }

        public Builder adicionarItem(ItemPedido item) {
            pedido.itens.add(item);
            return this;
        }

        public Pedido build() {
            return pedido;
        }
    }
}
```

```java
Pedido pedido = new Pedido.Builder()
    .cliente(cliente)
    .adicionarItem(item1)
    .adicionarItem(item2)
    .build();
```

### 9 - Identidade do cliente

```java
public class Cliente {
    private String cpf;

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (!(o instanceof Cliente cliente)) return false;
        return Objects.equals(cpf, cliente.cpf);
    }

    @Override
    public int hashCode() {
        return Objects.hash(cpf);
    }
}
```

### 10 - Portal de lojistas parceiros

```java
Map<String, Usuario> lojistas = new HashMap<>();
lojistas.put("loja_tech123", new Usuario("loja_tech123", "senhaSegura"));

Usuario lojista = lojistas.get("loja_tech123");
if (lojista != null && lojista.senha.equals(senhaDigitada)) {
    System.out.println("Acesso liberado");
}
```

### 11 - Relatório gerencial de produtos

```java
long total = produtos.size();
double mediaPreco = produtos.stream().mapToDouble(Produto::getPreco).average().orElse(0);
Produto maisCaro = produtos.stream().max(Comparator.comparingDouble(Produto::getPreco)).orElseThrow();
List<Produto> semEstoque = produtos.stream().filter(p -> p.getQuantidade() == 0).toList();

System.out.println("Total: " + total);
System.out.println("Media de preco: " + mediaPreco);
System.out.println("Mais caro: " + maisCaro.getNome());
System.out.println("Sem estoque: " + semEstoque.size());
```

### 12 - Equipe interna da TechShop

```java
public abstract class Funcionario {
    protected double salarioBase;
    public abstract double calcularBonus();
}

public class Atendente extends Funcionario {
    @Override
    public double calcularBonus() { return salarioBase * 0.05; }
}

public class Gerente extends Funcionario {
    @Override
    public double calcularBonus() { return salarioBase * 0.15; }
}

public class Entregador extends Funcionario {
    private int entregasNoMes;
    @Override
    public double calcularBonus() { return entregasNoMes * 2.0; }
}
```

### 13 - Árvore de categorias

```java
public class Categoria {
    String nome;
    List<Categoria> subcategorias = new ArrayList<>();

    public void listarTodas(int nivel) {
        System.out.println("  ".repeat(nivel) + nome);
        for (Categoria sub : subcategorias) {
            sub.listarTodas(nivel + 1);
        }
    }
}
```

Cada chamada de `listarTodas` imprime a própria categoria e delega às subcategorias — a recursão para naturalmente quando uma categoria não tem nenhuma subcategoria (o caso base implícito: lista vazia, o laço `for` simplesmente não executa).

### 14 - Carrinho de compras

```java
public class Carrinho {
    private Map<Produto, Integer> itens = new HashMap<>();

    public void adicionar(Produto produto, int quantidade) {
        itens.merge(produto, quantidade, Integer::sum);
    }

    public void remover(Produto produto) {
        itens.remove(produto);
    }

    public void alterarQuantidade(Produto produto, int novaQuantidade) {
        itens.put(produto, novaQuantidade);
    }

    public double calcularTotal() {
        return itens.entrySet().stream()
            .mapToDouble(e -> e.getKey().getPreco() * e.getValue())
            .sum();
    }

    public double aplicarDesconto(double percentual) {
        return calcularTotal() * (1 - percentual / 100);
    }
}
```

### 15 - Refatoração com SRP

```java
// Entidade: só dados
public class Produto {
    String nome;
    double preco;
    int quantidade;
}

// Serviço: regra de negócio
public class ProdutoService {
    private ProdutoRepository repository;

    public void cadastrar(Produto produto) {
        if (produto.preco < 0) throw new IllegalArgumentException("Preco invalido");
        repository.salvar(produto);
    }
}

// Repositório em memória: acesso a dados
public class ProdutoRepository {
    private Map<Integer, Produto> dados = new HashMap<>();
    private int proximoId = 1;

    public void salvar(Produto produto) {
        dados.put(proximoId++, produto);
    }

    public List<Produto> listarTodos() {
        return new ArrayList<>(dados.values());
    }
}

// Classe principal: só executa
public class Main {
    public static void main(String[] args) {
        ProdutoService service = new ProdutoService();
        service.cadastrar(new Produto());
    }
}
```

Cada classe agora muda por um único motivo: `Produto` muda se os dados do produto mudarem; `ProdutoService` muda se a regra de negócio mudar; `ProdutoRepository` muda se a forma de guardar dados mudar; `Main` muda só se o fluxo de execução mudar.
