# Folha de Exercícios - SQL

Esta folha foca SQL relacional: criação de tabelas, chaves primárias, chaves estrangeiras, inserção, atualização, remoção e consultas com relações. Não é necessário estudar PL/SQL aqui.

Evite usar IA para gerar o script final. Escreva manualmente para memorizar padrões de `CREATE TABLE`, `PRIMARY KEY`, `FOREIGN KEY`, `INSERT`, `SELECT` e `JOIN`.

---

## Fácil

1. Crie uma tabela `clientes` com `id`, `nome`, `email` e `data_nascimento`.
2. Crie uma tabela `produtos` com `id`, `nome`, `preco` e `quantidade_estoque`.
3. Insira três clientes na tabela `clientes`.
4. Insira cinco produtos na tabela `produtos`.
5. Consulte todos os clientes.
6. Consulte apenas nome e email dos clientes.
7. Consulte produtos com preço maior que 100.
8. Atualize o email de um cliente pelo `id`.
9. Atualize o preço de um produto pelo `id`.
10. Remova um produto pelo `id`.
11. Crie uma tabela `categorias` com `id` e `nome`.
12. Adicione uma coluna `categoria_id` em `produtos`, se seu banco suportar `ALTER TABLE`.
13. Crie uma tabela `pedidos` com `id`, `cliente_id`, `data_pedido` e `valor_total`.
14. Faça uma consulta com `ORDER BY` para listar produtos por preço.
15. Faça uma consulta com `COUNT` para contar clientes cadastrados.

---

## Médio

1. Modele clientes e endereços, considerando que um cliente pode ter vários endereços.
2. Crie tabelas `clientes`, `enderecos` e defina chave estrangeira.
3. Modele produtos e categorias, considerando que uma categoria pode ter vários produtos.
4. Crie uma consulta com `JOIN` entre produtos e categorias.
5. Modele pedidos e itens do pedido, considerando que um pedido tem vários itens.
6. Crie tabelas `pedidos` e `itens_pedido` com chaves estrangeiras.
7. Insira dados de exemplo em clientes, produtos, pedidos e itens.
8. Consulte pedidos mostrando nome do cliente e valor total.
9. Consulte itens de pedido mostrando nome do produto, quantidade e preço unitário.
10. Faça uma consulta que calcule subtotal de cada item: quantidade vezes preço unitário.
11. Liste produtos que nunca foram vendidos usando `LEFT JOIN`.
12. Liste clientes que já fizeram pelo menos um pedido.
13. Crie uma tabela `pagamentos` relacionada a `pedidos`.
14. Consulte pedidos com seus pagamentos.
15. Crie uma consulta agrupando total vendido por cliente.

---

## Difícil

1. Modele um mini e-commerce com tabelas `clientes`, `enderecos`, `categorias`, `produtos`, `pedidos`, `itens_pedido` e `pagamentos`.
2. Crie todas as tabelas do mini e-commerce com chaves primárias e estrangeiras.
3. Garanta que `itens_pedido` tenha relação com `pedidos` e `produtos`.
4. Garanta que `pagamentos` tenha relação com `pedidos`.
5. Insira um conjunto coerente de dados com pelo menos três clientes, cinco produtos, três pedidos e múltiplos itens por pedido.
6. Faça uma consulta que mostre cliente, pedido, produto, quantidade e subtotal.
7. Faça uma consulta que mostre o total vendido por produto.
8. Faça uma consulta que mostre o total comprado por cliente.
9. Faça uma consulta para identificar produtos sem estoque.
10. Faça uma consulta para identificar clientes sem pedidos.
11. Modele uma escola com tabelas `alunos`, `cursos`, `disciplinas`, `matriculas` e `notas`.
12. Crie as tabelas da escola com relações de muitos para muitos entre alunos e cursos usando tabela intermediária.
13. Faça uma consulta que liste alunos, cursos e disciplinas.
14. Faça uma consulta que calcule média de notas por aluno em uma disciplina.
15. Modele um sistema de biblioteca com `autores`, `livros`, `usuarios`, `emprestimos` e uma relação entre livros e autores. Crie as tabelas e uma consulta listando empréstimos com usuário, livro e autor.
