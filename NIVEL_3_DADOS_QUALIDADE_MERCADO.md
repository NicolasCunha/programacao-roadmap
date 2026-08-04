# Nível 3 - Dados, Qualidade e Mercado

Neste nível, o estudante aproxima os conhecimentos de um ambiente profissional. O foco é persistência de dados, testes, versionamento, qualidade de código, padrões de projeto, APIs REST, Spring Boot e portfólio.

---

## Índice de progresso

- [ ] [Objetivo do nível](#objetivo-do-nível)
- [ ] [Uso responsável de IA](#uso-responsável-de-ia)
- [ ] [Banco de dados e SQL](#banco-de-dados-e-sql)
- [ ] [Modelagem relacional](#modelagem-relacional)
- [ ] [Testes automatizados](#testes-automatizados)
- [ ] [Git e GitHub](#git-e-github)
- [ ] [Java Efetivo](#java-efetivo)
- [ ] [Design Patterns](#design-patterns)
- [ ] [Spring Boot e APIs REST](#spring-boot-e-apis-rest)
- [ ] [Projeto final e portfólio](#projeto-final-e-portfólio)
- [ ] [Checklist de conclusão](#checklist-de-conclusão)

---

## Objetivo do nível

Ao final deste nível, o estudante deve conseguir criar tabelas e consultas SQL, escrever testes, versionar projetos, aplicar boas práticas, reconhecer padrões de projeto e criar APIs REST com Spring Boot.

---

## Uso responsável de IA

Nesta fase, a IA pode ajudar a revisar arquitetura, explicar erros e sugerir melhorias. Ainda assim, evite pedir para ela gerar projetos completos.

O estudante deve escrever o SQL, os testes, as classes e os endpoints manualmente pelo menos nas primeiras versões. Isso ajuda a memorizar padrões profissionais.

---

## Banco de dados e SQL

Banco de dados armazena informações de forma persistente.

```sql
CREATE TABLE clientes (
    id INT PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    email VARCHAR(100) NOT NULL
);
```

```sql
INSERT INTO clientes (id, nome, email)
VALUES (1, 'Ana', 'ana@email.com');
```

```sql
SELECT id, nome, email
FROM clientes;
```

---

## Modelagem relacional

Relacionamentos conectam tabelas.

Exemplo:

```text
Cliente 1 ---- N Pedidos
```

```sql
CREATE TABLE pedidos (
    id INT PRIMARY KEY,
    cliente_id INT NOT NULL,
    valor_total DECIMAL(10, 2) NOT NULL,
    FOREIGN KEY (cliente_id) REFERENCES clientes(id)
);
```

```sql
SELECT clientes.nome, pedidos.valor_total
FROM clientes
JOIN pedidos ON pedidos.cliente_id = clientes.id;
```

---

## Testes automatizados

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

```bash
git init
git status
git add .
git commit -m "Adiciona exercícios iniciais"
git branch -M main
git remote add origin URL_DO_REPOSITORIO
git push -u origin main
```

---

## Java Efetivo

Tópicos iniciais recomendados:

- Static Factory Method.
- Builder.
- `equals` e `hashCode`.
- `toString`.
- Imutabilidade.
- Programação defensiva.

Exemplo de static factory:

```java
public static Cliente criarComEmail(String nome, String email) {
    return new Cliente(nome, email);
}
```

---

## Design Patterns

### Strategy

```java
interface EstrategiaPagamento {
    void pagar(double valor);
}
```

### Factory

```java
class PagamentoFactory {
    static EstrategiaPagamento criar(String tipo) {
        if (tipo.equals("PIX")) {
            return new PagamentoPix();
        }
        throw new IllegalArgumentException("Tipo invalido");
    }
}
```

### Facade

```java
class FinalizacaoCompraFacade {
    void finalizarCompra() {
        validarCarrinho();
        processarPagamento();
        baixarEstoque();
        enviarEmail();
    }

    private void validarCarrinho() {}
    private void processarPagamento() {}
    private void baixarEstoque() {}
    private void enviarEmail() {}
}
```

---

## Spring Boot e APIs REST

Endpoints comuns:

```text
GET /produtos
GET /produtos/1
POST /produtos
PUT /produtos/1
DELETE /produtos/1
```

Controller:

```java
@RestController
@RequestMapping("/produtos")
public class ProdutoController {
    private final ProdutoService service;

    public ProdutoController(ProdutoService service) {
        this.service = service;
    }

    @GetMapping
    public List<Produto> listar() {
        return service.listar();
    }
}
```

Service:

```java
@Service
public class ProdutoService {
    private final ProdutoRepository repository;

    public ProdutoService(ProdutoRepository repository) {
        this.repository = repository;
    }

    public List<Produto> listar() {
        return repository.findAll();
    }
}
```

Repository:

```java
public interface ProdutoRepository extends JpaRepository<Produto, Long> {
}
```

---

## Projeto final e portfólio

Projeto sugerido: **Sistema de E-commerce**.

Funcionalidades:

- Clientes.
- Produtos.
- Estoque.
- Carrinho.
- Pedidos.
- Pagamentos.
- Banco de dados.
- Testes.
- API REST.
- README detalhado.

---

## Checklist de conclusão

- [ ] Sei criar tabelas SQL.
- [ ] Sei modelar relações entre tabelas.
- [ ] Sei fazer consultas com JOIN.
- [ ] Criei testes com JUnit.
- [ ] Sei usar Git e GitHub.
- [ ] Entendo tópicos iniciais do Java Efetivo.
- [ ] Sei reconhecer Strategy, Factory e Facade.
- [ ] Sei explicar API REST.
- [ ] Sei separar Controller, Service e Repository.
- [ ] Publiquei um projeto final.
