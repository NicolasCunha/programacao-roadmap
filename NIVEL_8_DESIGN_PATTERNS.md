# Nível 8 - Design Patterns

Este nível vem depois de orientação a objetos, banco de dados e API de propósito. Design Patterns só fazem sentido depois que você já sentiu, na prática, os problemas que eles resolvem — código acoplado demais, construtores com parâmetros demais, `if/else` em cascata controlando variações de comportamento. Sem ter sentido a dor, um pattern vira decoração, aplicada porque "parece profissional", não porque resolve algo real.

---

## Índice de progresso

- [ ] [Objetivo do nível](#objetivo-do-nível)
- [ ] [Uso responsável de IA](#uso-responsável-de-ia)
- [ ] [O que é um Design Pattern](#o-que-é-um-design-pattern)
- [ ] [O que um Design Pattern não é](#o-que-um-design-pattern-não-é)
- [ ] [De onde vêm os padrões GoF](#de-onde-vêm-os-padrões-gof)
- [ ] [As três categorias GoF](#as-três-categorias-gof)
- [ ] [Como saber se você precisa de um pattern](#como-saber-se-você-precisa-de-um-pattern)
- [ ] [Os 8 padrões essenciais](#os-8-padrões-essenciais)
- [ ] [Catálogo completo: padrões criacionais](#catálogo-completo-padrões-criacionais)
- [ ] [Catálogo completo: padrões estruturais](#catálogo-completo-padrões-estruturais)
- [ ] [Catálogo completo: padrões comportamentais](#catálogo-completo-padrões-comportamentais)
- [ ] [Quando NÃO usar um pattern](#quando-não-usar-um-pattern)
- [ ] [Glossário rápido](#glossário-rápido)
- [ ] [Recapitulando](#recapitulando)
- [ ] [O que vem no próximo nível](#o-que-vem-no-próximo-nível)

---

## Objetivo do nível

Ao final deste nível, o estudante deve conseguir:

- Explicar o que é um Design Pattern e reconhecer um problema de design antes de sair procurando qual pattern "encaixa".
- Diferenciar padrões criacionais, estruturais e comportamentais pelo tipo de problema que resolvem.
- Implementar de memória os 8 padrões mais recorrentes no dia a dia (Factory Method, Builder, Singleton, Adapter, Facade, Strategy, Observer, Template Method).
- Reconhecer, mesmo sem implementar de cabeça, os outros 15 padrões do catálogo GoF e o problema que cada um resolve.
- Explicar o que é over-engineering e por que aplicar um pattern sem o problema correspondente piora o código em vez de melhorar.

## Uso responsável de IA

Peça para a IA explicar um pattern que você já tentou entender e ainda não ficou claro, ou para comparar duas formas de resolver o mesmo problema (com pattern e sem). Evite pedir para ela decidir qual pattern usar no seu projeto — essa decisão exige entender o problema específico que você tem na frente, e é exatamente esse julgamento que este nível quer que você desenvolva.

## O que é um Design Pattern

Um **Design Pattern** é uma solução reutilizável, com nome, para um problema recorrente de design de software — não um trecho de código pronto para copiar, mas uma estrutura de relacionamento entre classes e objetos que pode ser adaptada a diferentes contextos.

O catálogo mais influente vem do livro *Design Patterns: Elements of Reusable Object-Oriented Software* (1994), escrito por quatro autores conhecidos coletivamente como **GoF** (Gang of Four): Erich Gamma, Richard Helm, Ralph Johnson e John Vlissides. O livro cataloga 23 padrões, cada um batizado com um nome que virou vocabulário comum entre desenvolvedores — dizer "isso pede um Observer" comunica, em uma palavra, uma estrutura inteira de solução para quem também conhece o catálogo.

## O que um Design Pattern não é

- **Não é uma biblioteca ou framework**: é um conceito de estrutura, que você implementa com as ferramentas da própria linguagem, não algo que se importa com uma dependência.
- **Não é uma regra obrigatória**: nenhum pattern é "correto" de forma absoluta — cada um resolve um problema específico, e aplicá-lo sem esse problema presente costuma piorar o código, não melhorar.
- **Não é sinônimo de código bom**: código sem nenhum pattern pode ser excelente, se o problema que os patterns resolvem simplesmente não existir ali. Código cheio de patterns pode ser péssimo, se eles foram aplicados sem necessidade real (assunto da seção sobre [over-engineering](#quando-não-usar-um-pattern)).
- **Não é enfeite de currículo**: usar um pattern para "parecer sênior" é o oposto do espírito do catálogo — os padrões GoF documentam soluções que já apareciam repetidamente na prática, eles não existem para serem aplicados de propósito.

## De onde vêm os padrões GoF

Os autores do GoF não inventaram os 23 padrões do zero. Eles observaram soluções que apareciam repetidamente, de forma parecida, em sistemas orientados a objetos diferentes e não relacionados entre si — e documentaram essas soluções recorrentes, dando nome e estrutura formal a cada uma. Um Design Pattern é, nesse sentido, uma solução **descoberta e catalogada**, não inventada — o mesmo tipo de trabalho que um dicionário faz ao catalogar palavras que já circulavam na língua, não ao inventar palavras novas.

Isso explica por que os patterns continuam relevantes décadas depois, mesmo em linguagens e paradigmas que não existiam em 1994: eles documentam problemas estruturais de design orientado a objetos, e esses problemas (acoplamento, criação complexa, variação de comportamento) continuam surgindo, porque continuam surgindo os mesmos tipos de sistema.

## As três categorias GoF

- **Criacionais**: lidam com a criação de objetos, tornando esse processo mais flexível do que simplesmente espalhar `new` pelo código.
- **Estruturais**: lidam com como classes e objetos se combinam para formar estruturas maiores, mantendo essas estruturas flexíveis e fáceis de entender.
- **Comportamentais**: lidam com comunicação, responsabilidade e variação de comportamento entre objetos.

## Como saber se você precisa de um pattern

A ordem correta é sempre: **sentir o problema primeiro, buscar o pattern depois** — nunca o contrário. Alguns sinais concretos de que vale a pena procurar um pattern:

| Sintoma no código | Categoria de pattern a considerar |
|---|---|
| `new` da mesma família de classes espalhado por todo o código | Criacional |
| Construtor com muitos parâmetros, vários opcionais | Criacional (Builder) |
| Precisa garantir uma única instância de algo (configuração, conexão) | Criacional (Singleton) |
| Duas interfaces incompatíveis precisam se comunicar | Estrutural (Adapter) |
| Uma operação exige orquestrar muitos passos internos complicados | Estrutural (Facade) |
| Precisa adicionar comportamento sem alterar a classe original | Estrutural (Decorator) |
| `if/else` ou `switch` gigante escolhendo entre variações de um algoritmo | Comportamental (Strategy) |
| Vários objetos precisam ser avisados quando algo muda | Comportamental (Observer) |
| Comportamento muda dependendo de um "estado" do objeto | Comportamental (State) |

Se nenhum desses sintomas está presente, provavelmente não existe motivo para introduzir um pattern ainda — introduzi-lo "só por precaução" é exatamente o over-engineering discutido mais adiante.

---

## Os 8 padrões essenciais

Estes são os padrões mais recorrentes no dia a dia de quem está entrando no mercado — vale entendê-los a ponto de reconhecer o problema e implementar de memória, antes de se preocupar com o restante do catálogo.

### 1. Factory Method (criacional)

**Problema**: o código chama `new` diretamente em muitos lugares, e a decisão de qual implementação criar está espalhada, não centralizada.

```java
// Antes
Pagamento pagamento = new PagamentoPix();
```

```java
// Depois
interface PagamentoFactory {
    Pagamento criar();
}

class PixFactory implements PagamentoFactory {
    public Pagamento criar() {
        return new PagamentoPix();
    }
}
```

**Resolve porque**: centraliza a decisão de criação em um único lugar, permitindo trocar ou adicionar implementações sem alterar todo código que antes chamava `new PagamentoPix()` diretamente.

### 2. Builder (criacional)

**Problema**: um construtor com parâmetros demais, principalmente quando vários são opcionais, fica difícil de ler e fácil de errar a ordem dos argumentos.

```java
// Antes
Pedido pedido = new Pedido(cliente, endereco, itens, cupom, frete, pagamento);
```

```java
// Depois
Pedido pedido = new Pedido.Builder()
    .cliente(cliente)
    .cupom("PROMO10")
    .build();
```

**Resolve porque**: nomeia cada etapa de construção explicitamente, e permite omitir parâmetros opcionais sem precisar de vários construtores sobrecarregados (`overloads`). Já apareceu de forma aplicada no [Nível 7](./NIVEL_7_SPRING_BOOT.md#java-efetivo-aplicado-às-camadas).

### 3. Singleton (criacional)

**Problema**: algumas coisas no sistema devem ter, garantidamente, uma única instância — uma configuração global, uma conexão compartilhada.

```java
class Configuracao {
    private static final Configuracao INSTANCIA = new Configuracao();
    private Configuracao() {}
    public static Configuracao getInstancia() { return INSTANCIA; }
}
```

**Resolve porque**: o construtor privado impede criar novas instâncias de fora da classe, e `getInstancia()` sempre devolve a mesma. **Cuidado**: Singleton é o pattern mais fácil de abusar — ele introduz um estado global implícito no sistema, o que dificulta testes (um teste não consegue isolar ou trocar facilmente essa instância única) e cria acoplamento oculto entre partes distantes do código. Em uma aplicação Spring Boot, o próprio container já gerencia beans como instância única por padrão (Nível 7) — na prática, é raro precisar implementar Singleton manualmente nesse contexto.

### 4. Adapter (estrutural)

**Problema**: uma interface externa (uma biblioteca, uma API de terceiro) é incompatível com a interface que o seu código espera usar.

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

**Resolve porque**: cria uma ponte entre a interface que seu código espera (`Notificador`) e a interface real de uma dependência externa (`SmsLegado`), sem alterar nenhuma das duas.

### 5. Facade (estrutural)

**Problema**: uma operação de negócio exige orquestrar vários passos internos complicados, e cada lugar que precisa dessa operação teria que conhecer todos esses passos.

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

**Resolve porque**: oferece uma entrada única e simples para um subsistema complexo, escondendo a orquestração interna de quem só precisa do resultado final.

### 6. Decorator (estrutural)

**Problema**: adicionar comportamento a um objeto sem alterar a classe original nem criar uma explosão de subclasses para cada combinação possível.

```java
class CafeComLeite implements Cafe {
    private final Cafe cafe;
    CafeComLeite(Cafe cafe) { this.cafe = cafe; }
    public double preco() { return cafe.preco() + 2; }
}
```

**Resolve porque**: permite compor funcionalidades encadeando decoradores (`new CafeComLeite(new CafeComChocolate(new CafeSimples()))`), em vez de precisar de uma classe `CafeComLeiteEChocolate` para cada combinação possível.

### 7. Strategy (comportamental)

**Problema**: muitos algoritmos alternativos para a mesma operação, controlados por `if/else` ou `switch` que crescem a cada nova variação.

```java
// Antes
if (tipo.equals("PIX")) { /* ... */ }
else if (tipo.equals("CARTAO")) { /* ... */ }
```

```java
// Depois
interface EstrategiaPagamento {
    void pagar(BigDecimal valor);
}

class PagamentoPix implements EstrategiaPagamento {
    public void pagar(BigDecimal valor) { /* ... */ }
}
```

**Resolve porque**: cada variação vira uma classe própria, intercambiável, eliminando a necessidade de alterar um `if/else` central toda vez que uma variação nova é adicionada — o mesmo raciocínio do OCP (Open/Closed Principle), visto no [Nível 2](./NIVEL_2_DESENVOLVEDOR_JAVA.md#solid).

### 8. Observer (comportamental)

**Problema**: vários objetos precisam ser avisados automaticamente quando algo acontece em outro objeto, sem que esse objeto precise conhecer os detalhes de quem está interessado.

```java
interface ObservadorPedido {
    void aoAprovar(Pedido pedido);
}

class Pedido {
    private final List<ObservadorPedido> observadores = new ArrayList<>();

    void adicionarObservador(ObservadorPedido observador) {
        observadores.add(observador);
    }

    void aprovar() {
        observadores.forEach(o -> o.aoAprovar(this));
    }
}
```

**Resolve porque**: desacopla quem gera o evento (`Pedido`) de quem reage a ele (envio de email, atualização de estoque, notificação) — `Pedido` não precisa saber quantos ou quais observadores existem, só avisa a todos genericamente.

---

## Catálogo completo: padrões criacionais

Além de Factory Method, Builder e Singleton, já detalhados acima:

### Abstract Factory

**Problema**: criar famílias inteiras de objetos relacionados, garantindo que combinem entre si, sem misturar estilos por engano.

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

**Resolve porque**: garante que, ao trocar de fábrica (`WindowsFactory` por `MacFactory`), todos os componentes criados pertencem à mesma família consistente, sem risco de misturar um `BotaoWindows` com um `CampoTextoMac`.

### Prototype

**Problema**: criar um objeto do zero é caro (computacionalmente) ou repetitivo, quando um objeto muito parecido já existe.

```java
class Documento implements Cloneable {
    public Documento clone() {
        try { return (Documento) super.clone(); }
        catch (CloneNotSupportedException e) { throw new RuntimeException(e); }
    }
}
```

**Resolve porque**: clona um objeto-modelo já existente, em vez de reconstruir do zero cada novo objeto parecido.

## Catálogo completo: padrões estruturais

Além de Adapter, Facade e Decorator, já detalhados acima:

### Bridge

**Problema**: duas hierarquias de classes variam de forma independente, e herdar de ambas ao mesmo tempo geraria uma explosão combinatória de subclasses.

```java
interface CanalEnvio {
    void enviar(String texto);
}

abstract class Mensagem {
    protected CanalEnvio canal;
    Mensagem(CanalEnvio canal) { this.canal = canal; }
    abstract void enviar();
}
```

**Resolve porque**: separa a abstração (`Mensagem`, com suas variações de tipo) da implementação (`CanalEnvio`, com suas variações de canal), permitindo combinar as duas livremente sem uma subclasse para cada par.

### Composite

**Problema**: tratar um item individual e um grupo de itens da mesma forma, principalmente em estruturas de árvore.

```java
interface ComponenteMenu {
    void exibir();
}

class ItemMenu implements ComponenteMenu {
    public void exibir() { /* ... */ }
}

class GrupoMenu implements ComponenteMenu {
    List<ComponenteMenu> itens = new ArrayList<>();
    public void exibir() { itens.forEach(ComponenteMenu::exibir); }
}
```

**Resolve porque**: como `GrupoMenu` também implementa `ComponenteMenu`, o código que chama `exibir()` não precisa saber se está lidando com um item único ou com um grupo inteiro — a estrutura em árvore é transparente para quem consome.

### Flyweight

**Problema**: um grande número de objetos repete os mesmos dados internamente, desperdiçando memória.

```java
class TipoProduto {
    private final String categoria;
    TipoProduto(String categoria) { this.categoria = categoria; }
}
```

**Resolve porque**: compartilha o estado comum (`categoria`) entre muitas instâncias, em vez de cada objeto guardar sua própria cópia duplicada do mesmo dado.

### Proxy

**Problema**: controlar o acesso a outro objeto — adiando sua criação, checando permissão, ou adicionando um passo antes de delegar.

```java
class RelatorioProxy implements Relatorio {
    private RelatorioReal real;
    public void gerar() {
        if (real == null) real = new RelatorioReal();
        real.gerar();
    }
}
```

**Resolve porque**: intercepta o acesso ao objeto real, permitindo adicionar comportamento (aqui, criação tardia — o `RelatorioReal` só é criado na primeira vez que é realmente necessário) sem que quem usa `Relatorio` perceba a diferença.

## Catálogo completo: padrões comportamentais

Além de Strategy e Observer, já detalhados acima:

### Chain of Responsibility

**Problema**: uma requisição pode precisar ser processada por um entre vários possíveis handlers, sem que quem envia a requisição saiba qual vai efetivamente tratá-la.

```java
abstract class Validador {
    protected Validador proximo;
    void setProximo(Validador proximo) { this.proximo = proximo; }
    abstract void validar(Pedido pedido);
}
```

**Resolve porque**: encadeia handlers, cada um decidindo se trata a requisição ou passa adiante para o próximo da cadeia, sem que o chamador precise conhecer todos os handlers possíveis de antemão.

### Command

**Problema**: representar uma ação como um objeto, para poder enfileirar, logar, desfazer ou executar depois.

```java
interface Comando {
    void executar();
}

class EnviarEmailCommand implements Comando {
    public void executar() { System.out.println("Email enviado"); }
}
```

**Resolve porque**: transforma uma chamada de método em um objeto de primeira classe, que pode ser guardado, passado adiante e executado em outro momento — a base de filas de tarefas, histórico de ações e funcionalidade de "desfazer".

### Interpreter

**Problema**: interpretar frases de uma linguagem ou expressão simples e específica de um domínio.

```java
interface Expressao {
    int interpretar();
}

class Numero implements Expressao {
    private final int valor;
    Numero(int valor) { this.valor = valor; }
    public int interpretar() { return valor; }
}
```

**Resolve porque**: representa cada regra gramatical como uma classe, combinável em estrutura de árvore, para interpretar expressões de uma pequena linguagem sem precisar de um parser genérico completo.

### Iterator

**Problema**: percorrer os elementos de uma coleção sem expor como ela está implementada internamente.

```java
Iterator<String> iterator = nomes.iterator();
while (iterator.hasNext()) {
    System.out.println(iterator.next());
}
```

**Resolve porque**: oferece uma forma uniforme de percorrer qualquer coleção (lista, conjunto, árvore), sem que o código que percorre precise saber se por trás existe um array, uma lista encadeada ou outra estrutura. Você já usa esse pattern indiretamente todo `for-each` que escreve em Java.

### Mediator

**Problema**: muitos objetos se comunicam diretamente entre si, criando uma teia de dependências difícil de acompanhar.

```java
class ChatMediator {
    void enviar(String mensagem, Usuario usuario) { /* ... */ }
}
```

**Resolve porque**: centraliza a comunicação em um único mediador, para que os objetos conversem através dele em vez de conhecerem uns aos outros diretamente, reduzindo o número de conexões diretas entre eles.

### Memento

**Problema**: salvar o estado de um objeto para poder restaurá-lo depois, sem expor os detalhes internos desse estado para fora do objeto.

```java
record EditorMemento(String texto) {}
```

**Resolve porque**: permite guardar um "retrato" do estado interno em um objeto separado (o memento), possibilitando desfazer (undo) sem que outras partes do sistema precisem conhecer a estrutura interna do objeto original.

### State

**Problema**: o comportamento de um objeto muda de acordo com um estado interno, e isso é controlado por condicionais que crescem a cada novo estado.

```java
interface EstadoPedido {
    void avancar(Pedido pedido);
}

class PedidoPago implements EstadoPedido {
    public void avancar(Pedido p) { /* ... */ }
}
```

**Resolve porque**: cada estado vira uma classe própria, com seu próprio comportamento de transição, no lugar de um `if/else` gigante checando "qual é o estado atual" espalhado por vários métodos.

### Template Method

**Problema**: um fluxo de passos é sempre o mesmo, mas algumas etapas individuais variam dependendo do caso.

```java
abstract class ProcessoPedido {
    final void executar() {
        validar();
        processar();
        notificar();
    }
    abstract void processar();
    void validar() { /* padrão, pode ser sobrescrito */ }
    void notificar() { /* padrão, pode ser sobrescrito */ }
}
```

**Resolve porque**: fixa o esqueleto do algoritmo na classe base (`executar`, marcado como `final` para não ser alterado por subclasses), deixando só os passos que realmente variam para cada subclasse implementar.

### Visitor

**Problema**: adicionar novas operações a uma estrutura de objetos existente, sem alterar as classes dessa estrutura a cada operação nova.

```java
interface VisitanteRelatorio {
    void visitar(Produto produto);
}

interface Elemento {
    void aceitar(VisitanteRelatorio visitante);
}
```

**Resolve porque**: separa a operação (o "visitante") da estrutura de dados que ela percorre, permitindo adicionar operações novas (novos visitantes) sem modificar as classes da estrutura original — útil especialmente em estruturas complexas, como árvores.

---

## Quando NÃO usar um pattern

Aplicar um pattern sem o problema correspondente presente é chamado de **over-engineering**: código mais complexo do que o problema exige, justificado por uma solução que resolve algo que ainda não aconteceu.

Sintomas de over-engineering com Design Patterns:

- Um Strategy com uma única implementação, sem nenhuma variação real no horizonte — só uma interface e uma classe fazendo o trabalho de uma função direta.
- Um Factory Method para uma classe que nunca teve, e provavelmente nunca vai ter, uma segunda implementação.
- Um Observer para notificar um único interessado, que poderia simplesmente ser chamado diretamente.
- Builder para uma classe com dois ou três parâmetros obrigatórios, sem nenhum opcional.

O custo do over-engineering não é hipotético: cada camada de abstração introduzida — uma interface a mais, uma classe a mais — exige que quem lê o código salte entre mais arquivos para entender um fluxo simples. Um pattern aplicado sem necessidade não deixa o código "mais profissional"; deixa mais difícil de seguir, para um ganho de flexibilidade que ninguém vai usar.

A pergunta prática antes de introduzir qualquer pattern: **"esse problema já apareceu de verdade, ou estou resolvendo algo que talvez nunca aconteça?"** Padrões existem para serem aplicados quando o problema já dói, não para serem instalados preventivamente.

## Glossário rápido

| Termo | Definição curta |
|---|---|
| Design Pattern | Solução reutilizável, com nome, para um problema recorrente de design |
| GoF (Gang of Four) | Os quatro autores do catálogo original de 23 padrões (1994) |
| Padrão criacional | Resolve problemas relacionados à criação de objetos |
| Padrão estrutural | Resolve problemas de composição entre classes e objetos |
| Padrão comportamental | Resolve problemas de comunicação e variação de comportamento |
| Over-engineering | Aplicar complexidade (como um pattern) sem um problema real que a justifique |
| Acoplamento | O quanto uma classe depende dos detalhes internos de outra |

## Recapitulando

- [ ] Explicar a diferença entre padrão criacional, estrutural e comportamental.
- [ ] Implementar de memória Factory Method, Builder, Singleton, Adapter, Facade, Strategy, Observer e Template Method.
- [ ] Para pelo menos cinco dos outros 15 padrões, explicar o problema motivador sem consultar o texto.
- [ ] Explicar por que Singleton é o pattern mais fácil de abusar, e qual problema concreto isso cria em testes.
- [ ] Reconhecer um exemplo de over-engineering e explicar por que ele piora o código em vez de melhorar.

## O que vem no próximo nível

Você agora tem o vocabulário técnico completo deste roadmap: lógica, Java, banco de dados, testes, Git, APIs, Spring Boot e Design Patterns. No [Nível 9 - Projeto Final e Portfólio](./NIVEL_9_PROJETO_FINAL.md), tudo isso se junta em um projeto único, construído do zero, pensado para virar a primeira peça real do seu portfólio.
