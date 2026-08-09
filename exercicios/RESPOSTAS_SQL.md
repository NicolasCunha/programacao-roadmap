# Respostas - Exercícios SQL

> Use este arquivo apenas depois de tentar. Ajuste tipos conforme o SGBD — o roadmap recomenda MySQL. As respostas são referências, não a única forma correta.

## Nível Fácil — Vibe Store

### 1 e 2 - Tabelas de clientes e produtos

```sql
CREATE TABLE clientes (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100) NOT NULL,
    email VARCHAR(150) NOT NULL UNIQUE,
    data_nascimento DATE
);

CREATE TABLE produtos (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100) NOT NULL,
    preco DECIMAL(10,2) NOT NULL,
    quantidade_estoque INT NOT NULL DEFAULT 0
);
```

`AUTO_INCREMENT` evita ter que controlar manualmente qual `id` já foi usado; `email UNIQUE` impede dois cadastros com o mesmo email.

### 3 - Primeiros clientes cadastrados

```sql
INSERT INTO clientes (nome, email, data_nascimento) VALUES
    ('Ana', 'ana@email.com', '2000-01-01'),
    ('Bruno', 'bruno@email.com', '1998-05-10'),
    ('Carla', 'carla@email.com', '2001-09-20');
```

### 4 - Catálogo inicial

```sql
INSERT INTO produtos (nome, preco, quantidade_estoque) VALUES
    ('Camiseta do Festival', 60.00, 100),
    ('Bone Vibe Store', 45.00, 60),
    ('Ingresso VIP', 450.00, 20),
    ('Caneca Colecionavel', 35.00, 80),
    ('Pulseira LED', 25.00, 150);
```

### 5 - Lista completa de clientes

```sql
SELECT * FROM clientes;
```

### 6 - Lista de contatos

```sql
SELECT nome, email FROM clientes;
```

### 7 - Itens premium

```sql
SELECT * FROM produtos WHERE preco > 100;
```

### 8 - Cliente trocou de email

```sql
UPDATE clientes SET email = 'ana.nova@email.com' WHERE id = 1;
```

### 9 - Reajuste de preço

```sql
UPDATE produtos SET preco = 65.00 WHERE id = 1;
```

### 10 - Produto descontinuado

```sql
DELETE FROM produtos WHERE id = 5;
```

### 11 - Categorias da loja

```sql
CREATE TABLE categorias (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100) NOT NULL
);
```

### 12 - Produto com categoria

```sql
ALTER TABLE produtos ADD COLUMN categoria_id INT;
ALTER TABLE produtos ADD CONSTRAINT fk_produtos_categorias
    FOREIGN KEY (categoria_id) REFERENCES categorias(id);
```

### 13 - Tabela de pedidos

```sql
CREATE TABLE pedidos (
    id INT PRIMARY KEY AUTO_INCREMENT,
    cliente_id INT NOT NULL,
    data_pedido DATE NOT NULL,
    valor_total DECIMAL(10,2) NOT NULL,
    FOREIGN KEY (cliente_id) REFERENCES clientes(id)
);
```

### 14 - Vitrine ordenada

```sql
SELECT * FROM produtos ORDER BY preco ASC;
```

### 15 - Quantos clientes já temos

```sql
SELECT COUNT(*) AS total_clientes FROM clientes;
```

---

## Nível Médio — Vibe Store (expandindo o catálogo)

### 1 - Modelagem de clientes e endereços

Um cliente pode ter vários endereços, mas cada endereço pertence a um único cliente — relacionamento 1:N, com a chave estrangeira do lado "N" (`enderecos`).

### 2 - Criação de clientes e endereços

```sql
CREATE TABLE enderecos (
    id INT PRIMARY KEY AUTO_INCREMENT,
    cliente_id INT NOT NULL,
    rua VARCHAR(150) NOT NULL,
    cidade VARCHAR(100) NOT NULL,
    estado CHAR(2) NOT NULL,
    FOREIGN KEY (cliente_id) REFERENCES clientes(id)
);
```

### 3 - Produtos e categorias

Uma categoria tem vários produtos, mas cada produto pertence a uma única categoria — também 1:N, mesma lógica do exercício anterior.

### 4 - Vitrine com nome da categoria

```sql
SELECT p.nome AS produto, c.nome AS categoria
FROM produtos p
JOIN categorias c ON c.id = p.categoria_id;
```

### 5 - Pedidos e itens do pedido

Um pedido pode ter vários itens, e cada item se refere a um produto — `itens_pedido` guarda `pedido_id` e `produto_id`, sendo a ponte entre as duas coisas.

### 6 - Criação de itens do pedido

```sql
CREATE TABLE pedidos (
    id INT PRIMARY KEY AUTO_INCREMENT,
    cliente_id INT NOT NULL,
    data_pedido DATE NOT NULL,
    FOREIGN KEY (cliente_id) REFERENCES clientes(id)
);

CREATE TABLE itens_pedido (
    id INT PRIMARY KEY AUTO_INCREMENT,
    pedido_id INT NOT NULL,
    produto_id INT NOT NULL,
    quantidade INT NOT NULL,
    preco_unitario DECIMAL(10,2) NOT NULL,
    FOREIGN KEY (pedido_id) REFERENCES pedidos(id),
    FOREIGN KEY (produto_id) REFERENCES produtos(id)
);
```

`preco_unitario` fica guardado no item, não busca direto de `produtos.preco` — se o preço do produto mudar depois, o histórico do pedido antigo continua correto.

### 7 - Dados de exemplo

```sql
INSERT INTO pedidos (cliente_id, data_pedido) VALUES (1, '2026-06-10');

INSERT INTO itens_pedido (pedido_id, produto_id, quantidade, preco_unitario) VALUES
    (1, 1, 2, 60.00),
    (1, 4, 1, 35.00);
```

### 8 - Pedidos com nome do cliente

```sql
SELECT p.id AS pedido, c.nome AS cliente, p.data_pedido
FROM pedidos p
JOIN clientes c ON c.id = p.cliente_id;
```

### 9 - Itens com nome do produto

```sql
SELECT pr.nome AS produto, ip.quantidade, ip.preco_unitario
FROM itens_pedido ip
JOIN produtos pr ON pr.id = ip.produto_id;
```

### 10 - Subtotal por item

```sql
SELECT pr.nome AS produto, ip.quantidade * ip.preco_unitario AS subtotal
FROM itens_pedido ip
JOIN produtos pr ON pr.id = ip.produto_id;
```

### 11 - Produtos que ninguém comprou ainda

```sql
SELECT pr.*
FROM produtos pr
LEFT JOIN itens_pedido ip ON ip.produto_id = pr.id
WHERE ip.id IS NULL;
```

`LEFT JOIN` traz todo produto, mesmo sem correspondência em `itens_pedido`; `WHERE ip.id IS NULL` filtra exatamente os que não encontraram nenhuma venda.

### 12 - Clientes que já compraram

```sql
SELECT DISTINCT c.*
FROM clientes c
JOIN pedidos p ON p.cliente_id = c.id;
```

`DISTINCT` evita repetir o mesmo cliente várias vezes caso ele tenha feito mais de um pedido.

### 13 - Tabela de pagamentos

```sql
CREATE TABLE pagamentos (
    id INT PRIMARY KEY AUTO_INCREMENT,
    pedido_id INT NOT NULL,
    forma VARCHAR(30) NOT NULL,
    valor DECIMAL(10,2) NOT NULL,
    status VARCHAR(30) NOT NULL,
    FOREIGN KEY (pedido_id) REFERENCES pedidos(id)
);
```

### 14 - Pedidos com status de pagamento

```sql
SELECT p.id AS pedido, pg.forma, pg.status
FROM pedidos p
JOIN pagamentos pg ON pg.pedido_id = p.id;
```

### 15 - Ranking de clientes

```sql
SELECT c.nome, SUM(ip.quantidade * ip.preco_unitario) AS total_comprado
FROM clientes c
JOIN pedidos p ON p.cliente_id = c.id
JOIN itens_pedido ip ON ip.pedido_id = p.id
GROUP BY c.id, c.nome
ORDER BY total_comprado DESC;
```

---

## Nível Difícil

### Parte 1 - Vibe Store completa

#### 1 - Modelagem do e-commerce completo

```text
clientes 1---N enderecos
categorias 1---N produtos
clientes 1---N pedidos
pedidos 1---N itens_pedido N---1 produtos
pedidos 1---N pagamentos
```

#### 2, 3 e 4 - Criação de todas as tabelas

```sql
CREATE TABLE clientes (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100) NOT NULL,
    email VARCHAR(150) NOT NULL UNIQUE
);

CREATE TABLE enderecos (
    id INT PRIMARY KEY AUTO_INCREMENT,
    cliente_id INT NOT NULL,
    rua VARCHAR(150) NOT NULL,
    cidade VARCHAR(100) NOT NULL,
    FOREIGN KEY (cliente_id) REFERENCES clientes(id)
);

CREATE TABLE categorias (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100) NOT NULL
);

CREATE TABLE produtos (
    id INT PRIMARY KEY AUTO_INCREMENT,
    categoria_id INT NOT NULL,
    nome VARCHAR(100) NOT NULL,
    preco DECIMAL(10,2) NOT NULL,
    quantidade_estoque INT NOT NULL DEFAULT 0,
    FOREIGN KEY (categoria_id) REFERENCES categorias(id)
);

CREATE TABLE pedidos (
    id INT PRIMARY KEY AUTO_INCREMENT,
    cliente_id INT NOT NULL,
    data_pedido DATE NOT NULL,
    FOREIGN KEY (cliente_id) REFERENCES clientes(id)
);

CREATE TABLE itens_pedido (
    id INT PRIMARY KEY AUTO_INCREMENT,
    pedido_id INT NOT NULL,
    produto_id INT NOT NULL,
    quantidade INT NOT NULL,
    preco_unitario DECIMAL(10,2) NOT NULL,
    FOREIGN KEY (pedido_id) REFERENCES pedidos(id),
    FOREIGN KEY (produto_id) REFERENCES produtos(id)
);

CREATE TABLE pagamentos (
    id INT PRIMARY KEY AUTO_INCREMENT,
    pedido_id INT NOT NULL,
    forma VARCHAR(30) NOT NULL,
    valor DECIMAL(10,2) NOT NULL,
    status VARCHAR(30) NOT NULL,
    FOREIGN KEY (pedido_id) REFERENCES pedidos(id)
);
```

#### 5 - Massa de dados coerente

```sql
INSERT INTO clientes (nome, email) VALUES
    ('Ana', 'ana@email.com'), ('Bruno', 'bruno@email.com'), ('Carla', 'carla@email.com');

INSERT INTO categorias (nome) VALUES ('Roupas'), ('Acessorios'), ('Ingressos');

INSERT INTO produtos (categoria_id, nome, preco, quantidade_estoque) VALUES
    (1, 'Camiseta', 60.00, 100),
    (2, 'Bone', 45.00, 60),
    (3, 'Ingresso VIP', 450.00, 20),
    (2, 'Caneca', 35.00, 80),
    (2, 'Pulseira LED', 25.00, 150);

INSERT INTO pedidos (cliente_id, data_pedido) VALUES
    (1, '2026-06-10'), (2, '2026-06-11'), (1, '2026-06-12');

INSERT INTO itens_pedido (pedido_id, produto_id, quantidade, preco_unitario) VALUES
    (1, 1, 2, 60.00), (1, 4, 1, 35.00),
    (2, 3, 1, 450.00),
    (3, 2, 3, 45.00);
```

#### 6 - Consulta completa de pedidos

```sql
SELECT c.nome AS cliente, p.id AS pedido, pr.nome AS produto,
       ip.quantidade, ip.quantidade * ip.preco_unitario AS subtotal
FROM pedidos p
JOIN clientes c ON c.id = p.cliente_id
JOIN itens_pedido ip ON ip.pedido_id = p.id
JOIN produtos pr ON pr.id = ip.produto_id;
```

#### 7 - Produto mais vendido do festival

```sql
SELECT pr.nome, SUM(ip.quantidade) AS total_vendido
FROM produtos pr
JOIN itens_pedido ip ON ip.produto_id = pr.id
GROUP BY pr.id, pr.nome
ORDER BY total_vendido DESC;
```

#### 8 - Cliente que mais gastou

```sql
SELECT c.nome, SUM(ip.quantidade * ip.preco_unitario) AS total_gasto
FROM clientes c
JOIN pedidos p ON p.cliente_id = c.id
JOIN itens_pedido ip ON ip.pedido_id = p.id
GROUP BY c.id, c.nome
ORDER BY total_gasto DESC
LIMIT 1;
```

#### 9 - Itens que vão faltar

```sql
SELECT * FROM produtos WHERE quantidade_estoque = 0;
```

#### 10 - Clientes que só olharam a vitrine

```sql
SELECT c.*
FROM clientes c
LEFT JOIN pedidos p ON p.cliente_id = c.id
WHERE p.id IS NULL;
```

### Parte 2 - Academia de Magia

#### 11 - Modelagem da Academia

```text
alunos N---N cursos (via matriculas)
cursos 1---N disciplinas
matriculas 1---N notas (uma nota por disciplina cursada)
```

#### 12 - Relação muitos-para-muitos

```sql
CREATE TABLE alunos (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100) NOT NULL
);

CREATE TABLE cursos (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100) NOT NULL
);

CREATE TABLE disciplinas (
    id INT PRIMARY KEY AUTO_INCREMENT,
    curso_id INT NOT NULL,
    nome VARCHAR(100) NOT NULL,
    FOREIGN KEY (curso_id) REFERENCES cursos(id)
);

CREATE TABLE matriculas (
    aluno_id INT NOT NULL,
    curso_id INT NOT NULL,
    data_matricula DATE NOT NULL,
    PRIMARY KEY (aluno_id, curso_id),
    FOREIGN KEY (aluno_id) REFERENCES alunos(id),
    FOREIGN KEY (curso_id) REFERENCES cursos(id)
);

CREATE TABLE notas (
    id INT PRIMARY KEY AUTO_INCREMENT,
    aluno_id INT NOT NULL,
    disciplina_id INT NOT NULL,
    valor DECIMAL(4,2) NOT NULL,
    FOREIGN KEY (aluno_id) REFERENCES alunos(id),
    FOREIGN KEY (disciplina_id) REFERENCES disciplinas(id)
);
```

`matriculas` é a tabela associativa clássica de N:N: nenhuma FK sozinha resolveria "vários aprendizes em várias trilhas ao mesmo tempo".

#### 13 - Consulta aluno, curso e disciplina

```sql
SELECT a.nome AS aluno, c.nome AS curso, d.nome AS disciplina
FROM matriculas m
JOIN alunos a ON a.id = m.aluno_id
JOIN cursos c ON c.id = m.curso_id
JOIN disciplinas d ON d.curso_id = c.id;
```

#### 14 - Média por aprendiz

```sql
SELECT a.nome, d.nome AS disciplina, AVG(n.valor) AS media
FROM notas n
JOIN alunos a ON a.id = n.aluno_id
JOIN disciplinas d ON d.id = n.disciplina_id
GROUP BY a.id, a.nome, d.id, d.nome;
```

### Parte 3 - Biblioteca da Academia

#### 15 - Biblioteca de grimórios

```sql
CREATE TABLE autores (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100) NOT NULL
);

CREATE TABLE livros (
    id INT PRIMARY KEY AUTO_INCREMENT,
    titulo VARCHAR(150) NOT NULL
);

CREATE TABLE livros_autores (
    livro_id INT NOT NULL,
    autor_id INT NOT NULL,
    PRIMARY KEY (livro_id, autor_id),
    FOREIGN KEY (livro_id) REFERENCES livros(id),
    FOREIGN KEY (autor_id) REFERENCES autores(id)
);

CREATE TABLE usuarios (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100) NOT NULL
);

CREATE TABLE emprestimos (
    id INT PRIMARY KEY AUTO_INCREMENT,
    livro_id INT NOT NULL,
    usuario_id INT NOT NULL,
    data_emprestimo DATE NOT NULL,
    data_devolucao DATE,
    FOREIGN KEY (livro_id) REFERENCES livros(id),
    FOREIGN KEY (usuario_id) REFERENCES usuarios(id)
);
```

`livros_autores` resolve o mesmo problema de N:N de `matriculas`, agora entre grimórios e mestres bruxos: um grimório pode ter mais de um autor, e um mestre pode ter escrito mais de um grimório.

```sql
SELECT u.nome AS usuario, l.titulo AS livro, a.nome AS autor
FROM emprestimos e
JOIN usuarios u ON u.id = e.usuario_id
JOIN livros l ON l.id = e.livro_id
JOIN livros_autores la ON la.livro_id = l.id
JOIN autores a ON a.id = la.autor_id;
```
