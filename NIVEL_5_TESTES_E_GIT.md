# Nível 5 - Testes Automatizados e Git

O Git básico (`init`, `add`, `commit`, `push`) já foi introduzido no [Nível 2](./NIVEL_2_DESENVOLVEDOR_JAVA.md#controle-de-versão-com-git), como hábito desde o início do curso. Este nível tem dois focos, que parecem separados mas resolvem o mesmo problema de fundo: **como confiar em um código que você não escreveu ontem**. Testes automatizados garantem que o comportamento continua correto. Git avançado garante que você sabe exatamente o que mudou, quando, e consegue voltar atrás com segurança.

---

## Índice de progresso

- [ ] [Objetivo do nível](#objetivo-do-nível)
- [ ] [Uso responsável de IA](#uso-responsável-de-ia)
- [ ] [O problema que testes automatizados resolvem](#o-problema-que-testes-automatizados-resolvem)
- [ ] [O que é um teste automatizado](#o-que-é-um-teste-automatizado)
- [ ] [A pirâmide de testes](#a-pirâmide-de-testes)
- [ ] [Estrutura de um teste: Arrange, Act, Assert](#estrutura-de-um-teste-arrange-act-assert)
- [ ] [Escrevendo o primeiro teste com JUnit](#escrevendo-o-primeiro-teste-com-junit)
- [ ] [Asserções comuns](#asserções-comuns)
- [ ] [Nomeando testes](#nomeando-testes)
- [ ] [Casos de borda](#casos-de-borda)
- [ ] [Rodando os testes: Maven e Gradle](#rodando-os-testes-maven-e-gradle)
- [ ] [Cobertura de testes: o que é e sua armadilha](#cobertura-de-testes-o-que-é-e-sua-armadilha)
- [ ] [O que é um mock](#o-que-é-um-mock)
- [ ] [Testabilidade como sinal de design](#testabilidade-como-sinal-de-design)
- [ ] [TDD, rapidamente](#tdd-rapidamente)
- [ ] [Revisando o Git básico](#revisando-o-git-básico)
- [ ] [Branches: o problema que resolvem](#branches-o-problema-que-resolvem)
- [ ] [Trabalhando com branches](#trabalhando-com-branches)
- [ ] [Merge: fast-forward x three-way](#merge-fast-forward-x-three-way)
- [ ] [Conflitos de merge](#conflitos-de-merge)
- [ ] [Visualizando o histórico com git log --graph](#visualizando-o-histórico-com-git-log---graph)
- [ ] [git pull x git fetch](#git-pull-x-git-fetch)
- [ ] [.gitignore](#gitignore)
- [ ] [Commits atômicos e mensagens](#commits-atômicos-e-mensagens)
- [ ] [Pull Request](#pull-request)
- [ ] [Uma nota sobre rebase](#uma-nota-sobre-rebase)
- [ ] [Erros comuns de Git](#erros-comuns-de-git)
- [ ] [Glossário rápido](#glossário-rápido)
- [ ] [Recapitulando](#recapitulando)
- [ ] [O que vem no próximo nível](#o-que-vem-no-próximo-nível)

---

## Objetivo do nível

Ao final deste nível, o estudante deve conseguir:

- Explicar por que testar manualmente não escala, e o que um teste automatizado garante que um teste manual não garante.
- Escrever um teste JUnit seguindo Arrange-Act-Assert, cobrindo o caminho principal e pelo menos um caso de borda.
- Explicar o que é cobertura de testes e por que 100% de cobertura não significa "sem bugs".
- Explicar o que é um mock e por que ele existe.
- Criar e trabalhar em branches, resolver um conflito de merge, e explicar a diferença entre `git pull` e `git fetch`.
- Explicar o que é um Pull Request e por que times usam esse fluxo em vez de commitar direto na `main`.

## Uso responsável de IA

Para testes, evite pedir para a IA escrever o teste inteiro antes de você tentar — parte do valor de escrever testes é pensar em quais cenários podem quebrar o código, e isso é raciocínio que se perde se for delegado. Para Git, é seguro pedir ajuda para entender uma mensagem de erro ou um conflito de merge que você não está conseguindo interpretar — isso não compromete o aprendizado do jeito que copiar uma solução pronta compromete.

---

# Parte 1: Testes Automatizados

## O problema que testes automatizados resolvem

Lembre do exemplo do Nível 3: sem banco de dados, você reescreveria manualmente a lógica de ler e filtrar dados toda vez que precisasse consultar algo. Testes resolvem um problema parecido, só que sobre **comportamento do código**, não sobre dados.

Imagine uma classe `CalculadoraDesconto`, com um método `aplicar`. Para saber se ela está correta, você poderia abrir um `main`, chamar o método com alguns valores, e olhar o resultado impresso no console. Isso funciona, uma vez. O problema aparece depois:

- Você altera a classe `CalculadoraDesconto` duas semanas depois, para adicionar uma regra nova. Como saber se a regra antiga continua funcionando? Testar manualmente nesse momento significa lembrar todos os cenários que você testou da primeira vez — e lembrar de tudo raramente acontece.
- Um colega de equipe altera o mesmo código, sem saber quais cenários você já validou manualmente. Ele não tem como reproduzir sua validação, porque ela não ficou registrada em lugar nenhum, só na sua cabeça, uma vez.
- O projeto cresce para 50 classes interligadas. Uma alteração em uma classe pode quebrar o comportamento de outra sem ninguém perceber, porque validar manualmente 50 classes a cada alteração pequena é impraticável.

Um **teste automatizado** resolve isso registrando o cenário de validação como código, que pode ser executado automaticamente, quantas vezes for preciso, em segundos, sempre da mesma forma exata. Ele transforma "eu testei isso uma vez, e acho que ainda funciona" em "isso está sendo verificado toda vez que o código muda".

## O que é um teste automatizado

Um teste automatizado é código que executa outro código (o "código de produção") com entradas conhecidas, e verifica se a saída é a esperada — sem intervenção humana.

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

Rodar essa classe de teste executa o método `aplicar(100, 10)` e verifica automaticamente se o resultado é `90`. Se algum dia alguém alterar `CalculadoraDesconto` de um jeito que quebre essa conta, o teste falha imediatamente, apontando exatamente o que quebrou — sem que ninguém precise lembrar de testar aquilo manualmente de novo.

## A pirâmide de testes

Nem todo teste automatizado testa a mesma coisa, no mesmo nível. É comum representar os tipos de teste como uma pirâmide, indicando a proporção recomendada de cada tipo em um projeto saudável:

```text
        /\
       /  \      Testes E2E (poucos, lentos, caros)
      /----\
     /      \    Testes de Integração (alguns)
    /--------\
   /          \  Testes Unitários (muitos, rápidos, baratos)
  /------------\
```

- **Teste unitário**: testa uma unidade isolada de código — um método, uma classe — sem depender de banco de dados, rede ou sistema de arquivos. É o mais rápido de rodar e o mais fácil de escrever, por isso deve ser a maioria. O exemplo de `CalculadoraDesconto` acima é um teste unitário.
- **Teste de integração**: testa como duas ou mais partes reais do sistema funcionam juntas — por exemplo, um `Repository` conversando com um banco de dados de teste de verdade. Mais lento que um teste unitário, porque depende de recursos externos reais (ou de versões próximas do real).
- **Teste end-to-end (E2E)**: testa o sistema inteiro, do jeito que um usuário real usaria — por exemplo, uma requisição HTTP completa passando por Controller, Service, Repository e banco. É o mais lento e o mais caro de manter, então deve ser o menos numeroso, reservado para os fluxos mais críticos.

Este nível foca em testes unitários — são a base da pirâmide, e o que você mais vai escrever no dia a dia. Testes de integração voltam a aparecer no [Nível 7](./NIVEL_7_SPRING_BOOT.md), quando existir uma aplicação Spring Boot completa para testar de ponta a ponta.

## Estrutura de um teste: Arrange, Act, Assert

Praticamente todo teste unitário segue o mesmo esqueleto de três partes, conhecido como **AAA**:

```java
@Test
void deveAplicarDesconto() {
    // Arrange: prepara o cenário
    CalculadoraDesconto calculadora = new CalculadoraDesconto();
    double valor = 100;
    double percentual = 10;

    // Act: executa a ação que está sendo testada
    double resultado = calculadora.aplicar(valor, percentual);

    // Assert: verifica se o resultado é o esperado
    assertEquals(90, resultado);
}
```

- **Arrange (organizar)**: cria os objetos e valores necessários para o cenário do teste.
- **Act (agir)**: executa o método que está sendo testado, uma única vez.
- **Assert (verificar)**: compara o resultado obtido com o resultado esperado.

Manter essas três partes visualmente separadas — mesmo sem os comentários, só pela ordem — torna qualquer teste mais fácil de ler: quem abre o teste entende de imediato o que estava sendo preparado, o que foi executado, e o que deveria ter acontecido.

## Escrevendo o primeiro teste com JUnit

JUnit é o framework de testes mais usado em Java. A anotação `@Test` marca um método como um teste que deve ser executado automaticamente pela ferramenta de build (Maven ou Gradle) ou pela IDE.

```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class CalculadoraDescontoTest {

    @Test
    void deveAplicarDescontoDeDezPorcento() {
        CalculadoraDesconto calculadora = new CalculadoraDesconto();
        assertEquals(90, calculadora.aplicar(100, 10));
    }

    @Test
    void deveRetornarValorCheioQuandoDescontoForZero() {
        CalculadoraDesconto calculadora = new CalculadoraDesconto();
        assertEquals(100, calculadora.aplicar(100, 0));
    }
}
```

Por convenção, a classe de teste tem o mesmo nome da classe testada, com o sufixo `Test` (`CalculadoraDesconto` → `CalculadoraDescontoTest`), e vive em uma pasta separada do código de produção (`src/test/java`, espelhando o pacote de `src/main/java`) — assim testes e código de produção nunca se misturam, mas continuam organizados lado a lado.

## Asserções comuns

| Método | Verifica |
|---|---|
| `assertEquals(esperado, obtido)` | Os dois valores são iguais |
| `assertNotEquals(esperado, obtido)` | Os dois valores são diferentes |
| `assertTrue(condicao)` | A condição é verdadeira |
| `assertFalse(condicao)` | A condição é falsa |
| `assertNull(valor)` | O valor é `null` |
| `assertNotNull(valor)` | O valor não é `null` |
| `assertThrows(Excecao.class, () -> codigo)` | O código executado lança a exceção esperada |

```java
@Test
void deveLancarExcecaoQuandoSaldoInsuficiente() {
    Conta conta = new Conta();
    assertThrows(IllegalStateException.class, () -> {
        conta.sacar(1000);
    });
}
```

`assertThrows` merece destaque porque é um erro comum de iniciante esquecer de testar o **caminho de erro** de um código — testar só que ele funciona quando tudo dá certo, e nunca testar que ele se comporta corretamente quando algo dá errado.

## Nomeando testes

Um nome de teste ruim (`teste1`, `testCalculadora`) obriga quem lê o código a abrir o método inteiro para entender o que ele verifica. Um bom nome de teste descreve o cenário e o resultado esperado, funcionando quase como documentação executável:

```text
deveAplicarDescontoDeDezPorcento
deveRetornarValorCheioQuandoDescontoForZero
deveLancarExcecaoQuandoSaldoInsuficiente
```

Um padrão comum: `deve<ResultadoEsperado>Quando<Condicao>`. Não é uma regra rígida do JUnit, mas é uma convenção amplamente adotada porque torna a saída de uma bateria de testes (a lista de nomes que passaram ou falharam) legível por si só, sem precisar abrir o código.

## Casos de borda

Um **caso de borda** (edge case) é um cenário nos limites do que a função aceita — não o uso "normal", mas o que acontece exatamente na fronteira, ou além dela, de onde a lógica pode se comportar de um jeito inesperado.

Para `CalculadoraDesconto.aplicar(valor, percentual)`, o caminho principal é "desconto entre 0% e 100%, valor positivo". Casos de borda incluem:

- `percentual = 0`: já testado acima — o resultado deveria ser o valor cheio.
- `percentual = 100`: o resultado deveria ser `0`. O código atual aplica corretamente?
- `percentual` maior que `100`: o resultado ficaria negativo — provavelmente uma regra de negócio inválida que a classe não está impedindo hoje.
- `valor = 0`: o resultado deveria ser `0`, independente do percentual.
- `valor` negativo: provavelmente também deveria ser rejeitado, não calculado silenciosamente.

```java
@Test
void deveRetornarZeroQuandoDescontoForCemPorcento() {
    CalculadoraDesconto calculadora = new CalculadoraDesconto();
    assertEquals(0, calculadora.aplicar(100, 100));
}
```

A maior parte dos bugs que escapam para produção não acontece no caminho principal — acontece exatamente nesses limites, porque são os cenários que uma pessoa testando manualmente, apressada, tende a pular. Um bom conjunto de testes cobre o caminho principal e os casos de borda relevantes, não só o primeiro.

> **Pare e pense:** o método `aplicar(valor, percentual)` também deveria ser testado com `percentual = -10`? Por quê?
>
> <details><summary>Ver resposta</summary>
>
> Sim — um percentual negativo não faz sentido de negócio (um "desconto negativo" seria, na prática, um acréscimo disfarçado), mas nada no código atual impede essa chamada. Escrever esse teste tem dois resultados possíveis, os dois úteis: ou o teste revela que o método aceita silenciosamente um valor inválido (um bug a corrigir, adicionando validação), ou o teste documenta explicitamente que esse caso já é tratado, virando uma proteção contra alguém remover essa validação sem querer no futuro.
> </details>

## Rodando os testes: Maven e Gradle

Até aqui, os exemplos mostraram só o código do teste em si. Na prática, testes são executados por uma ferramenta de build, a mesma que compila e empacota o projeto — normalmente Maven ou Gradle em projetos Java.

```bash
# Maven
mvn test

# Gradle
./gradlew test
```

Os dois comandos procuram automaticamente por classes de teste no projeto (seguindo a convenção `src/test/java`, mencionada antes), executam todos os métodos anotados com `@Test`, e mostram um resumo: quantos passaram, quantos falharam, e o detalhe de cada falha (o valor esperado contra o valor obtido).

Rodar os testes deve ser parte do fluxo normal de trabalho: antes de um `git commit`, antes de abrir um Pull Request (assunto da Parte 2 deste nível), e sempre depois de qualquer alteração em código já existente — é exatamente esse hábito, repetido em segundos, que substitui a validação manual repetitiva descrita na abertura deste nível.

## Cobertura de testes: o que é e sua armadilha

**Cobertura de testes** (test coverage) é a porcentagem de linhas (ou branches de decisão) do código de produção que são executadas por pelo menos um teste. Ferramentas como JaCoCo calculam isso automaticamente e geram um relatório.

A armadilha: cobertura alta não significa ausência de bugs. É possível ter 100% de cobertura com testes fracos:

```java
@Test
void testeFraco() {
    CalculadoraDesconto calculadora = new CalculadoraDesconto();
    calculadora.aplicar(100, 10);
    // nenhum assert! a linha foi executada, conta como "coberta",
    // mas nada foi de fato verificado
}
```

Esse teste passa pela linha do método (contando como "coberto" no relatório), mas não verifica absolutamente nada sobre o resultado — um bug que trocasse a subtração por uma soma não seria detectado por esse teste, mesmo com 100% de cobertura naquela linha.

Cobertura é uma métrica útil para encontrar código **completamente sem teste nenhum**, não uma prova de qualidade. Um conjunto pequeno de testes bem pensados, cobrindo caminho principal e casos de borda com asserções reais, vale mais do que um número alto de cobertura obtido só para bater uma meta.

## O que é um mock

Muitas classes reais dependem de outras — um `Service` depende de um `Repository`, que depende de um banco de dados de verdade. Testar o `Service` de forma unitária (rápida, isolada, sem precisar de um banco rodando) exige uma forma de "fingir" o comportamento do `Repository`, sem usar o real.

Um **mock** é um objeto de mentira, que imita o comportamento de uma dependência real, com respostas programadas de antemão, só para viabilizar o teste daquilo que realmente importa naquele momento.

```java
// Exemplo conceitual, sem framework de mock ainda:
class RepositorioFalso implements ProdutoRepository {
    public Produto buscarPorId(Long id) {
        return new Produto("Produto de teste", new BigDecimal("10.00"));
    }
}
```

Com esse repositório falso, um teste do `Service` pode verificar a lógica de negócio dele sem depender de um banco de dados real estar rodando, sem dados de teste precisarem existir no banco, e rodando em milissegundos em vez de segundos. Frameworks como o Mockito (que você vai encontrar pronto no `spring-boot-starter-test`, no [Nível 7](./NIVEL_7_SPRING_BOOT.md)) automatizam a criação desses objetos falsos, sem precisar escrever uma classe inteira como no exemplo acima. Por enquanto, o objetivo é entender o conceito: mock existe para isolar o que está sendo testado das dependências que não são o foco daquele teste específico.

## Testabilidade como sinal de design

Uma observação que só fica óbvia depois de escrever bastante teste: uma classe **difícil de testar** costuma ser sintoma de um **design ruim**, não só um problema do teste em si.

Se testar uma classe exige um banco de dados real rodando, várias outras classes configuradas, e um cenário elaborado só para verificar uma regra de negócio simples, isso normalmente indica responsabilidades misturadas — a mesma classe decidindo regra de negócio, acessando dados e lidando com detalhes de infraestrutura ao mesmo tempo. Isso deveria soar familiar: é exatamente o problema que o SRP (Single Responsibility Principle), visto no [Nível 2](./NIVEL_2_DESENVOLVEDOR_JAVA.md#solid), existe para evitar.

Uma classe que só recebe valores, aplica uma regra e devolve um resultado — sem depender de banco, rede ou arquivo — é trivial de testar, como `CalculadoraDesconto` neste nível. Isso não é coincidência: código bem separado por responsabilidade tende a ser naturalmente fácil de testar, e código difícil de testar é, com bastante frequência, um sinal de alerta valendo a pena investigar antes mesmo de pensar em cobertura.

## TDD, rapidamente

**TDD** (Test-Driven Development) é uma forma de trabalhar onde o teste é escrito **antes** do código de produção, em um ciclo curto conhecido como Red-Green-Refactor:

1. **Red**: escreva um teste para um comportamento que ainda não existe. Ele falha (fica "vermelho"), porque o código ainda não foi implementado.
2. **Green**: escreva o código mínimo necessário para o teste passar (ficar "verde") — sem se preocupar ainda com elegância.
3. **Refactor**: com o teste passando como rede de segurança, melhore o código (nomes, duplicação, clareza), rodando o teste de novo a cada mudança para garantir que nada quebrou.

TDD é uma disciplina que vale a pena experimentar, mas não é obrigatória para aproveitar tudo que este nível ensinou — escrever bons testes depois do código, cobrindo caminho principal e casos de borda, já traz a maior parte do benefício. O ganho adicional do TDD é forçar você a pensar no comportamento esperado antes de pensar na implementação, o que tende a produzir um design de código mais simples de testar.

---

# Parte 2: Git Avançado

## Revisando o Git básico

O [Nível 2](./NIVEL_2_DESENVOLVEDOR_JAVA.md#controle-de-versão-com-git) já cobriu `git init`, `git status`, `git add`, `git commit` e `git push` — o suficiente para guardar seu próprio histórico de progresso, sozinho, na branch `main`. O que falta é o que aparece assim que mais de uma pessoa (ou mais de uma tarefa sua ao mesmo tempo) mexe no mesmo repositório: branches, merge, conflitos e Pull Request.

## Branches: o problema que resolvem

Imagine que você está no meio de uma alteração arriscada no projeto do e-commerce — trocando como o carrinho de compras calcula o frete — e, no meio do caminho, precisa corrigir um bug urgente e completamente separado no cadastro de clientes. Se tudo vive na `main`, seus commits incompletos do frete ficam misturados com a correção do bug, e não existe uma forma limpa de "publicar só a correção" sem also publicar o trabalho de frete pela metade.

Uma **branch** é uma linha de desenvolvimento independente, uma cópia do histórico que pode receber commits próprios sem afetar outras branches, até o momento em que você decide juntar (merge) o trabalho de volta.

```text
main:      A---B---C-------F
                    \      /
feature:            D----E
```

O trabalho de frete acontece inteiramente numa branch separada (`feature/calculo-frete`), enquanto a `main` continua limpa e estável, disponível para a correção urgente entrar direto nela, sem esperar o frete terminar.

## Trabalhando com branches

```bash
git branch
```

Lista as branches existentes no repositório, com um `*` indicando em qual você está.

```bash
git checkout -b feature/calculo-frete
```

Cria uma nova branch a partir da branch atual e já muda para ela. Equivalente a `git branch feature/calculo-frete` seguido de `git checkout feature/calculo-frete`.

```bash
git switch feature/calculo-frete
```

Forma mais recente do Git, dedicada a trocar de branch (`checkout` acumulou responsabilidades demais ao longo dos anos; `switch` existe para deixar essa ação específica mais clara). Para criar e trocar ao mesmo tempo: `git switch -c feature/calculo-frete`.

```bash
git checkout main
git merge feature/calculo-frete
```

Volta para a `main` e traz para ela todos os commits feitos na branch `feature/calculo-frete`.

### Convenção de nomes de branch

Um padrão comum no mercado: `tipo/descricao-curta`, como `feature/calculo-frete`, `fix/bug-cadastro-cliente`, `chore/atualiza-dependencias`. Isso deixa claro, só pelo nome, a natureza do trabalho ali dentro, antes mesmo de abrir qualquer commit.

## Merge: fast-forward x three-way

Existem duas formas de o Git combinar uma branch com outra, dependendo do que aconteceu enquanto a branch existiu:

**Fast-forward**: se a `main` não recebeu nenhum commit novo desde que a branch foi criada, o Git só "avança o ponteiro" da `main` até o último commit da branch — não existe combinação de verdade a fazer, porque não houve divergência.

```text
main:      A---B
                \
feature:         C---D

Depois do merge (fast-forward):
main:      A---B---C---D
```

**Three-way merge**: se a `main` recebeu commits próprios enquanto a branch existia, o Git precisa combinar as duas histórias, criando um commit de merge específico para isso, que tem dois "pais":

```text
main:      A---B-------F (commit de merge)
                \      /
feature:         C----D
```

Na prática, você não precisa escolher qual tipo de merge vai acontecer — o Git decide automaticamente com base no histórico. O que importa é entender que um commit de merge com dois pais é normal, não um erro.

## Conflitos de merge

Um **conflito de merge** acontece quando a mesma parte de um arquivo foi alterada de formas diferentes em duas branches, e o Git não tem como decidir sozinho qual versão deveria "vencer".

Exemplo mínimo reproduzível: duas branches alterando a mesma linha do mesmo arquivo.

```java
// main, depois de um commit:
double taxaFrete = 15.0;

// feature/calculo-frete, depois de outro commit, a partir do mesmo ponto:
double taxaFrete = 20.0;
```

Ao rodar `git merge feature/calculo-frete` estando na `main`, o Git para e marca o arquivo como conflitado:

```java
<<<<<<< HEAD
double taxaFrete = 15.0;
=======
double taxaFrete = 20.0;
>>>>>>> feature/calculo-frete
```

- `<<<<<<< HEAD` até `=======`: o conteúdo da branch atual (`main`).
- `=======` até `>>>>>>> feature/calculo-frete`: o conteúdo da branch que está sendo trazida.

Resolver o conflito significa editar o arquivo manualmente, decidindo o valor correto (ou combinando os dois de alguma forma), removendo as marcações `<<<<<<<`, `=======` e `>>>>>>>`, e então:

```bash
git add ArquivoResolvido.java
git commit
```

O `commit` aqui finaliza o merge com a resolução escolhida. Não existe mágica na resolução de conflito — é sempre uma decisão humana sobre qual código deveria prevalecer, que o Git não pode tomar sozinho.

> **Pare e pense:** um conflito de merge aconteceu porque o Git não conseguiu decidir sozinho. Em que situação duas alterações na mesma área de um arquivo **não** geram conflito, mesmo as duas mexendo "perto" uma da outra?
>
> <details><summary>Ver resposta</summary>
>
> Quando as alterações estão em linhas diferentes, mesmo que próximas — por exemplo, uma branch altera a linha 10 e a outra altera a linha 25 do mesmo arquivo. O Git resolve automaticamente combinando as duas mudanças, sem pedir intervenção, porque não há sobreposição real de texto. Conflito só acontece quando a mesma linha (ou linhas adjacentes o suficiente para o algoritmo de diff não conseguir separar) foi alterada de formas diferentes nas duas branches.
> </details>

## Visualizando o histórico com git log --graph

Depois de algumas branches e merges, o histórico deixa de ser uma linha reta. `git log --graph` desenha essa estrutura diretamente no terminal:

```bash
git log --graph --oneline --all
```

```text
*   f3a9c21 (HEAD -> main) Merge branch 'feature/calculo-frete'
|\
| * d2b8e11 (feature/calculo-frete) Adiciona regra de frete por regiao
| * a19f004 Cria estrutura inicial do calculo de frete
|/
* c001d4a Corrige bug no cadastro de clientes
* 8e3a2b0 Commit inicial
```

- `--oneline`: mostra cada commit em uma linha, com hash curto e mensagem.
- `--all`: inclui todas as branches, não só a atual.
- `--graph`: desenha as linhas e o ponto de merge (`f3a9c21`, com dois pais) visualmente.

Esse comando é útil sempre que o histórico parecer confuso — ele mostra de forma direta onde cada branch começou, onde ela foi mesclada de volta, e em que ordem os commits realmente aconteceram.

## git pull x git fetch

Os dois comandos buscam atualizações do repositório remoto (GitHub), mas fazem coisas diferentes, e confundir os dois é uma fonte comum de surpresa:

- **`git fetch`**: baixa os commits novos do remoto, mas **não** altera nada no seu código local — só atualiza o Git sobre o que existe no remoto, para você olhar antes de decidir o que fazer.
- **`git pull`**: faz um `git fetch` e, em seguida, automaticamente tenta um `merge` (ou `rebase`, dependendo da configuração) dos commits baixados na sua branch atual.

```bash
git fetch origin
git log origin/main    # olha o que mudou, sem alterar nada local ainda

git pull origin main   # busca e já combina com a branch atual
```

Na prática do dia a dia, `git pull` é o mais usado, exatamente por já fazer as duas etapas de uma vez. `git fetch` sozinho vale a pena quando você quer inspecionar o que mudou no remoto antes de trazer para o seu código — por exemplo, antes de atualizar uma branch onde você tem alterações locais não commitadas, para evitar um merge inesperado.

## .gitignore

Nem todo arquivo de um projeto deveria ir para o repositório: arquivos compilados (`.class`), pastas geradas por ferramentas de build (`target/`, `build/`), configurações locais da IDE (`.idea/`, `.vscode/`), e — especialmente importante — arquivos com senhas ou credenciais.

O arquivo `.gitignore`, na raiz do projeto, lista padrões de arquivos e pastas que o Git deve ignorar, nunca rastreando nem sugerindo commitar:

```text
# Build
target/
build/
*.class

# IDE
.idea/
.vscode/
*.iml

# Configuração local
.env
application-local.properties
```

Vale criar o `.gitignore` **antes** do primeiro commit do projeto. Se um arquivo sensível já foi commitado antes de existir um `.gitignore`, adicionar a regra depois não remove o arquivo do histórico — ele continua lá, em commits antigos, exigindo um processo separado (e mais delicado) para removê-lo de verdade.

## Commits atômicos e mensagens

Um commit **atômico** contém uma única mudança lógica coesa — não meia funcionalidade, não três funcionalidades diferentes misturadas. Isso importa por um motivo prático: se algo quebrar depois, `git log` mostra exatamente qual commit introduziu qual mudança, e reverter um commit atômico desfaz exatamente aquela mudança, sem efeito colateral em outra coisa não relacionada.

Um formato comum para mensagens, conhecido como **Conventional Commits**, prefixa a mensagem com o tipo de mudança:

```text
feat: adiciona calculo de frete por regiao
fix: corrige calculo de desconto quando percentual e zero
test: adiciona casos de borda para CalculadoraDesconto
refactor: extrai validacao de saldo para metodo separado
docs: atualiza README com instrucoes de instalacao
```

- `feat`: nova funcionalidade.
- `fix`: correção de bug.
- `test`: adição ou ajuste de testes, sem mudar comportamento de produção.
- `refactor`: reorganização de código, sem mudar comportamento.
- `docs`: mudança de documentação.

Não é obrigatório seguir esse padrão exato, mas adotar algum padrão consistente (mesmo que mais simples) transforma o `git log` em um histórico legível do "porquê" de cada mudança, não só do "o quê".

## Pull Request

Um **Pull Request** (PR), no GitHub, é uma solicitação formal para trazer os commits de uma branch para outra (normalmente, de uma branch de trabalho para a `main`), abrindo espaço para revisão antes da mudança ser aceita.

Isso existe por um motivo concreto, não só burocracia: um PR dá a outra pessoa (ou a você mesmo, revisando com calma depois) a chance de olhar o diff completo antes que ele vire parte definitiva do projeto, comentar em linhas específicas, sugerir mudanças, e rodar verificações automáticas (testes, build) antes do merge — em times profissionais, é comum um PR só poder ser aceito depois que os testes automatizados do Nível 5 (os que você acabou de aprender) passarem no CI, o ambiente que roda tudo automaticamente a cada PR aberto.

Fluxo básico:

```bash
git checkout -b feature/calculo-frete
# ... commits ...
git push -u origin feature/calculo-frete
```

Depois do `push`, o GitHub oferece a opção de abrir um Pull Request diretamente na interface web, comparando a branch nova com a `main`. Mesmo trabalhando sozinho neste roadmap, vale adotar o hábito de abrir PRs para funcionalidades maiores — é uma forma de simular o fluxo profissional e de revisar o próprio código com mais distância antes de aceitar o merge.

## Uma nota sobre rebase

Existe uma alternativa ao `merge` chamada `rebase`, que reescreve o histórico de uma branch como se ela tivesse sido criada a partir do estado mais recente da outra, em vez de criar um commit de merge com dois pais. Ela produz um histórico mais linear, mas reescreve commits — o que é perigoso em branches que outras pessoas já compartilham, porque os commits "antigos" deixam de existir, substituídos por novos, com hashes diferentes.

Para este roadmap, e para a maior parte do trabalho do dia a dia, `merge` é a escolha mais segura, especialmente para quem está começando: ele nunca reescreve histórico já existente. Vale saber que `rebase` existe e o que ele faz, mas não é necessário usá-lo agora.

## Erros comuns de Git

**"detached HEAD state"**
Acontece quando você faz `checkout` direto de um commit específico (não de uma branch). Qualquer commit novo feito nesse estado fica "solto", sem branch nenhuma apontando para ele, e pode ser perdido depois. Solução: crie uma branch a partir dali antes de commitar (`git checkout -b nome-da-branch`), ou volte para uma branch existente (`git checkout main`).

**"Your branch is ahead of 'origin/main' by N commits"**
Você tem commits locais que ainda não foram enviados ao remoto. Normal — resolve com `git push`.

**"Updates were rejected because the remote contains work that you do not have locally"**
Alguém enviou commits para o remoto depois da sua última sincronização. Resolve-se com `git pull` antes de tentar `git push` de novo — isso traz e combina os commits novos primeiro.

**Commitar um arquivo sensível por engano**
Se aconteceu e o commit ainda não foi enviado ao remoto (`git push`), dá para desfazer localmente (`git reset`) antes que vaze. Se já foi enviado, considere a credencial exposta comprometida — o caminho seguro é revogá-la e gerar uma nova, mesmo removendo o arquivo do histórico depois, porque não há garantia de que ninguém já a viu.

## Glossário rápido

| Termo | Definição curta |
|---|---|
| Teste automatizado | Código que executa e verifica outro código, sem intervenção humana |
| Teste unitário | Testa uma unidade isolada, sem dependências externas reais |
| Teste de integração | Testa partes reais do sistema funcionando juntas |
| Teste E2E | Testa o sistema inteiro, do ponto de vista do usuário |
| AAA | Arrange, Act, Assert — estrutura padrão de um teste |
| Caso de borda | Cenário no limite do que uma função aceita |
| Cobertura de testes | Porcentagem do código exercitada por pelo menos um teste |
| Mock | Objeto de mentira que imita uma dependência real, para isolar o teste |
| TDD | Escrever o teste antes do código de produção (Red-Green-Refactor) |
| Branch | Linha de desenvolvimento independente dentro do mesmo repositório |
| Merge | Combinar o histórico de duas branches |
| Conflito de merge | Quando a mesma parte de um arquivo mudou de formas diferentes em duas branches |
| `git fetch` | Baixa commits do remoto sem alterar o código local |
| `git pull` | `fetch` seguido de `merge` automático |
| Commit atômico | Commit que contém uma única mudança lógica coesa |
| Pull Request | Solicitação de revisão antes de trazer commits de uma branch para outra |
| Rebase | Reescreve o histórico de uma branch como se ela partisse de outro ponto |

## Recapitulando

- [ ] Explicar por que testar manualmente não escala conforme o projeto cresce.
- [ ] Escrever um teste seguindo Arrange-Act-Assert, com pelo menos um caso de borda.
- [ ] Explicar por que 100% de cobertura não é garantia de ausência de bugs.
- [ ] Explicar o que é um mock e por que ele existe.
- [ ] Criar uma branch, fazer commits nela, e trazê-la de volta para a `main` com merge.
- [ ] Resolver um conflito de merge manualmente.
- [ ] Explicar a diferença entre `git pull` e `git fetch`.
- [ ] Explicar por que um Pull Request existe, além de só "juntar código".

## O que vem no próximo nível

Você agora sabe garantir, de forma automatizada, que seu código continua correto conforme muda, e sabe colaborar em um repositório Git do jeito que times profissionais trabalham. No [Nível 6 - HTTP, REST e APIs](./NIVEL_6_HTTP_REST_APIS.md), o foco muda para como sistemas diferentes conversam entre si pela internet — o primeiro passo antes de construir uma API de verdade.
