# Nível 3 - Dados, Qualidade, APIs e Mercado

Neste nível, o estudante começa a sair do console e a pensar como desenvolvedor de sistemas reais. O foco é banco de dados, modelagem, SQL, testes, Git, JAX-RS, Spring Boot, APIs REST, Design Patterns e portfólio.

A ideia aqui não é decorar frameworks. Antes de usar Spring Boot, o estudante precisa entender os fundamentos que o framework automatiza.

---

## Índice de progresso

- [ ] [Objetivo do nível](#objetivo-do-nível)
- [ ] [Uso responsável de IA](#uso-responsável-de-ia)
- [ ] [O que é um banco de dados](#o-que-é-um-banco-de-dados)
- [ ] [SGBD](#sgbd)
- [ ] [Banco relacional](#banco-relacional)
- [ ] [Tabelas, colunas, linhas e chaves](#tabelas-colunas-linhas-e-chaves)
- [ ] [Relacionamentos](#relacionamentos)
- [ ] [Normalização](#normalização)
- [ ] [SQL](#sql)
- [ ] [MySQL: instalação e primeiros passos](#mysql-instalação-e-primeiros-passos)
- [ ] [DDL, DML, DQL, DCL e TCL](#ddl-dml-dql-dcl-e-tcl)
- [ ] [Consultas com JOIN](#consultas-com-join)
- [ ] [Testes automatizados](#testes-automatizados)
- [ ] [Git e GitHub](#git-e-github)
- [ ] [JAX-RS antes do Spring Boot](#jax-rs-antes-do-spring-boot)
- [ ] [HTTP, recursos e endpoints](#http-recursos-e-endpoints)
- [ ] [Spring Boot](#spring-boot)
- [ ] [Java Efetivo](#java-efetivo)
- [ ] [Design Patterns](#design-patterns)
- [ ] [Todos os padrões GoF](#todos-os-padrões-gof)
- [ ] [Projeto final e portfólio](#projeto-final-e-portfólio)

---

## Objetivo do nível

Ao final deste nível, o estudante deve conseguir:

- Explicar banco de dados, SGBD, tabela, coluna, linha, chave primária e chave estrangeira.
- Entender relações 1:1, 1:N e N:N.
- Criar tabelas normalizadas em MySQL.
- Escrever consultas SQL com `SELECT`, `WHERE`, `JOIN`, `GROUP BY` e `ORDER BY`.
- Entender HTTP, REST, recurso, endpoint, método e status code.
- Criar um recurso simples com JAX-RS.
- Entender o que Spring Boot automatiza.
- Separar Controller, Service, Repository, DTO e Entity.
- Reconhecer e aplicar os 23 padrões GoF de forma didática.

---

## Uso responsável de IA

Nesta fase, a IA pode ajudar a revisar modelagem, explicar erros e comparar soluções. Ainda assim, evite pedir para ela gerar scripts, endpoints ou implementações completas antes de tentar.

O objetivo é memorizar padrões profissionais:

- Como criar tabelas.
- Como escrever `JOIN`.
- Como organizar camadas.
- Como estruturar um endpoint.
- Como reconhecer quando um pattern resolve um problema real.

---

# Banco de Dados

## O que é um banco de dados

Banco de dados é uma forma estruturada de armazenar informações para consulta e manutenção posterior.

Exemplo real:

Um e-commerce precisa guardar:

- clientes;
- produtos;
- categorias;
- pedidos;
- itens do pedido;
- pagamentos;
- endereços.

Sem banco de dados, as informações poderiam desaparecer quando o programa fosse fechado.

---

## SGBD

SGBD significa **Sistema Gerenciador de Banco de Dados**.

Ele é o software responsável por:

- criar bancos;
- criar tabelas;
- guardar dados;
- consultar dados;
- controlar permissões;
- garantir integridade;
- lidar com transações;
- controlar acesso simultâneo.

Exemplos de SGBD:

- MySQL;
- PostgreSQL;
- SQL Server;
- Oracle Database;
- MariaDB;
- SQLite.

Neste roadmap, usaremos **MySQL**, por ser popular, gratuito na edição Community e muito comum em estudos iniciais.

---

## Banco relacional

Banco relacional organiza dados em tabelas relacionadas.

A ideia central é evitar dados duplicados e representar relações entre conceitos.

Exemplo:

Em vez de guardar o nome do cliente repetido em todos os pedidos, criamos uma tabela `clientes` e uma tabela `pedidos`. O pedido guarda apenas o `cliente_id`.

```text
clientes
id | nome
1  | Ana

pedidos
id | cliente_id | valor_total
10 | 1          | 250.00
```

O `cliente_id` aponta para o cliente real.

---

## Tabelas, colunas, linhas e chaves

### Tabela

Tabela é uma estrutura que guarda dados de uma entidade.

Exemplos:

- `clientes`;
- `produtos`;
- `pedidos`.

### Coluna

Coluna é um atributo da tabela.

Na tabela `clientes`, podemos ter:

- `id`;
- `nome`;
- `email`;
- `data_nascimento`.

### Linha

Linha é um registro.

Exemplo:

```text
id | nome | email
1  | Ana  | ana@email.com
```

### Chave primária

Chave primária identifica uma linha de forma única.

Normalmente usamos `id`.

### Chave estrangeira

Chave estrangeira liga uma tabela a outra.

Exemplo:

`pedidos.cliente_id` aponta para `clientes.id`.

---

## Relacionamentos

### 1:1

Um registro se relaciona com apenas um registro de outra tabela.

Exemplo:

```text
Pessoa 1 ---- 1 Documento
```

### 1:N

Um registro se relaciona com vários registros de outra tabela.

Exemplo:

```text
Cliente 1 ---- N Pedidos
```

### N:N

Vários registros se relacionam com vários registros.

Exemplo:

```text
Aluno N ---- N Curso
```

Em banco relacional, relação N:N costuma exigir tabela intermediária.

```text
alunos
cursos
matriculas
```

---

## Normalização

Normalização é um conjunto de regras para organizar dados e reduzir duplicidade.

### Primeira Forma Normal, 1FN

Cada campo deve guardar um valor atômico, ou seja, indivisível para o contexto.

Ruim:

```text
cliente: Ana
telefones: 9999-1111, 9999-2222
```

Melhor:

```text
clientes
telefones_cliente
```

### Segunda Forma Normal, 2FN

A tabela deve estar na 1FN e os campos devem depender da chave inteira.

Isso aparece mais claramente em tabelas com chave composta.

### Terceira Forma Normal, 3FN

A tabela deve estar na 2FN e os campos não devem depender de outros campos não-chave.

Exemplo ruim:

```text
pedido_id | cliente_id | cliente_nome
```

O nome depende do cliente, não do pedido. Melhor guardar nome em `clientes`.

### Regra prática inicial

Se a mesma informação aparece repetida em muitos lugares, provavelmente existe uma tabela faltando.

---

## SQL

SQL significa **Structured Query Language**. É a linguagem usada para trabalhar com bancos relacionais.

SQL possui padrões internacionais, mas cada SGBD pode ter variações próprias. Ao longo da história, padrões como SQL-86, SQL-89, SQL-92, SQL:1999, SQL:2003, SQL:2008, SQL:2011, SQL:2016 e SQL:2023 ajudaram a consolidar a linguagem. Na prática, ao estudar MySQL, o estudante aprende SQL padrão com algumas particularidades do MySQL.

---

## MySQL: instalação e primeiros passos

### Windows

1. Acesse <https://www.mysql.com/downloads/>.
2. Baixe o **MySQL Community Server** ou o **MySQL Installer**.
3. Durante a instalação, escolha uma opção que inclua:
   - MySQL Server;
   - MySQL Workbench;
   - MySQL Shell, se disponível.
4. Defina uma senha para o usuário `root`.
5. Abra o MySQL Workbench.
6. Crie uma conexão local.
7. Teste com:

```sql
SELECT 1;
```

### Criando um banco de estudos

```sql
CREATE DATABASE estudos_java;
USE estudos_java;
```

---

## DDL, DML, DQL, DCL e TCL

### DDL

Data Definition Language. Define estruturas.

```sql
CREATE TABLE clientes (
    id INT PRIMARY KEY,
    nome VARCHAR(100) NOT NULL
);
```

### DML

Data Manipulation Language. Manipula dados.

```sql
INSERT INTO clientes (id, nome)
VALUES (1, 'Ana');
```

```sql
UPDATE clientes
SET nome = 'Ana Silva'
WHERE id = 1;
```

```sql
DELETE FROM clientes
WHERE id = 1;
```

### DQL

Data Query Language. Consulta dados.

```sql
SELECT id, nome
FROM clientes;
```

### DCL

Data Control Language. Controla permissões.

```sql
GRANT SELECT ON estudos_java.* TO 'usuario'@'localhost';
```

### TCL

Transaction Control Language. Controla transações.

```sql
START TRANSACTION;
UPDATE contas SET saldo = saldo - 100 WHERE id = 1;
UPDATE contas SET saldo = saldo + 100 WHERE id = 2;
COMMIT;
```

---

## Consultas com JOIN

Exemplo de tabelas:

```sql
CREATE TABLE clientes (
    id INT PRIMARY KEY,
    nome VARCHAR(100) NOT NULL
);

CREATE TABLE pedidos (
    id INT PRIMARY KEY,
    cliente_id INT NOT NULL,
    valor_total DECIMAL(10,2) NOT NULL,
    FOREIGN KEY (cliente_id) REFERENCES clientes(id)
);
```

Consulta:

```sql
SELECT clientes.nome, pedidos.valor_total
FROM clientes
JOIN pedidos ON pedidos.cliente_id = clientes.id;
```

`JOIN` junta registros relacionados.

---

# Testes, Git e APIs

## Testes automatizados

Teste automatizado é código que verifica se outro código funciona.

```java
public class CalculadoraDesconto {
    public double aplicar(double valor, double percentual) {
        return valor - (valor * percentual / 100);
    }
}
```

```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.assertEquals;

class CalculadoraDescontoTest {
    @Test
    void deveAplicarDesconto() {
        CalculadoraDesconto calculadora = new CalculadoraDesconto();
        double resultado = calculadora.aplicar(100, 10);
        assertEquals(90, resultado);
    }
}
```

---

## Git e GitHub

Git registra versões. GitHub hospeda o código.

```bash
git init
git add .
git commit -m "Adiciona modelagem inicial"
git branch -M main
git remote add origin URL_DO_REPOSITORIO
git push -u origin main
```

---

## JAX-RS antes do Spring Boot

JAX-RS, hoje Jakarta RESTful Web Services, é uma especificação Java para criar serviços REST usando anotações.

A ideia é entender REST e HTTP antes de usar Spring Boot.

Exemplo conceitual:

```java
@Path("/produtos")
public class ProdutoResource {

    @GET
    @Produces(MediaType.APPLICATION_JSON)
    public List<Produto> listar() {
        return List.of(new Produto("Mouse", 50.0));
    }

    @GET
    @Path("/{id}")
    @Produces(MediaType.APPLICATION_JSON)
    public Produto buscarPorId(@PathParam("id") Long id) {
        return new Produto("Teclado", 120.0);
    }
}
```

Conceitos:

- `@Path`: define a URL relativa do recurso.
- `@GET`: atende requisições HTTP GET.
- `@POST`: atende criação de recurso.
- `@PUT`: atende atualização completa.
- `@DELETE`: atende remoção.
- `@PathParam`: captura parte da URL.
- `@Produces`: define formato da resposta.
- `@Consumes`: define formato esperado na entrada.

---

## HTTP, recursos e endpoints

### HTTP

HTTP é o protocolo usado por navegadores, APIs e muitos sistemas web.

### Recurso

Recurso é uma coisa do domínio exposta pela API.

Exemplos:

- cliente;
- produto;
- pedido.

### Endpoint

Endpoint é uma URL específica que permite interagir com um recurso.

```text
GET /produtos
GET /produtos/10
POST /produtos
PUT /produtos/10
DELETE /produtos/10
```

### Status codes

- `200 OK`: sucesso.
- `201 Created`: recurso criado.
- `204 No Content`: sucesso sem corpo de resposta.
- `400 Bad Request`: requisição inválida.
- `404 Not Found`: recurso não encontrado.
- `500 Internal Server Error`: erro inesperado no servidor.

---

## Spring Boot

Spring Boot é uma forma opinativa e produtiva de criar aplicações Spring.

Ele ajuda com:

- configuração automática;
- servidor embutido;
- dependências starters;
- criação de APIs;
- integração com banco;
- testes;
- métricas e health checks.

### Bibliotecas comuns

- `spring-boot-starter-web`: APIs web com Spring MVC.
- `spring-boot-starter-data-jpa`: persistência com JPA.
- `spring-boot-starter-validation`: validação com anotações.
- `spring-boot-starter-test`: testes.
- Driver JDBC do MySQL.

### Controller

Camada que recebe requisições HTTP.

```java
@RestController
@RequestMapping("/produtos")
public class ProdutoController {
    private final ProdutoService service;

    public ProdutoController(ProdutoService service) {
        this.service = service;
    }

    @GetMapping
    public List<ProdutoResponse> listar() {
        return service.listar();
    }
}
```

### Service

Camada de regra de negócio.

```java
@Service
public class ProdutoService {
    private final ProdutoRepository repository;

    public ProdutoService(ProdutoRepository repository) {
        this.repository = repository;
    }

    public List<ProdutoResponse> listar() {
        return repository.findAll().stream()
            .map(produto -> new ProdutoResponse(produto.getId(), produto.getNome()))
            .toList();
    }
}
```

### Repository

Camada de acesso a dados.

```java
public interface ProdutoRepository extends JpaRepository<Produto, Long> {
}
```

### Entity

Classe mapeada para tabela.

```java
@Entity
public class Produto {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String nome;
    private BigDecimal preco;
}
```

### DTO

Objeto de entrada ou saída da API.

```java
public record ProdutoResponse(Long id, String nome) {
}
```

---

# Java Efetivo

## Java Efetivo

Tópicos iniciais recomendados:

- Static Factory Method.
- Builder.
- `equals` e `hashCode`.
- `toString`.
- Imutabilidade.
- Programação defensiva.

Exemplo de Builder:

```java
Pedido pedido = new Pedido.Builder()
    .cliente("Ana")
    .total(new BigDecimal("150.00"))
    .build();
```

---

# Design Patterns

## Design Patterns

Design Patterns são soluções reutilizáveis para problemas recorrentes de design de software.

Eles não servem para enfeitar código. Servem para resolver pressões reais:

- criação de objetos ficou complexa;
- muitas classes estão acopladas;
- muitos `if/else` controlam variações;
- objetos precisam ser notificados;
- uma API externa tem interface incompatível;
- o código precisa ser extensível sem quebrar o existente.

Categorias GoF:

- **Criacionais**: criação de objetos.
- **Estruturais**: composição entre classes e objetos.
- **Comportamentais**: comunicação, fluxo e variação de comportamento.

---

## Todos os padrões GoF

### 1. Factory Method, criacional

Problema: o código chama `new` diretamente em muitos lugares.

Antes:

```java
Pagamento pagamento = new PagamentoPix();
```

Depois:

```java
interface PagamentoFactory {
    Pagamento criar();
}

class PixFactory implements PagamentoFactory {
    public Pagamento criar() {
        return new PagamentoPix();
    }
}
```

Resolve porque centraliza a decisão de criação e permite trocar implementações.

### 2. Abstract Factory, criacional

Problema: criar famílias de objetos relacionados sem misturar estilos.

```java
interface UIFactory {
    Botao criarBotao();
    CampoTexto criarCampoTexto();
}

class WindowsFactory implements UIFactory {
    public Botao criarBotao() { return new BotaoWindows(); }
    public CampoTexto criarCampoTexto() { return new CampoTextoWindows(); }
}
```

Resolve quando vários objetos precisam combinar entre si.

### 3. Builder, criacional

Problema: construtor com parâmetros demais.

Antes:

```java
Pedido pedido = new Pedido(cliente, endereco, itens, cupom, frete, pagamento);
```

Depois:

```java
Pedido pedido = new Pedido.Builder()
    .cliente(cliente)
    .cupom("PROMO10")
    .build();
```

Resolve porque nomeia etapas de construção.

### 4. Prototype, criacional

Problema: criar objeto do zero é caro ou repetitivo.

```java
class Documento implements Cloneable {
    public Documento clone() {
        try { return (Documento) super.clone(); }
        catch (CloneNotSupportedException e) { throw new RuntimeException(e); }
    }
}
```

Resolve clonando um modelo existente.

### 5. Singleton, criacional

Problema: garantir uma única instância.

```java
class Configuracao {
    private static final Configuracao INSTANCIA = new Configuracao();
    private Configuracao() {}
    public static Configuracao getInstancia() { return INSTANCIA; }
}
```

Use com cuidado, pois pode dificultar testes.

### 6. Adapter, estrutural

Problema: interface externa incompatível.

```java
interface Notificador {
    void enviar(String mensagem);
}

class SmsAdapter implements Notificador {
    private final SmsLegado sms;
    SmsAdapter(SmsLegado sms) { this.sms = sms; }
    public void enviar(String mensagem) { sms.enviarSms(mensagem); }
}
```

Resolve criando uma ponte entre interfaces.

### 7. Bridge, estrutural

Problema: duas hierarquias variam independentemente.

```java
interface CanalEnvio { void enviar(String texto); }
abstract class Mensagem {
    protected CanalEnvio canal;
    Mensagem(CanalEnvio canal) { this.canal = canal; }
    abstract void enviar();
}
```

Resolve separando abstração de implementação.

### 8. Composite, estrutural

Problema: tratar item simples e grupo do mesmo jeito.

```java
interface ComponenteMenu { void exibir(); }
class ItemMenu implements ComponenteMenu { public void exibir() {} }
class GrupoMenu implements ComponenteMenu {
    List<ComponenteMenu> itens = new ArrayList<>();
    public void exibir() { itens.forEach(ComponenteMenu::exibir); }
}
```

Resolve estruturas em árvore.

### 9. Decorator, estrutural

Problema: adicionar comportamento sem alterar a classe original.

```java
class CafeComLeite implements Cafe {
    private final Cafe cafe;
    CafeComLeite(Cafe cafe) { this.cafe = cafe; }
    public double preco() { return cafe.preco() + 2; }
}
```

Resolve composição flexível de funcionalidades.

### 10. Facade, estrutural

Problema: uma operação exige muitos passos internos.

```java
class CheckoutFacade {
    void finalizar() {
        validarCarrinho();
        pagar();
        baixarEstoque();
        enviarEmail();
    }
}
```

Resolve oferecendo uma entrada simples para um subsistema complexo.

### 11. Flyweight, estrutural

Problema: muitos objetos repetem dados iguais.

```java
class TipoProduto {
    private final String categoria;
    TipoProduto(String categoria) { this.categoria = categoria; }
}
```

Resolve compartilhando estado comum entre muitos objetos.

### 12. Proxy, estrutural

Problema: controlar acesso a outro objeto.

```java
class RelatorioProxy implements Relatorio {
    private RelatorioReal real;
    public void gerar() {
        if (real == null) real = new RelatorioReal();
        real.gerar();
    }
}
```

Resolve carregamento tardio, segurança ou controle de acesso.

### 13. Chain of Responsibility, comportamental

Problema: vários objetos podem processar uma requisição.

```java
abstract class Validador {
    protected Validador proximo;
    void setProximo(Validador proximo) { this.proximo = proximo; }
    abstract void validar(Pedido pedido);
}
```

Resolve encadeando responsabilidades.

### 14. Command, comportamental

Problema: representar uma ação como objeto.

```java
interface Comando { void executar(); }
class EnviarEmailCommand implements Comando {
    public void executar() { System.out.println("Email enviado"); }
}
```

Resolve filas, logs, undo e execução posterior.

### 15. Interpreter, comportamental

Problema: interpretar uma linguagem ou expressão simples.

```java
interface Expressao { int interpretar(); }
class Numero implements Expressao {
    private final int valor;
    Numero(int valor) { this.valor = valor; }
    public int interpretar() { return valor; }
}
```

Resolve regras expressas como gramática simples.

### 16. Iterator, comportamental

Problema: percorrer coleção sem expor implementação.

```java
Iterator<String> iterator = nomes.iterator();
while (iterator.hasNext()) {
    System.out.println(iterator.next());
}
```

Resolve travessia uniforme.

### 17. Mediator, comportamental

Problema: muitos objetos conversam diretamente entre si.

```java
class ChatMediator {
    void enviar(String mensagem, Usuario usuario) {}
}
```

Resolve centralizando comunicação.

### 18. Memento, comportamental

Problema: salvar e restaurar estado.

```java
record EditorMemento(String texto) {}
```

Resolve histórico e undo sem expor detalhes internos.

### 19. Observer, comportamental

Problema: avisar vários interessados quando algo acontece.

```java
interface ObservadorPedido { void aoAprovar(Pedido pedido); }
```

Resolve notificação desacoplada.

### 20. State, comportamental

Problema: comportamento muda conforme estado interno.

```java
interface EstadoPedido { void avancar(Pedido pedido); }
class PedidoPago implements EstadoPedido { public void avancar(Pedido p) {} }
```

Resolve substituindo condicionais por objetos de estado.

### 21. Strategy, comportamental

Problema: muitos algoritmos alternativos.

Antes:

```java
if (tipo.equals("PIX")) { }
else if (tipo.equals("CARTAO")) { }
```

Depois:

```java
interface EstrategiaPagamento { void pagar(BigDecimal valor); }
```

Resolve tornando algoritmos intercambiáveis.

### 22. Template Method, comportamental

Problema: fluxo fixo com etapas variáveis.

```java
abstract class ProcessoPedido {
    final void executar() {
        validar();
        processar();
        notificar();
    }
    abstract void processar();
    void validar() {}
    void notificar() {}
}
```

Resolve reaproveitamento de esqueleto de algoritmo.

### 23. Visitor, comportamental

Problema: adicionar operações a uma estrutura sem mudar suas classes.

```java
interface VisitanteRelatorio { void visitar(Produto produto); }
interface Elemento { void aceitar(VisitanteRelatorio visitante); }
```

Resolve operações sobre estruturas complexas, como árvores.

---

## Projeto final e portfólio

Projeto sugerido: **Sistema de E-commerce**.

Funcionalidades:

- clientes;
- produtos;
- categorias;
- estoque;
- carrinho;
- pedidos;
- pagamentos;
- banco MySQL;
- testes;
- API REST;
- README detalhado.

Estrutura sugerida:

```text
src/main/java/br/com/estudos/ecommerce
├── controller
├── service
├── repository
├── domain
├── dto
├── exception
└── config
```
