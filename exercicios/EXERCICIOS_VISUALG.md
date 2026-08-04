# Folha de Exercícios - VisuAlg

> **Objetivo:** praticar lógica de programação em VisuAlg com exercícios progressivos. Não use IA para gerar o algoritmo final. Primeiro tente escrever os passos em português, depois transforme os passos em código.

## Como resolver cada exercício

Para cada atividade:

1. Leia o enunciado com calma.
2. Escreva os passos da solução em português.
3. Identifique as variáveis necessárias.
4. Crie o algoritmo no VisuAlg.
5. Execute e teste com mais de um valor.
6. Comente o que o algoritmo faz.
7. Salve o arquivo `.alg` no repositório.

---

# Nível Fácil

## Exercício 1 - Olá Mundo

Crie um algoritmo que imprima na tela a mensagem:

```text
Ola, mundo!
```

O objetivo é garantir que o estudante consegue criar, executar e visualizar a saída de um algoritmo no VisuAlg.

## Exercício 2 - Apresentação pessoal

Crie um algoritmo que leia o nome de uma pessoa e mostre uma mensagem de saudação personalizada.

Exemplo de saída:

```text
Ola, Ana!
```

## Exercício 3 - Verificação de maioridade

Crie um algoritmo que leia a idade de uma pessoa e informe se ela é maior ou menor de idade.

Regra:

- idade maior ou igual a 18: maior de idade;
- idade menor que 18: menor de idade.

## Exercício 4 - Calculadora simples

Crie um algoritmo que leia dois números e mostre:

- soma;
- subtração;
- multiplicação;
- divisão.

## Exercício 5 - Média de notas

Crie um algoritmo que leia duas notas, calcule a média e mostre o resultado.

A média deve ser calculada somando as duas notas e dividindo por 2.

## Exercício 6 - Desconto de produto

Crie um algoritmo que leia o preço de um produto e calcule o valor final com 10% de desconto.

Mostre:

- preço original;
- valor do desconto;
- preço final.

## Exercício 7 - Tabuada

Crie um algoritmo que leia um número inteiro e mostre sua tabuada de 1 até 10.

## Exercício 8 - Maior de três números

Crie um algoritmo que leia três números e mostre qual deles é o maior.

## Exercício 9 - Par ou ímpar

Crie um algoritmo que leia um número inteiro e informe se ele é par ou ímpar.

## Exercício 10 - Conversão de moeda

Crie um algoritmo que leia um valor em reais e converta para dólares usando uma cotação fixa definida no próprio algoritmo.

Exemplo:

```text
cotacao <- 5.00
```

## Exercício 11 - Aumento salarial

Crie um algoritmo que leia o salário de uma pessoa e calcule um novo salário com aumento de 8%.

## Exercício 12 - Área de um retângulo

Crie um algoritmo que leia a base e a altura de um retângulo e calcule sua área.

Fórmula:

```text
area = base * altura
```

## Exercício 13 - Login simples

Crie um algoritmo que leia usuário e senha.

O acesso deve ser liberado apenas se:

- usuário for `admin`;
- senha for `1234`.

Caso contrário, mostre uma mensagem de acesso negado.

## Exercício 14 - Contagem até N

Crie um algoritmo que leia um número inteiro positivo e conte de 1 até esse número usando estrutura de repetição.

## Exercício 15 - Lista de nomes

Crie um algoritmo que leia cinco nomes usando vetor e depois mostre todos os nomes cadastrados.

---

# Nível Médio

## Exercício 1 - Caixa eletrônico simples

Crie um algoritmo que simule um saque bancário.

Regras:

- o saldo inicial deve ser definido no algoritmo;
- o usuário informa o valor do saque;
- saque menor ou igual a zero deve ser recusado;
- saque maior que o saldo deve ser recusado;
- saque válido deve atualizar o saldo.

## Exercício 2 - Média da turma

Crie um algoritmo que leia a quantidade de alunos de uma turma, leia a nota de cada aluno e calcule a média geral da turma.

## Exercício 3 - Menu interativo

Crie um algoritmo com menu contendo as opções:

1. Cadastrar;
2. Listar;
3. Sair.

O menu deve continuar aparecendo até o usuário escolher sair.

## Exercício 4 - Estatísticas de vetor

Crie um algoritmo que leia dez números em um vetor e mostre:

- soma;
- média;
- maior número;
- menor número.

## Exercício 5 - Produtos e preços

Crie um algoritmo que leia cinco produtos e seus respectivos preços usando vetores.

Ao final, mostre:

- produto mais caro;
- produto mais barato.

## Exercício 6 - Cálculo de frete

Crie um algoritmo que calcule o frete de uma compra.

Regras:

- se o valor da compra for maior ou igual a 300, o frete será grátis;
- caso contrário, o frete será a distância multiplicada por 1,50.

## Exercício 7 - Leitura até zero

Crie um algoritmo que leia números até o usuário digitar 0.

Ao final, mostre:

- quantidade de números digitados, sem contar o zero;
- soma dos números digitados.

## Exercício 8 - Jogo de adivinhação

Crie um algoritmo com um número secreto fixo.

O usuário deve tentar adivinhar o número até acertar.

## Exercício 9 - Matriz 2x3

Crie um algoritmo que leia valores para uma matriz 2x3 e depois mostre todos os valores cadastrados.

## Exercício 10 - Função dobro

Crie uma função que receba um número inteiro e retorne o dobro desse número.

Depois, use a função dentro do algoritmo principal.

## Exercício 11 - Função desconto

Crie uma função que receba valor e percentual de desconto, calcule e retorne o valor do desconto.

## Exercício 12 - Procedimento de cabeçalho

Crie um procedimento que mostre um cabeçalho do sistema.

Use esse procedimento em pelo menos três momentos diferentes do algoritmo.

## Exercício 13 - Senha com tentativas limitadas

Crie um algoritmo que peça uma senha ao usuário.

Regras:

- a senha correta é `1234`;
- o usuário tem no máximo três tentativas;
- se acertar, mostre acesso liberado;
- se errar três vezes, mostre acesso bloqueado.

## Exercício 14 - Notas acima da média

Crie um algoritmo que leia um vetor de notas, calcule a média e mostre quantas notas ficaram acima da média.

## Exercício 15 - Cadastro de produtos com vetores

Crie um algoritmo que cadastre produtos usando vetores paralelos para:

- nome;
- preço;
- quantidade.

Ao final, liste todos os produtos cadastrados.

---

# Nível Difícil

## Exercício 1 - Controle de estoque

Crie um mini-sistema de controle de estoque com menu.

Funcionalidades:

- cadastrar produto;
- listar produtos;
- atualizar quantidade;
- sair.

Use vetores para armazenar os dados.

## Exercício 2 - Sistema de biblioteca

Crie um sistema de biblioteca usando vetores para armazenar:

- título do livro;
- autor;
- disponibilidade.

Funcionalidades:

- listar livros;
- emprestar livro;
- devolver livro.

## Exercício 3 - Sistema de notas

Crie um algoritmo que cadastre alunos e três notas por aluno.

Ao final, mostre:

- média de cada aluno;
- situação aprovado ou reprovado;
- média geral da turma.

## Exercício 4 - Caixa eletrônico completo

Crie um caixa eletrônico com menu:

1. consultar saldo;
2. depositar;
3. sacar;
4. sair.

Valide valores inválidos em todas as operações.

## Exercício 5 - Reserva de cinema

Crie um algoritmo que represente assentos de cinema usando matriz.

Funcionalidades:

- listar assentos;
- reservar assento;
- impedir reserva duplicada.

## Exercício 6 - Fatorial recursivo

Crie uma função recursiva para calcular o fatorial de um número.

O algoritmo principal deve ler o número e mostrar o resultado.

## Exercício 7 - Soma recursiva

Crie uma função recursiva que calcule a soma de 1 até N.

Exemplo:

```text
N = 5
resultado = 1 + 2 + 3 + 4 + 5
```

## Exercício 8 - Ordenação simples

Crie um algoritmo que leia dez números em um vetor e ordene os valores em ordem crescente.

Use lógica simples de comparação e troca.

## Exercício 9 - Login com múltiplos usuários

Crie um sistema de login com três usuários e três senhas usando vetores.

O usuário deve ter no máximo três tentativas.

## Exercício 10 - Carrinho de compras

Crie um simulador de carrinho de compras.

Funcionalidades:

- cadastrar produtos;
- adicionar produto ao carrinho;
- calcular total da compra.

## Exercício 11 - Folha de pagamento

Crie um algoritmo que calcule a folha de pagamento de vários funcionários.

Para cada funcionário, leia:

- nome;
- salário base;
- bônus;
- desconto.

Mostre o salário líquido.

## Exercício 12 - Sistema de votação

Crie um sistema de votação com três candidatos.

Funcionalidades:

- registrar votos;
- encerrar votação;
- mostrar total de votos por candidato;
- mostrar vencedor.

## Exercício 13 - Diagonal principal

Crie um algoritmo que leia uma matriz 3x3 e calcule a soma da diagonal principal.

## Exercício 14 - Contador de vogais

Crie um algoritmo que leia uma palavra e conte quantas vogais ela possui.

Se sua versão do VisuAlg tiver limitação com manipulação de caracteres, descreva a lógica esperada em comentários.

## Exercício 15 - Mini-sistema de pedidos

Crie um mini-sistema de pedidos que leia:

- cliente;
- produto;
- quantidade;
- preço unitário.

O sistema deve calcular subtotal, aplicar desconto quando necessário e exibir um relatório final.
