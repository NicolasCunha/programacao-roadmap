# Folha de Exercícios - Java

> **Objetivo:** praticar sintaxe, orientação a objetos e estruturas comuns em Java. Não use IA para gerar a solução final. Escreva manualmente para memorizar os padrões da linguagem.

## Como resolver cada exercício

Para cada atividade:

1. Leia o enunciado.
2. Defina quais classes, métodos e variáveis serão necessários.
3. Escreva uma primeira solução simples.
4. Execute e teste.
5. Refatore nomes e responsabilidades.
6. Salve no GitHub.

---

# Nível Fácil

## Exercício 1 - Olá Mundo

Crie uma classe Java chamada `OlaMundo` que imprima a mensagem `Ola, mundo!`.

## Exercício 2 - Cadastro simples

Crie um programa que leia nome e idade usando `Scanner` e mostre os dados na tela.

## Exercício 3 - Calculadora simples

Crie um programa que leia dois números e mostre soma, subtração, multiplicação e divisão.

## Exercício 4 - Maioridade

Crie um programa que leia a idade de uma pessoa e informe se ela é maior ou menor de idade.

## Exercício 5 - Média de notas

Crie um programa que leia duas notas, calcule a média e mostre o resultado.

## Exercício 6 - Método somar

Crie um método chamado `somar` que receba dois inteiros e retorne a soma.

## Exercício 7 - Método par ou ímpar

Crie um método chamado `ehPar` que receba um número inteiro e retorne `true` se ele for par e `false` caso contrário.

## Exercício 8 - Tabuada

Crie um programa que leia um número e mostre sua tabuada de 1 até 10 usando `for`.

## Exercício 9 - Contagem regressiva

Crie um programa que conte de 10 até 1 usando `while`.

## Exercício 10 - Classe Cliente

Crie uma classe `Cliente` com os atributos `nome` e `email`.

## Exercício 11 - Objeto Cliente

Crie um objeto da classe `Cliente`, atribua valores aos atributos e mostre os dados na tela.

## Exercício 12 - Classe Produto

Crie uma classe `Produto` com os atributos:

- nome;
- preço;
- quantidade.

## Exercício 13 - Lista de nomes

Crie uma lista de nomes usando `ArrayList` e imprima todos os nomes cadastrados.

## Exercício 14 - Mapa de produtos

Crie um `Map<Integer, String>` para guardar código e nome de produtos.

Depois, busque um produto pelo código.

## Exercício 15 - Tratamento de divisão por zero

Crie um programa que tente dividir dois números inteiros e trate o erro de divisão por zero usando `try/catch`.

---

# Nível Médio

## Exercício 1 - Classe Conta

Crie uma classe `Conta` com:

- atributo privado `saldo`;
- método `depositar`;
- método `sacar`;
- método `getSaldo`.

Valide valores inválidos.

## Exercício 2 - Sistema bancário com menu

Crie um sistema de console com menu para:

1. depositar;
2. sacar;
3. consultar saldo;
4. sair.

Use a classe `Conta`.

## Exercício 3 - Produto com validação

Crie uma classe `Produto` que não permita preço negativo.

A validação pode estar no construtor ou em um método de alteração de preço.

## Exercício 4 - Agenda de contatos

Crie uma agenda usando `List<Contato>`.

Cada contato deve ter nome, telefone e email.

## Exercício 5 - Busca de contato

Adicione na agenda uma funcionalidade para buscar contato pelo nome.

## Exercício 6 - Controle de estoque com Map

Crie um controle de estoque usando `Map<Integer, Produto>`, onde a chave é o código do produto.

## Exercício 7 - Exceção personalizada

Crie uma exceção personalizada chamada `SaldoInsuficienteException` e use-a na classe `Conta`.

## Exercício 8 - Interface de pagamento

Crie uma interface `Pagamento` com o método `pagar`.

Depois, crie duas implementações:

- `PagamentoPix`;
- `PagamentoCartao`.

## Exercício 9 - Polimorfismo com pagamentos

Crie uma lista de pagamentos e processe todos usando o mesmo método da interface `Pagamento`.

## Exercício 10 - Filtro com Stream

Crie uma lista de produtos e use `stream` para filtrar produtos com preço maior que 100.

## Exercício 11 - Busca com Optional

Crie um método que busque cliente por CPF e retorne `Optional<Cliente>`.

## Exercício 12 - Classe imutável CPF

Crie uma classe imutável chamada `Cpf`.

Regras:

- atributo privado e final;
- sem setter;
- validação para não aceitar texto vazio.

## Exercício 13 - Fatorial recursivo

Crie um método recursivo para calcular o fatorial de um número.

## Exercício 14 - Soma recursiva

Crie um método recursivo que calcule a soma de 1 até N.

## Exercício 15 - Testes manuais da Conta

Crie uma classe com método `main` para testar manualmente:

- depósito válido;
- saque válido;
- saque com saldo insuficiente;
- depósito inválido.

---

# Nível Difícil

## Exercício 1 - Sistema de biblioteca

Crie um sistema de biblioteca com as classes:

- `Livro`;
- `Usuario`;
- `Emprestimo`.

O sistema deve permitir emprestar e devolver livros.

## Exercício 2 - Sistema bancário com herança

Crie um sistema bancário com:

- `Cliente`;
- `Conta`;
- `ContaCorrente`;
- `ContaPoupanca`.

As contas devem ter regras diferentes de saque.

## Exercício 3 - Controle de estoque completo

Crie um sistema de estoque com:

- cadastro;
- atualização;
- busca;
- remoção;
- relatório de produtos abaixo do mínimo.

## Exercício 4 - Sistema de pedidos

Crie um sistema de pedidos com:

- `Cliente`;
- `Produto`;
- `ItemPedido`;
- `Pedido`.

O pedido deve calcular o total com base nos itens.

## Exercício 5 - Strategy para pagamento

Aplique o padrão Strategy para permitir diferentes formas de pagamento.

Exemplos:

- Pix;
- cartão;
- boleto.

## Exercício 6 - Factory de pagamento

Crie uma Factory que receba uma opção e retorne a estratégia de pagamento correta.

## Exercício 7 - Facade de checkout

Crie uma Facade para finalizar uma compra.

Ela deve coordenar:

- validação do carrinho;
- cálculo do total;
- pagamento;
- baixa no estoque.

## Exercício 8 - Builder de Pedido

Crie um `Builder` para construir objetos `Pedido` com mais clareza.

## Exercício 9 - equals e hashCode

Implemente `equals` e `hashCode` na classe `Cliente`, considerando o CPF como identificador.

## Exercício 10 - Login com Map

Crie um sistema de login usando `Map<String, Usuario>`, onde a chave é o nome de usuário.

## Exercício 11 - Relatório com Streams

Crie um relatório de produtos usando streams.

Mostre:

- total de produtos;
- média de preço;
- produto mais caro;
- produtos sem estoque.

## Exercício 12 - Funcionários com polimorfismo

Crie uma hierarquia de funcionários com cálculo de bônus usando polimorfismo.

## Exercício 13 - Categorias recursivas

Crie uma estrutura de categorias em que uma categoria pode ter subcategorias.

Implemente uma forma recursiva de listar todas as categorias.

## Exercício 14 - Carrinho de compras

Crie um carrinho de compras com:

- adicionar item;
- remover item;
- alterar quantidade;
- calcular total;
- aplicar desconto.

## Exercício 15 - Refatoração com SRP

Escolha um exercício médio e refatore aplicando SRP.

Separe:

- entidade;
- serviço;
- repositório em memória;
- classe principal de execução.
