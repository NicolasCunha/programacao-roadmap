# Folha de Exercícios - SQL

> **Objetivo:** praticar modelagem relacional e SQL usando MySQL. O foco é criação de tabelas, chaves, relações e consultas. Não é necessário usar PL/SQL.

## Como resolver cada exercício

Para cada atividade:

1. Leia o problema de dados.
2. Identifique entidades e atributos.
3. Defina chaves primárias.
4. Defina chaves estrangeiras quando houver relacionamento.
5. Escreva o SQL.
6. Execute no MySQL.
7. Teste com dados inseridos manualmente.

---

# Nível Fácil

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

## Exercício 3 - Inserção de clientes

Insira três clientes na tabela `clientes`.

## Exercício 4 - Inserção de produtos

Insira cinco produtos na tabela `produtos`.

## Exercício 5 - Consulta de clientes

Crie uma consulta que liste todos os clientes cadastrados.

## Exercício 6 - Consulta parcial de clientes

Crie uma consulta que mostre apenas nome e email dos clientes.

## Exercício 7 - Filtro de produtos por preço

Crie uma consulta que liste produtos com preço maior que 100.

## Exercício 8 - Atualização de email

Crie um comando SQL para atualizar o email de um cliente usando o `id`.

## Exercício 9 - Atualização de preço

Crie um comando SQL para atualizar o preço de um produto usando o `id`.

## Exercício 10 - Remoção de produto

Crie um comando SQL para remover um produto usando o `id`.

## Exercício 11 - Tabela de categorias

Crie uma tabela chamada `categorias` com os campos:

- id;
- nome.

## Exercício 12 - Relacionamento produto-categoria

Altere a tabela de produtos para que cada produto possa pertencer a uma categoria.

Use chave estrangeira.

## Exercício 13 - Tabela de pedidos

Crie uma tabela chamada `pedidos` com os campos:

- id;
- cliente_id;
- data do pedido;
- valor total.

Relacione `pedidos` com `clientes`.

## Exercício 14 - Ordenação de produtos

Crie uma consulta que liste produtos ordenados por preço.

## Exercício 15 - Contagem de clientes

Crie uma consulta que conte quantos clientes estão cadastrados.

---

# Nível Médio

## Exercício 1 - Modelagem de clientes e endereços

Modele um cenário em que um cliente pode ter vários endereços.

Identifique as tabelas necessárias e seus relacionamentos.

## Exercício 2 - Criação de clientes e endereços

Crie as tabelas `clientes` e `enderecos`, incluindo chave estrangeira em `enderecos`.

## Exercício 3 - Produtos e categorias

Modele a relação entre produtos e categorias, considerando que uma categoria pode ter vários produtos.

## Exercício 4 - JOIN entre produtos e categorias

Crie uma consulta que mostre o nome do produto e o nome da categoria usando `JOIN`.

## Exercício 5 - Pedidos e itens do pedido

Modele um cenário em que um pedido pode ter vários itens.

Cada item deve estar relacionado a um produto.

## Exercício 6 - Criação de itens do pedido

Crie as tabelas `pedidos` e `itens_pedido` com suas respectivas chaves estrangeiras.

## Exercício 7 - Dados de exemplo

Insira dados de exemplo em:

- clientes;
- produtos;
- pedidos;
- itens_pedido.

## Exercício 8 - Pedidos com cliente

Crie uma consulta que mostre pedidos junto com o nome do cliente.

## Exercício 9 - Itens com produto

Crie uma consulta que mostre itens do pedido com:

- nome do produto;
- quantidade;
- preço unitário.

## Exercício 10 - Subtotal por item

Crie uma consulta que calcule o subtotal de cada item do pedido.

Fórmula:

```text
quantidade * preco_unitario
```

## Exercício 11 - Produtos nunca vendidos

Crie uma consulta que liste produtos que nunca apareceram em itens de pedido.

Dica: use `LEFT JOIN`.

## Exercício 12 - Clientes com pedidos

Crie uma consulta que liste clientes que já fizeram pelo menos um pedido.

## Exercício 13 - Tabela de pagamentos

Crie uma tabela `pagamentos` relacionada a `pedidos`.

Ela deve guardar:

- id;
- pedido_id;
- forma de pagamento;
- valor;
- status.

## Exercício 14 - Pedidos com pagamentos

Crie uma consulta que mostre pedidos junto com seus pagamentos.

## Exercício 15 - Total comprado por cliente

Crie uma consulta que agrupe os pedidos por cliente e mostre o total comprado por cada um.

---

# Nível Difícil

## Exercício 1 - Modelagem de mini e-commerce

Modele um mini e-commerce com as tabelas:

- clientes;
- endereços;
- categorias;
- produtos;
- pedidos;
- itens_pedido;
- pagamentos.

## Exercício 2 - Criação das tabelas do e-commerce

Crie todas as tabelas do mini e-commerce com chaves primárias e estrangeiras.

## Exercício 3 - Relação entre pedido, item e produto

Garanta que `itens_pedido` esteja relacionado a `pedidos` e `produtos`.

## Exercício 4 - Relação entre pedido e pagamento

Garanta que `pagamentos` esteja relacionado a `pedidos`.

## Exercício 5 - Massa de dados coerente

Insira dados coerentes com pelo menos:

- três clientes;
- cinco produtos;
- três pedidos;
- múltiplos itens por pedido.

## Exercício 6 - Consulta completa de pedidos

Crie uma consulta que mostre:

- cliente;
- pedido;
- produto;
- quantidade;
- subtotal.

## Exercício 7 - Total vendido por produto

Crie uma consulta que mostre o total vendido por produto.

## Exercício 8 - Total comprado por cliente

Crie uma consulta que mostre o total comprado por cliente.

## Exercício 9 - Produtos sem estoque

Crie uma consulta que liste produtos com estoque igual a zero.

## Exercício 10 - Clientes sem pedidos

Crie uma consulta que liste clientes que ainda não fizeram pedidos.

## Exercício 11 - Modelagem de escola

Modele um sistema escolar com:

- alunos;
- cursos;
- disciplinas;
- matrículas;
- notas.

## Exercício 12 - Relação muitos-para-muitos

Crie as tabelas da escola considerando relação muitos-para-muitos entre alunos e cursos.

Use uma tabela intermediária.

## Exercício 13 - Consulta aluno, curso e disciplina

Crie uma consulta que liste alunos, cursos e disciplinas relacionadas.

## Exercício 14 - Média por aluno

Crie uma consulta que calcule a média de notas por aluno em uma disciplina.

## Exercício 15 - Sistema de biblioteca

Modele um sistema de biblioteca com:

- autores;
- livros;
- usuários;
- empréstimos;
- relação entre livros e autores.

Depois, crie uma consulta listando empréstimos com usuário, livro e autor.
