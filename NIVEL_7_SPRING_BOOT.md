# Nível 7 - Spring Boot

No Nível 6, você criou um recurso REST "na mão" com JAX-RS: anotou uma classe, mapeou métodos HTTP, devolveu status codes manualmente. Este nível mostra o que acontece quando um framework assume boa parte desse trabalho repetitivo — e, mais importante, **por que** ele consegue fazer isso, porque você já sabe o que estava escondido atrás da automação.

---

## Índice de progresso

- [ ] [Objetivo do nível](#objetivo-do-nível)
- [ ] [Uso responsável de IA](#uso-responsável-de-ia)
- [ ] [O que o Spring Boot resolve](#o-que-o-spring-boot-resolve)
- [ ] [Inversão de controle](#inversão-de-controle)
- [ ] [Injeção de dependência](#injeção-de-dependência)
- [ ] [O container do Spring](#o-container-do-spring)
- [ ] [Configuração automática e servidor embutido](#configuração-automática-e-servidor-embutido)
- [ ] [Starters](#starters)
- [ ] [Criando o primeiro projeto](#criando-o-primeiro-projeto)
- [ ] [Arquitetura em camadas](#arquitetura-em-camadas)
- [ ] [Entity: mapeando uma classe para uma tabela](#entity-mapeando-uma-classe-para-uma-tabela)
- [ ] [Repository: acesso a dados sem escrever SQL manualmente](#repository-acesso-a-dados-sem-escrever-sql-manualmente)
- [ ] [Service: onde mora a regra de negócio](#service-onde-mora-a-regra-de-negócio)
- [ ] [Controller: recebendo requisições HTTP](#controller-recebendo-requisições-http)
- [ ] [DTO: por que não expor a Entity direto](#dto-por-que-não-expor-a-entity-direto)
- [ ] [Um CRUD completo, de ponta a ponta](#um-crud-completo-de-ponta-a-ponta)
- [ ] [Validação com Bean Validation](#validação-com-bean-validation)
- [ ] [Tratando erros de forma centralizada](#tratando-erros-de-forma-centralizada)
- [ ] [Paginação com Spring Data](#paginação-com-spring-data)
- [ ] [Testando uma aplicação Spring Boot](#testando-uma-aplicação-spring-boot)
- [ ] [Java Efetivo aplicado às camadas](#java-efetivo-aplicado-às-camadas)
- [ ] [Erros comuns neste nível](#erros-comuns-neste-nível)
- [ ] [Glossário rápido](#glossário-rápido)
- [ ] [Recapitulando](#recapitulando)
- [ ] [O que vem no próximo nível](#o-que-vem-no-próximo-nível)

---

## Objetivo do nível

Ao final deste nível, o estudante deve conseguir:

- Explicar o que é inversão de controle e injeção de dependência, e por que o Spring gerencia os objetos em vez do próprio código fazer `new`.
- Explicar o que cada starter do Spring Boot automatiza, em relação ao que seria necessário configurar manualmente.
- Separar um sistema em Controller, Service, Repository, Entity e DTO, explicando a responsabilidade de cada camada.
- Construir um CRUD completo com Spring Web e Spring Data JPA, incluindo validação e tratamento de erros.
- Escrever um teste de integração simples para um endpoint Spring Boot.
- Aplicar Builder, `equals`/`hashCode` e imutabilidade nas classes do projeto, com justificativa.

## Uso responsável de IA

O Spring Boot tem muita "mágica" por anotação, e é tentador pedir para a IA gerar um Controller, Service e Repository inteiros de uma vez. Resista a isso pelo menos na primeira vez que cada peça aparece — escrever manualmente uma vez é o que transforma "colei isso e funcionou" em "eu sei por que isso funciona". Depois de entender o padrão, repetir a estrutura para um segundo recurso é razoável e mais rápido, mas a primeira vez vale ser sua.

## O que o Spring Boot resolve

Recapitulando o que o Nível 6 exigiu manualmente com JAX-RS: anotar cada endpoint, converter objetos para JSON, mapear status codes um por um. E isso nem tocou ainda em conectar ao banco de dados — o Nível 3 mostrou JDBC puro, exigindo abrir conexão, escrever SQL, mapear cada `ResultSet` para um objeto Java à mão, linha por linha.

Um sistema real combina as duas coisas: uma API REST que lê e escreve em um banco de dados, com regras de negócio no meio, validação de entrada, tratamento de erro consistente, testes. Fazer tudo isso manualmente para cada recurso do sistema (produtos, clientes, pedidos, categorias...) significa repetir a mesma estrutura dezenas de vezes, com muita chance de inconsistência entre elas.

**Spring Boot** é um framework que automatiza essa estrutura repetitiva: configuração de servidor, conversão JSON, acesso a dados, validação, tratamento de erro — permitindo que o código escrito por você foque na regra de negócio específica do seu domínio, não na "cola" necessária para todo sistema web funcionar.

## Inversão de controle

Até aqui, todo objeto que você usou foi criado por você mesmo, com `new`, exatamente onde era necessário:

```java
ProdutoRepository repository = new ProdutoRepositoryImpl();
ProdutoService service = new ProdutoService(repository);
```

Quem decide quando e como criar cada objeto é o seu próprio código — o controle do "ciclo de vida" dos objetos está com você.

**Inversão de controle** (IoC) significa entregar essa responsabilidade para um framework: em vez de você criar e conectar os objetos manualmente, você declara *o que* cada classe precisa, e um container externo (o Spring, nesse caso) decide *quando* e *como* criar e conectar tudo. O controle sobre a criação dos objetos é "invertido" — sai das suas mãos e vai para o framework.

## Injeção de dependência

**Injeção de dependência** (DI) é a técnica específica através da qual a inversão de controle acontece na prática: em vez de uma classe criar suas próprias dependências com `new`, ela apenas declara o que precisa (normalmente no construtor), e outra coisa externa "injeta" essa dependência pronta.

```java
@Service
public class ProdutoService {

    private final ProdutoRepository repository;

    public ProdutoService(ProdutoRepository repository) {
        this.repository = repository;
    }

    // ...
}
```

`ProdutoService` nunca escreve `new ProdutoRepositoryImpl()`. Ele só declara, no construtor, que precisa de um `ProdutoRepository`. O Spring, ao identificar essa dependência (através da anotação `@Service`, que marca a classe como algo que ele deve gerenciar), cria a instância de `ProdutoRepository` e a entrega pronta no construtor, automaticamente, sem nenhuma linha de código seu fazendo essa ligação manualmente.

O ganho prático, além de menos código repetitivo: trocar a implementação de `ProdutoRepository` (por exemplo, para um repositório falso em um teste, como no [Nível 5](./NIVEL_5_TESTES_E_GIT.md#o-que-é-um-mock)) não exige alterar `ProdutoService` — só o que é injetado no lugar muda, a classe que depende dele nem percebe a diferença.

## O container do Spring

O **container** (também chamado de contexto de aplicação, `ApplicationContext`) é a peça do Spring responsável por criar, configurar e conectar todos os objetos gerenciados por ele — chamados de **beans**.

Anotações como `@Service`, `@Repository`, `@Component` e `@RestController` marcam uma classe como um bean, dizendo ao Spring "gerencie essa classe para mim". Na inicialização da aplicação, o container:

1. Varre o projeto procurando classes marcadas com essas anotações.
2. Para cada uma, identifica suas dependências (olhando o construtor).
3. Cria as instâncias na ordem certa, resolvendo as dependências entre elas.
4. Mantém essas instâncias vivas, entregando a mesma instância (por padrão) sempre que outra classe precisar daquele bean.

Isso é o "por trás dos panos" de cada `@Service`, `@Repository` e `@RestController` que você vai escrever daqui para frente: cada anotação dessas é um pedido para o container assumir a responsabilidade de gerenciar aquele objeto, em vez de você.

## Configuração automática e servidor embutido

Uma aplicação web tradicional, em Java, historicamente exigia instalar e configurar um servidor de aplicação separado (como Tomcat), empacotar o projeto em um formato específico (`.war`) e implantá-lo dentro desse servidor.

Spring Boot embute um servidor (Tomcat, por padrão) dentro do próprio projeto. Rodar a aplicação significa apenas executar o `.jar` gerado — o servidor sobe junto, já configurado, sem instalação separada:

```bash
mvn spring-boot:run
```

Combinado a isso, a **configuração automática** (`auto-configuration`) do Spring Boot detecta o que está no classpath do projeto (por exemplo, se o driver do MySQL está presente) e configura automaticamente os componentes correspondentes com valores padrão razoáveis — você só precisa sobrescrever o que for diferente do padrão, normalmente em poucas linhas de um arquivo `application.properties`.

## Starters

Um **starter** é um conjunto de dependências relacionadas, empacotadas juntas sob um nome só, evitando que você precise descobrir e declarar manualmente cada biblioteca necessária para uma funcionalidade.

| Starter | O que traz |
|---|---|
| `spring-boot-starter-web` | Servidor embutido, Spring MVC, conversão JSON — a base para criar APIs REST |
| `spring-boot-starter-data-jpa` | Hibernate/JPA, integração com bancos relacionais |
| `spring-boot-starter-validation` | Bean Validation, para validar dados de entrada com anotações |
| `spring-boot-starter-test` | JUnit, Mockito, e utilitários de teste para aplicações Spring |
| Driver JDBC do MySQL (`mysql-connector-j`) | Permite ao Spring Data JPA conversar com o MySQL especificamente |

Declarar `spring-boot-starter-web` no `pom.xml` (Maven) já traz, transitivamente, tudo que é necessário para expor um endpoint REST — o equivalente a várias bibliotecas que, no Nível 6 com JAX-RS puro, precisariam ser identificadas e configuradas uma a uma.

## Criando o primeiro projeto

A forma mais comum de começar um projeto Spring Boot é pelo [Spring Initializr](https://start.spring.io), que gera a estrutura inicial com os starters escolhidos:

1. Escolha Maven (ou Gradle), Java, e a versão do Spring Boot.
2. Adicione os starters: Spring Web, Spring Data JPA, MySQL Driver, Validation, Spring Boot DevTools.
3. Baixe o projeto gerado e abra na IDE.
4. Configure a conexão com o banco em `src/main/resources/application.properties`:

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/estudos_java
spring.datasource.username=root
spring.datasource.password=sua_senha
spring.jpa.hibernate.ddl-auto=update
```

`spring.jpa.hibernate.ddl-auto=update` diz ao Hibernate para criar ou ajustar automaticamente as tabelas do banco com base nas classes `@Entity` do projeto — conveniente para desenvolvimento, mas normalmente evitado em produção, onde alterações de schema costumam passar por um controle mais cuidadoso (ferramentas como Flyway ou Liquibase, fora do escopo deste roadmap por enquanto).

## Arquitetura em camadas

O restante deste nível organiza o código em camadas com responsabilidades separadas — o mesmo princípio de responsabilidade única do [SRP](./NIVEL_2_DESENVOLVEDOR_JAVA.md#solid), aplicado à arquitetura inteira de um recurso:

```text
Requisição HTTP
      ↓
  Controller   → recebe a requisição, delega para o Service, devolve a resposta
      ↓
   Service     → aplica regras de negócio
      ↓
 Repository    → acessa o banco de dados
      ↓
   Database
```

Cada camada só conhece a camada imediatamente abaixo dela. O Controller não sabe como os dados são persistidos; o Repository não sabe nada sobre HTTP; o Service não sabe se quem o chamou foi um Controller REST, um job agendado ou um teste. Essa separação é o que torna cada peça testável isoladamente (retomando a ideia de [testabilidade como sinal de design](./NIVEL_5_TESTES_E_GIT.md#testabilidade-como-sinal-de-design), do Nível 5) e substituível sem afetar o resto.

## Entity: mapeando uma classe para uma tabela

Uma **Entity** é uma classe Java mapeada para uma tabela do banco, através de anotações JPA. Cada instância representa uma linha; cada atributo, uma coluna.

```java
@Entity
@Table(name = "produtos")
public class Produto {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false, length = 150)
    private String nome;

    @Column(nullable = false, precision = 10, scale = 2)
    private BigDecimal preco;

    @ManyToOne
    @JoinColumn(name = "categoria_id", nullable = false)
    private Categoria categoria;

    // getters e setters
}
```

Ligando isso ao Nível 3 e ao Nível 4:

- `@Entity` + `@Table`: equivale ao `CREATE TABLE produtos`.
- `@Id` + `@GeneratedValue(strategy = GenerationType.IDENTITY)`: equivale à chave primária com `AUTO_INCREMENT`.
- `@Column(nullable = false, length = 150)`: equivale a `VARCHAR(150) NOT NULL`.
- `precision = 10, scale = 2` em um campo `BigDecimal`: equivale a `DECIMAL(10,2)` — o mesmo cuidado com dinheiro do Nível 3, agora do lado Java. `BigDecimal`, e não `double`, pelo mesmo motivo que `DECIMAL` e não `FLOAT`: evitar erro de arredondamento binário em valores monetários.
- `@ManyToOne` + `@JoinColumn(name = "categoria_id")`: equivale à chave estrangeira `categoria_id`, modelando o relacionamento 1:N entre `Categoria` e `Produto` visto no Nível 4 (um produto tem uma categoria; uma categoria tem vários produtos).

## Repository: acesso a dados sem escrever SQL manualmente

Um **Repository**, com Spring Data JPA, é uma interface que já ganha implementações de CRUD prontas, sem que você escreva nenhum SQL:

```java
public interface ProdutoRepository extends JpaRepository<Produto, Long> {
}
```

Só isso já disponibiliza métodos como `findAll()`, `findById(id)`, `save(produto)` e `deleteById(id)` — a mesma lista de operações do CRUD ensinado com SQL puro no Nível 3, agora gerada automaticamente pelo Spring Data a partir do tipo da Entity (`Produto`) e do tipo da chave primária (`Long`).

Consultas mais específicas podem ser criadas só pelo nome do método, sem escrever a consulta:

```java
public interface ProdutoRepository extends JpaRepository<Produto, Long> {
    List<Produto> findByCategoriaId(Long categoriaId);
    List<Produto> findByPrecoLessThan(BigDecimal preco);
    Optional<Produto> findByNome(String nome);
}
```

O Spring Data interpreta o nome do método (`findByCategoriaId`, `findByPrecoLessThan`) e gera a consulta SQL correspondente automaticamente — por trás dos panos, o equivalente a um `SELECT * FROM produtos WHERE categoria_id = ?` do Nível 3, sem uma linha de SQL visível no seu código.

## Service: onde mora a regra de negócio

O **Service** é onde vive a lógica que não é nem "acessar dados" (Repository) nem "lidar com HTTP" (Controller) — as regras específicas do domínio.

```java
@Service
public class ProdutoService {

    private final ProdutoRepository repository;

    public ProdutoService(ProdutoRepository repository) {
        this.repository = repository;
    }

    public List<ProdutoResponse> listar() {
        return repository.findAll().stream()
            .map(produto -> new ProdutoResponse(produto.getId(), produto.getNome(), produto.getPreco()))
            .toList();
    }

    public ProdutoResponse buscarPorId(Long id) {
        Produto produto = repository.findById(id)
            .orElseThrow(() -> new ProdutoNaoEncontradoException(id));
        return new ProdutoResponse(produto.getId(), produto.getNome(), produto.getPreco());
    }
}
```

Repare que `buscarPorId` já toma uma decisão de negócio: o que fazer quando o produto não existe. Essa decisão não pertence ao Repository (que só sabe buscar ou não encontrar) nem ao Controller (que só sabe traduzir para HTTP) — pertence ao Service, que entende a regra "produto inexistente é um erro de negócio que precisa virar uma exceção específica", tratada mais adiante nesta camada ou capturada centralizadamente, como a seção sobre [tratamento de erros](#tratando-erros-de-forma-centralizada) mostra.

## Controller: recebendo requisições HTTP

O **Controller** é a camada mais próxima do que o Nível 6 já ensinou com JAX-RS — só que com bem menos código repetitivo:

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

    @GetMapping("/{id}")
    public ProdutoResponse buscarPorId(@PathVariable Long id) {
        return service.buscarPorId(id);
    }

    @PostMapping
    @ResponseStatus(HttpStatus.CREATED)
    public ProdutoResponse criar(@RequestBody ProdutoRequest request) {
        return service.criar(request);
    }
}
```

Comparando diretamente com o JAX-RS do Nível 6:

| JAX-RS | Spring Boot | Papel |
|---|---|---|
| `@Path("/produtos")` | `@RequestMapping("/produtos")` | Define a URL base |
| `@GET` | `@GetMapping` | Mapeia `GET` |
| `@POST` | `@PostMapping` | Mapeia `POST` |
| `@PathParam("id")` | `@PathVariable Long id` | Captura parte da URL |
| Corpo desserializado manualmente | `@RequestBody` | Converte o corpo JSON em objeto Java |
| `Response.status(201)` | `@ResponseStatus(HttpStatus.CREATED)` | Define o status code da resposta |

A conversão entre objeto Java e JSON (que no Nível 6 exigia `@Produces`/`@Consumes` explícitos) acontece automaticamente aqui, porque `spring-boot-starter-web` já traz e configura uma biblioteca de serialização JSON (Jackson) por padrão.

## DTO: por que não expor a Entity direto

Repare que o Controller acima não recebe nem devolve `Produto` (a Entity) diretamente — ele usa `ProdutoRequest` e `ProdutoResponse`. Isso não é acidental.

```java
public record ProdutoResponse(Long id, String nome, BigDecimal preco) {
}

public record ProdutoRequest(String nome, BigDecimal preco, Long categoriaId) {
}
```

Um **DTO** (Data Transfer Object) é um objeto dedicado só a entrar ou sair pela API, separado da Entity que representa a tabela do banco. Expor a Entity diretamente na API criaria problemas concretos:

- **Vazamento de dados sensíveis**: se `Produto` um dia ganhar um campo `custoInterno` (o custo de compra, não o de venda), expor a Entity direto vazaria esse dado para qualquer cliente da API, mesmo sem intenção.
- **Acoplamento entre schema de banco e contrato de API**: renomear uma coluna no banco (uma decisão interna) quebraria o formato do JSON que clientes externos já dependem (um contrato público) — dois motivos de mudança completamente diferentes, amarrados sem necessidade.
- **Referências circulares**: `Produto` referenciando `Categoria`, que referencia de volta uma lista de `Produto`, gera um objeto JSON infinito ao tentar serializar a Entity diretamente.

O DTO resolve os três problemas de uma vez: ele declara explicitamente o que entra e o que sai pela API, independente de como os dados estão organizados internamente no banco.

## Um CRUD completo, de ponta a ponta

Juntando as camadas anteriores, o fluxo completo de uma requisição `GET /produtos/10`:

```text
1. Controller recebe GET /produtos/10, extrai id=10 de @PathVariable
2. Controller chama service.buscarPorId(10)
3. Service chama repository.findById(10)
4. Repository executa, por trás dos panos, o equivalente a
   SELECT * FROM produtos WHERE id = 10 (Nível 3)
5. Repository devolve um Optional<Produto>
6. Service converte o Produto (Entity) em ProdutoResponse (DTO)
7. Controller devolve o ProdutoResponse, que o Spring serializa para JSON
8. Cliente recebe 200 OK com o corpo JSON
```

Esse é exatamente o mesmo fluxo conceitual do Nível 6 (requisição → processamento → resposta) e do Nível 3 (consulta SQL → resultado), só que cada etapa automatizada por uma camada e uma anotação específicas, em vez de escrita manualmente.

## Validação com Bean Validation

`spring-boot-starter-validation` permite declarar regras de validação diretamente nos DTOs, com anotações:

```java
public record ProdutoRequest(
    @NotBlank(message = "nome é obrigatório")
    String nome,

    @NotNull(message = "preço é obrigatório")
    @DecimalMin(value = "0.01", message = "preço deve ser maior que zero")
    BigDecimal preco,

    @NotNull(message = "categoria é obrigatória")
    Long categoriaId
) {
}
```

```java
@PostMapping
@ResponseStatus(HttpStatus.CREATED)
public ProdutoResponse criar(@Valid @RequestBody ProdutoRequest request) {
    return service.criar(request);
}
```

`@Valid`, no parâmetro do Controller, instrui o Spring a validar o `ProdutoRequest` recebido antes mesmo do método rodar. Se alguma regra falhar, o Spring já devolve automaticamente um `400 Bad Request`, sem que o código do Controller precise checar cada campo manualmente — o equivalente, na camada de API, às constraints (`NOT NULL`, `CHECK`) que o Nível 3 aplicou diretamente no banco. A prática recomendada é validar nos dois lugares: na API (resposta rápida, sem round-trip ao banco) e no banco (última linha de defesa, contra qualquer caminho que não passe pela API).

## Tratando erros de forma centralizada

Lançar uma exceção específica no Service, como `ProdutoNaoEncontradoException` na seção anterior, não é suficiente sozinho — sem tratamento, ela viraria um `500 Internal Server Error` genérico, escondendo que o problema real era "recurso não encontrado" (`404`, pela tabela do Nível 6).

`@RestControllerAdvice` centraliza esse tratamento, convertendo exceções específicas em respostas HTTP apropriadas, em um único lugar do projeto, em vez de repetir `try/catch` em cada Controller:

```java
@RestControllerAdvice
public class TratadorDeErros {

    @ExceptionHandler(ProdutoNaoEncontradoException.class)
    @ResponseStatus(HttpStatus.NOT_FOUND)
    public ErroResponse tratarProdutoNaoEncontrado(ProdutoNaoEncontradoException ex) {
        return new ErroResponse(ex.getMessage());
    }

    @ExceptionHandler(MethodArgumentNotValidException.class)
    @ResponseStatus(HttpStatus.BAD_REQUEST)
    public ErroResponse tratarValidacao(MethodArgumentNotValidException ex) {
        return new ErroResponse("Dados inválidos: " + ex.getMessage());
    }
}
```

```java
public record ErroResponse(String mensagem) {
}
```

Com isso, toda exceção `ProdutoNaoEncontradoException` lançada em qualquer Service do projeto — não só em `ProdutoService` — é automaticamente convertida em `404`, com uma mensagem consistente. Essa centralização evita que cada Controller reinvente sua própria forma de tratar o mesmo tipo de erro.

## Paginação com Spring Data

O Nível 6 mostrou paginação como conceito de API. Spring Data JPA já vem com suporte embutido:

```java
public interface ProdutoRepository extends JpaRepository<Produto, Long> {
}
```

```java
@GetMapping
public Page<ProdutoResponse> listar(Pageable pageable) {
    return repository.findAll(pageable)
        .map(p -> new ProdutoResponse(p.getId(), p.getNome(), p.getPreco()));
}
```

```text
GET /produtos?page=0&size=20&sort=nome,asc
```

`Pageable`, injetado automaticamente a partir dos parâmetros de query (`page`, `size`, `sort`), e `Page<T>` no retorno já incluem os metadados de paginação (total de elementos, total de páginas) descritos no Nível 6 — sem que você escreva `LIMIT`/`OFFSET` manualmente.

## Testando uma aplicação Spring Boot

O Nível 5 ensinou testes unitários, isolados de qualquer dependência externa. Uma aplicação Spring Boot também se beneficia de testes de integração, que sobem um contexto Spring real (ou parte dele) para verificar que as camadas funcionam corretas juntas:

```java
@SpringBootTest
@AutoConfigureMockMvc
class ProdutoControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @Test
    void deveRetornar404QuandoProdutoNaoExiste() throws Exception {
        mockMvc.perform(get("/produtos/999"))
            .andExpect(status().isNotFound());
    }
}
```

`MockMvc` simula requisições HTTP contra os Controllers sem precisar de um servidor real rodando em uma porta de verdade — mais rápido que um teste end-to-end completo, mas ainda exercitando Controller, Service e (dependendo da configuração) Repository juntos, retomando a [pirâmide de testes](./NIVEL_5_TESTES_E_GIT.md#a-pirâmide-de-testes) do Nível 5: isso fica no meio da pirâmide, não na base.

## Java Efetivo aplicado às camadas

Alguns tópicos do livro *Effective Java*, de Joshua Bloch, aparecem naturalmente ao construir as classes deste nível.

### Builder, para objetos com muitos parâmetros opcionais

```java
Pedido pedido = new Pedido.Builder()
    .cliente(cliente)
    .cupom("PROMO10")
    .build();
```

Útil quando um construtor teria parâmetros demais, alguns opcionais — o mesmo problema motivador já visto no catálogo de Design Patterns do [Nível 8](./NIVEL_8_DESIGN_PATTERNS.md).

### equals e hashCode, para Entities

```java
@Override
public boolean equals(Object o) {
    if (this == o) return true;
    if (!(o instanceof Produto produto)) return false;
    return Objects.equals(id, produto.id);
}

@Override
public int hashCode() {
    return Objects.hash(id);
}
```

Sem sobrescrever `equals`, duas instâncias de `Produto` representando a mesma linha do banco (mesmo `id`) seriam consideradas diferentes por padrão — Java compara objetos por referência de memória, não por conteúdo. Isso quebra comparações intuitivas (`produtoBuscado.equals(produtoEsperado)` em um teste, por exemplo) e o comportamento de coleções como `HashSet`, que dependem de `equals`/`hashCode` consistentes para funcionar corretamente.

### Imutabilidade, para DTOs

Os `record` usados como DTO neste nível (`ProdutoResponse`, `ProdutoRequest`) já são imutáveis por padrão — depois de criados, seus campos não podem mudar. Isso é uma escolha deliberada: um DTO que representa "os dados desta requisição específica" não deveria mudar depois de criado, e imutabilidade elimina de saída uma categoria inteira de bug (algum código no meio do caminho alterando um DTO que outra parte do sistema já leu, sem que ninguém perceba).

### Programação defensiva

```java
public ProdutoResponse criar(ProdutoRequest request) {
    if (request.preco().compareTo(BigDecimal.ZERO) <= 0) {
        throw new IllegalArgumentException("Preço deve ser maior que zero");
    }
    // ...
}
```

Mesmo com Bean Validation cobrindo a entrada da API, vale que o Service não confie cegamente que toda chamada futura a esse método sempre virá de um Controller validado — um Service pode ser chamado de outro lugar do sistema (um job agendado, outro Service) que não passou pela validação da camada web. Validar de novo no ponto de entrada de cada camada relevante é mais seguro do que confiar que a camada anterior sempre vai ter feito esse trabalho.

## Erros comuns neste nível

**`Consider defining a bean of type '...' in your configuration`**
O Spring não encontrou uma implementação para injetar em algum construtor. Geralmente falta a anotação (`@Service`, `@Repository`, `@Component`) na classe, ou ela não está em um pacote que o Spring está escaneando.

**`404` em todos os endpoints, mesmo os que deveriam existir**
Confira se o Controller está anotado com `@RestController` (não só `@Controller`, que espera retornar nomes de views HTML, não JSON) e se `@RequestMapping`/`@GetMapping` batem exatamente com a URL chamada.

**`could not execute statement; SQL... constraint`**
Um erro de banco (Nível 3/4) subindo através das camadas até aparecer no teste ou na resposta da API — a mensagem de erro real geralmente está encadeada dentro da exceção do Spring. Vale sempre olhar a causa raiz (`getCause()`), não só a mensagem externa.

**Referência circular ao serializar JSON**
Sintoma do problema descrito na seção sobre [DTO](#dto-por-que-não-expor-a-entity-direto) — Entities relacionadas em ambas as direções (`Produto` → `Categoria` → lista de `Produto`) sendo serializadas diretamente. Resolve-se usando DTOs em vez de Entities na resposta, não anotações para "quebrar" o ciclo na Entity.

## Glossário rápido

| Termo | Definição curta |
|---|---|
| Inversão de controle (IoC) | O framework, não o código do desenvolvedor, controla a criação dos objetos |
| Injeção de dependência (DI) | Técnica pela qual uma dependência é entregue pronta a uma classe, em vez de criada por ela |
| Bean | Objeto gerenciado pelo container do Spring |
| Container / `ApplicationContext` | Peça do Spring que cria, configura e conecta os beans |
| Starter | Conjunto de dependências relacionadas, empacotadas sob um nome só |
| Auto-configuration | Configuração automática baseada no que está disponível no projeto |
| Entity | Classe Java mapeada para uma tabela do banco |
| Repository | Camada de acesso a dados, com CRUD gerado automaticamente pelo Spring Data |
| Service | Camada onde vive a regra de negócio |
| Controller | Camada que recebe requisições HTTP e delega para o Service |
| DTO | Objeto dedicado à entrada/saída da API, separado da Entity |
| Bean Validation | Validação declarativa de dados através de anotações |
| `@RestControllerAdvice` | Tratamento centralizado de exceções, convertendo-as em respostas HTTP |

## Recapitulando

- [ ] Explicar inversão de controle e injeção de dependência com suas próprias palavras.
- [ ] Explicar o que cada camada (Controller, Service, Repository) sabe e não sabe sobre o resto do sistema.
- [ ] Explicar por que a Entity não deveria ser exposta diretamente na API.
- [ ] Construir um CRUD completo com Spring Web e Spring Data JPA.
- [ ] Adicionar validação a um DTO e tratar o erro resultante de forma centralizada.
- [ ] Escrever um teste de integração para um Controller usando `MockMvc`.

## O que vem no próximo nível

Você agora sabe construir uma API completa, ligada a um banco de dados, com validação e tratamento de erro — a base técnica de praticamente qualquer sistema de backend profissional. No [Nível 8 - Design Patterns](./NIVEL_8_DESIGN_PATTERNS.md), o foco muda para um vocabulário de soluções reutilizáveis para problemas de design que aparecem justamente quando um sistema como esse cresce: muitas classes acopladas, criação de objetos cada vez mais complexa, variações de comportamento controladas por `if/else` em cascata.
