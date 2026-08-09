# Nível 2 - Desenvolvedor Java

No [Nível 1](./NIVEL_1_FUNDAMENTOS_VISUALG.md), você aprendeu a pensar em algoritmos — condição, repetição, vetor, função — em um ambiente que isolava essa lógica da sintaxe de uma linguagem real. Este nível reintroduz essa mesma lógica dentro do Java, a linguagem que este roadmap usa como base, e adiciona o que o VisuAlg deliberadamente deixou de fora: tipagem rigorosa, orientação a objetos e as ferramentas que qualquer desenvolvedor Java profissional usa no dia a dia.

---

## Índice de progresso

- [ ] [Objetivo do nível](#objetivo-do-nível)
- [ ] [Uso responsável de IA](#uso-responsável-de-ia)
- [ ] [De VisuAlg para Java: o que muda](#de-visualg-para-java-o-que-muda)
- [ ] [JVM, JDK, JRE e bytecode](#jvm-jdk-jre-e-bytecode)
- [ ] [Instalando o Java e primeiro programa](#instalando-o-java-e-primeiro-programa)
- [ ] [Estrutura de uma classe Java](#estrutura-de-uma-classe-java)
- [ ] [Tipos primitivos](#tipos-primitivos)
- [ ] [Tipagem estática](#tipagem-estática)
- [ ] [Operadores em Java](#operadores-em-java)
- [ ] [Estruturas condicionais em Java](#estruturas-condicionais-em-java)
- [ ] [Estruturas de repetição em Java](#estruturas-de-repetição-em-java)
- [ ] [Arrays em Java](#arrays-em-java)
- [ ] [Métodos](#métodos)
- [ ] [Stack, heap, referência e Garbage Collector](#stack-heap-referência-e-garbage-collector)
- [ ] [Recursão em Java](#recursão-em-java)
- [ ] [Controle de versão com Git](#controle-de-versão-com-git)
- [ ] [Orientação a objetos: classe e objeto](#orientação-a-objetos-classe-e-objeto)
- [ ] [Construtores](#construtores)
- [ ] [Encapsulamento](#encapsulamento)
- [ ] [Modificadores de acesso](#modificadores-de-acesso)
- [ ] [Herança](#herança)
- [ ] [Polimorfismo](#polimorfismo)
- [ ] [Interface x classe abstrata](#interface-x-classe-abstrata)
- [ ] [Composição x herança](#composição-x-herança)
- [ ] [Collections](#collections)
- [ ] [Exceções](#exceções)
- [ ] [Streams](#streams)
- [ ] [SOLID](#solid)
- [ ] [Erros comuns em Java](#erros-comuns-em-java)
- [ ] [Glossário rápido](#glossário-rápido)
- [ ] [Recapitulando](#recapitulando)
- [ ] [O que vem no próximo nível](#o-que-vem-no-próximo-nível)

---

## Objetivo do nível

Ao final deste nível, o estudante deve conseguir:

- Explicar o que é a JVM, o JDK e o bytecode, e por que Java compila para um formato intermediário em vez de direto para código de máquina.
- Traduzir os conceitos de lógica do Nível 1 (condição, repetição, vetor, função) para a sintaxe Java correspondente.
- Explicar a diferença entre tipo primitivo e tipo referência, e o que isso implica para `==` e para `null`.
- Criar classes com atributos, construtores, encapsulamento e herança, aplicando o modificador de acesso correto em cada situação.
- Explicar a diferença entre sobrecarga e sobrescrita de método, e entre interface e classe abstrata.
- Usar `List`, `Set` e `Map`, escolhendo a estrutura certa para cada cenário.
- Tratar exceções corretamente, diferenciando checked de unchecked.
- Aplicar os cinco princípios SOLID, reconhecendo a violação de cada um em um exemplo de código.
- Ler uma `NullPointerException` (e outras exceções comuns) e identificar a causa provável.

## Uso responsável de IA

Não use IA para gerar classes prontas. Escreva manualmente para memorizar sintaxe, chaves, ponto e vírgula, tipos e mensagens de erro — a fluência de escrever Java sem parar a cada linha para consultar algo só vem de repetição própria, não de ler código gerado. Use a IA para entender uma mensagem de erro do compilador ou de uma exceção em tempo de execução que não fez sentido de primeira.

## De VisuAlg para Java: o que muda

Toda lógica que você praticou no Nível 1 continua válida — a diferença agora é a sintaxe e um conjunto de regras mais rígidas que o Java exige. Uma tabela de tradução direta ajuda a ancorar o que já é familiar:

| VisuAlg | Java | O que muda |
|---|---|---|
| `algoritmo "Nome"` ... `fimalgoritmo` | `public class Nome { public static void main(String[] args) { ... } }` | Java exige uma classe e um método `main` como ponto de entrada |
| `var idade: inteiro` | `int idade;` | O tipo vem antes do nome, não depois |
| `idade <- 18` | `idade = 18;` | Java usa `=` para atribuição — não existe `<-` |
| `idade = 18` (comparação) | `idade == 18` | Java usa `==` para comparação, dobrando o sinal para diferenciar de atribuição |
| `se ... entao ... senao ... fimse` | `if (...) { ... } else { ... }` | Chaves `{}` delimitam blocos, parênteses são obrigatórios na condição |
| `para i de 1 ate 10 faca ... fimpara` | `for (int i = 1; i <= 10; i++) { ... }` | O `for` do Java expõe explicitamente início, condição de parada e incremento |
| `enquanto ... faca ... fimenquanto` | `while (...) { ... }` | Praticamente idêntico em lógica |
| `vetor[1..5] de inteiro` | `int[] vetor = new int[5];` | Índices começam em `0`, não em `1` |
| `funcao nome(...): tipo` | `tipo nome(...) { ... return valor; }` | `return` é explícito, em vez de atribuir ao nome da função |
| `procedimento nome(...)` | `void nome(...) { ... }` | `void` indica "não devolve valor" |

Cada linha dessa tabela é detalhada nas seções seguintes — a tabela serve como mapa geral antes de entrar em cada assunto.

## JVM, JDK, JRE e bytecode

Java tem uma característica que o diferencia de linguagens que compilam direto para código de máquina (como C) ou que são interpretadas linha a linha (como Python, na sua forma mais simples): ele compila para um formato intermediário chamado **bytecode**, que não é o código de máquina de nenhum processador específico.

```text
.java -> javac -> .class (bytecode) -> JVM -> código de máquina da plataforma atual
```

- **JDK** (Java Development Kit): o kit completo para desenvolver, incluindo o compilador `javac`.
- **JRE** (Java Runtime Environment): o ambiente necessário só para executar aplicações Java já compiladas, sem o compilador.
- **JVM** (Java Virtual Machine): a máquina virtual que lê o bytecode e o executa, traduzindo para instruções da máquina real, no momento da execução.
- **Bytecode**: o conteúdo do arquivo `.class`, gerado pelo compilador a partir do `.java` — não é o código-fonte, nem é código de máquina direto; é um formato intermediário, específico da JVM.

Essa camada extra existe por um motivo concreto: bytecode roda em qualquer sistema operacional que tenha uma JVM instalada, sem recompilar o código-fonte. Esse princípio é resumido pelo lema histórico de Java, "write once, run anywhere" (escreva uma vez, rode em qualquer lugar) — o mesmo arquivo `.class`, compilado uma única vez, roda igualmente em Windows, Linux ou macOS, desde que cada um tenha sua própria JVM instalada, que sabe traduzir aquele bytecode para as instruções nativas daquele sistema específico.

## Instalando o Java e primeiro programa

1. Baixe um JDK (a distribuição **Eclipse Temurin**, gratuita, é uma escolha comum) em <https://adoptium.net/>.
2. Instale e confirme com `java -version` e `javac -version` no terminal.
3. Instale uma IDE, como **IntelliJ IDEA** (Community, gratuita) ou **VS Code** com a extensão de Java.

```java
public class OlaMundo {
    public static void main(String[] args) {
        System.out.println("Ola, mundo!");
    }
}
```

O nome do arquivo (`OlaMundo.java`) precisa ser **exatamente igual** ao nome da classe pública que ele contém — essa é uma regra do Java, não uma convenção opcional. Para compilar e rodar manualmente, sem IDE:

```bash
javac OlaMundo.java
java OlaMundo
```

`javac` gera o `OlaMundo.class` (bytecode); `java` inicia a JVM e executa esse bytecode. Uma IDE faz essas duas etapas automaticamente ao clicar em "run", mas vale saber o que está acontecendo por trás do botão.

## Estrutura de uma classe Java

```java
public class OlaMundo {
    public static void main(String[] args) {
        System.out.println("Ola, mundo!");
    }
}
```

Decompondo cada peça:

- `public class OlaMundo`: declara uma classe pública chamada `OlaMundo` — em Java, todo código vive dentro de uma classe, não existe comando "solto" fora de uma.
- `public static void main(String[] args)`: o **ponto de entrada** do programa — é o primeiro método executado quando a JVM inicia a aplicação. `public` para a JVM conseguir chamá-lo de fora; `static` porque ele roda sem precisar criar um objeto `OlaMundo` primeiro (assunto da seção sobre [classe e objeto](#orientação-a-objetos-classe-e-objeto)); `void` porque não devolve valor; `String[] args` recebe argumentos de linha de comando, se houver.
- `System.out.println(...)`: imprime uma linha na saída padrão — o equivalente Java ao `escreval` do VisuAlg. `System.out.print(...)`, sem o `ln`, imprime sem pular linha, equivalente ao `escreva`.

## Tipos primitivos

Java tem oito tipos primitivos — mais granulares que os quatro do VisuAlg, porque precisam refletir exatamente como cada valor é armazenado em memória:

| Tipo | Guarda | Faixa/observação |
|---|---|---|
| `byte` | Inteiro pequeno | -128 a 127 |
| `short` | Inteiro médio | -32.768 a 32.767 |
| `int` | Inteiro comum | ±2,1 bilhões — o inteiro padrão do dia a dia |
| `long` | Inteiro grande | ±9,2 quintilhões — sufixo `L` no literal (`10000000000L`) |
| `float` | Decimal de precisão simples | Sufixo `f` no literal (`10.5f`) — raramente usado |
| `double` | Decimal de precisão dupla | O decimal padrão do dia a dia |
| `char` | Um único caractere | Aspas simples (`'A'`), não duplas |
| `boolean` | Verdadeiro ou falso | `true` / `false`, minúsculo |

```java
int idade = 20;
double preco = 10.50;
String nome = "Ana";
boolean ativo = true;
```

Repare que `String` (texto) não está na tabela de primitivos — diferente de `int` ou `boolean`, `String` é um **tipo referência** (uma classe, não um primitivo), assunto retomado na seção sobre [stack, heap e referência](#stack-heap-referência-e-garbage-collector). Comparado ao VisuAlg, `int` corresponde a `inteiro`, `double` corresponde a `real`, `boolean` corresponde a `logico`, e `String`/`char` correspondem a `caractere` — com a diferença de que Java separa "um caractere" (`char`) de "uma cadeia de caracteres" (`String`), algo que o VisuAlg não distingue.

## Tipagem estática

Java é uma linguagem de **tipagem estática**: o tipo de cada variável é fixado na declaração e verificado pelo compilador **antes** do programa rodar, não durante a execução.

```java
int idade = 20;
idade = "vinte"; // erro de compilação, nem chega a rodar
```

Isso já era verdade no VisuAlg (declarar `idade: inteiro` também travava o tipo), mas em Java essa verificação é mais rigorosa e acontece em uma etapa de compilação separada e explícita (`javac`), antes de qualquer execução. A vantagem prática: uma categoria inteira de erro (usar um texto onde um número era esperado) é pega antes do programa sequer rodar, em vez de quebrar silenciosamente ou gerar um erro confuso no meio da execução, como aconteceria em uma linguagem de tipagem dinâmica.

## Operadores em Java

A maioria dos operadores aritméticos e lógicos do Nível 1 se transfere diretamente, com pequenas mudanças de símbolo:

| VisuAlg | Java | Operação |
|---|---|---|
| `<-` | `=` | Atribuição |
| `=` | `==` | Comparação de igualdade |
| `<>` | `!=` | Diferente |
| `e` | `&&` | E lógico |
| `ou` | `\|\|` | Ou lógico |
| `nao` | `!` | Negação lógica |
| `div` | `/` (entre `int`) | Divisão inteira |
| `mod` | `%` | Resto da divisão |

```java
if (idade >= 18 && possuiCarteira) {
    System.out.println("Pode dirigir");
}
```

### Uma armadilha real: `==` com objetos

`==` compara valores diretamente para tipos primitivos (`int`, `boolean`...), mas para tipos referência (como `String`), `==` compara se as duas variáveis apontam para o **mesmo objeto na memória**, não se o conteúdo é igual:

```java
String a = new String("Ana");
String b = new String("Ana");
System.out.println(a == b);        // false! objetos diferentes na memória
System.out.println(a.equals(b));   // true — compara o conteúdo
```

Essa distinção entre comparar referência e comparar conteúdo é a mesma que motiva o `equals`/`hashCode` sobrescritos em Entities, visto no [Nível 7](./NIVEL_7_SPRING_BOOT.md#java-efetivo-aplicado-às-camadas) — o problema já aparece aqui, na base da linguagem, bem antes de qualquer framework.

## Estruturas condicionais em Java

```java
if (idade >= 18) {
    System.out.println("Maior");
} else {
    System.out.println("Menor");
}
```

Equivalente direto ao `se/senao/fimse` do Nível 1 — a diferença sintática é a exigência de parênteses na condição e chaves `{}` delimitando cada bloco (o VisuAlg usa `entao`/`senao`/`fimse` como palavras-chave para o mesmo propósito).

```java
switch (diaDaSemana) {
    case 1:
        System.out.println("Domingo");
        break;
    case 3, 4, 5, 6:
        System.out.println("Meio da semana");
        break;
    default:
        System.out.println("Sabado");
}
```

Equivalente ao `escolha/caso/outrocaso` do Nível 1. `break` é necessário ao final de cada `case` (exceto na sintaxe mais nova de `case` com `->`, disponível em versões recentes do Java) para impedir que a execução "caia" para o próximo `case` — esquecer o `break` é um erro clássico, listado na seção de [erros comuns](#erros-comuns-em-java) deste nível.

## Estruturas de repetição em Java

```java
for (int i = 1; i <= 10; i++) {
    System.out.println(i);
}
```

O `for` do Java expõe explicitamente as três partes que o `para` do VisuAlg escondia por trás da sintaxe `de ... ate ... faca`: inicialização (`int i = 1`), condição de parada (`i <= 10`) e incremento (`i++`).

```java
while (!senha.equals("1234")) {
    System.out.print("Digite a senha: ");
    senha = scanner.nextLine();
}
```

Equivalente direto ao `enquanto` do Nível 1 — testa a condição antes de cada repetição.

```java
do {
    System.out.print("Digite a senha: ");
    senha = scanner.nextLine();
} while (!senha.equals("1234"));
```

`do...while` é o equivalente Java do `repita...até` do Nível 1: testa a condição **depois** de cada repetição, garantindo pelo menos uma execução — a mesma justificativa de "menu que precisa aparecer ao menos uma vez", já discutida no Nível 1, se aplica aqui sem mudança.

## Arrays em Java

```java
int[] notas = new int[5];
notas[0] = 8;
notas[1] = 7;

for (int i = 0; i < notas.length; i++) {
    System.out.println(notas[i]);
}
```

A diferença mais importante em relação ao `vetor` do Nível 1: **arrays em Java começam no índice `0`**, não em `1`. Um array declarado com tamanho `5` tem posições válidas de `0` a `4` — tentar acessar a posição `5` gera uma exceção em tempo de execução (`ArrayIndexOutOfBoundsException`, na seção de [erros comuns](#erros-comuns-em-java)). `notas.length` devolve o tamanho do array, o equivalente ao limite superior que o VisuAlg já conhecia de antemão pela declaração `vetor[1..5]`.

## Métodos

```java
static int somar(int a, int b) {
    return a + b;
}

static void saudar(String nome) {
    System.out.println("Ola, " + nome + "!");
}
```

Direto do Nível 1: um método que devolve um valor (`int somar`) equivale a uma **função** do VisuAlg; um método `void` (`void saudar`) equivale a um **procedimento**. `return` é explícito em Java — a execução do método termina imediatamente na linha `return`, devolvendo o valor indicado, diferente do VisuAlg, onde o retorno era implícito (atribuir ao nome da função).

## Stack, heap, referência e Garbage Collector

- **Stack**: pilha de chamadas de métodos e variáveis locais de tipo primitivo. Cada chamada de método empilha um novo "quadro" na stack; quando o método termina, o quadro é removido.
- **Heap**: área de memória onde ficam os objetos criados com `new` — instâncias de classes, incluindo `String` (apesar de literais de `String` terem um comportamento especial de reaproveitamento, fora do escopo deste nível).
- **Referência**: uma variável de tipo não-primitivo não guarda o objeto em si — guarda um **endereço** que aponta para onde o objeto vive na heap.
- **Garbage Collector (GC)**: um processo automático da JVM que identifica objetos na heap que não têm mais nenhuma referência apontando para eles, e libera essa memória, sem que o desenvolvedor precise gerenciar isso manualmente (diferente de linguagens como C, onde liberar memória é responsabilidade explícita do programador).

```java
Cliente a = new Cliente();
Cliente b = a; // a e b apontam para o mesmo objeto na heap
```

```text
Stack               Heap
a  ----\
        \--> [ objeto Cliente ]
b  ----/
```

Alterar um atributo através de `b` é visível através de `a` também, porque as duas variáveis apontam para o mesmo objeto — não são duas cópias independentes. Isso explica por que `Cliente a = new Cliente();` seguido de `Cliente b = a;` não cria dois clientes, cria uma única instância com dois nomes apontando para ela.

### null

Uma variável de tipo referência que não aponta para nenhum objeto tem o valor especial `null` — o equivalente, em conceito, ao `NULL` do banco de dados visto no [Nível 3](./NIVEL_3_BANCO_DE_DADOS.md#o-valor-null): ausência de valor, não um valor "vazio" comum. Tentar chamar um método em uma variável `null` gera a exceção mais comum do dia a dia Java, `NullPointerException`, detalhada na seção de [erros comuns](#erros-comuns-em-java).

Recursão sem parada — sem um caso base que interrompa as chamadas, como visto no Nível 1 — empilha quadros na stack indefinidamente, até estourar o espaço reservado para ela, gerando `StackOverflowError`.

## Recursão em Java

```java
static int fatorial(int n) {
    if (n <= 1) return 1;
    return n * fatorial(n - 1);
}
```

A mesma lógica do [Nível 1](./NIVEL_1_FUNDAMENTOS_VISUALG.md#recursão-no-visualg), agora com sintaxe Java: `if (n <= 1) return 1;` é o caso base, exatamente como `se n <= 1 entao fatorial <- 1` no VisuAlg. A diferença sintática é o `return` explícito, no lugar da atribuição implícita ao nome da função — o restante do raciocínio (por que o caso base é obrigatório, como a pilha de chamadas se desenrola) já foi coberto em profundidade no nível anterior e continua valendo sem alteração.

## Controle de versão com Git

### O problema que o Git resolve

Enquanto você resolve os exercícios deste roadmap, vai errar código, sobrescrever tentativas boas com tentativas piores e, às vezes, vai querer voltar para uma versão de ontem que funcionava. Sem um histórico de alterações, a única saída é reescrever tudo de novo, de memória.

O Git é uma ferramenta de controle de versão: ele guarda o histórico completo das mudanças em um projeto. Cada vez que você salva um ponto de progresso (um "commit"), o Git registra uma fotografia do código naquele momento. Você pode voltar para qualquer fotografia anterior sempre que precisar.

É parecido com o histórico de versões de um documento do Google Docs, mas pensado para código, funcionando no seu computador (não precisa de internet) e com controle total sobre quando cada fotografia é tirada.

### Git local x GitHub

- **Git**: o programa que roda no seu computador e guarda o histórico do projeto em uma pasta oculta `.git`.
- **GitHub**: um site que hospeda uma cópia desse histórico na internet, permitindo backup, colaboração e portfólio público.

Você pode usar Git sem GitHub. Mas para este roadmap, a recomendação é subir cada exercício resolvido para um repositório no GitHub, começando o hábito de portfólio desde já.

### Comandos essenciais

```bash
git init
```

Transforma a pasta atual em um repositório Git. Só precisa ser feito uma vez por projeto.

```bash
git status
```

Mostra o que mudou desde o último commit: arquivos novos, alterados ou removidos.

```bash
git add NomeDoArquivo.java
git add .
```

Marca arquivos para entrarem no próximo commit. `git add .` marca tudo que mudou na pasta atual.

```bash
git commit -m "Resolve exercicio 3 - verificacao de maioridade"
```

Cria uma fotografia (commit) com os arquivos marcados. A mensagem deve explicar o que mudou, não apenas "ajustes" ou "fix".

```bash
git log
```

Mostra o histórico de commits, do mais recente para o mais antigo.

### Enviando para o GitHub

```bash
git remote add origin URL_DO_REPOSITORIO
git branch -M main
git push -u origin main
```

- `git remote add origin`: liga o repositório local a um repositório vazio criado no GitHub.
- `git push`: envia os commits locais para o GitHub.

### Hábito recomendado a partir de agora

Sempre que resolver um exercício deste roadmap:

1. `git add .`
2. `git commit -m "mensagem descrevendo o que foi feito"`
3. `git push`

Isso cria um histórico real de progresso, visível para você e para quem for avaliar seu portfólio depois. No [Nível 5](./NIVEL_5_TESTES_E_GIT.md) você vai aprofundar em branches, colaboração e fluxo de Pull Request.

## Orientação a objetos: classe e objeto

Tudo até aqui usou métodos soltos (`static`), sem agrupar dados e comportamento relacionados. Imagine controlar dados de vários clientes usando só variáveis separadas — `nomeCliente1`, `emailCliente1`, `nomeCliente2`, `emailCliente2` — o mesmo problema de escala que motivou vetores no Nível 1, agora aplicado a dados de tipos diferentes agrupados conceitualmente (nome e email pertencem à mesma "coisa": um cliente).

Uma **classe** é um molde que define quais atributos (dados) e métodos (comportamento) um tipo de objeto vai ter. Um **objeto** é uma instância concreta desse molde, criada com `new`, com valores próprios para cada atributo.

```java
class Conta {
    private double saldo;

    public void depositar(double valor) {
        saldo += valor;
    }

    public double getSaldo() {
        return saldo;
    }
}
```

```java
Conta contaDaAna = new Conta();
contaDaAna.depositar(100);
System.out.println(contaDaAna.getSaldo()); // 100
```

`Conta` é a classe — o molde, definido uma única vez. `contaDaAna` é um objeto — uma instância concreta, com seu próprio `saldo`, independente de qualquer outra `Conta` criada a partir do mesmo molde.

## Construtores

Um **construtor** é um método especial, chamado automaticamente na criação de um objeto (`new`), usado para inicializar seus atributos com valores válidos desde o início — em vez de criar um objeto "vazio" e preenchê-lo depois, campo por campo.

```java
class Conta {
    private String titular;
    private double saldo;

    public Conta(String titular) {
        this.titular = titular;
        this.saldo = 0;
    }

    public Conta(String titular, double saldoInicial) {
        this.titular = titular;
        this.saldo = saldoInicial;
    }
}
```

```java
Conta a = new Conta("Ana");
Conta b = new Conta("Bruno", 500);
```

`this.titular = titular;` distingue o atributo da classe (`this.titular`) do parâmetro recebido (`titular`), quando os dois têm o mesmo nome — uma convenção comum para deixar claro, sem inventar nomes diferentes só para evitar a colisão, que o valor do parâmetro está sendo guardado no atributo correspondente. Ter dois construtores diferentes, como acima, é chamado de **sobrecarga** (mais detalhada na seção sobre [polimorfismo](#polimorfismo)) — o Java escolhe automaticamente qual construtor chamar, com base em quantos e quais argumentos foram passados no `new`.

## Encapsulamento

**Encapsulamento** é o princípio de proteger o estado interno de um objeto, expondo apenas o necessário através de métodos controlados, em vez de deixar qualquer código externo alterar os atributos livremente.

```java
// Sem encapsulamento — qualquer código pode quebrar a regra de negócio
class ContaRuim {
    public double saldo;
}

ContaRuim conta = new ContaRuim();
conta.saldo = -500; // nada impede um saldo negativo inválido
```

```java
// Com encapsulamento — a regra vive dentro da própria classe
class Conta {
    private double saldo;

    public void depositar(double valor) {
        if (valor <= 0) {
            throw new IllegalArgumentException("Valor deve ser positivo");
        }
        saldo += valor;
    }

    public double getSaldo() {
        return saldo;
    }
}
```

Tornar `saldo` `private` impede qualquer código externo de atribuir um valor diretamente a ele — a única forma de alterar o saldo passa por `depositar`, que pode validar a operação antes de aplicá-la. Esse é o motivo pelo qual "getters e setters" existem: não é burocracia por burocracia, é a forma de controlar *como* um atributo pode ser lido ou alterado, em vez de expô-lo sem nenhuma proteção.

## Modificadores de acesso

| Modificador | Visível para |
|---|---|
| `private` | Só dentro da própria classe |
| (nada, "package-private") | Classes do mesmo pacote |
| `protected` | Mesmo pacote, e subclasses de outros pacotes |
| `public` | Qualquer código, de qualquer lugar |

A prática recomendada é começar sempre pelo modificador mais restritivo (`private`) e só abrir acesso (`protected`, `public`) quando existir uma razão concreta para outro código precisar acessar aquilo diretamente — o mesmo raciocínio do [princípio do menor privilégio](./NIVEL_4_SQL_MODELAGEM.md#dcl-controlando-permissões-de-acesso), visto no Nível 4 para usuários de banco de dados, aplicado agora ao design de uma classe.

## Herança

**Herança** permite que uma classe reaproveite atributos e métodos de outra, especializando ou estendendo o que já existe, em vez de reescrever tudo do zero.

```java
class Funcionario {
    protected String nome;
    protected double salarioBase;

    public double calcularSalario() {
        return salarioBase;
    }
}

class Gerente extends Funcionario {
    private double bonus;

    @Override
    public double calcularSalario() {
        return salarioBase + bonus;
    }
}
```

`Gerente extends Funcionario` significa que `Gerente` herda `nome`, `salarioBase` e `calcularSalario()` de `Funcionario`, podendo sobrescrever o que for específico dela (assunto da próxima seção). `super` referencia explicitamente a classe pai, útil para chamar um construtor ou método dela:

```java
class Gerente extends Funcionario {
    private double bonus;

    public Gerente(String nome, double salarioBase, double bonus) {
        super.nome = nome;
        super.salarioBase = salarioBase;
        this.bonus = bonus;
    }
}
```

Herança deve representar uma relação **"é um"** genuína: um `Gerente` **é um** `Funcionario`, então a herança faz sentido. Usar herança só para reaproveitar código, sem essa relação real existir, é um erro de design comum — a alternativa correta para esse caso, reaproveitar comportamento sem uma relação "é um", é composição, discutida mais adiante.

## Polimorfismo

**Polimorfismo** significa "muitas formas" — a capacidade de tratar objetos de tipos diferentes através do mesmo contrato (um método com a mesma assinatura), cada um respondendo de um jeito próprio.

### Sobrescrita (override): mesmo método, comportamento diferente por subclasse

```java
class Funcionario {
    public double calcularSalario() { return 0; }
}

class Gerente extends Funcionario {
    @Override
    public double calcularSalario() { return 5000; }
}

class Estagiario extends Funcionario {
    @Override
    public double calcularSalario() { return 1500; }
}
```

```java
List<Funcionario> equipe = List.of(new Gerente(), new Estagiario());
for (Funcionario f : equipe) {
    System.out.println(f.calcularSalario()); // cada um calcula do seu próprio jeito
}
```

O código que percorre `equipe` não precisa saber se cada item é um `Gerente` ou um `Estagiario` — ele chama `calcularSalario()` genericamente, e o comportamento correto de cada subclasse é executado automaticamente. `@Override` não é obrigatório tecnicamente, mas é fortemente recomendado: ele avisa o compilador da intenção de sobrescrever, e gera um erro de compilação se a assinatura não bater exatamente com um método da classe pai — pegando, por exemplo, um erro de digitação no nome do método que, sem `@Override`, criaria silenciosamente um método novo em vez de sobrescrever o esperado.

### Sobrecarga (overload): mesmo nome, parâmetros diferentes

```java
class Calculadora {
    int somar(int a, int b) { return a + b; }
    double somar(double a, double b) { return a + b; }
    int somar(int a, int b, int c) { return a + b + c; }
}
```

Sobrecarga é decidida em tempo de **compilação**, com base nos tipos e na quantidade de argumentos passados — diferente de sobrescrita, que é decidida em tempo de **execução**, com base no tipo real do objeto. São dois mecanismos diferentes, frequentemente confundidos por compartilhar parte do nome ("override" e "overload" soam parecido em português, "sobrescrita" e "sobrecarga").

## Interface x classe abstrata

Ambas permitem definir um contrato que outras classes devem seguir, mas com propósitos diferentes:

```java
interface Notificador {
    void enviar(String mensagem);
}

abstract class FuncionarioBase {
    protected String nome;

    public abstract double calcularSalario();

    public String getNome() {
        return nome;
    }
}
```

| | Interface | Classe abstrata |
|---|---|---|
| Herança múltipla | Uma classe pode implementar várias interfaces | Uma classe só pode estender uma classe abstrata |
| Estado (atributos) | Não guarda estado próprio (só constantes) | Pode ter atributos e construtores |
| Implementação padrão | Métodos `default` são possíveis, mas incomuns | Pode ter métodos com implementação completa, prontos para reaproveitar |
| Quando usar | Definir um contrato de comportamento, sem relação "é um" hierárquica | Compartilhar código entre classes que têm uma relação "é um" genuína, com alguma parte comum já implementada |

Uma regra prática: se você só precisa garantir que várias classes, sem relação entre si, implementem um mesmo método (como `Notificador`, que pode ser implementado por email, SMS ou push, sem essas três terem nada em comum além do contrato), use interface. Se existe uma hierarquia real, com atributos e comportamento parcialmente compartilhados (como `FuncionarioBase`, `Gerente`, `Estagiario`), classe abstrata se encaixa melhor.

## Composição x herança

A seção sobre herança já alertou: herança deve representar uma relação "é um" genuína. Quando essa relação não existe, mas você ainda quer reaproveitar comportamento de outra classe, a alternativa é **composição**: montar um objeto usando outros objetos como atributos, em vez de herdar deles.

```java
// Herança mal aplicada: um Pedido "é um" CalculadoraDeFrete? Não faz sentido.
class Pedido extends CalculadoraDeFrete { }

// Composição: um Pedido "tem uma" forma de calcular o frete.
class Pedido {
    private CalculadoraDeFrete calculadoraFrete;

    public double calcularFrete() {
        return calculadoraFrete.calcular(this);
    }
}
```

Composição tende a ser mais flexível que herança: trocar `calculadoraFrete` por outra implementação em tempo de execução é simples; trocar a superclasse de `Pedido` depois que o sistema já depende dela é uma mudança estrutural muito mais arriscada. Esse é o princípio de design resumido pela frase "prefira composição a herança" — não uma proibição de usar herança, mas um lembrete de que ela deveria ser a exceção justificada, não o primeiro instinto para todo reaproveitamento de código. O padrão Strategy, no [Nível 8](./NIVEL_8_DESIGN_PATTERNS.md#7-strategy-comportamental), é essencialmente esse mesmo princípio aplicado de forma sistemática.

## Collections

Um array (visto anteriormente neste nível) tem tamanho fixo, definido na criação — inadequado quando você não sabe de antemão quantos elementos vai precisar guardar. O pacote `java.util` oferece estruturas de dados flexíveis para isso, conhecidas coletivamente como **Collections**.

### List: coleção ordenada, com repetição permitida

```java
List<String> nomes = new ArrayList<>();
nomes.add("Ana");
nomes.add("Bruno");
nomes.add("Ana"); // repetição permitida
System.out.println(nomes.get(0)); // "Ana", acesso por posição
```

`ArrayList` é a implementação mais comum de `List` — cresce dinamicamente, e o acesso por posição (`get(i)`) é rápido. `LinkedList`, uma alternativa menos usada no dia a dia, é mais eficiente para inserções e remoções no meio da lista, mas mais lenta para acesso por posição — na prática, `ArrayList` é a escolha padrão a menos que exista um motivo específico para o contrário.

### Set: coleção sem repetição, sem posição garantida

```java
Set<String> cpfs = new HashSet<>();
cpfs.add("11122233344");
cpfs.add("11122233344"); // ignorado, já existe
System.out.println(cpfs.size()); // 1
```

`HashSet` garante que nenhum valor se repita — a mesma regra de unicidade de um `UNIQUE` no banco de dados (Nível 3), aplicada em memória. Use `Set` sempre que a regra de negócio for "esse valor não pode se repetir na coleção", em vez de usar `List` e checar duplicidade manualmente antes de cada inserção.

### Map: pares chave-valor

```java
Map<Integer, String> produtos = new HashMap<>();
produtos.put(1, "Mouse");
produtos.put(2, "Teclado");
System.out.println(produtos.get(1)); // "Mouse"
```

`HashMap` associa uma chave única a um valor, com busca por chave muito mais rápida do que procurar em uma `List` item por item. Use `Map` sempre que o acesso natural a um dado for "eu tenho o identificador, quero o objeto correspondente" — o mesmo padrão de acesso que uma chave primária resolve no banco de dados (Nível 3), agora em memória.

### Escolhendo a estrutura certa

| Pergunta | Estrutura |
|---|---|
| Preciso de ordem e permito repetição? | `List` |
| Preciso garantir que não haja repetição? | `Set` |
| Preciso buscar rapidamente por um identificador? | `Map` |

### Iterando

```java
for (String nome : nomes) {
    System.out.println(nome);
}

for (Map.Entry<Integer, String> entrada : produtos.entrySet()) {
    System.out.println(entrada.getKey() + ": " + entrada.getValue());
}
```

O `for-each` (`for (Tipo item : colecao)`) é a forma mais comum de percorrer qualquer Collection, escondendo o uso de um `Iterator` por trás — o mesmo `Iterator` que vai aparecer formalizado como Design Pattern no [Nível 8](./NIVEL_8_DESIGN_PATTERNS.md#iterator).

## Exceções

### O problema que exceções resolvem

Uma operação pode falhar por motivos fora do controle direto do código que a chamou: um saldo insuficiente, um arquivo que não existe, uma divisão por zero. Sem um mecanismo dedicado, cada método precisaria devolver algum código especial de erro, e cada chamador precisaria lembrar de checar esse código manualmente — fácil de esquecer, e fácil de misturar com o valor de retorno normal.

**Exceções** são um mecanismo separado, dedicado a sinalizar e tratar erros, sem se misturar com o valor de retorno normal do método.

```java
class Conta {
    private double saldo;

    public void sacar(double valor) {
        if (valor > saldo) {
            throw new IllegalStateException("Saldo insuficiente");
        }
        saldo -= valor;
    }
}
```

```java
try {
    conta.sacar(1000);
} catch (IllegalStateException e) {
    System.out.println("Erro: " + e.getMessage());
} finally {
    System.out.println("Tentativa de saque finalizada");
}
```

- `throw`: lança uma exceção, interrompendo a execução normal do método imediatamente.
- `try`: delimita o bloco onde uma exceção pode ocorrer.
- `catch`: captura e trata uma exceção de um tipo específico.
- `finally`: bloco executado sempre, tenha a exceção acontecido ou não — útil para liberar recursos (fechar uma conexão, um arquivo).

### Checked x unchecked

| | Checked | Unchecked |
|---|---|---|
| Exemplo | `IOException` | `IllegalStateException`, `NullPointerException` |
| Herda de | `Exception` (não `RuntimeException`) | `RuntimeException` |
| Obrigatório tratar ou declarar? | Sim — o compilador exige `try/catch` ou `throws` na assinatura | Não — o compilador não obriga nada |
| Uso típico | Falhas externas previsíveis (arquivo, rede) | Erros de programação ou regra de negócio violada |

```java
// Checked: o compilador exige tratamento
public void lerArquivo() throws IOException {
    // ...
}

// Unchecked: tratamento é opcional para o compilador
public void sacar(double valor) {
    if (valor > saldo) throw new IllegalStateException("Saldo insuficiente");
}
```

### Criando uma exceção customizada

```java
class SaldoInsuficienteException extends RuntimeException {
    public SaldoInsuficienteException(String mensagem) {
        super(mensagem);
    }
}
```

Uma exceção customizada, com nome específico do domínio (`SaldoInsuficienteException`, em vez de um genérico `IllegalStateException`), comunica com mais precisão o que deu errado, e permite que quem captura a exceção trate esse caso especificamente, sem precisar inspecionar o texto da mensagem. Esse mesmo padrão reaparece no [Nível 7](./NIVEL_7_SPRING_BOOT.md#service-onde-mora-a-regra-de-negócio), com `ProdutoNaoEncontradoException` sendo capturada centralizadamente para virar um `404 Not Found`.

## Streams

### O problema que streams resolvem

Filtrar e transformar uma lista, do jeito imperativo, exige um laço explícito com uma lista auxiliar:

```java
List<String> filtrados = new ArrayList<>();
for (String nome : nomes) {
    if (nome.startsWith("A")) {
        filtrados.add(nome);
    }
}
```

**Streams** oferecem uma forma declarativa de expressar a mesma operação, encadeando transformações:

```java
List<String> filtrados = nomes.stream()
    .filter(n -> n.startsWith("A"))
    .toList();
```

- `.stream()`: inicia um pipeline de operações sobre a coleção.
- `.filter(condicao)`: mantém só os elementos que satisfazem a condição — uma expressão lambda (`n -> n.startsWith("A")`) que devolve `true` ou `false` para cada elemento.
- `.toList()`: encerra o pipeline, coletando o resultado em uma nova `List`.

### Operações comuns encadeadas

```java
double totalDosCaros = produtos.stream()
    .filter(p -> p.getPreco() > 100)
    .map(Produto::getPreco)
    .reduce(0.0, Double::sum);
```

- `.map(...)`: transforma cada elemento em outro valor (aqui, de `Produto` para `double`, extraindo só o preço).
- `.reduce(valorInicial, operacao)`: combina todos os elementos em um único resultado (aqui, somando).
- `Produto::getPreco`: uma **referência de método**, forma abreviada de escrever `p -> p.getPreco()`.

Streams não substituem laços em todo lugar — para lógica simples, um `for` continua sendo mais direto. Streams valem a pena quando a operação já é naturalmente uma sequência de filtrar/transformar/agregar, deixando essa intenção explícita no código, em vez de escondida dentro de um laço com variáveis auxiliares.

## SOLID

SOLID é um conjunto de cinco princípios de design orientado a objetos, cada um endereçando uma forma específica de acoplamento excessivo ou fragilidade estrutural.

### SRP — Single Responsibility Principle (responsabilidade única)

**Problema**: uma classe acumula motivos diferentes para mudar.

```java
// Antes: Pedido calcula total, salva no banco e envia email
class Pedido {
    double calcularTotal() { /* ... */ return 0; }
    void salvar() { /* SQL direto aqui */ }
    void enviarEmailConfirmacao() { /* ... */ }
}
```

```java
// Depois: cada responsabilidade em sua própria classe
class Pedido { /* só dados do pedido */ }
class CalculadoraPedido { double calcularTotal(Pedido p) { return 0; } }
class PedidoRepository { void salvar(Pedido p) { } }
class ServicoEmail { void enviarConfirmacao(Pedido p) { } }
```

**Resolve porque**: uma mudança na forma de calcular o total não arrisca quebrar a lógica de salvar ou de enviar email — cada classe muda por um único motivo. Esse mesmo princípio é a base da [arquitetura em camadas](./NIVEL_7_SPRING_BOOT.md#arquitetura-em-camadas) (Controller/Service/Repository) do Nível 7.

### OCP — Open/Closed Principle (aberto para extensão, fechado para modificação)

**Problema**: adicionar uma variação nova exige alterar código já existente e testado.

```java
// Antes: adicionar uma forma de pagamento nova exige mexer neste método
double calcularTaxa(String tipo) {
    if (tipo.equals("PIX")) return 0;
    if (tipo.equals("CARTAO")) return 0.03;
    // adicionar boleto exige adicionar mais um if aqui
    return 0;
}
```

```java
// Depois: adicionar uma forma de pagamento nova = criar uma classe nova
interface FormaPagamento {
    double calcularTaxa();
}
class Pix implements FormaPagamento {
    public double calcularTaxa() { return 0; }
}
class Boleto implements FormaPagamento {
    public double calcularTaxa() { return 0.02; } // novo, sem tocar no que já existia
}
```

**Resolve porque**: o mesmo problema motivador do Strategy, visto no [Nível 8](./NIVEL_8_DESIGN_PATTERNS.md#7-strategy-comportamental) — uma variação nova vira uma classe nova, sem alterar (e arriscar quebrar) o código das variações já existentes e testadas.

### LSP — Liskov Substitution Principle (substituição de Liskov)

**Problema**: uma subclasse quebra o comportamento esperado da classe pai, mesmo respeitando a assinatura dos métodos.

```java
class Retangulo {
    protected double largura, altura;
    void setLargura(double l) { largura = l; }
    void setAltura(double a) { altura = a; }
    double area() { return largura * altura; }
}

// Quadrado "é um" Retângulo? Tecnicamente na geometria, mas...
class Quadrado extends Retangulo {
    @Override
    void setLargura(double l) { largura = altura = l; } // efeito colateral inesperado
    @Override
    void setAltura(double a) { largura = altura = a; }
}
```

Um código que espera um `Retangulo` e chama `setLargura(5)` seguido de `setAltura(10)`, esperando área `50`, recebe área `100` se o objeto na verdade for um `Quadrado` — a substituição quebrou uma expectativa razoável de quem programou contra `Retangulo`. **Resolve-se** não forçando essa herança: `Quadrado` e `Retangulo` deveriam ser modelados de forma que uma subclasse nunca surpreenda quem programa contra o tipo da superclasse.

### ISP — Interface Segregation Principle (segregação de interface)

**Problema**: uma interface grande demais obriga classes a implementar métodos que não fazem sentido para elas.

```java
// Antes: uma interface inchada
interface Trabalhador {
    void trabalhar();
    void almocar();
}

class Robo implements Trabalhador {
    public void trabalhar() { /* ... */ }
    public void almocar() { throw new UnsupportedOperationException(); } // robô não almoça
}
```

```java
// Depois: interfaces menores e específicas
interface Trabalhavel { void trabalhar(); }
interface Almocavel { void almocar(); }

class Robo implements Trabalhavel { public void trabalhar() { } }
class Pessoa implements Trabalhavel, Almocavel { public void trabalhar() {} public void almocar() {} }
```

**Resolve porque**: nenhuma classe é forçada a implementar um método que não faz sentido para ela — cada classe implementa só as interfaces que genuinamente se aplicam a ela.

### DIP — Dependency Inversion Principle (inversão de dependência)

**Problema**: uma classe de alto nível (regra de negócio) depende diretamente de uma implementação concreta de baixo nível (detalhe técnico), acoplando as duas.

```java
// Antes: ProdutoService depende diretamente do MySQL
class ProdutoService {
    private MySqlProdutoRepository repository = new MySqlProdutoRepository();
}
```

```java
// Depois: ProdutoService depende de uma abstração
class ProdutoService {
    private final ProdutoRepository repository; // interface, não implementação concreta
    ProdutoService(ProdutoRepository repository) { this.repository = repository; }
}
```

**Resolve porque**: `ProdutoService` passa a depender de uma abstração (`ProdutoRepository`, interface), não de uma implementação específica — trocar o banco de dados, ou usar um repositório falso em teste (Nível 5), não exige alterar `ProdutoService`. Esse é exatamente o princípio por trás da injeção de dependência do Spring, detalhada no [Nível 7](./NIVEL_7_SPRING_BOOT.md#injeção-de-dependência).

## Erros comuns em Java

**`NullPointerException`**
A exceção mais comum do dia a dia Java: um método foi chamado em uma variável que valia `null` — sem nenhum objeto real por trás dela. Confira se toda variável foi de fato inicializada (`new` ou atribuída) antes de chamar um método nela.

**`ArrayIndexOutOfBoundsException`**
Acesso a uma posição de array que não existe — índice negativo, ou maior/igual ao `length` do array. Lembre-se: arrays em Java começam no índice `0`.

**`ClassCastException`**
Tentativa de converter um objeto para um tipo com o qual ele não é compatível — por exemplo, tratar um `Object` que na verdade guarda um `String` como se fosse um `Integer`.

**`StackOverflowError`**
Recursão sem caso base (ou com caso base que nunca é alcançado), empilhando chamadas até estourar a stack — o mesmo problema já discutido no Nível 1, agora com o nome real do erro em Java.

**Esquecer `break` em um `switch`**
Sem `break`, a execução "cai" (fall-through) para o `case` seguinte, executando blocos que não deveriam rodar para aquele valor. Confira se todo `case` termina com `break`, `return`, ou é intencionalmente agrupado como `case 3, 4, 5:`.

**Comparar `String` com `==` em vez de `.equals()`**
Já detalhado na seção de [operadores](#operadores-em-java) — `==` compara referência, não conteúdo, para tipos referência como `String`.

## Glossário rápido

| Termo | Definição curta |
|---|---|
| JVM | Máquina virtual que executa bytecode Java |
| Bytecode | Formato intermediário gerado pelo compilador, executado pela JVM |
| Tipagem estática | Tipo de cada variável verificado em tempo de compilação |
| Tipo primitivo | Tipo básico (`int`, `boolean`...) armazenado diretamente, sem referência |
| Tipo referência | Tipo que guarda um endereço para um objeto na heap |
| Stack | Memória de chamadas de método e variáveis locais primitivas |
| Heap | Memória onde vivem os objetos criados com `new` |
| Garbage Collector | Processo que libera memória de objetos sem referências ativas |
| Classe | Molde que define atributos e métodos de um tipo de objeto |
| Objeto | Instância concreta de uma classe |
| Construtor | Método especial que inicializa um objeto na criação |
| Encapsulamento | Proteção do estado interno de um objeto, exposto só por métodos controlados |
| Herança | Reaproveitamento de atributos/métodos de uma classe por outra, via `extends` |
| Polimorfismo | Tratar objetos de tipos diferentes através do mesmo contrato |
| Sobrescrita (override) | Redefinir, em uma subclasse, um método já existente na classe pai |
| Sobrecarga (overload) | Vários métodos com o mesmo nome, parâmetros diferentes |
| Composição | Montar um objeto usando outros objetos como atributos, em vez de herdar |
| Exceção | Mecanismo dedicado a sinalizar e tratar erros |
| Checked exception | Exceção que o compilador obriga a tratar ou declarar |
| Unchecked exception | Exceção que o compilador não obriga a tratar |
| Stream | Pipeline declarativo de operações sobre uma coleção |
| SOLID | Cinco princípios de design orientado a objetos (SRP, OCP, LSP, ISP, DIP) |

## Recapitulando

- [ ] Explicar o que é a JVM, o JDK e o bytecode, e por que Java compila para um formato intermediário.
- [ ] Traduzir uma condição, uma repetição e uma função do VisuAlg (Nível 1) para Java sem consultar a tabela de tradução.
- [ ] Explicar a diferença entre tipo primitivo e tipo referência, e o que isso implica para `==` e `null`.
- [ ] Criar uma classe com construtor, encapsulamento e pelo menos um método que aplique uma regra de negócio.
- [ ] Explicar a diferença entre herança e composição, e quando cada uma é apropriada.
- [ ] Explicar a diferença entre sobrescrita e sobrecarga de método.
- [ ] Escolher entre `List`, `Set` e `Map` para um cenário dado, justificando a escolha.
- [ ] Tratar uma exceção com `try/catch`, e explicar a diferença entre checked e unchecked.
- [ ] Reconhecer a violação de cada um dos cinco princípios SOLID em um trecho de código.
- [ ] Explicar a causa provável de uma `NullPointerException` a partir do stack trace.

## O que vem no próximo nível

Você agora tem a base completa da linguagem Java e de orientação a objetos. No [Nível 3 - Fundamentos de Banco de Dados](./NIVEL_3_BANCO_DE_DADOS.md), o foco muda para onde os dados manipulados por essas classes realmente vivem entre uma execução e outra do programa.
