# Nível 4 - SQL Avançado e Modelagem

O [Nível 3](./NIVEL_3_BANCO_DE_DADOS.md) te deu o vocabulário fundamental de banco de dados usando uma única tabela: o que é uma tabela, uma coluna, uma chave primária, uma chave estrangeira. Este nível parte de onde aquele parou e responde à pergunta natural seguinte: **como modelar e consultar um sistema com várias tabelas conectadas, do jeito que um sistema real de verdade exige?**

---

## Índice de progresso

- [ ] [Objetivo do nível](#objetivo-do-nível)
- [ ] [Uso responsável de IA](#uso-responsável-de-ia)
- [ ] [Recapitulando o Nível 3 em uma frase](#recapitulando-o-nível-3-em-uma-frase)
- [ ] [Cardinalidade: o vocabulário dos relacionamentos](#cardinalidade-o-vocabulário-dos-relacionamentos)
- [ ] [Relacionamento 1:1](#relacionamento-11)
- [ ] [Relacionamento 1:N](#relacionamento-1n)
- [ ] [Relacionamento N:N](#relacionamento-nn)
- [ ] [Notação de diagrama entidade-relacionamento](#notação-de-diagrama-entidade-relacionamento)
- [ ] [Normalização: revisando o problema](#normalização-revisando-o-problema)
- [ ] [Primeira Forma Normal (1FN)](#primeira-forma-normal-1fn)
- [ ] [Segunda Forma Normal (2FN)](#segunda-forma-normal-2fn)
- [ ] [Terceira Forma Normal (3FN)](#terceira-forma-normal-3fn)
- [ ] [Além da 3FN, rapidamente](#além-da-3fn-rapidamente)
- [ ] [Exemplo completo: normalizando do zero](#exemplo-completo-normalizando-do-zero)
- [ ] [Desnormalização: quebrando a regra de propósito](#desnormalização-quebrando-a-regra-de-propósito)
- [ ] [O problema que o JOIN resolve](#o-problema-que-o-join-resolve)
- [ ] [INNER JOIN](#inner-join)
- [ ] [LEFT JOIN e RIGHT JOIN](#left-join-e-right-join)
- [ ] [FULL OUTER JOIN (e por que o MySQL não tem)](#full-outer-join-e-por-que-o-mysql-não-tem)
- [ ] [UNION e UNION ALL](#union-e-union-all)
- [ ] [JOIN com três ou mais tabelas](#join-com-três-ou-mais-tabelas)
- [ ] [Self JOIN](#self-join)
- [ ] [GROUP BY e HAVING](#group-by-e-having)
- [ ] [Subconsultas (subqueries)](#subconsultas-subqueries)
- [ ] [Views: consultas salvas como se fossem tabelas](#views-consultas-salvas-como-se-fossem-tabelas)
- [ ] [DCL: controlando permissões de acesso](#dcl-controlando-permissões-de-acesso)
- [ ] [TCL: transações na prática](#tcl-transações-na-prática)
- [ ] [Índices em chaves estrangeiras](#índices-em-chaves-estrangeiras)
- [ ] [Erros comuns neste nível](#erros-comuns-neste-nível)
- [ ] [Glossário rápido](#glossário-rápido)
- [ ] [Recapitulando](#recapitulando)
- [ ] [O que vem no próximo nível](#o-que-vem-no-próximo-nível)

---

## Objetivo do nível

Ao final deste nível, o estudante deve conseguir:

- Identificar se um relacionamento entre duas entidades é 1:1, 1:N ou N:N, e modelar cada um corretamente em SQL.
- Aplicar 1FN, 2FN e 3FN a um modelo de dados e explicar, com exemplo, qual anomalia cada forma normal elimina.
- Escrever consultas com `INNER JOIN`, `LEFT JOIN`, múltiplas tabelas e `GROUP BY`/`HAVING`.
- Diferenciar `WHERE` de `HAVING` e explicar por que essa diferença existe.
- Escrever uma subconsulta simples e uma view.
- Controlar permissões de acesso com DCL e garantir consistência com transações (TCL), incluindo `ROLLBACK` diante de uma falha no meio do processo.

## Uso responsável de IA

Nesta fase, evite pedir para a IA desenhar o modelo de dados ou escrever a consulta com `JOIN` do zero. Modelagem é a parte mais difícil de errar e mais cara de corrigir depois — o raciocínio de "que tabelas eu preciso, e como elas se conectam" precisa ser seu. Use a IA para revisar um modelo que você já desenhou, ou para entender por que uma consulta sua não retornou o que esperava.

## Recapitulando o Nível 3 em uma frase

Uma tabela sozinha guarda um tipo de coisa (`clientes`). Uma chave estrangeira conecta um tipo de coisa a outro (`pedidos.cliente_id` aponta para `clientes.id`). Este nível generaliza essa ideia: relacionamentos entre entidades assumem formas diferentes, cada uma com uma forma diferente de ser modelada em tabelas.

## Cardinalidade: o vocabulário dos relacionamentos

**Cardinalidade** é o nome técnico para "quantos registros de um lado se relacionam com quantos registros do outro lado". Existem três formas possíveis entre duas entidades:

- **1:1 (um para um)**: um registro de A se relaciona com, no máximo, um registro de B, e vice-versa.
- **1:N (um para muitos)**: um registro de A se relaciona com vários registros de B, mas cada registro de B se relaciona com apenas um registro de A.
- **N:N (muitos para muitos)**: vários registros de A se relacionam com vários registros de B, sem essa restrição de "um lado só".

O restante desta seção detalha cada tipo, sempre partindo de um exemplo motivador antes da sintaxe — modelar errado a cardinalidade é o erro de modelagem mais comum entre quem está começando, e normalmente vem de pular direto para a tabela sem antes se perguntar "quantos de um lado, para quantos do outro?".

## Relacionamento 1:1

### O problema

Imagine que, além dos dados básicos do cliente (nome, email), seu e-commerce também precisa guardar dados sensíveis de verificação de identidade — CPF, documento de identidade, foto do documento — usados só em um fluxo específico de verificação antifraude, raramente consultados no dia a dia.

Você poderia colocar tudo em `clientes`:

```sql
CREATE TABLE clientes (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100) NOT NULL,
    email VARCHAR(150) NOT NULL UNIQUE,
    cpf VARCHAR(11),
    numero_documento VARCHAR(20),
    foto_documento_url VARCHAR(255)
);
```

Isso funciona, mas mistura dois assuntos diferentes na mesma tabela: dados de cadastro (consultados o tempo todo) e dados de verificação (sensíveis, raramente acessados, talvez com regras de acesso mais restritas). Quando um "pedaço" da entidade tem um ciclo de vida, frequência de acesso ou necessidade de segurança diferente do resto, faz sentido separar em duas tabelas conectadas 1:1.

### A modelagem

```sql
CREATE TABLE clientes (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100) NOT NULL,
    email VARCHAR(150) NOT NULL UNIQUE
);

CREATE TABLE verificacoes_identidade (
    id INT PRIMARY KEY AUTO_INCREMENT,
    cliente_id INT NOT NULL UNIQUE,
    cpf VARCHAR(11) NOT NULL,
    numero_documento VARCHAR(20) NOT NULL,
    foto_documento_url VARCHAR(255),
    FOREIGN KEY (cliente_id) REFERENCES clientes(id)
);
```

O detalhe que transforma isso em 1:1 (e não em 1:N) é o `UNIQUE` na coluna `cliente_id`: sem ele, nada impediria o mesmo cliente de ter duas linhas em `verificacoes_identidade`, o que tornaria a relação 1:N. Com `UNIQUE`, o banco garante que cada cliente tem, no máximo, uma verificação associada.

### Quando vale a pena separar

Na prática, 1:1 é o relacionamento menos comum dos três, porque, se as duas tabelas sempre são lidas juntas e não têm motivo de segurança ou de organização para estarem separadas, geralmente é mais simples deixar tudo em uma única tabela. Separe em 1:1 quando existir uma razão concreta: dados sensíveis com controle de acesso à parte, uma parte da entidade que é opcional para a maioria dos registros, ou uma parte que muda de forma e frequência muito diferente do restante.

## Relacionamento 1:N

Esse é o relacionamento mais comum em sistemas de negócio, e já apareceu informalmente no Nível 3: um cliente pode ter vários pedidos, mas cada pedido pertence a exatamente um cliente.

```text
[Clientes] 1 ---- N [Pedidos]
```

A regra de modelagem para 1:N é simples e sempre a mesma: **a chave estrangeira vive do lado "N"**, nunca do lado "1". Isso faz sentido porque é o pedido que só pode apontar para um cliente — o cliente, por outro lado, precisaria de várias colunas (ou de uma lista, que SQL não representa bem em uma única célula) para apontar para vários pedidos, o que não é como uma coluna funciona.

```sql
CREATE TABLE pedidos (
    id INT PRIMARY KEY AUTO_INCREMENT,
    cliente_id INT NOT NULL,
    data_pedido DATE NOT NULL,
    valor_total DECIMAL(10,2) NOT NULL,
    FOREIGN KEY (cliente_id) REFERENCES clientes(id)
);
```

Outros exemplos de 1:N dentro do domínio de e-commerce deste roadmap: uma categoria tem vários produtos, mas cada produto pertence a uma categoria; um pedido tem vários itens de pedido, mas cada item de pedido pertence a um único pedido.

```sql
CREATE TABLE categorias (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100) NOT NULL
);

CREATE TABLE produtos (
    id INT PRIMARY KEY AUTO_INCREMENT,
    categoria_id INT NOT NULL,
    nome VARCHAR(150) NOT NULL,
    preco DECIMAL(10,2) NOT NULL,
    FOREIGN KEY (categoria_id) REFERENCES categorias(id)
);
```

> **Pare e pense:** e se, no futuro, um produto precisasse pertencer a mais de uma categoria ao mesmo tempo (por exemplo, um tênis em "Calçados" e em "Promoções")? O modelo 1:N acima ainda dá conta disso?
>
> <details><summary>Ver resposta</summary>
>
> Não. `produtos.categoria_id` só guarda um valor por produto, então cada produto só pode pertencer a uma categoria nesse modelo. Um produto pertencendo a várias categorias, e uma categoria tendo vários produtos, é a definição de um relacionamento N:N — exatamente o assunto da próxima seção.
> </details>

## Relacionamento N:N

### Por que uma chave estrangeira direta não resolve

Pegue o exemplo clássico: alunos se matriculam em cursos. Um aluno pode fazer vários cursos ao mesmo tempo, e um curso tem vários alunos matriculados. Não existe um "lado 1" aqui — os dois lados são "muitos".

Tente modelar com uma FK direta em `alunos`:

```sql
CREATE TABLE alunos (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100) NOT NULL,
    curso_id INT,
    FOREIGN KEY (curso_id) REFERENCES cursos(id)
);
```

Isso só permite **um** curso por aluno — o oposto do que queremos. Colocar a FK do outro lado (`cursos.aluno_id`) tem o problema espelhado: só permitiria um aluno por curso.

### A solução: tabela associativa

A solução relacional para N:N é sempre a mesma: criar uma terceira tabela, cuja única função é registrar cada combinação existente entre as duas entidades, guardando uma chave estrangeira para cada lado.

```sql
CREATE TABLE alunos (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100) NOT NULL
);

CREATE TABLE cursos (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100) NOT NULL
);

CREATE TABLE matriculas (
    aluno_id INT NOT NULL,
    curso_id INT NOT NULL,
    data_matricula DATE NOT NULL,
    PRIMARY KEY (aluno_id, curso_id),
    FOREIGN KEY (aluno_id) REFERENCES alunos(id),
    FOREIGN KEY (curso_id) REFERENCES cursos(id)
);
```

Cada linha em `matriculas` representa "este aluno específico está matriculado neste curso específico". A chave primária composta `(aluno_id, curso_id)`, já vista no Nível 3, impede que o mesmo aluno seja matriculado duas vezes no mesmo curso, mas permite livremente que ele apareça em várias linhas com cursos diferentes, e que o mesmo curso apareça em várias linhas com alunos diferentes.

### A tabela associativa pode ter seus próprios atributos

Repare que `matriculas` tem uma coluna própria, `data_matricula`, que não pertence nem a `alunos` nem a `cursos` — pertence à **relação entre os dois**. Isso é comum: a tabela associativa não é só uma "ponte burra", ela pode guardar informação que só existe no contexto daquela combinação específica (nota final, status da matrícula, data de início).

No domínio do e-commerce, o mesmo padrão aparece na relação entre pedidos e produtos: um pedido tem vários produtos, e um produto pode aparecer em vários pedidos diferentes — outro N:N, resolvido com a tabela `itens_pedido`, que já carrega naturalmente a quantidade e o preço daquele item naquele pedido específico:

```sql
CREATE TABLE itens_pedido (
    pedido_id INT NOT NULL,
    produto_id INT NOT NULL,
    quantidade INT NOT NULL,
    preco_unitario DECIMAL(10,2) NOT NULL,
    PRIMARY KEY (pedido_id, produto_id),
    FOREIGN KEY (pedido_id) REFERENCES pedidos(id),
    FOREIGN KEY (produto_id) REFERENCES produtos(id)
);
```

`preco_unitario` está aqui, e não em `produtos`, de propósito: o preço do produto pode mudar amanhã, mas o valor cobrado naquele pedido específico, no passado, precisa continuar sendo o que foi cobrado na hora da compra. Copiar o preço para dentro do item de pedido, nesse caso, não é a anomalia de duplicação vista no Nível 3 — é uma decisão de modelagem correta, porque o preço no momento da compra é uma informação histórica que não deve mudar retroativamente.

## Notação de diagrama entidade-relacionamento

O Nível 3 introduziu o DER de forma textual simples (`[Cliente] 1 ---- N [Pedido]`). Para descrever os três tipos de relacionamento com mais precisão, a notação mais usada no mercado é a **"pé de galinha"** (crow's foot), representando cardinalidade nas pontas da linha:

```text
[Clientes] ||------o{ [Pedidos]
```

- `||` de um lado: exatamente um.
- `o{` do outro lado: zero ou muitos.

Lendo a linha completa: "cada pedido pertence a exatamente um cliente; cada cliente tem zero ou muitos pedidos." A tabela abaixo resume os símbolos mais comuns:

| Símbolo | Significado |
|---|---|
| `││` | Exatamente um (obrigatório) |
| `o│` | Zero ou um (opcional) |
| `│{` | Um ou muitos |
| `o{` | Zero ou muitos |

Ferramentas como MySQL Workbench (aba "Database" → "Reverse Engineer") e sites como dbdiagram.io geram esse tipo de diagrama automaticamente a partir do SQL, ou permitem desenhar visualmente e gerar o SQL a partir do diagrama — vale explorar uma dessas ferramentas ao modelar o projeto final do [Nível 9](./NIVEL_9_PROJETO_FINAL.md).

## Normalização: revisando o problema

O Nível 3 mostrou, informalmente, o que acontece quando dados são duplicados em uma única tabela: anomalias de atualização, inserção e remoção. **Normalização** é o processo formal de aplicar um conjunto de regras (as "formas normais") para eliminar essas anomalias, uma de cada vez, cada regra construída sobre a anterior.

## Primeira Forma Normal (1FN)

**Regra**: cada coluna deve guardar um valor atômico — indivisível para o contexto do sistema — e não pode haver grupos de colunas repetidos para representar uma lista de valores.

Viola a 1FN:

```text
clientes
id | nome | telefones
1  | Ana  | 11999998888, 11888887777
```

A coluna `telefones` guarda uma lista dentro de um único campo de texto. Isso quebra várias coisas: não dá para buscar "todos os clientes com o telefone X" com um `WHERE` simples, não dá para garantir que cada telefone seja válido individualmente, e não dá para saber quantos telefones um cliente tem sem processar a string manualmente.

Também viola a 1FN um design como este, com colunas repetidas para simular uma lista:

```text
clientes
id | nome | telefone1     | telefone2
1  | Ana  | 11999998888   | 11888887777
```

Aqui o problema é outro: e se a Ana tiver um terceiro telefone? A estrutura da tabela precisaria mudar (`ALTER TABLE` para adicionar `telefone3`). Um schema que precisa mudar toda vez que a quantidade de "coisas" de um registro varia é sinal de que falta uma tabela.

**Solução, em 1FN**: extrair a lista para uma tabela própria, em relacionamento 1:N com `clientes`:

```sql
CREATE TABLE telefones_cliente (
    id INT PRIMARY KEY AUTO_INCREMENT,
    cliente_id INT NOT NULL,
    numero VARCHAR(20) NOT NULL,
    FOREIGN KEY (cliente_id) REFERENCES clientes(id)
);
```

Agora cada telefone é uma linha própria, buscável, validável, e a Ana pode ter quantos telefones precisar sem alterar a estrutura de nenhuma tabela.

## Segunda Forma Normal (2FN)

**Regra**: a tabela precisa estar na 1FN, e todo atributo que não faz parte da chave precisa depender da **chave inteira**, não de apenas uma parte dela.

Essa regra só faz sentido — e só é possível violar — quando a chave primária é **composta** (mais de uma coluna). Pegue uma tabela de itens de pedido mal modelada:

```text
itens_pedido
pedido_id | produto_id | quantidade | nome_produto | categoria_produto
```

Chave primária composta: `(pedido_id, produto_id)`. Só que `nome_produto` e `categoria_produto` não dependem do par completo — eles dependem só de `produto_id`. Isso é uma **dependência parcial**, e ela recria o mesmo tipo de anomalia de duplicação vista no Nível 3: o nome do produto fica repetido em cada linha de `itens_pedido` que o referencia, e atualizar o nome de um produto exigiria atualizar todas as linhas.

**Solução, em 2FN**: mover `nome_produto` e `categoria_produto` para a tabela `produtos`, onde eles realmente pertencem (dependem da chave inteira daquela tabela, `produto_id`), e deixar em `itens_pedido` apenas o que de fato depende do par `(pedido_id, produto_id)`, como `quantidade` e `preco_unitario` (o preço no momento da compra, como já discutido).

```text
itens_pedido
pedido_id | produto_id | quantidade | preco_unitario

produtos
id | nome | categoria_id
```

## Terceira Forma Normal (3FN)

**Regra**: a tabela precisa estar na 2FN, e nenhum atributo que não seja chave pode depender de outro atributo que também não seja chave — só pode depender da chave primária diretamente.

Exemplo com dependência transitiva:

```text
pedidos
id | cliente_id | cliente_nome | cliente_cidade
```

Aqui, `cliente_nome` e `cliente_cidade` não dependem de `id` (a chave de `pedidos`) diretamente — eles dependem de `cliente_id`, que por sua vez é que identifica o cliente. Ou seja: `id → cliente_id → cliente_nome`. Essa cadeia de dependência (chave → atributo não-chave → outro atributo não-chave) é a dependência transitiva que a 3FN proíbe.

**Solução, em 3FN**: `cliente_nome` e `cliente_cidade` pertencem à tabela `clientes`, não a `pedidos`. `pedidos` deve guardar apenas `cliente_id`, e buscar nome e cidade através dele quando precisar (usando `JOIN`, próximo assunto deste nível).

### Regra prática para lembrar as três formas normais juntas

Uma forma resumida e informal, mas útil no dia a dia, de lembrar as três formas normais:

> Cada atributo deve depender da chave, da chave inteira, e de nada além da chave.

- "Da chave": 1FN — valores atômicos, sem repetição estrutural.
- "Da chave inteira": 2FN — sem dependência parcial em chaves compostas.
- "Nada além da chave": 3FN — sem dependência transitiva através de outro atributo não-chave.

## Além da 3FN, rapidamente

Existem formas normais mais rigorosas — Forma Normal de Boyce-Codd (BCNF), 4FN, 5FN — que tratam casos mais específicos de dependência entre atributos. Na prática do dia a dia de um sistema de negócio comum, chegar até a 3FN já elimina a grande maioria das anomalias reais. As formas além da 3FN valem a pena estudar quando você já estiver confortável modelando e sentir, na prática, um cenário que a 3FN não resolveu — não é necessário decorá-las agora.

## Exemplo completo: normalizando do zero

As três seções anteriores mostraram cada forma normal isoladamente. Vale ver as três aplicadas em sequência, sobre a mesma tabela, do jeito que aconteceria na prática.

Ponto de partida — uma única tabela, sem nenhuma normalização, tentando guardar um pedido inteiro:

```text
pedidos_bruto
pedido_id | cliente_nome | cliente_email | produto_id | produto_nome | produto_categoria | quantidade | telefones_cliente
1         | Ana          | ana@email.com | 10         | Mouse        | Periféricos        | 2          | 11999998888, 11888887777
```

**Aplicando 1FN**: `telefones_cliente` guarda uma lista em uma célula só. Extrai-se para uma tabela própria, `telefones_cliente(id, cliente_referencia, numero)`, relacionada 1:N ao cliente. A tabela principal perde essa coluna.

**Aplicando 2FN**: a chave natural dessa tabela seria `(pedido_id, produto_id)`. Mas `cliente_nome`, `cliente_email`, `produto_nome` e `produto_categoria` não dependem do par inteiro — `cliente_nome`/`cliente_email` dependem só do cliente do pedido, e `produto_nome`/`produto_categoria` dependem só do produto. São dependências parciais. Extraem-se `clientes(id, nome, email)` e `produtos(id, nome, categoria_id)` como tabelas próprias.

**Aplicando 3FN**: dentro da nova tabela `produtos`, `categoria_produto` (o texto "Periféricos") depende de `categoria_id`, não diretamente da chave `produtos.id` — outra dependência transitiva. Extrai-se `categorias(id, nome)`, e `produtos` passa a guardar só `categoria_id`.

Resultado final, normalizado até a 3FN:

```text
clientes(id, nome, email)
telefones_cliente(id, cliente_id, numero)
categorias(id, nome)
produtos(id, nome, categoria_id)
pedidos(id, cliente_id, data_pedido)
itens_pedido(pedido_id, produto_id, quantidade, preco_unitario)
```

Seis tabelas pequenas e coesas, no lugar de uma tabela larga cheia de duplicação. Qualquer informação completa (por exemplo, "nome do cliente, produto e categoria de cada item de um pedido") volta a ficar disponível através de `JOIN`, assunto retomado logo a seguir.

## Desnormalização: quebrando a regra de propósito

Normalização otimiza para **integridade** (dados corretos, sem duplicação) — mas isso tem um custo em **performance de leitura**: para reconstruir uma informação completa (por exemplo, "nome do cliente e valor do pedido"), é preciso combinar tabelas com `JOIN`, o que tem custo computacional maior do que ler uma única tabela.

**Desnormalização** é a decisão consciente de reintroduzir alguma duplicação controlada, de propósito, para acelerar leituras que acontecem com muita frequência em sistemas de grande escala — normalmente em relatórios ou painéis onde o dado não muda com frequência e a velocidade de leitura importa mais do que economizar espaço.

Isso é uma técnica avançada de otimização, não um ponto de partida. A recomendação para todo o restante deste roadmap, incluindo o projeto final, é modelar sempre normalizado até a 3FN por padrão, e considerar desnormalizar apenas depois de identificar um problema real de performance — nunca antes, "por precaução".

## O problema que o JOIN resolve

Depois de normalizar, a informação que antes vivia em uma única linha agora está espalhada em várias tabelas. "Nome do cliente e valor do pedido" exige ler `clientes` e `pedidos` ao mesmo tempo, casando as linhas certas de uma com as linhas certas da outra através da chave estrangeira.

Fazer isso manualmente, linha por linha, dentro do código Java, seria exatamente o tipo de trabalho lento e sujeito a erro que o SQL existe para evitar (o mesmo raciocínio da seção sobre arquivos de texto no Nível 3). `JOIN` é o comando SQL que faz esse cruzamento diretamente no banco, de forma otimizada.

## INNER JOIN

```sql
SELECT clientes.nome, pedidos.valor_total
FROM clientes
INNER JOIN pedidos ON pedidos.cliente_id = clientes.id;
```

`INNER JOIN` retorna apenas as linhas onde existe correspondência **nas duas tabelas**. Se um cliente não tiver nenhum pedido, ele não aparece no resultado — não existe linha de `pedidos` para casar com ele.

```text
clientes                    pedidos
id | nome                   id | cliente_id | valor_total
1  | Ana                    1  | 1          | 250.00
2  | Bruno                  2  | 1          | 89.90
3  | Carla (sem pedidos)

Resultado do INNER JOIN:
nome | valor_total
Ana  | 250.00
Ana  | 89.90
```

Carla desaparece do resultado porque não tem nenhum pedido — é assim que `INNER JOIN` funciona por definição, não é um bug.

### Sintaxe com apelidos (alias)

Em consultas mais longas, é comum abreviar o nome das tabelas com um alias, para deixar a consulta mais legível:

```sql
SELECT c.nome, p.valor_total
FROM clientes AS c
INNER JOIN pedidos AS p ON p.cliente_id = c.id;
```

`AS` é opcional na prática (`clientes c` funciona igual a `clientes AS c`), mas escrever por extenso deixa a intenção mais clara para quem lê depois.

## LEFT JOIN e RIGHT JOIN

E se você quiser justamente o caso que o `INNER JOIN` exclui — "todos os clientes, incluindo os que não têm nenhum pedido ainda"? Esse é o problema que `LEFT JOIN` resolve.

```sql
SELECT c.nome, p.valor_total
FROM clientes AS c
LEFT JOIN pedidos AS p ON p.cliente_id = c.id;
```

```text
Resultado do LEFT JOIN:
nome  | valor_total
Ana   | 250.00
Ana   | 89.90
Bruno | 120.00
Carla | NULL
```

`LEFT JOIN` traz **todas** as linhas da tabela à esquerda (`clientes`, a primeira mencionada na consulta), e preenche com `NULL` as colunas da tabela à direita quando não existe correspondência. A Carla aparece, com `valor_total` como `NULL` — lembrando a seção sobre [NULL](./NIVEL_3_BANCO_DE_DADOS.md#o-valor-null) do Nível 3, essa é exatamente a situação para a qual `NULL` existe: ausência real de um pedido, não um erro.

`RIGHT JOIN` é o espelho: traz todas as linhas da tabela à direita, preenchendo com `NULL` o que não existir na esquerda. Na prática, `RIGHT JOIN` é usado raramente, porque qualquer `RIGHT JOIN` pode ser reescrito como um `LEFT JOIN` invertendo a ordem das tabelas — a maioria dos times prefere manter só um estilo (`LEFT JOIN`) por consistência.

### Quando usar cada um

| Pergunta que você quer responder | JOIN a usar |
|---|---|
| "Clientes e o valor de cada pedido deles" | `INNER JOIN` |
| "Todos os clientes, mesmo os sem pedido" | `LEFT JOIN` |
| "Clientes que nunca fizeram pedido" | `LEFT JOIN` + `WHERE p.id IS NULL` |

```sql
SELECT c.nome
FROM clientes AS c
LEFT JOIN pedidos AS p ON p.cliente_id = c.id
WHERE p.id IS NULL;
```

Esse último padrão — `LEFT JOIN` seguido de `WHERE ... IS NULL` na tabela da direita — é uma forma comum de responder "o que existe de um lado e não existe do outro", útil sempre que a pergunta de negócio é sobre uma ausência.

## FULL OUTER JOIN (e por que o MySQL não tem)

Em teoria, existe um quarto tipo: `FULL OUTER JOIN`, que traria todas as linhas de ambos os lados, casadas onde há correspondência e preenchidas com `NULL` onde não há — a união do que `LEFT JOIN` e `RIGHT JOIN` trazem cada um separadamente.

O MySQL, diferente de PostgreSQL e SQL Server, **não implementa `FULL OUTER JOIN` diretamente**. É importante saber disso para não perder tempo procurando um erro de sintaxe onde na verdade falta o recurso. A forma de emular o mesmo resultado no MySQL é combinar um `LEFT JOIN` com um `RIGHT JOIN` (ou dois `LEFT JOIN` invertidos) usando `UNION`:

```sql
SELECT c.nome, p.valor_total
FROM clientes AS c
LEFT JOIN pedidos AS p ON p.cliente_id = c.id

UNION

SELECT c.nome, p.valor_total
FROM clientes AS c
RIGHT JOIN pedidos AS p ON p.cliente_id = c.id;
```

Isso é um caso de uso relativamente raro no dia a dia (a maior parte das perguntas de negócio se resolve com `INNER JOIN` ou `LEFT JOIN`), mas vale reconhecer a limitação específica do MySQL, já que ela aparece de vez em quando ao migrar consultas escritas originalmente para outro SGBD.

## UNION e UNION ALL

O exemplo anterior já usou `UNION` de passagem: ele combina o resultado de dois `SELECT` diferentes em um único conjunto de linhas, desde que as duas consultas tenham o mesmo número de colunas, em tipos compatíveis.

```sql
SELECT nome, 'cliente' AS origem FROM clientes
UNION
SELECT nome, 'fornecedor' AS origem FROM fornecedores;
```

- **`UNION`**: combina os resultados e remove linhas duplicadas automaticamente (compara todas as colunas de cada linha).
- **`UNION ALL`**: combina os resultados sem remover duplicatas — e, por não precisar comparar e filtrar, é mais rápido.

Use `UNION ALL` sempre que souber que não pode haver duplicatas reais entre as duas consultas (como no exemplo do `FULL OUTER JOIN` emulado, onde remover a etapa de deduplicação pode até ser desejável dependendo do caso), e reserve `UNION` para quando a remoção de duplicatas for parte do que você está pedindo.

## JOIN com três ou mais tabelas

Nada impede encadear vários `JOIN` na mesma consulta. Para listar cada item vendido, com o nome do produto e o nome do cliente que comprou:

```sql
SELECT c.nome AS cliente, pr.nome AS produto, ip.quantidade, ip.preco_unitario
FROM pedidos AS pe
INNER JOIN clientes AS c ON c.id = pe.cliente_id
INNER JOIN itens_pedido AS ip ON ip.pedido_id = pe.id
INNER JOIN produtos AS pr ON pr.id = ip.produto_id;
```

Cada `JOIN` adicional casa mais uma tabela ao resultado que já vinha se formando. Não existe limite prático de quantos `JOIN`s uma consulta pode ter, mas consultas muito longas, com muitas tabelas, costumam ser mais fáceis de entender quebradas em [views](#views-consultas-salvas-como-se-fossem-tabelas) nomeadas.

## Self JOIN

Às vezes uma tabela precisa se relacionar com ela mesma. O exemplo clássico é uma hierarquia de funcionários, onde cada funcionário tem um gerente que também é um funcionário:

```sql
CREATE TABLE funcionarios (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100) NOT NULL,
    gerente_id INT,
    FOREIGN KEY (gerente_id) REFERENCES funcionarios(id)
);
```

Para listar cada funcionário junto do nome do gerente dele, a tabela precisa ser combinada com ela mesma, usando dois apelidos diferentes:

```sql
SELECT f.nome AS funcionario, g.nome AS gerente
FROM funcionarios AS f
LEFT JOIN funcionarios AS g ON f.gerente_id = g.id;
```

`LEFT JOIN` aqui, e não `INNER JOIN`, porque a pessoa no topo da hierarquia não tem gerente — `gerente_id` dela é `NULL`, e um `INNER JOIN` a excluiria do resultado.

> **Pare e pense:** por que essa consulta precisa de dois apelidos (`f` e `g`) para a mesma tabela `funcionarios`, em vez de conseguir usar só `funcionarios.nome` nas duas partes do `SELECT`?
>
> <details><summary>Ver resposta</summary>
>
> Porque o MySQL precisa distinguir "a linha do funcionário" da "linha do gerente" dentro da mesma consulta, mesmo as duas vindo fisicamente da mesma tabela. Sem dois apelidos diferentes, `funcionarios.nome` seria ambíguo — o banco não teria como saber se você quer o nome vindo da instância "funcionário" ou da instância "gerente" daquele `JOIN`. Cada apelido funciona como se fosse uma cópia lógica separada da tabela, só para efeito daquela consulta.
> </details>

## GROUP BY e HAVING

O Nível 3 mostrou funções de agregação (`COUNT`, `SUM`, `AVG`...) aplicadas à tabela inteira. `GROUP BY` permite aplicar essas mesmas funções **por grupo**, respondendo perguntas como "quanto cada cliente gastou no total?":

```sql
SELECT c.nome, SUM(p.valor_total) AS total_gasto
FROM clientes AS c
INNER JOIN pedidos AS p ON p.cliente_id = c.id
GROUP BY c.id, c.nome;
```

Isso agrupa todas as linhas do resultado por cliente, e calcula `SUM(p.valor_total)` dentro de cada grupo, separadamente. Uma regra importante: toda coluna no `SELECT` que não está dentro de uma função de agregação precisa aparecer também no `GROUP BY` — o MySQL até tolera algumas exceções nisso, mas contar com esse comportamento tolerante é fonte comum de resultado inesperado.

### Filtrando grupos com HAVING

`WHERE` filtra linhas **antes** de agrupar. `HAVING` filtra grupos **depois** de agrupar, com base no resultado da agregação — e é exatamente por isso que `WHERE` não pode ser usado para filtrar por uma função de agregação:

```sql
-- ERRADO: WHERE não pode referenciar SUM diretamente
SELECT c.nome, SUM(p.valor_total) AS total_gasto
FROM clientes AS c
INNER JOIN pedidos AS p ON p.cliente_id = c.id
WHERE SUM(p.valor_total) > 200
GROUP BY c.id, c.nome;

-- CORRETO: HAVING filtra depois do agrupamento
SELECT c.nome, SUM(p.valor_total) AS total_gasto
FROM clientes AS c
INNER JOIN pedidos AS p ON p.cliente_id = c.id
GROUP BY c.id, c.nome
HAVING SUM(p.valor_total) > 200;
```

A ordem lógica de execução de uma consulta com todas essas cláusulas ajuda a fixar por que isso acontece:

```text
FROM/JOIN → WHERE → GROUP BY → HAVING → SELECT → ORDER BY → LIMIT
```

`WHERE` roda antes de qualquer agrupamento existir, então ele simplesmente não tem acesso a um total que ainda não foi calculado. `HAVING` roda depois, quando os grupos e suas agregações já existem.

> **Pare e pense:** "listar só os clientes com pedido acima de R$200" e "listar clientes cujo total de pedidos passa de R$200" parecem parecidas, mas usam `WHERE` e `HAVING` de formas diferentes. Qual é qual?
>
> <details><summary>Ver resposta</summary>
>
> "Pedido acima de R$200" filtra linhas individuais de pedido, antes de qualquer agrupamento — isso é `WHERE p.valor_total > 200`, sem `GROUP BY` nenhum envolvido na condição. "Total de pedidos passa de R$200" depende de somar vários pedidos de um mesmo cliente primeiro — isso exige `GROUP BY` seguido de `HAVING SUM(p.valor_total) > 200`. A palavra-chave que denuncia a diferença é "total": sempre que a condição depende de uma agregação (soma, contagem, média), é `HAVING`; quando depende só de uma coluna da própria linha, é `WHERE`.
> </details>

## Subconsultas (subqueries)

Uma subconsulta é um `SELECT` dentro de outro comando SQL. Útil quando a resposta de uma pergunta depende do resultado de outra pergunta.

```sql
SELECT nome
FROM clientes
WHERE id IN (
    SELECT cliente_id
    FROM pedidos
    WHERE valor_total > 200
);
```

A subconsulta interna (`SELECT cliente_id FROM pedidos WHERE valor_total > 200`) roda primeiro, produzindo uma lista de ids. A consulta externa então busca os clientes cujo `id` está nessa lista. Isso poderia, nesse caso específico, também ser escrito com `JOIN` — vale como exercício mental comparar as duas formas:

```sql
SELECT DISTINCT c.nome
FROM clientes AS c
INNER JOIN pedidos AS p ON p.cliente_id = c.id
WHERE p.valor_total > 200;
```

Não existe uma regra rígida de "sempre use subconsulta" ou "sempre use `JOIN`" — em geral, `JOIN` tende a ter melhor performance quando você também precisa trazer colunas da outra tabela no resultado, e subconsulta tende a deixar a intenção mais legível quando você só precisa filtrar por uma condição de outra tabela, sem trazer dados dela.

## Views: consultas salvas como se fossem tabelas

Uma **view** é uma consulta salva no banco com um nome, que pode ser consultada como se fosse uma tabela comum, sem duplicar os dados de verdade — toda vez que a view é consultada, o SQL por trás dela roda de novo.

```sql
CREATE VIEW resumo_pedidos_cliente AS
SELECT c.id AS cliente_id, c.nome, SUM(p.valor_total) AS total_gasto
FROM clientes AS c
INNER JOIN pedidos AS p ON p.cliente_id = c.id
GROUP BY c.id, c.nome;

SELECT * FROM resumo_pedidos_cliente WHERE total_gasto > 200;
```

Views são úteis para esconder a complexidade de uma consulta longa com vários `JOIN`s atrás de um nome simples e reutilizável, e para dar acesso restrito a uma "fatia" dos dados sem expor a tabela inteira (por exemplo, uma view que já exclui colunas sensíveis). Elas não substituem a modelagem correta das tabelas reais — são uma camada de conveniência por cima dela.

## DCL: controlando permissões de acesso

Até aqui, todo exemplo assumiu o usuário `root`, com acesso total ao banco. Em um ambiente real, isso é evitado: cada aplicação e cada pessoa deve ter apenas o acesso estritamente necessário para o que precisa fazer — o **princípio do menor privilégio**. DCL (Data Control Language) é o conjunto de comandos que controla isso.

```sql
CREATE USER 'app_ecommerce'@'localhost' IDENTIFIED BY 'senha_forte_aqui';

GRANT SELECT, INSERT, UPDATE ON estudos_java.* TO 'app_ecommerce'@'localhost';

REVOKE UPDATE ON estudos_java.* FROM 'app_ecommerce'@'localhost';
```

- `CREATE USER`: cria uma credencial de acesso separada da do administrador.
- `GRANT`: concede privilégios específicos (`SELECT`, `INSERT`, `UPDATE`, `DELETE`, entre outros) sobre um banco (`estudos_java.*` significa "todas as tabelas desse banco").
- `REVOKE`: remove um privilégio concedido anteriormente.

Por que isso importa na prática: se a aplicação web só precisa ler e escrever pedidos, não faz sentido ela se conectar ao banco com um usuário que também pode `DROP TABLE`. Se essa aplicação for comprometida por alguma vulnerabilidade (como o SQL Injection visto no Nível 3), o estrago possível fica limitado ao que aquele usuário específico tem permissão de fazer — outra camada de defesa, além do `PreparedStatement`, não um substituto dele.

## TCL: transações na prática

O Nível 3 introduziu transações de forma conceitual, ligada às garantias ACID. Aqui está a sintaxe completa, incluindo o caminho de falha:

```sql
START TRANSACTION;

UPDATE contas SET saldo = saldo - 100 WHERE id = 1;
UPDATE contas SET saldo = saldo + 100 WHERE id = 2;

COMMIT;
```

Se, no meio da transação, algo indicar que ela não deve continuar — uma regra de negócio quebrada, um erro inesperado — `ROLLBACK` desfaz tudo desde o `START TRANSACTION`:

```sql
START TRANSACTION;

UPDATE contas SET saldo = saldo - 100 WHERE id = 1;

-- suponha que aqui a aplicação detecta que a conta de origem ficaria negativa
ROLLBACK;
```

Depois do `ROLLBACK`, é como se o primeiro `UPDATE` nunca tivesse acontecido — o saldo da conta 1 volta ao valor de antes da transação começar.

### SAVEPOINT: desfazendo só uma parte

Para transações mais longas, é possível marcar um ponto intermediário e voltar só até ali, sem descartar tudo:

```sql
START TRANSACTION;

UPDATE contas SET saldo = saldo - 100 WHERE id = 1;
SAVEPOINT antes_do_credito;

UPDATE contas SET saldo = saldo + 100 WHERE id = 2;

-- algo dá errado só na parte seguinte
ROLLBACK TO antes_do_credito;

COMMIT;
```

### Uma nota sobre concorrência

Quando duas transações rodam ao mesmo tempo sobre os mesmos dados, o SGBD precisa decidir o quanto uma pode enxergar da outra antes de qualquer `COMMIT` — isso é controlado pelo **nível de isolamento** da transação (`READ COMMITTED`, `REPEATABLE READ`, entre outros), que é o "I" de ACID sendo configurável. O padrão do MySQL (`REPEATABLE READ`) é adequado para a grande maioria dos casos deste roadmap; ajustar isso manualmente é um assunto mais avançado, que vale revisitar quando um cenário real de concorrência aparecer.

## Índices em chaves estrangeiras

O Nível 3 explicou [índices](./NIVEL_3_BANCO_DE_DADOS.md#índices) no contexto de uma tabela só. Com `JOIN` em cena, essa decisão fica mais importante: toda vez que o MySQL casa linhas de duas tabelas através de uma chave estrangeira, ele precisa localizar rapidamente, do lado referenciado, a linha correspondente — e é exatamente esse tipo de busca que um índice acelera.

No MySQL, com o motor de armazenamento padrão (InnoDB), colunas de chave estrangeira já ganham um índice automaticamente ao serem declaradas com `FOREIGN KEY`, então você normalmente não precisa criar esse índice manualmente. Vale saber que essa automação existe, e por que ela existe: sem índice na coluna do lado "N" de um relacionamento 1:N, um `JOIN` sobre uma tabela grande recairia no mesmo problema de table scan descrito no Nível 3, para cada linha da tabela do lado "1".

## Erros comuns neste nível

**`Error Code: 1052. Column '...' in field list is ambiguous`**
Você usou o nome de uma coluna que existe em mais de uma das tabelas do `JOIN` (como `id` ou `nome`) sem prefixar com o alias da tabela. Solução: sempre qualificar (`c.id`, `p.id`) quando há `JOIN` envolvido.

**`Error Code: 1055. Expression not in GROUP BY`**
Uma coluna no `SELECT` não está dentro de uma função de agregação nem está listada no `GROUP BY`, e o MySQL está configurado em modo estrito (`ONLY_FULL_GROUP_BY`). Solução: adicionar a coluna ao `GROUP BY`, ou envolvê-la em uma função de agregação como `MIN()`/`MAX()` se ela for redundante dentro do grupo.

**Resultado vazio inesperado com `INNER JOIN`**
Normalmente significa que a condição de `ON` está errada (comparando colunas que não deveriam se corresponder), ou que de fato não existe correspondência entre as tabelas para aquele filtro — nesse segundo caso, um `LEFT JOIN` ajuda a diagnosticar, porque ele mostra o lado que está "sobrando" com `NULL`.

**`Error Code: 1215. Cannot add foreign key constraint`**
Geralmente acontece quando o tipo da coluna da chave estrangeira não é exatamente igual ao tipo da coluna referenciada (por exemplo, `INT` de um lado e `BIGINT` do outro), ou quando as tabelas usam motores de armazenamento incompatíveis. Confira se os dois tipos batem exatamente.

## Glossário rápido

| Termo | Definição curta |
|---|---|
| Cardinalidade | Quantidade de registros de um lado que se relacionam com quantos do outro lado |
| Tabela associativa | Tabela criada para representar um relacionamento N:N |
| Dependência funcional | Um atributo determina o valor de outro |
| Dependência parcial | Um atributo depende só de parte de uma chave composta (viola 2FN) |
| Dependência transitiva | Um atributo depende de outro atributo não-chave, não diretamente da chave (viola 3FN) |
| Desnormalização | Reintrodução controlada de duplicação, por performance |
| `INNER JOIN` | Retorna só as linhas com correspondência nas duas tabelas |
| `LEFT JOIN` | Retorna todas as linhas da tabela à esquerda, com `NULL` onde não há correspondência |
| Self JOIN | Uma tabela combinada com ela mesma |
| `GROUP BY` | Agrupa linhas para aplicar funções de agregação por grupo |
| `HAVING` | Filtra grupos depois da agregação, ao contrário do `WHERE` |
| Subconsulta | Um `SELECT` usado dentro de outro comando SQL |
| View | Consulta salva, consultável como se fosse uma tabela |
| DCL | Comandos de controle de permissão (`GRANT`, `REVOKE`) |
| TCL | Comandos de controle de transação (`START TRANSACTION`, `COMMIT`, `ROLLBACK`) |
| Princípio do menor privilégio | Conceder só o acesso estritamente necessário para cada usuário/aplicação |
| `FULL OUTER JOIN` | Une o que `LEFT` e `RIGHT JOIN` trazem separadamente; não existe nativamente no MySQL |
| `UNION` / `UNION ALL` | Combinam o resultado de dois `SELECT`; `UNION` remove duplicatas, `UNION ALL` não |
| `SAVEPOINT` | Ponto intermediário dentro de uma transação, para desfazer só uma parte com `ROLLBACK TO` |
| Nível de isolamento | Controla o quanto uma transação em andamento enxerga de outra transação simultânea |

## Recapitulando

- [ ] Explicar a diferença entre relacionamento 1:1, 1:N e N:N, com um exemplo próprio de cada.
- [ ] Explicar por que a chave estrangeira sempre vive do lado "N" em um relacionamento 1:N.
- [ ] Explicar por que N:N exige uma tabela associativa, e por que ela pode ter atributos próprios.
- [ ] Aplicar 1FN, 2FN e 3FN a um modelo de dados dado, identificando a anomalia que cada uma resolve.
- [ ] Explicar a diferença entre `INNER JOIN` e `LEFT JOIN` com um exemplo de quando cada um traz um resultado diferente.
- [ ] Explicar por que `WHERE` não pode filtrar pelo resultado de uma função de agregação, e `HAVING` pode.
- [ ] Explicar o que uma transação garante, e o que `ROLLBACK` desfaz.

## O que vem no próximo nível

Você agora consegue modelar um domínio inteiro com várias tabelas relacionadas e consultar essas tabelas de forma rica. No [Nível 5 - Testes Automatizados e Git](./NIVEL_5_TESTES_E_GIT.md), o foco muda de banco de dados para qualidade e processo de trabalho: como garantir, de forma automatizada, que o código que acessa esse banco continua correto conforme o sistema cresce, e como colaborar em equipe usando Git de forma profissional.
