# Folha de Exercícios - Java

Esta folha deve ser resolvida manualmente. Evite pedir para IA gerar classes completas. O foco é memorizar estruturas, praticar sintaxe Java e reforçar padrões básicos.

---

## Fácil

1. Crie um programa `OlaMundo` que imprima `Ola, mundo!`.
2. Leia nome e idade com `Scanner` e mostre os dados.
3. Leia dois números e mostre soma, subtração, multiplicação e divisão.
4. Leia idade e diga se é maior de idade.
5. Leia duas notas e calcule a média.
6. Crie um método `somar(int a, int b)`.
7. Crie um método `ehPar(int numero)` que retorna `boolean`.
8. Faça uma tabuada usando `for`.
9. Conte de 10 até 1 usando `while`.
10. Crie uma classe `Cliente` com `nome` e `email`.
11. Crie um objeto `Cliente` e imprima seus dados.
12. Crie uma classe `Produto` com `nome`, `preco` e `quantidade`.
13. Crie uma lista de nomes usando `ArrayList` e imprima todos.
14. Crie um `Map<Integer, String>` para guardar códigos e nomes de produtos.
15. Crie um programa que trate divisão por zero com `try/catch`.

---

## Médio

1. Crie uma classe `Conta` com saldo privado, métodos `depositar`, `sacar` e `getSaldo`.
2. Crie um sistema bancário simples com menu para depositar, sacar, consultar saldo e sair.
3. Crie uma classe `Produto` com validação para impedir preço negativo.
4. Crie uma agenda de contatos usando `List<Contato>`.
5. Crie busca de contato por nome usando laço.
6. Crie um controle de estoque com produtos em `Map<Integer, Produto>`.
7. Crie uma exceção personalizada `SaldoInsuficienteException`.
8. Crie uma interface `Pagamento` com implementações `PagamentoPix` e `PagamentoCartao`.
9. Use polimorfismo para processar uma lista de pagamentos.
10. Use `stream` para filtrar produtos com preço maior que 100.
11. Use `Optional` para representar uma busca de cliente por CPF.
12. Crie uma classe imutável `Cpf` com validação simples de texto vazio.
13. Crie método recursivo para calcular fatorial.
14. Crie método recursivo para somar números de 1 até N.
15. Crie testes manuais no `main` para validar comportamentos da classe `Conta`.

---

## Difícil

1. Crie um sistema de biblioteca com classes `Livro`, `Usuario`, `Emprestimo` e menu de operações.
2. Crie um sistema bancário com `Cliente`, `Conta`, `ContaCorrente`, `ContaPoupanca` e regras diferentes de saque.
3. Crie um controle de estoque com cadastro, atualização, busca, remoção e relatório de produtos abaixo do mínimo.
4. Crie um sistema de pedidos com `Cliente`, `Produto`, `ItemPedido` e `Pedido`, calculando total.
5. Aplique Strategy para formas de pagamento em um checkout.
6. Aplique Factory para criar a estratégia de pagamento a partir de uma opção digitada.
7. Crie uma Facade para finalizar pedido, validando estoque, calculando total e processando pagamento.
8. Crie uma classe `Pedido` usando Builder.
9. Implemente `equals` e `hashCode` em `Cliente` considerando CPF.
10. Crie um sistema de login com usuários em `Map<String, Usuario>` e valide senha.
11. Crie relatório usando streams: total de produtos, média de preço, produto mais caro e produtos sem estoque.
12. Crie uma hierarquia de funcionários com cálculo de bônus usando polimorfismo.
13. Crie um algoritmo recursivo para percorrer uma estrutura simples de categorias, onde uma categoria pode ter subcategorias.
14. Simule um carrinho de compras com adição, remoção, alteração de quantidade e cálculo de descontos.
15. Refatore um exercício médio aplicando SRP, separando entidades, serviços e repositórios em memória.
