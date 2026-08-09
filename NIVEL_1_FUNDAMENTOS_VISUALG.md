# Nível 1 - Fundamentos com VisuAlg

Antes de qualquer linguagem de programação real, existe uma pergunta mais básica: **como transformar um problema do mundo real em uma sequência de passos que outra coisa — uma pessoa, uma máquina — consiga seguir sem ambiguidade?** Este nível ensina isso usando VisuAlg, uma ferramenta pensada para praticar lógica em português, sem a sintaxe rígida de uma linguagem de produção no caminho.

---

## Índice de progresso

- [ ] [Objetivo do nível](#objetivo-do-nível)
- [ ] [Uso responsável de IA](#uso-responsável-de-ia)
- [ ] [O que é um algoritmo](#o-que-é-um-algoritmo)
- [ ] [O que é programar](#o-que-é-programar)
- [ ] [Por que aprender lógica antes de uma linguagem real](#por-que-aprender-lógica-antes-de-uma-linguagem-real)
- [ ] [Instalação e primeiro programa](#instalação-e-primeiro-programa)
- [ ] [Estrutura de um algoritmo VisuAlg](#estrutura-de-um-algoritmo-visualg)
- [ ] [Variáveis: memória com nome](#variáveis-memória-com-nome)
- [ ] [Tipos de dado](#tipos-de-dado)
- [ ] [Atribuição x comparação](#atribuição-x-comparação)
- [ ] [Entrada e saída](#entrada-e-saída)
- [ ] [Operadores](#operadores)
- [ ] [Estruturas condicionais](#estruturas-condicionais)
- [ ] [Estrutura condicional de múltipla escolha](#estrutura-condicional-de-múltipla-escolha)
- [ ] [Estruturas de repetição](#estruturas-de-repetição)
- [ ] [Repita...até: repetição com teste no final](#repitaaté-repetição-com-teste-no-final)
- [ ] [Vetores](#vetores)
- [ ] [Matrizes, rapidamente](#matrizes-rapidamente)
- [ ] [Funções e procedimentos](#funções-e-procedimentos)
- [ ] [Escopo de variáveis](#escopo-de-variáveis)
- [ ] [Recursão no VisuAlg](#recursão-no-visualg)
- [ ] [Erros comuns no VisuAlg](#erros-comuns-no-visualg)
- [ ] [Glossário rápido](#glossário-rápido)
- [ ] [Recapitulando](#recapitulando)
- [ ] [O que vem no próximo nível](#o-que-vem-no-próximo-nível)

---

## Objetivo do nível

Ao final deste nível, o estudante deve conseguir:

- Explicar o que é um algoritmo com um exemplo do dia a dia, sem envolver código.
- Declarar variáveis com o tipo correto para cada situação, e explicar a diferença entre atribuição e comparação.
- Escrever algoritmos usando condições, repetições (`para`, `enquanto`, `repita...até`), vetores e funções.
- Explicar a diferença entre uma função e um procedimento, e o que é escopo de variável.
- Escrever uma função recursiva simples, identificando o caso base e explicando por que ele é obrigatório.

## Uso responsável de IA

Não use IA para gerar o código final dos exercícios. Use para entender erros, revisar uma tentativa já feita, ou pedir uma explicação alternativa de um conceito que não ficou claro. O objetivo deste nível não é memorizar sintaxe do VisuAlg — é treinar o raciocínio de quebrar um problema em passos, e isso só se desenvolve tentando, errando e corrigindo, não copiando uma solução pronta.

## O que é um algoritmo

Um **algoritmo** é uma sequência finita de passos, bem definidos e sem ambiguidade, que resolve um problema ou realiza uma tarefa. Essa definição não tem nada de específico a computadores — algoritmos existem no mundo todo, o tempo todo:

- Uma receita de bolo é um algoritmo: passos ordenados (misture, asse, espere) que, seguidos corretamente, produzem um resultado esperado.
- Um manual de "como trocar um pneu" é um algoritmo: levante o carro, solte os parafusos, troque o pneu, aperte os parafusos, abaixe o carro — na ordem certa, ou o resultado não funciona (soltar todos os parafusos antes de levantar o carro é uma ordem errada, e perigosa).
- As instruções de montagem de um móvel são um algoritmo.

O que diferencia um algoritmo de uma lista qualquer de instruções é a ausência de ambiguidade: cada passo precisa ser claro o suficiente para que qualquer pessoa (ou máquina) capaz de seguir instruções chegue ao mesmo resultado. "Asse até dar uma olhada boa" é ambíguo. "Asse por 40 minutos a 180°C" não é.

## O que é programar

**Programar** é escrever um algoritmo em uma linguagem que um computador consegue executar. A lógica por trás — os passos, as decisões, as repetições — é a mesma coisa que você já usa para explicar uma receita para alguém. O que muda é a exigência de precisão: um computador não interpreta intenção, só executa exatamente o que foi escrito, sem preencher lacunas com bom senso.

Isso explica por que erros de programação, para quem está começando, costumam vir de detalhes que uma pessoa ignoraria naturalmente — uma condição que esqueceu de cobrir um caso, uma repetição que nunca para, uma variável usada antes de receber um valor. Todos esses erros já existiam na lógica antes de qualquer linguagem entrar em cena — é por isso que praticar lógica isoladamente, como este nível propõe, importa tanto quanto aprender sintaxe depois.

## Por que aprender lógica antes de uma linguagem real

Uma linguagem de programação real, como Java (Nível 2), exige resolver dois problemas ao mesmo tempo: a lógica do algoritmo (o que fazer) e a sintaxe da linguagem (como escrever isso exatamente, com chaves, ponto e vírgula, tipos declarados de um jeito específico). Para quem nunca programou, misturar os dois problemas de uma vez torna difícil saber se um erro é de raciocínio ou só de sintaxe.

**VisuAlg** é uma ferramenta educacional que usa pseudocódigo em português, com uma sintaxe simplificada e mensagens de erro mais diretas, para isolar o primeiro problema (lógica) do segundo (sintaxe de uma linguagem real). Ele não é usado para construir sistemas de verdade — nenhuma empresa roda VisuAlg em produção — mas é amplamente usado em cursos introdutórios exatamente por essa razão pedagógica: treinar o raciocínio antes de adicionar a complexidade de uma linguagem real por cima dele.

## Instalação e primeiro programa

Use o **VisuAlg desktop** (Windows, mais comum em cursos) ou o **VisuAlg Online** (roda no navegador, sem instalação, útil se você não estiver no Windows). Depois de instalado ou aberto, o primeiro programa para copiar e colar:

```visualg
algoritmo "OlaMundo"
// Define o nome do algoritmo.

inicio
   // Início dos comandos.
   escreval("Ola, mundo!")
   // Imprime uma mensagem.
fimalgoritmo
// Fim do algoritmo.
```

Para executar, use o atalho **F9** (ou o botão de "play" na barra de ferramentas). O VisuAlg abre uma janela separada mostrando a saída do programa — é nessa janela que `escreval` imprime o texto.

## Estrutura de um algoritmo VisuAlg

```visualg
algoritmo "Nome"
var
   // variáveis
inicio
   // comandos
fimalgoritmo
```

- `algoritmo`: nome do programa, entre aspas.
- `var`: bloco onde toda variável usada no programa precisa ser declarada antes de `inicio`.
- `inicio` / `fimalgoritmo`: delimitam onde a execução realmente começa e termina — só o que está entre essas duas palavras é executado, na ordem em que aparece.
- `//`: comentário. Tudo depois de `//` na mesma linha é ignorado pelo VisuAlg, servindo só para quem está lendo o código.

Essa estrutura fixa (nome, variáveis, comandos) é a mesma de praticamente qualquer programa que você vai escrever daqui para frente, em qualquer linguagem: primeiro se declara o que vai ser usado, depois se define o que fazer com isso.

## Variáveis: memória com nome

Uma **variável** é um espaço nomeado de memória, usado para guardar um valor que pode ser lido e alterado durante a execução do programa. Pense nela como uma caixa com uma etiqueta: a etiqueta (o nome da variável) não muda, mas o que está dentro da caixa (o valor) pode ser trocado a qualquer momento.

```visualg
var idade: inteiro
```

Essa linha cria uma caixa chamada `idade`, reservada para guardar apenas números inteiros — o VisuAlg não deixa você colocar um texto dentro dela depois, porque o tipo já foi fixado na declaração. Isso não é uma limitação arbitrária: é o que permite ao VisuAlg (e a qualquer linguagem tipada, como Java, no Nível 2) detectar erros antes mesmo de o programa rodar, em vez de descobrir só quando algo já quebrou no meio da execução.

## Tipos de dado

O VisuAlg tem quatro tipos primitivos, cada um reservado para uma categoria de valor:

| Tipo | Guarda | Exemplo de valor |
|---|---|---|
| `inteiro` | Números sem casas decimais | `18`, `-3`, `0` |
| `real` | Números com casas decimais | `10.50`, `-2.3` |
| `caractere` | Texto (uma letra ou uma cadeia inteira, apesar do nome) | `"Ana"`, `"a"` |
| `logico` | Verdadeiro ou falso | `verdadeiro`, `falso` |

```visualg
algoritmo "Tipos"
var
   idade: inteiro
   preco: real
   nome: caractere
   ativo: logico
inicio
   nome <- "Ana"
   idade <- 20
   preco <- 10.50
   ativo <- verdadeiro

   escreval(nome)
   escreval(idade)
fimalgoritmo
```

Escolher o tipo errado para um dado tem consequência prática: declarar `idade` como `real` quando ela deveria ser sempre um número inteiro permite valores sem sentido no domínio do problema (uma idade de `18.5` anos não faz sentido na maioria dos contextos). Esse mesmo cuidado — escolher o tipo que reflete o que o dado realmente representa — reaparece, com peso ainda maior, na escolha de tipos de coluna em banco de dados no [Nível 3](./NIVEL_3_BANCO_DE_DADOS.md#tipos-de-dado).

## Atribuição x comparação

Um dos erros mais comuns de quem está começando é confundir **atribuir** um valor com **comparar** dois valores — no VisuAlg, os símbolos são propositalmente diferentes, exatamente para evitar essa confusão:

```visualg
idade <- 18      // atribuição: guarda 18 dentro da variável idade
idade = 18       // comparação: pergunta "idade é igual a 18?", devolve verdadeiro ou falso
```

- `<-`: **atribuição**. Pega o valor do lado direito e guarda na variável do lado esquerdo. Depois dessa linha, `idade` passa a valer `18`.
- `=`: **comparação de igualdade**, usada dentro de condições (próxima seção), nunca para guardar um valor.

> **Pare e pense:** o que `idade = 18` faz sozinho, em uma linha, fora de uma condição `se`?
>
> <details><summary>Ver resposta</summary>
>
> Nada de útil por si só — `=` calcula se `idade` é igual a `18` e produz um valor lógico (`verdadeiro` ou `falso`), mas se essa comparação não estiver dentro de um `se` (ou outra estrutura que use esse resultado), o valor calculado é descartado, sem efeito nenhum no programa. Esse é um erro sutil: o VisuAlg não acusa erro de sintaxe, o programa só "não faz nada" onde deveria tomar uma decisão.
> </details>

## Entrada e saída

Um programa que sempre produz o mesmo resultado, sem depender de nada externo, tem utilidade limitada. **Entrada** é como um programa recebe dados de fora (do usuário, tipicamente); **saída** é como ele devolve um resultado.

```visualg
algoritmo "Entrada"
var nome: caractere
inicio
   escreva("Digite seu nome: ")
   leia(nome)
   escreval("Ola, ", nome)
fimalgoritmo
```

- `leia(variavel)`: pausa a execução esperando o usuário digitar um valor, que é guardado na variável indicada.
- `escreva(...)`: imprime na tela, sem pular linha ao final — útil para uma mensagem seguida de uma entrada na mesma linha, como no exemplo acima.
- `escreval(...)`: imprime na tela e pula linha ao final — mais comum para mensagens de resultado.

## Operadores

### Aritméticos

| Operador | Operação |
|---|---|
| `+` | Soma |
| `-` | Subtração |
| `*` | Multiplicação |
| `/` | Divisão (sempre resulta em `real`) |
| `div` | Divisão inteira (descarta a parte decimal) |
| `mod` | Resto da divisão |
| `^` | Potenciação |

```visualg
escreval(7 / 2)     // 3.5
escreval(7 div 2)   // 3
escreval(7 mod 2)   // 1
```

### Relacionais

| Operador | Significado |
|---|---|
| `=` | Igual |
| `<>` | Diferente |
| `>`, `<` | Maior, menor |
| `>=`, `<=` | Maior ou igual, menor ou igual |

### Lógicos

| Operador | Significado |
|---|---|
| `e` | Verdadeiro só se os dois lados forem verdadeiros |
| `ou` | Verdadeiro se pelo menos um lado for verdadeiro |
| `nao` | Inverte o valor lógico |

```visualg
se (idade >= 18) e (possuiCarteira = verdadeiro) entao
   escreval("Pode dirigir")
fimse
```

### Precedência

Operadores aritméticos são calculados antes dos relacionais, que são calculados antes dos lógicos — a mesma ordem intuitiva da matemática básica. Na dúvida, parênteses explícitos, como no exemplo acima, deixam a ordem clara para quem lê, sem depender de decorar a tabela de precedência.

## Estruturas condicionais

Um algoritmo que sempre executa os mesmos passos, sem nunca decidir nada, resolve poucos problemas reais. **Condições** permitem que um algoritmo escolha um caminho entre vários, dependendo do estado atual dos dados.

```visualg
se idade >= 18 entao
   escreval("Maior")
senao
   escreval("Menor")
fimse
```

- `se ... entao`: executa o bloco seguinte só se a condição for verdadeira.
- `senao`: bloco alternativo, executado quando a condição é falsa. Opcional — um `se` sem `senao` simplesmente não faz nada quando a condição é falsa.

Condições podem ser aninhadas, para representar decisões com mais de duas alternativas:

```visualg
se media >= 7 entao
   escreval("Aprovado")
senao
   se media >= 5 entao
      escreval("Recuperacao")
   senao
      escreval("Reprovado")
   fimse
fimse
```

## Estrutura condicional de múltipla escolha

Quando uma mesma variável é comparada contra vários valores possíveis, encadear `se/senao` repetidamente fica repetitivo e difícil de ler. O VisuAlg oferece `escolha`, equivalente ao que outras linguagens chamam de `switch`:

```visualg
escolha diaDaSemana
   caso 1
      escreval("Domingo")
   caso 2
      escreval("Segunda")
   caso 3, 4, 5, 6
      escreval("Meio da semana")
   outrocaso
      escreval("Sabado")
fimescolha
```

- `caso valor`: compara `diaDaSemana` contra o valor indicado, executando o bloco correspondente se bater.
- `caso 3, 4, 5, 6`: um único `caso` pode agrupar vários valores que levam ao mesmo resultado.
- `outrocaso`: bloco padrão, executado se nenhum dos casos anteriores bateu — equivalente ao `senao` de um `se`.

`escolha` só compara igualdade contra valores fixos — para condições envolvendo faixas (`>=`, `<`) ou combinações lógicas (`e`, `ou`), `se/senao` continua sendo a ferramenta correta.

## Estruturas de repetição

Repetir um bloco de código copiando e colando ele várias vezes é frágil (corrigir um erro exige lembrar de corrigir em cada cópia) e não escala (não dá para "copiar e colar" 10 mil vezes). **Estruturas de repetição** (loops) resolvem isso, executando o mesmo bloco várias vezes sem duplicar o código.

### Para: repetição com número de vezes conhecido

```visualg
para i de 1 ate 10 faca
   escreval(i)
fimpara
```

Use `para` quando o número de repetições é conhecido de antemão (ou calculável antes do laço começar) — percorrer os números de 1 a 10, processar cada posição de um vetor de tamanho fixo, repetir algo exatamente `n` vezes.

```visualg
para i de 10 ate 1 passo -1 faca
   escreval(i)
fimpara
```

`passo` controla o incremento a cada repetição — negativo, como acima, faz o laço contar regressivamente.

### Enquanto: repetição com teste no início

```visualg
enquanto senha <> "1234" faca
   leia(senha)
fimenquanto
```

Use `enquanto` quando o número de repetições não é conhecido de antemão — depende de uma condição que só pode ser avaliada durante a execução (aqui, quantas vezes o usuário vai errar a senha é imprevisível). A condição é testada **antes** de cada repetição, inclusive da primeira: se `senha` já começar como `"1234"`, o bloco dentro do `enquanto` nunca executa nenhuma vez.

## Repita...até: repetição com teste no final

Existe uma terceira forma de repetição, com uma diferença sutil mas importante: `repita...até` testa a condição **depois** de cada repetição, garantindo que o bloco execute **pelo menos uma vez**, mesmo que a condição já comece satisfeita.

```visualg
repita
   escreva("Digite a senha: ")
   leia(senha)
ate senha = "1234"
```

Compare com a versão usando `enquanto`, que exigiria uma leitura duplicada (uma antes do laço, só para ter um valor inicial para testar, e outra dentro dele) para produzir o mesmo comportamento de "pedir a senha pelo menos uma vez". `repita...até` existe exatamente para esse padrão: quando o bloco sempre precisa rodar ao menos uma vez antes de qualquer decisão de parar.

| Estrutura | Quando testa a condição | Executa ao menos uma vez? |
|---|---|---|
| `para` | Controle automático pelo intervalo | Sim, se o intervalo for válido |
| `enquanto` | Antes de cada repetição | Não, necessariamente |
| `repita...até` | Depois de cada repetição | Sim, sempre |

> **Pare e pense:** um menu de programa que deve aparecer pelo menos uma vez, e continuar aparecendo até o usuário escolher "sair", deveria usar `enquanto` ou `repita...até`?
>
> <details><summary>Ver resposta</summary>
>
> `repita...até`. Um menu precisa aparecer na tela pelo menos uma vez, mesmo antes de qualquer escolha do usuário existir para testar — exatamente o comportamento que `repita...até` garante por construção. Usar `enquanto` exigiria duplicar a exibição do menu (uma vez antes do laço, outra dentro dele) só para ter algo a testar na condição inicial.
> </details>

## Vetores

Imagine precisar guardar as notas de 5 alunos. Declarar cinco variáveis separadas (`nota1`, `nota2`, `nota3`, `nota4`, `nota5`) funciona, mas quebra assim que o número de alunos muda, e não permite processar todas com um único laço `para` — cada uma precisaria ser tratada individualmente, no código.

Um **vetor** é uma coleção de valores do mesmo tipo, acessados por posição (índice), guardados sob um único nome:

```visualg
var notas: vetor[1..5] de real
```

```visualg
algoritmo "Vetores"
var
   notas: vetor[1..5] de real
   i: inteiro
inicio
   para i de 1 ate 5 faca
      escreva("Nota do aluno ", i, ": ")
      leia(notas[i])
   fimpara

   para i de 1 ate 5 faca
      escreval("Nota ", i, ": ", notas[i])
   fimpara
fimalgoritmo
```

`notas[i]` acessa a posição `i` do vetor. No VisuAlg, por padrão, os índices começam em `1` (diferente de Java, que você vai ver no Nível 2, onde índices de array começam em `0` — vale já notar essa diferença, para não estranhar depois). Combinar vetor com `para` é o padrão mais comum de uso: o laço percorre cada posição, uma de cada vez, sem precisar de uma linha de código por elemento.

## Matrizes, rapidamente

Uma **matriz** é um vetor de duas dimensões — útil para representar dados organizados em linhas e colunas, como um tabuleiro de jogo ou uma planilha simples.

```visualg
var tabuleiro: vetor[1..3, 1..3] de caractere
```

```visualg
para linha de 1 ate 3 faca
   para coluna de 1 ate 3 faca
      tabuleiro[linha, coluna] <- "-"
   fimpara
fimpara
```

Percorrer uma matriz exige dois laços `para` aninhados, um para cada dimensão — o de fora controlando a linha, o de dentro controlando a coluna. Matrizes aparecem com menos frequência que vetores simples nos exercícios deste roadmap, mas vale reconhecer a estrutura: ela é a base de qualquer representação bidimensional de dados.

## Funções e procedimentos

Conforme um algoritmo cresce, repetir a mesma lógica em vários pontos (calcular uma média em três lugares diferentes do código, por exemplo) tem os mesmos problemas já vistos com repetição de código: corrigir um bug exige lembrar de corrigir em todos os lugares. **Funções** e **procedimentos** resolvem isso, isolando um bloco de lógica reutilizável sob um nome.

### Função: devolve um valor

```visualg
funcao dobro(n: inteiro): inteiro
inicio
   dobro <- n * 2
fimfuncao
```

```visualg
escreval(dobro(5))   // 10
```

Uma função recebe parâmetros (`n`, aqui), executa um cálculo, e devolve um valor — a própria linha `dobro <- n * 2` atribui o resultado ao "nome" da função, que é como o VisuAlg representa o valor de retorno.

### Procedimento: não devolve valor

```visualg
procedimento saudar(nome: caractere)
inicio
   escreval("Ola, ", nome, "!")
fimprocedimento
```

```visualg
saudar("Ana")
```

Um procedimento executa uma ação (aqui, imprimir uma mensagem), mas não devolve nenhum valor que possa ser usado em uma expressão depois. A escolha entre função e procedimento depende de uma pergunta simples: **essa operação produz um valor que outra parte do código vai usar?** Se sim, função. Se a operação só realiza uma ação (imprimir, alterar algo), procedimento.

## Escopo de variáveis

**Escopo** é a região do código onde uma variável existe e pode ser usada. Uma variável declarada dentro de uma função só existe dentro dela — é uma variável **local**, inacessível de fora.

```visualg
funcao calcularQuadrado(n: inteiro): inteiro
var
   resultado: inteiro
inicio
   resultado <- n * n
   calcularQuadrado <- resultado
fimfuncao
```

A variável `resultado`, declarada dentro da função, deixa de existir assim que a função termina — tentar usá-la fora da função seria um erro. Isso é uma proteção, não uma limitação: sem escopo local, toda variável usada em qualquer função poderia acidentalmente colidir com uma variável de mesmo nome usada em outro lugar do programa, um tipo de bug difícil de rastrear.

Variáveis declaradas no bloco `var` principal do algoritmo, fora de qualquer função, são **globais** — visíveis de qualquer lugar do programa, incluindo de dentro das funções. Usar variáveis globais com moderação é uma boa prática mesmo no VisuAlg: quanto mais uma função depende de estado externo a ela, mais difícil fica prever o que ela faz só olhando sua própria definição — o mesmo princípio de isolamento que vai reaparecer, com mais peso, na discussão de [testabilidade como sinal de design](./NIVEL_5_TESTES_E_GIT.md#testabilidade-como-sinal-de-design) no Nível 5.

## Recursão no VisuAlg

**Recursão** é quando uma função chama a si mesma para resolver uma versão menor do mesmo problema, até chegar em um caso simples o bastante para responder diretamente — o **caso base**.

```visualg
algoritmo "Fatorial"

funcao fatorial(n: inteiro): inteiro
inicio
   se n <= 1 entao
      fatorial <- 1
   senao
      fatorial <- n * fatorial(n - 1)
   fimse
fimfuncao

var numero: inteiro
inicio
   leia(numero)
   escreval(fatorial(numero))
fimalgoritmo
```

Para `fatorial(4)`, a execução se desenrola assim:

```text
fatorial(4) = 4 * fatorial(3)
fatorial(3) = 3 * fatorial(2)
fatorial(2) = 2 * fatorial(1)
fatorial(1) = 1                  <- caso base, para de chamar a si mesma

Voltando:
fatorial(2) = 2 * 1 = 2
fatorial(3) = 3 * 2 = 6
fatorial(4) = 4 * 6 = 24
```

O **caso base** (`n <= 1`) é o que impede a recursão de chamar a si mesma para sempre. Sem ele, `fatorial(4)` chamaria `fatorial(3)`, que chamaria `fatorial(2)`, e assim por diante, indefinidamente — cada chamada empilhada sobre a anterior, até esgotar a memória disponível para essa pilha de chamadas (o que, em Java, no Nível 2, gera um erro específico, `StackOverflowError`, exatamente por esse motivo).

> **Pare e pense:** o que aconteceria se a condição do caso base fosse escrita como `n = 1` em vez de `n <= 1`, e a função fosse chamada como `fatorial(0)`?
>
> <details><summary>Ver resposta</summary>
>
> A recursão nunca encontraria o caso base: `fatorial(0)` chamaria `fatorial(-1)`, que chamaria `fatorial(-2)`, e assim por diante, para sempre, porque nenhum desses valores é exatamente igual a `1`. `n <= 1` cobre corretamente tanto `n = 1` quanto `n = 0` (cujo fatorial, por definição matemática, também é `1`), além de proteger contra entradas negativas não previstas. Esse é um exemplo real de por que o caso base precisa ser pensado com cuidado, cobrindo toda a faixa de entrada possível, não só o valor "esperado".
> </details>

## Erros comuns no VisuAlg

**"variavel não foi declarada"**
Toda variável usada precisa aparecer no bloco `var` antes de `inicio`. Esse erro geralmente é digitação (nome da variável escrito diferente do que foi declarado) ou esquecimento mesmo da declaração.

**Laço `enquanto` que nunca termina**
Sinal de que a condição do `enquanto` nunca se torna falsa — geralmente porque a variável testada na condição nunca é alterada dentro do laço. Revise se toda variável envolvida na condição de parada está sendo atualizada a cada repetição.

**Confundir `<-` com `=`**
Já discutido na seção sobre [atribuição x comparação](#atribuição-x-comparação) — um erro silencioso, sem mensagem de erro do VisuAlg, que só aparece como comportamento errado do programa.

**Índice de vetor fora do intervalo declarado**
Acessar `notas[6]` em um vetor declarado como `vetor[1..5]` gera erro em tempo de execução. Confira se os limites do `para` usado para percorrer o vetor batem exatamente com os limites declarados.

## Glossário rápido

| Termo | Definição curta |
|---|---|
| Algoritmo | Sequência finita de passos, sem ambiguidade, que resolve um problema |
| Variável | Espaço nomeado de memória, com um tipo fixo, que guarda um valor |
| Atribuição (`<-`) | Guarda um valor dentro de uma variável |
| Comparação (`=`) | Verifica se dois valores são iguais, dentro de uma condição |
| Condição | Estrutura que escolhe um caminho de execução com base em um teste lógico |
| Repetição (loop) | Estrutura que executa um bloco de código várias vezes |
| Vetor | Coleção de valores do mesmo tipo, acessados por posição |
| Matriz | Vetor de duas dimensões |
| Função | Bloco de código reutilizável que devolve um valor |
| Procedimento | Bloco de código reutilizável que não devolve valor |
| Escopo | Região do código onde uma variável existe e pode ser usada |
| Recursão | Uma função chamando a si mesma, até atingir um caso base |
| Caso base | Condição que impede uma função recursiva de chamar a si mesma indefinidamente |

## Recapitulando

- [ ] Explicar o que é um algoritmo com um exemplo que não seja de programação.
- [ ] Explicar a diferença entre `<-` e `=`, e o que acontece se um for usado no lugar do outro.
- [ ] Escrever uma condição com `se/senao` e uma com `escolha`, explicando quando cada uma é mais apropriada.
- [ ] Explicar a diferença entre `para`, `enquanto` e `repita...até`, e escolher a estrutura certa para um cenário dado.
- [ ] Percorrer um vetor com um laço `para`, lendo e depois exibindo seus valores.
- [ ] Explicar a diferença entre função e procedimento, e o que é escopo de variável.
- [ ] Escrever uma função recursiva simples, identificando o caso base.

## O que vem no próximo nível

Você agora tem o vocabulário de lógica que qualquer linguagem de programação real vai usar por baixo de uma sintaxe diferente — condição, repetição, vetor, função, recursão. No [Nível 2 - Desenvolvedor Java](./NIVEL_2_DESENVOLVEDOR_JAVA.md), cada um desses conceitos reaparece, agora com a sintaxe, o rigor de tipos e as ferramentas de uma linguagem usada em produção de verdade.
