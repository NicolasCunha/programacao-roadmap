# Folha de Exercícios - Java

> **Objetivo:** praticar sintaxe, orientação a objetos e estruturas comuns em Java. Não use IA para gerar a solução final. Escreva manualmente para memorizar os padrões da linguagem. Fique à vontade para fazer pesquisas na internet, mas tente não encontrar respostas prontas.

## Como resolver cada exercício

Para cada atividade:

1. Leia o enunciado, incluindo o contexto — ele geralmente indica que classes fazem sentido no domínio.
2. Defina quais classes, métodos e variáveis serão necessários.
3. Escreva uma primeira solução simples.
4. Execute e teste.
5. Refatore nomes e responsabilidades.
6. Salve no GitHub.

Exercícios marcados com 🎁 têm um desafio bônus opcional.

---

# Nível Fácil — 🚀 Estação Espacial

Você é o programador de bordo da estação espacial *Aurora*. Os sistemas de navegação são antigos e simples, mas cada um deles precisa funcionar perfeitamente — a tripulação depende disso a 400km de altitude.

## Exercício 1 - Primeira transmissão

Antes de qualquer sistema entrar em operação, o controle da missão em terra pede uma transmissão de teste.

Crie uma classe Java chamada `Aurora` que imprima a mensagem `Ola, mundo!`.

## Exercício 2 - Cadastro de tripulante

Um novo astronauta acabou de embarcar e precisa ser registrado no sistema da estação.

Crie um programa que leia nome e idade usando `Scanner` e mostre os dados na tela.

## Exercício 3 - Consumo de combustível

O painel de controle precisa mostrar rapidamente quatro operações básicas sobre dois valores de combustível (em litros), usados o tempo todo pelos cálculos de manobra.

Crie um programa que leia dois números e mostre soma, subtração, multiplicação e divisão.

## Exercício 4 - Cadete apto para EVA

Uma caminhada espacial (atividade extraveicular, EVA) só pode ser feita por astronautas já maiores de idade — regra da agência espacial.

Crie um programa que leia a idade de uma pessoa e informe se ela é maior ou menor de idade.

## Exercício 5 - Leituras de oxigênio

O sensor de oxigênio da cabine registrou duas leituras diferentes em sequência, e o sistema precisa calcular a média entre elas para decidir se está tudo normal.

Crie um programa que leia duas leituras, calcule a média e mostre o resultado.

## Exercício 6 - Módulo de acoplamento

Dois módulos vão se acoplar à estação, e o sistema de bordo precisa somar a capacidade de cada um para saber o total resultante.

Crie um método chamado `somar` que receba dois inteiros e retorne a soma.

## Exercício 7 - Ala Leste ou Oeste

Cada compartimento da estação tem um número: os pares ficam na Ala Leste, os ímpares na Ala Oeste.

Crie um método chamado `ehPar` que receba um número inteiro e retorne `true` se ele for par e `false` caso contrário.

## Exercício 8 - Órbitas por dia

O sistema de navegação quer mostrar quantas órbitas completas a estação dá em cada um dos primeiros 10 dias, para um número de órbitas por dia informado.

Crie um programa que leia um número e mostre sua tabuada de 1 até 10 usando `for`.

## Exercício 9 - Contagem regressiva de lançamento

Nenhuma missão espacial começa sem a clássica contagem regressiva.

Crie um programa que conte de 10 até 1 usando `while`.

## Exercício 10 - Classe Tripulante

O sistema precisa de uma estrutura para representar cada pessoa a bordo.

Crie uma classe `Tripulante` com os atributos `nome` e `email`.

## Exercício 11 - Objeto Tripulante

Depois de definir a estrutura, é hora de registrar a primeira tripulante de verdade.

Crie um objeto da classe `Tripulante`, atribua valores aos atributos e mostre os dados na tela.

## Exercício 12 - Classe Equipamento

Todo equipamento a bordo da estação precisa ser inventariado.

Crie uma classe `Equipamento` com os atributos:

- nome;
- preço (custo de reposição);
- quantidade.

## Exercício 13 - Tripulação na ponte de comando

A ponte de comando quer uma lista simples com o nome de todos os tripulantes de plantão no momento.

Crie uma lista de nomes usando `ArrayList` e imprima todos os nomes cadastrados.

## Exercício 14 - Mapa de equipamentos por compartimento

Cada equipamento tem um código de compartimento onde está guardado, e o sistema precisa localizar rapidamente qual equipamento está em qual código.

Crie um `Map<Integer, String>` para guardar código e nome de equipamentos.

Depois, busque um equipamento pelo código.

## Exercício 15 - Distribuição de oxigênio 🎁

O sistema de suporte à vida divide o oxigênio disponível igualmente entre todos os tripulantes — mas se, por algum erro de sensor, o número de tripulantes registrados for zero, isso não pode travar o sistema.

Crie um programa que tente dividir dois números inteiros e trate o erro de divisão por zero usando `try/catch`.

**Desafio bônus:** faça o programa mostrar uma mensagem de alerta específica ("Sensor de tripulação com falha") em vez de só capturar o erro genericamente.

---

# Nível Médio — 💳 Fintech BeeBank

Você entrou como desenvolvedor na BeeBank, uma fintech que está construindo um banco digital do zero. O time é pequeno, então cada funcionalidade nova passa pelas suas mãos antes de ir para o time de qualidade.

## Exercício 1 - Classe Conta

A funcionalidade mais básica de qualquer banco: uma conta que guarda saldo.

Crie uma classe `Conta` com:

- atributo privado `saldo`;
- método `depositar`;
- método `sacar`;
- método `getSaldo`.

Valide valores inválidos.

## Exercício 2 - App da BeeBank no terminal

Antes do app mobile ficar pronto, o time de produto quer testar o fluxo em um protótipo de console.

Crie um sistema de console com menu para:

1. depositar;
2. sacar;
3. consultar saldo;
4. sair.

Use a classe `Conta`.

## Exercício 3 - Produto de investimento

A BeeBank está lançando produtos de investimento, cada um com um valor mínimo de aplicação — e esse valor nunca pode ser negativo.

Crie uma classe `Produto` que não permita preço (valor mínimo de aplicação) negativo.

A validação pode estar no construtor ou em um método de alteração de preço.

## Exercício 4 - Beneficiários cadastrados

Para facilitar transferências, o app permite salvar pessoas como beneficiários favoritos.

Crie uma agenda usando `List<Contato>`.

Cada contato deve ter nome, telefone e email.

## Exercício 5 - Busca de beneficiário

O time de UX pediu uma forma rápida de encontrar um beneficiário já salvo, digitando parte do nome.

Adicione na agenda uma funcionalidade para buscar contato pelo nome.

## Exercício 6 - Catálogo de produtos financeiros

A BeeBank já tem vários produtos financeiros no catálogo, cada um identificado por um código.

Crie um controle de produtos usando `Map<Integer, Produto>`, onde a chave é o código do produto.

## Exercício 7 - Saldo insuficiente

Quando um saque é maior que o saldo disponível, o time de risco quer um erro específico, não um erro genérico do Java.

Crie uma exceção personalizada chamada `SaldoInsuficienteException` e use-a na classe `Conta`.

## Exercício 8 - Formas de pagamento

A BeeBank quer aceitar mais de uma forma de pagamento no checkout de produtos financeiros.

Crie uma interface `Pagamento` com o método `pagar`.

Depois, crie duas implementações:

- `PagamentoPix`;
- `PagamentoCartao`.

## Exercício 9 - Processando pagamentos, qualquer que seja o tipo

O time de checkout não quer saber qual forma de pagamento foi escolhida — só quer processar todas do mesmo jeito.

Crie uma lista de pagamentos e processe todos usando o mesmo método da interface `Pagamento`.

## Exercício 10 - Produtos premium

O time de marketing quer destacar, na home do app, só os produtos de investimento com valor mínimo acima de R$100.

Crie uma lista de produtos e use `stream` para filtrar produtos com preço maior que 100.

## Exercício 11 - Busca de cliente por CPF

O time de atendimento precisa localizar um cliente pelo CPF, mas nem todo CPF digitado corresponde a um cliente real cadastrado.

Crie um método que busque cliente por CPF e retorne `Optional<Cliente>`.

## Exercício 12 - Classe imutável Cpf

Um CPF, depois de cadastrado, nunca deveria poder ser alterado silenciosamente em memória — é um dado sensível demais para isso.

Crie uma classe imutável chamada `Cpf`.

Regras:

- atributo privado e final;
- sem setter;
- validação para não aceitar texto vazio.

## Exercício 13 - Simulação de senha

O time de segurança pediu para simular, de forma recursiva, quantas combinações possíveis existem para um cofre de N dígitos (o clássico cálculo de fatorial).

Crie um método recursivo para calcular o fatorial de um número.

## Exercício 14 - Parcelas acumuladas

Um cliente quer simular quanto vai pagar, no total, se somar suas parcelas de 1 até N.

Crie um método recursivo que calcule a soma de 1 até N.

## Exercício 15 - Testes manuais da Conta

Antes de escrever testes automatizados de verdade (isso vem no Nível 5), o time quer garantir manualmente que os cenários principais da `Conta` funcionam.

Crie uma classe com método `main` para testar manualmente:

- depósito válido;
- saque válido;
- saque com saldo insuficiente;
- depósito inválido.

---

# Nível Difícil — 🛒 Marketplace TechShop

A TechShop é um marketplace de eletrônicos que está crescendo rápido, e o time de engenharia está reescrevendo o sistema do zero, peça por peça. Você foi alocado no time principal. Estes exercícios constroem, em Java puro, a mesma base do sistema que vai reaparecer — agora com banco de dados, API e tudo mais — no [projeto final do Nível 9](../NIVEL_9_PROJETO_FINAL.md).

## Exercício 1 - Laboratório de testes da TechShop

A TechShop lançou um programa onde clientes podem pedir emprestado um gadget por alguns dias antes de decidir comprar.

Crie um sistema com as classes:

- `Gadget`;
- `Cliente`;
- `Emprestimo`.

O sistema deve permitir emprestar e devolver gadgets.

## Exercício 2 - TechPay, a carteira digital da loja

A TechShop lançou sua própria carteira digital, a TechPay, para os clientes acumularem crédito e cashback.

Crie um sistema com:

- `Cliente`;
- `Conta`;
- `ContaCorrente`;
- `ContaPoupanca`.

As contas devem ter regras diferentes de saque.

## Exercício 3 - Estoque completo

O estoque da TechShop cresceu demais para o controle simples de antes — agora precisa de operações completas e de um alerta de reposição.

Crie um sistema de estoque com:

- cadastro;
- atualização;
- busca;
- remoção;
- relatório de produtos abaixo do mínimo.

## Exercício 4 - Sistema de pedidos

O carrinho de compras da TechShop precisa virar um pedido de verdade, com itens e total calculado.

Crie um sistema de pedidos com:

- `Cliente`;
- `Produto`;
- `ItemPedido`;
- `Pedido`.

O pedido deve calcular o total com base nos itens.

## Exercício 5 - Formas de pagamento do checkout

O checkout da TechShop precisa aceitar múltiplas formas de pagamento sem um `if/else` gigante controlando qual usar.

Aplique o padrão Strategy para permitir diferentes formas de pagamento.

Exemplos:

- Pix;
- cartão;
- boleto.

## Exercício 6 - Fábrica de formas de pagamento

Escolher qual estratégia de pagamento instanciar, espalhado pelo código, está gerando bugs — hora de centralizar essa decisão.

Crie uma Factory que receba uma opção e retorne a estratégia de pagamento correta.

## Exercício 7 - Fachada do checkout

Finalizar uma compra na TechShop envolve vários passos internos, e cada tela que finaliza um pedido está reimplementando essa sequência manualmente.

Crie uma Facade para finalizar uma compra.

Ela deve coordenar:

- validação do carrinho;
- cálculo do total;
- pagamento;
- baixa no estoque.

## Exercício 8 - Builder de Pedido

Montar um `Pedido` direto pelo construtor está exigindo passar parâmetros demais, muitos deles opcionais.

Crie um `Builder` para construir objetos `Pedido` com mais clareza.

## Exercício 9 - Identidade do cliente

Dois objetos `Cliente`, com o mesmo CPF, deveriam ser tratados como o mesmo cliente pelo sistema — hoje isso não acontece.

Implemente `equals` e `hashCode` na classe `Cliente`, considerando o CPF como identificador.

## Exercício 10 - Portal de lojistas parceiros

A TechShop é um marketplace: além de clientes, existem lojistas parceiros que vendem seus próprios produtos e precisam de login separado.

Crie um sistema de login usando `Map<String, Usuario>`, onde a chave é o nome de usuário.

## Exercício 11 - Relatório gerencial de produtos

A diretoria pediu um relatório rápido do catálogo inteiro da TechShop para a reunião de segunda-feira.

Crie um relatório de produtos usando streams.

Mostre:

- total de produtos;
- média de preço;
- produto mais caro;
- produtos sem estoque.

## Exercício 12 - Equipe interna da TechShop

Além do marketplace em si, a TechShop tem uma equipe interna — atendentes, gerentes, entregadores — cada um com uma regra diferente de cálculo de bônus.

Crie uma hierarquia de funcionários com cálculo de bônus usando polimorfismo.

## Exercício 13 - Árvore de categorias

O catálogo da TechShop organiza produtos em categorias que podem ter subcategorias (Eletrônicos > Áudio > Fones de ouvido, por exemplo).

Crie uma estrutura de categorias em que uma categoria pode ter subcategorias.

Implemente uma forma recursiva de listar todas as categorias.

## Exercício 14 - Carrinho de compras

O carrinho de compras do site principal da TechShop precisa de todas as operações que um carrinho de e-commerce de verdade tem.

Crie um carrinho de compras com:

- adicionar item;
- remover item;
- alterar quantidade;
- calcular total;
- aplicar desconto.

## Exercício 15 - Refatoração com SRP 🎁

O tech lead revisou um dos sistemas que você construiu neste bloco e apontou que uma das classes está fazendo coisa demais.

Escolha um exercício deste nível médio ou difícil e refatore aplicando SRP.

Separe:

- entidade;
- serviço;
- repositório em memória;
- classe principal de execução.

**Desafio bônus:** depois de separar em camadas, compare o resultado com a [arquitetura em camadas do Nível 7](../NIVEL_7_SPRING_BOOT.md#arquitetura-em-camadas) — você acabou de construir, à mão, a mesma estrutura que o Spring Boot vai automatizar mais adiante.
