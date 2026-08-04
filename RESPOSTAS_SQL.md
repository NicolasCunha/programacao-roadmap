# Respostas - Exercícios SQL

> Use depois de tentar. Ajuste tipos conforme o SGBD. O roadmap recomenda MySQL.

## Fácil

### 1 e 2
```sql
CREATE TABLE clientes (
    id INT PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    email VARCHAR(100) NOT NULL,
    data_nascimento DATE
);

CREATE TABLE produtos (
    id INT PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    preco DECIMAL(10,2) NOT NULL,
    quantidade_estoque INT NOT NULL
);
```

### 3 a 15
```sql
INSERT INTO clientes VALUES (1, 'Ana', 'ana@email.com', '2000-01-01');
INSERT INTO clientes VALUES (2, 'Bruno', 'bruno@email.com', '1998-05-10');
INSERT INTO clientes VALUES (3, 'Carla', 'carla@email.com', '2001-09-20');

INSERT INTO produtos VALUES (1, 'Mouse', 50.00, 10);
INSERT INTO produtos VALUES (2, 'Teclado', 120.00, 5);
INSERT INTO produtos VALUES (3, 'Monitor', 900.00, 2);
INSERT INTO produtos VALUES (4, 'Cabo HDMI', 30.00, 20);
INSERT INTO produtos VALUES (5, 'Headset', 200.00, 4);

SELECT * FROM clientes;
SELECT nome, email FROM clientes;
SELECT * FROM produtos WHERE preco > 100;
UPDATE clientes SET email = 'novo@email.com' WHERE id = 1;
UPDATE produtos SET preco = 130.00 WHERE id = 2;
DELETE FROM produtos WHERE id = 4;

CREATE TABLE categorias (id INT PRIMARY KEY, nome VARCHAR(100) NOT NULL);
ALTER TABLE produtos ADD categoria_id INT;
ALTER TABLE produtos ADD CONSTRAINT fk_produtos_categorias FOREIGN KEY (categoria_id) REFERENCES categorias(id);

CREATE TABLE pedidos (
    id INT PRIMARY KEY,
    cliente_id INT NOT NULL,
    data_pedido DATE NOT NULL,
    valor_total DECIMAL(10,2) NOT NULL,
    FOREIGN KEY (cliente_id) REFERENCES clientes(id)
);

SELECT * FROM produtos ORDER BY preco;
SELECT COUNT(*) FROM clientes;
```

## Médio

```sql
CREATE TABLE enderecos (
    id INT PRIMARY KEY,
    cliente_id INT NOT NULL,
    rua VARCHAR(100) NOT NULL,
    cidade VARCHAR(100) NOT NULL,
    estado CHAR(2) NOT NULL,
    FOREIGN KEY (cliente_id) REFERENCES clientes(id)
);

CREATE TABLE itens_pedido (
    id INT PRIMARY KEY,
    pedido_id INT NOT NULL,
    produto_id INT NOT NULL,
    quantidade INT NOT NULL,
    preco_unitario DECIMAL(10,2) NOT NULL,
    FOREIGN KEY (pedido_id) REFERENCES pedidos(id),
    FOREIGN KEY (produto_id) REFERENCES produtos(id)
);

CREATE TABLE pagamentos (
    id INT PRIMARY KEY,
    pedido_id INT NOT NULL,
    forma VARCHAR(30) NOT NULL,
    valor DECIMAL(10,2) NOT NULL,
    status VARCHAR(30) NOT NULL,
    FOREIGN KEY (pedido_id) REFERENCES pedidos(id)
);
```

Consultas referência:

```sql
SELECT p.nome, c.nome AS categoria
FROM produtos p
JOIN categorias c ON c.id = p.categoria_id;

SELECT c.nome, p.id, p.valor_total
FROM pedidos p
JOIN clientes c ON c.id = p.cliente_id;

SELECT pr.nome, ip.quantidade, ip.preco_unitario
FROM itens_pedido ip
JOIN produtos pr ON pr.id = ip.produto_id;

SELECT pr.nome, ip.quantidade * ip.preco_unitario AS subtotal
FROM itens_pedido ip
JOIN produtos pr ON pr.id = ip.produto_id;

SELECT pr.*
FROM produtos pr
LEFT JOIN itens_pedido ip ON ip.produto_id = pr.id
WHERE ip.id IS NULL;

SELECT c.nome, SUM(p.valor_total) AS total_comprado
FROM clientes c
JOIN pedidos p ON p.cliente_id = c.id
GROUP BY c.nome;
```

## Difícil

### Mini e-commerce

```sql
CREATE TABLE clientes (id INT PRIMARY KEY, nome VARCHAR(100) NOT NULL, email VARCHAR(100) NOT NULL);
CREATE TABLE enderecos (id INT PRIMARY KEY, cliente_id INT NOT NULL, rua VARCHAR(100), cidade VARCHAR(100), FOREIGN KEY (cliente_id) REFERENCES clientes(id));
CREATE TABLE categorias (id INT PRIMARY KEY, nome VARCHAR(100) NOT NULL);
CREATE TABLE produtos (id INT PRIMARY KEY, categoria_id INT NOT NULL, nome VARCHAR(100), preco DECIMAL(10,2), estoque INT, FOREIGN KEY (categoria_id) REFERENCES categorias(id));
CREATE TABLE pedidos (id INT PRIMARY KEY, cliente_id INT NOT NULL, data_pedido DATE, FOREIGN KEY (cliente_id) REFERENCES clientes(id));
CREATE TABLE itens_pedido (id INT PRIMARY KEY, pedido_id INT NOT NULL, produto_id INT NOT NULL, quantidade INT, preco_unitario DECIMAL(10,2), FOREIGN KEY (pedido_id) REFERENCES pedidos(id), FOREIGN KEY (produto_id) REFERENCES produtos(id));
CREATE TABLE pagamentos (id INT PRIMARY KEY, pedido_id INT NOT NULL, forma VARCHAR(30), valor DECIMAL(10,2), FOREIGN KEY (pedido_id) REFERENCES pedidos(id));
```

Consultas principais:

```sql
SELECT c.nome AS cliente, p.id AS pedido, pr.nome AS produto, ip.quantidade, ip.quantidade * ip.preco_unitario AS subtotal
FROM pedidos p
JOIN clientes c ON c.id = p.cliente_id
JOIN itens_pedido ip ON ip.pedido_id = p.id
JOIN produtos pr ON pr.id = ip.produto_id;

SELECT pr.nome, SUM(ip.quantidade * ip.preco_unitario) AS total_vendido
FROM produtos pr
JOIN itens_pedido ip ON ip.produto_id = pr.id
GROUP BY pr.nome;

SELECT c.nome, SUM(ip.quantidade * ip.preco_unitario) AS total_comprado
FROM clientes c
JOIN pedidos p ON p.cliente_id = c.id
JOIN itens_pedido ip ON ip.pedido_id = p.id
GROUP BY c.nome;

SELECT * FROM produtos WHERE estoque = 0;

SELECT c.*
FROM clientes c
LEFT JOIN pedidos p ON p.cliente_id = c.id
WHERE p.id IS NULL;
```

### Escola e biblioteca

- Escola: crie `alunos`, `cursos`, `disciplinas`, `matriculas`, `notas`; use `matriculas` como tabela intermediária.
- Biblioteca: crie `autores`, `livros`, `livros_autores`, `usuarios`, `emprestimos`; use `livros_autores` para N:N.
- Média por aluno: `AVG(n.valor)` com `GROUP BY aluno, disciplina`.
- Empréstimos: `JOIN usuarios`, `JOIN livros`, `JOIN livros_autores`, `JOIN autores`.
