# Folha de Exercícios - SQL

> **Objetivo:** praticar modelagem relacional e SQL usando MySQL. O foco é criação de tabelas, chaves, relações e consultas. Não é necessário usar PL/SQL. Fique à vontade para fazer pesquisas na internet, mas tente não encontrar respostas prontas.

## Como resolver cada exercício

Para cada atividade:

1. Leia o problema de dados, incluindo o contexto.
2. Identifique entidades e atributos.
3. Defina chaves primárias.
4. Defina chaves estrangeiras quando houver relacionamento.
5. Escreva o SQL.
6. Execute no MySQL.
7. Teste com dados inseridos manualmente.

Exercícios marcados com 🎁 têm um desafio bônus opcional.

---

# Nível Fácil — 🎵 Vibe Store

A **Vibe Store** é a lojinha oficial de um festival de música que acontece uma vez por ano. Camisetas, bonés, ingressos VIP — tudo é vendido tanto no site quanto numa barraca física durante o evento. Você foi chamado para tirar o controle de vendas do papel e colocar em um banco de dados de verdade. Os exercícios daqui até o nível difícil constroem, tabela por tabela, o banco completo da loja.

## Exercício 1 - Tabela de clientes

Crie uma tabela chamada `clientes` com os campos:

- id;
- nome;
- email;
- data de nascimento.

## Exercício 2 - Tabela de produtos

Crie uma tabela chamada `produtos` com os campos:

- id;
- nome;
- preço;
- quantidade em estoque.

## Exercício 3 - Primeiros clientes cadastrados

A loja abriu as vendas antecipadas, e os três primeiros compradores já podem ser cadastrados.

Insira três clientes na tabela `clientes`.

## Exercício 4 - Catálogo inicial

Insira cinco produtos na tabela `produtos` (camisetas, bonés, ingressos, o que fizer sentido para um festival de música).

## Exercício 5 - Lista completa de clientes

O time de marketing quer disparar um email promocional para todo mundo já cadastrado.

Crie uma consulta que liste todos os clientes cadastrados.

## Exercício 6 - Lista de contatos

Para o disparo de email, na verdade só interessam nome e email — o resto é ruído.

Crie uma consulta que mostre apenas nome e email dos clientes.

## Exercício 7 - Itens premium

O gerente quer destacar na vitrine só os produtos mais caros da loja.

Crie uma consulta que liste produtos com preço maior que 100.

## Exercício 8 - Cliente trocou de email

Uma cliente avisou que trocou de email e pediu para atualizar o cadastro.

Crie um comando SQL para atualizar o email de um cliente usando o `id`.

## Exercício 9 - Reajuste de preço

Um dos produtos precisou de reajuste de preço depois de uma renegociação com o fornecedor.

Crie um comando SQL para atualizar o preço de um produto usando o `id`.

## Exercício 10 - Produto descontinuado

Um dos itens saiu de linha e não será mais vendido.

Crie um comando SQL para remover um produto usando o `id`.

## Exercício 11 - Categorias da loja

A loja quer organizar os produtos em categorias, tipo "Roupas", "Acessórios" e "Ingressos".

Crie uma tabela chamada `categorias` com os campos:

- id;
- nome.

## Exercício 12 - Produto com categoria

Cada produto do catálogo precisa pertencer a uma categoria.

Altere a tabela de produtos para que cada produto possa pertencer a uma categoria.

Use chave estrangeira.

## Exercício 13 - Tabela de pedidos

A loja está pronta para registrar as primeiras vendas de verdade.

Crie uma tabela chamada `pedidos` com os campos:

- id;
- cliente_id;
- data do pedido;
- valor total.

Relacione `pedidos` com `clientes`.

## Exercício 14 - Vitrine ordenada

O site da loja quer mostrar os produtos do mais barato para o mais caro.

Crie uma consulta que liste produtos ordenados por preço.

## Exercício 15 - Quantos clientes já temos

A gerência pediu um número rápido para a reunião de segunda-feira.

Crie uma consulta que conte quantos clientes estão cadastrados.

---

# Nível Médio — 🎵 Vibe Store (expandindo o catálogo)

As vendas da Vibe Store decolaram, e agora a loja precisa de endereço de entrega, pedidos com múltiplos itens e controle de pagamento — o básico do nível fácil não é mais suficiente.

## Exercício 1 - Modelagem de clientes e endereços

Um cliente pode querer entregar em mais de um endereço (casa, trabalho, a casa de um amigo perto do festival).

Modele esse cenário. Identifique as tabelas necessárias e seus relacionamentos.

## Exercício 2 - Criação de clientes e endereços

Crie as tabelas `clientes` e `enderecos`, incluindo chave estrangeira em `enderecos`.

## Exercício 3 - Produtos e categorias

Modele a relação entre produtos e categorias, considerando que uma categoria pode ter vários produtos.

## Exercício 4 - Vitrine com nome da categoria

O site quer mostrar, ao lado de cada produto, o nome da categoria dele.

Crie uma consulta que mostre o nome do produto e o nome da categoria usando `JOIN`.

## Exercício 5 - Pedidos e itens do pedido

Um pedido normalmente tem mais de um produto — alguém que compra uma camiseta também costuma levar um boné.

Modele esse cenário. Cada item deve estar relacionado a um produto.

## Exercício 6 - Criação de itens do pedido

Crie as tabelas `pedidos` e `itens_pedido` com suas respectivas chaves estrangeiras.

## Exercício 7 - Dados de exemplo

Insira dados de exemplo em:

- clientes;
- produtos;
- pedidos;
- itens_pedido.

## Exercício 8 - Pedidos com nome do cliente

O time de atendimento precisa ver rapidamente quem fez cada pedido, sem precisar cruzar tabelas na mão.

Crie uma consulta que mostre pedidos junto com o nome do cliente.

## Exercício 9 - Itens com nome do produto

A tela de detalhe de um pedido precisa mostrar o nome de cada produto comprado, não só o `produto_id`.

Crie uma consulta que mostre itens do pedido com:

- nome do produto;
- quantidade;
- preço unitário.

## Exercício 10 - Subtotal por item

Cada linha do pedido precisa mostrar quanto custou aquele item especificamente.

Crie uma consulta que calcule o subtotal de cada item do pedido.

Fórmula:

```text
quantidade * preco_unitario
```

## Exercício 11 - Produtos que ninguém comprou ainda

O gerente quer saber quais produtos do catálogo nunca venderam nada, para decidir se vale a pena manter no site.

Crie uma consulta que liste produtos que nunca apareceram em itens de pedido.

Dica: use `LEFT JOIN`.

## Exercício 12 - Clientes que já compraram

O time de fidelidade quer uma lista só de quem já é cliente de verdade (fez pelo menos um pedido), para uma campanha exclusiva.

Crie uma consulta que liste clientes que já fizeram pelo menos um pedido.

## Exercício 13 - Tabela de pagamentos

Cada pedido precisa registrar como foi pago e se o pagamento já foi confirmado.

Crie uma tabela `pagamentos` relacionada a `pedidos`.

Ela deve guardar:

- id;
- pedido_id;
- forma de pagamento;
- valor;
- status.

## Exercício 14 - Pedidos com status de pagamento

O financeiro quer ver, lado a lado, cada pedido e a situação do pagamento dele.

Crie uma consulta que mostre pedidos junto com seus pagamentos.

## Exercício 15 - Ranking de clientes

A loja quer premiar quem mais gastou até agora com um brinde exclusivo no dia do festival.

Crie uma consulta que agrupe os pedidos por cliente e mostre o total comprado por cada um.

---

# Nível Difícil

## Parte 1 — 🎵 Vibe Store completa

O festival está crescendo tanto que a Vibe Store virou, na prática, um e-commerce completo, com o mesmo tanto de complexidade de qualquer loja online.

### Exercício 1 - Modelagem do e-commerce completo

Modele a Vibe Store como um e-commerce completo, com as tabelas:

- clientes;
- endereços;
- categorias;
- produtos;
- pedidos;
- itens_pedido;
- pagamentos.

### Exercício 2 - Criação de todas as tabelas

Crie todas as tabelas da Vibe Store com chaves primárias e estrangeiras.

### Exercício 3 - Relação entre pedido, item e produto

Garanta que `itens_pedido` esteja relacionado a `pedidos` e `produtos`.

### Exercício 4 - Relação entre pedido e pagamento

Garanta que `pagamentos` esteja relacionado a `pedidos`.

### Exercício 5 - Massa de dados coerente

Para testar o sistema de verdade, insira dados coerentes com pelo menos:

- três clientes;
- cinco produtos;
- três pedidos;
- múltiplos itens por pedido.

### Exercício 6 - Consulta completa de pedidos

O relatório mensal da diretoria pediu tudo junto, numa única visão.

Crie uma consulta que mostre:

- cliente;
- pedido;
- produto;
- quantidade;
- subtotal.

### Exercício 7 - Produto mais vendido do festival

A diretoria quer saber qual item vendeu mais, para garantir estoque extra no ano que vem.

Crie uma consulta que mostre o total vendido por produto.

### Exercício 8 - Cliente que mais gastou

Quem vai ganhar o brinde exclusivo mencionado no nível médio?

Crie uma consulta que mostre o total comprado por cliente.

### Exercício 9 - Itens que vão faltar

O time de logística precisa saber, antes do dia do evento, quais produtos já zeraram no estoque.

Crie uma consulta que liste produtos com estoque igual a zero.

### Exercício 10 - Clientes que só olharam a vitrine

Alguns clientes se cadastraram no site mas nunca finalizaram uma compra.

Crie uma consulta que liste clientes que ainda não fizeram pedidos.

## Parte 2 — 🧙 Academia de Magia

O festival também tem uma atração paralela: uma "Academia de Magia" temática, com oficinas e trilhas de aprendizado para o público. A organização pediu um sistema simples para controlar matrículas e notas das oficinas.

### Exercício 11 - Modelagem da Academia

Modele um sistema para a Academia de Magia com:

- alunos (os aprendizes);
- cursos (as trilhas de magia, ex: "Poções", "Feitiços Básicos");
- disciplinas (os módulos dentro de cada trilha);
- matrículas;
- notas.

### Exercício 12 - Relação muitos-para-muitos

Um aprendiz pode se matricular em várias trilhas, e cada trilha tem vários aprendizes.

Crie as tabelas da Academia considerando essa relação muitos-para-muitos entre alunos e cursos.

Use uma tabela intermediária.

### Exercício 13 - Consulta aluno, curso e disciplina

A secretaria da Academia quer ver, de uma vez, qual aprendiz está em qual trilha e quais disciplinas cada trilha inclui.

Crie uma consulta que liste alunos, cursos e disciplinas relacionadas.

### Exercício 14 - Média por aprendiz

Ao final de cada disciplina, a Academia calcula a média do aprendiz.

Crie uma consulta que calcule a média de notas por aluno em uma disciplina.

## Parte 3 — 📚 Biblioteca da Academia

### Exercício 15 - Biblioteca de grimórios

Toda academia de magia que se preze tem uma biblioteca de grimórios (livros de feitiços) emprestáveis, cada um escrito por um ou mais mestres bruxos.

Modele uma biblioteca com:

- autores (os mestres bruxos);
- livros (os grimórios);
- usuários (quem pega emprestado);
- empréstimos;
- relação entre livros e autores (um grimório pode ter mais de um autor).

Depois, crie uma consulta listando empréstimos com usuário, livro e autor.
