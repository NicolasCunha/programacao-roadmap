# Roadmap de Estudos: Do Zero ao Desenvolvedor Java

> **Objetivo:** orientar uma pessoa sem conhecimento prévio em tecnologia a construir uma base sólida em programação, começando por lógica com **VisuAlg/Portugol**, evoluindo para Java, orientação a objetos, banco de dados, testes, boas práticas, padrões de projeto, Spring Boot e projetos reais.

Este roadmap foi desenhado para ser **suave, estável e sem pressão de prazo**. A ideia é que o estudante avance quando conseguir explicar, praticar e aplicar cada assunto, não quando uma data chegar.

---

# Índice de Progresso

Use este índice como checklist. Marque o que já estudou e use os links para voltar ao ponto em que parou.

## Preparação

- [ ] [Como usar este roadmap](#como-usar-este-roadmap)
- [ ] [Como estudar sem pressa](#como-estudar-sem-pressa)
- [ ] [Visão geral da jornada](#visão-geral-da-jornada)

## Nível 1 - Fundamentos com VisuAlg

- [ ] [Por que começar com VisuAlg](#por-que-começar-com-visualg)
- [ ] [Como baixar e executar o VisuAlg](#como-baixar-e-executar-o-visualg)
- [ ] [Primeiro programa: Olá Mundo](#primeiro-programa-olá-mundo)
- [ ] [O que é um algoritmo](#o-que-é-um-algoritmo)
- [ ] [Estrutura básica do VisuAlg](#estrutura-básica-do-visualg)
- [ ] [Comentários](#comentários)
- [ ] [Bloco VAR](#bloco-var)
- [ ] [Tipos de dados](#tipos-de-dados)
- [ ] [Atribuição de valores](#atribuição-de-valores)
- [ ] [Comandos de saída](#comandos-de-saída)
- [ ] [Comandos de entrada](#comandos-de-entrada)
- [ ] [Strings, caracteres e aspas](#strings-caracteres-e-aspas)
- [ ] [Parênteses e ordem de operações](#parênteses-e-ordem-de-operações)
- [ ] [Operadores aritméticos](#operadores-aritméticos)
- [ ] [Operadores relacionais](#operadores-relacionais)
- [ ] [Operadores lógicos](#operadores-lógicos)
- [ ] [Condicionais](#condicionais)
- [ ] [Repetição com PARA](#repetição-com-para)
- [ ] [Repetição com ENQUANTO](#repetição-com-enquanto)
- [ ] [Vetores](#vetores)
- [ ] [Matrizes](#matrizes)
- [ ] [Procedimentos](#procedimentos)
- [ ] [Funções](#funções)
- [ ] [Mini-projetos em VisuAlg](#mini-projetos-em-visualg)
- [ ] [Erros comuns no VisuAlg](#erros-comuns-no-visualg)

## Nível 2 - Desenvolvedor Java

- [ ] [Ponte entre VisuAlg e Java](#ponte-entre-visualg-e-java)
- [ ] [Preparando o ambiente Java](#preparando-o-ambiente-java)
- [ ] [Primeiro programa em Java](#primeiro-programa-em-java)
- [ ] [Tipos, variáveis e entrada de dados em Java](#tipos-variáveis-e-entrada-de-dados-em-java)
- [ ] [Condicionais e repetições em Java](#condicionais-e-repetições-em-java)
- [ ] [Classes, objetos e métodos](#classes-objetos-e-métodos)
- [ ] [Encapsulamento](#encapsulamento)
- [ ] [Collections](#collections)
- [ ] [Exceções](#exceções)
- [ ] [Lambdas, Streams e Optional](#lambdas-streams-e-optional)
- [ ] [Orientação a objetos](#orientação-a-objetos)
- [ ] [SOLID](#solid)

## Nível 3 - Dados, Qualidade e Mercado

- [ ] [Banco de dados e SQL](#banco-de-dados-e-sql)
- [ ] [Testes automatizados](#testes-automatizados)
- [ ] [Git e GitHub](#git-e-github)
- [ ] [Java Efetivo](#java-efetivo)
- [ ] [Design Patterns](#design-patterns)
- [ ] [Spring Boot e APIs REST](#spring-boot-e-apis-rest)
- [ ] [Projeto final e portfólio](#projeto-final-e-portfólio)

---

# Como Usar Este Roadmap

Este documento não é apenas uma lista de assuntos. Ele também funciona como uma introdução prática para que o estudante consiga pesquisar fora com mais segurança.

A cada tema, o estudante deve tentar responder:

1. O que é isso?
2. Para que serve?
3. Onde aparece em um sistema real?
4. Consigo escrever um exemplo pequeno?
5. Consigo explicar para outra pessoa?

Se a resposta for “não”, não é problema. Significa apenas que aquele tema merece mais prática.

---

# Como Estudar Sem Pressa

Evite tratar o roadmap como uma corrida. Uma pessoa iniciante precisa de tempo para errar, testar, apagar, reescrever e entender.

Uma boa rotina é:

- Ler o conceito.
- Copiar um exemplo funcional.
- Executar.
- Alterar pequenos pontos.
- Quebrar o código de propósito.
- Corrigir.
- Criar um exercício parecido do zero.
- Salvar no GitHub.

O objetivo não é decorar. O objetivo é construir raciocínio.

---

# Visão Geral da Jornada

```text
NÍVEL 1 - Fundamentos com VisuAlg
  Preparação do ambiente
  Conceitos básicos de algoritmo
  Variáveis, entrada e saída
  Condições e repetições
  Vetores, matrizes, procedimentos e funções
  Mini-projetos em Portugol

NÍVEL 2 - Desenvolvedor Java
  Sintaxe Java
  Classes e objetos
  Orientação a objetos
  Collections, exceções, streams e boas práticas

NÍVEL 3 - Dados, Qualidade e Mercado
  Banco de dados
  Testes automatizados
  Git e GitHub
  Java Efetivo
  Design Patterns
  Spring Boot e APIs REST
  Projeto final de portfólio
```

---

# Nível 1 - Fundamentos com VisuAlg

## Por que começar com VisuAlg

O VisuAlg é uma ferramenta usada para estudar lógica de programação com uma linguagem próxima do português, normalmente chamada de **Portugol**.

Para uma primeira linguagem, ele é uma escolha muito boa porque permite focar no raciocínio antes de lidar com detalhes mais pesados de linguagens profissionais.

Em Java, um primeiro programa exige entender `class`, `public static void main`, chaves, tipos, importações e ponto e vírgula. No VisuAlg, o estudante começa com uma estrutura mais direta:

```visualg
algoritmo "MeuPrograma"
inicio
   escreval("Ola, mundo!")
fimalgoritmo
```

A lógica aprendida aqui será reaproveitada em Java, JavaScript, Python, C# ou qualquer outra linguagem.

---

## Como Baixar e Executar o VisuAlg

### Opção 1 - VisuAlg no computador

1. Acesse a página do VisuAlg 3.0 no SourceForge:  
   <https://sourceforge.net/projects/visualg30/>

2. Clique em **Download**.

3. Extraia o arquivo baixado.

4. Abra a pasta extraída.

5. Execute o arquivo do VisuAlg, normalmente com nome parecido com:

```text
visualg.exe
```

ou

```text
VisuAlg3.exe
```

6. Quando o programa abrir, apague qualquer texto inicial, cole o exemplo abaixo e execute.

### Exemplo para colar e executar

```visualg
algoritmo "OlaMundo"
inicio
   escreval("Ola, mundo!")
fimalgoritmo
```

Se tudo estiver correto, a saída será algo parecido com:

```text
Ola, mundo!
```

### Opção 2 - VisuAlg Online

Se não for possível instalar o programa no computador, use uma versão online:

<https://visualg.com.br/>

Nesse caso, basta abrir o site, colar o algoritmo e executar no navegador.

> Observação: algumas versões do VisuAlg podem ter comportamento diferente com acentos. No começo, se algo estranho acontecer, escreva mensagens sem acentos, como `Ola` em vez de `Olá`.

---

## Primeiro Programa: Olá Mundo

O primeiro programa clássico de quase toda linguagem de programação é o **Olá Mundo**.

Ele serve para confirmar que o ambiente está funcionando.

```visualg
algoritmo "OlaMundo"
// A linha acima define o nome do algoritmo.
// O nome fica entre aspas duplas.

inicio
   // A palavra "inicio" indica onde os comandos começam.

   escreval("Ola, mundo!")
   // escreval mostra uma mensagem na tela e pula uma linha.
   // A mensagem precisa estar entre aspas duplas.

fimalgoritmo
// A palavra "fimalgoritmo" indica que o algoritmo terminou.
```

Neste primeiro momento, o estudante não precisa entender tudo profundamente. O mais importante é:

- Colar o código.
- Executar.
- Ver a mensagem na tela.
- Alterar a mensagem.
- Executar novamente.

### Exercício simples

Troque a frase `Ola, mundo!` por uma apresentação pessoal.

Exemplo:

```visualg
algoritmo "MinhaApresentacao"
inicio
   escreval("Meu nome e Ana.")
   escreval("Estou aprendendo logica de programacao.")
fimalgoritmo
```

---

## O que é um Algoritmo

Um algoritmo é uma sequência de passos para resolver um problema.

Exemplo fora da tecnologia:

```text
Algoritmo para fazer café:
1. Colocar água na cafeteira.
2. Colocar o pó no filtro.
3. Ligar a cafeteira.
4. Esperar ficar pronto.
5. Servir.
```

Na programação, fazemos algo parecido, mas com dados e comandos.

Exemplo de problema:

> Calcular a média de duas notas.

Passos:

```text
1. Ler a primeira nota.
2. Ler a segunda nota.
3. Somar as notas.
4. Dividir por 2.
5. Mostrar a média.
```

Só depois de entender os passos é que escrevemos o código.

---

## Estrutura Básica do VisuAlg

Antes de criar algoritmos completos, entenda as partes principais.

```visualg
algoritmo "NomeDoAlgoritmo"
// Define o nome do algoritmo.

var
   // Aqui ficam as variáveis.
   // Variáveis são espaços para guardar informações.

inicio
   // Aqui ficam os comandos que serão executados.

fimalgoritmo
// Finaliza o algoritmo.
```

### O que significa cada parte?

#### `algoritmo "NomeDoAlgoritmo"`

Define o nome do programa. O nome é colocado entre aspas duplas.

#### `var`

Inicia o bloco de declaração de variáveis. É nele que avisamos quais informações o algoritmo vai guardar.

#### `inicio`

Indica onde a execução começa.

#### `fimalgoritmo`

Indica onde a execução termina.

---

## Comentários

Comentários são textos ignorados pelo computador. Eles servem para explicar o código para pessoas.

No VisuAlg, comentários podem ser escritos com `//`.

Exemplo:

```visualg
algoritmo "ExemploComentarios"
// Este é o nome do algoritmo.

inicio
   // A linha abaixo imprime uma mensagem na tela.
   escreval("Estou aprendendo comentarios.")

   // Esta outra linha imprime uma segunda mensagem.
   escreval("Comentarios ajudam a entender o codigo.")
fimalgoritmo
```

Nos primeiros estudos, é recomendável comentar bastante. Com o tempo, os comentários devem ser usados com mais equilíbrio, explicando intenções importantes e não apenas repetindo o óbvio.

---

## Bloco VAR

O bloco `var` é usado para declarar variáveis.

Declarar uma variável significa informar ao VisuAlg:

- O nome da variável.
- O tipo de informação que ela vai guardar.

Exemplo:

```visualg
algoritmo "ExemploVar"

var
   // A variável idade guardará números inteiros.
   idade: inteiro

   // A variável nome guardará texto.
   nome: caractere

inicio
   // Aqui ainda não estamos lendo dados do usuário.
   // Estamos apenas atribuindo valores manualmente.

   idade <- 25
   nome <- "Maria"

   escreval("Nome: ", nome)
   escreval("Idade: ", idade)
fimalgoritmo
```

### Importante

Variáveis devem ser declaradas antes de serem usadas.

Errado:

```visualg
algoritmo "ErroVariavel"
inicio
   idade <- 20
   escreval(idade)
fimalgoritmo
```

Certo:

```visualg
algoritmo "VariavelCorreta"
var
   idade: inteiro
inicio
   idade <- 20
   escreval(idade)
fimalgoritmo
```

---

## Tipos de Dados

Tipos de dados indicam o tipo de informação que uma variável pode guardar.

### Tipo `inteiro`

Usado para números sem casas decimais.

Exemplos:

- Idade.
- Quantidade de produtos.
- Número de parcelas.

```visualg
algoritmo "TipoInteiro"
var
   idade: inteiro
inicio
   idade <- 30
   escreval("Idade: ", idade)
fimalgoritmo
```

### Tipo `real`

Usado para números com casas decimais.

Exemplos:

- Preço.
- Saldo.
- Média.

```visualg
algoritmo "TipoReal"
var
   preco: real
inicio
   preco <- 49.90
   escreval("Preco: R$ ", preco)
fimalgoritmo
```

### Tipo `caractere`

Usado para textos.

Exemplos:

- Nome.
- E-mail.
- CPF como texto.
- Endereço.

```visualg
algoritmo "TipoCaractere"
var
   nome: caractere
inicio
   nome <- "Joao"
   escreval("Nome: ", nome)
fimalgoritmo
```

### Tipo `logico`

Usado para valores de verdadeiro ou falso.

Exemplos:

- Produto disponível.
- Pagamento aprovado.
- Cliente ativo.

```visualg
algoritmo "TipoLogico"
var
   pagamentoAprovado: logico
inicio
   pagamentoAprovado <- verdadeiro
   escreval("Pagamento aprovado? ", pagamentoAprovado)
fimalgoritmo
```

---

## Atribuição de Valores

Atribuir um valor significa guardar uma informação dentro de uma variável.

No VisuAlg, usamos `<-` para atribuição.

```visualg
idade <- 20
nome <- "Ana"
preco <- 15.50
```

Exemplo comentado:

```visualg
algoritmo "Atribuicao"
var
   nome: caractere
   idade: inteiro
inicio
   // Guardando o texto "Carlos" dentro da variável nome.
   nome <- "Carlos"

   // Guardando o número 22 dentro da variável idade.
   idade <- 22

   // Mostrando os valores guardados.
   escreval("Nome: ", nome)
   escreval("Idade: ", idade)
fimalgoritmo
```

### Diferença entre atribuição e comparação

Atribuição usa `<-`:

```visualg
idade <- 18
```

Comparação de igualdade usa `=`:

```visualg
se idade = 18 entao
   escreval("Tem 18 anos.")
fimse
```

---

## Comandos de Saída

Comandos de saída mostram informações na tela.

### `escreva`

Mostra uma mensagem sem pular linha.

```visualg
algoritmo "ExemploEscreva"
inicio
   escreva("Nome: ")
   escreva("Maria")
fimalgoritmo
```

Saída esperada:

```text
Nome: Maria
```

### `escreval`

Mostra uma mensagem e pula linha.

```visualg
algoritmo "ExemploEscreval"
inicio
   escreval("Linha 1")
   escreval("Linha 2")
fimalgoritmo
```

Saída esperada:

```text
Linha 1
Linha 2
```

---

## Comandos de Entrada

Comandos de entrada permitem que o usuário digite dados.

No VisuAlg, usamos `leia`.

```visualg
algoritmo "ExemploLeia"
var
   nome: caractere
inicio
   // Primeiro, mostramos uma orientação para o usuário.
   escreva("Digite seu nome: ")

   // Depois, lemos o que ele digitou e guardamos na variável nome.
   leia(nome)

   // Por fim, mostramos uma mensagem usando o valor digitado.
   escreval("Ola, ", nome, "!")
fimalgoritmo
```

### Padrão comum

Na maioria das vezes, usamos `escreva` antes de `leia` para orientar o usuário.

```visualg
escreva("Digite sua idade: ")
leia(idade)
```

---

## Strings, Caracteres e Aspas

Em programação, textos costumam ser chamados de **strings**.

No VisuAlg, textos devem ficar entre aspas duplas.

```visualg
nome <- "Maria"
escreval("Cadastro realizado")
```

Caracteres individuais também podem aparecer como texto. Em várias linguagens, caracteres isolados usam aspas simples, como `'M'`. Porém, no VisuAlg, para evitar problemas e manter consistência, use **aspas duplas** para textos e letras.

```visualg
letra <- "A"
```

### Erro comum

Errado:

```visualg
nome <- Maria
```

O VisuAlg tentará entender `Maria` como se fosse uma variável.

Certo:

```visualg
nome <- "Maria"
```

---

## Parênteses e Ordem de Operações

Parênteses ajudam a controlar a ordem dos cálculos.

Sem parênteses:

```visualg
media <- nota1 + nota2 / 2
```

Esse cálculo faz primeiro `nota2 / 2`, depois soma `nota1`.

Com parênteses:

```visualg
media <- (nota1 + nota2) / 2
```

Agora o algoritmo soma as duas notas primeiro e só depois divide por 2.

Exemplo:

```visualg
algoritmo "MediaComParenteses"
var
   nota1, nota2, media: real
inicio
   nota1 <- 8
   nota2 <- 6

   // Os parênteses garantem que a soma aconteça antes da divisão.
   media <- (nota1 + nota2) / 2

   escreval("Media: ", media)
fimalgoritmo
```

---

## Operadores Aritméticos

Operadores aritméticos são usados para cálculos.

- `+` soma
- `-` subtração
- `*` multiplicação
- `/` divisão

Exemplo simples:

```visualg
algoritmo "OperadoresAritmeticos"
var
   a, b: inteiro
inicio
   a <- 10
   b <- 2

   escreval("Soma: ", a + b)
   escreval("Subtracao: ", a - b)
   escreval("Multiplicacao: ", a * b)
   escreval("Divisao: ", a / b)
fimalgoritmo
```

---

## Operadores Relacionais

Operadores relacionais comparam valores.

- `>` maior que
- `<` menor que
- `>=` maior ou igual
- `<=` menor ou igual
- `=` igual
- `<>` diferente

Exemplo:

```visualg
algoritmo "OperadoresRelacionais"
var
   idade: inteiro
inicio
   idade <- 18

   escreval("Idade maior ou igual a 18? ", idade >= 18)
   escreval("Idade diferente de 20? ", idade <> 20)
fimalgoritmo
```

---

## Operadores Lógicos

Operadores lógicos combinam condições.

- `e`: as duas condições precisam ser verdadeiras.
- `ou`: pelo menos uma condição precisa ser verdadeira.
- `nao`: inverte o resultado lógico.

Exemplo com `e`:

```visualg
algoritmo "OperadorE"
var
   idade: inteiro
   possuiDocumento: logico
inicio
   idade <- 20
   possuiDocumento <- verdadeiro

   se (idade >= 18) e (possuiDocumento = verdadeiro) entao
      escreval("Entrada permitida.")
   senao
      escreval("Entrada bloqueada.")
   fimse
fimalgoritmo
```

Exemplo com `ou`:

```visualg
algoritmo "OperadorOu"
var
   idade: inteiro
inicio
   idade <- 65

   se (idade < 12) ou (idade > 60) entao
      escreval("Entrada gratuita.")
   senao
      escreval("Entrada paga.")
   fimse
fimalgoritmo
```

---

## Condicionais

Condicionais permitem que o algoritmo tome decisões.

Estrutura:

```visualg
se condicao entao
   // comandos se a condição for verdadeira
senao
   // comandos se a condição for falsa
fimse
```

Exemplo comentado:

```visualg
algoritmo "Maioridade"
var
   idade: inteiro
inicio
   // Pedimos a idade para o usuário.
   escreva("Digite sua idade: ")

   // Guardamos a idade digitada na variável idade.
   leia(idade)

   // Conferimos se a idade é maior ou igual a 18.
   se idade >= 18 entao
      escreval("Pessoa maior de idade.")
   senao
      escreval("Pessoa menor de idade.")
   fimse
fimalgoritmo
```

---

## Repetição com PARA

Use `para` quando souber quantas vezes quer repetir algo.

Estrutura:

```visualg
para variavel de inicio ate fim faca
   // comandos repetidos
fimpara
```

Exemplo comentado:

```visualg
algoritmo "ContarAteCinco"
var
   i: inteiro
inicio
   // O laço começa com i valendo 1.
   // A cada repetição, i aumenta automaticamente.
   // O laço termina quando i chegar a 5.
   para i de 1 ate 5 faca
      escreval("Valor de i: ", i)
   fimpara
fimalgoritmo
```

Exemplo real: tabuada.

```visualg
algoritmo "Tabuada"
var
   numero, i, resultado: inteiro
inicio
   escreva("Digite um numero: ")
   leia(numero)

   para i de 1 ate 10 faca
      resultado <- numero * i
      escreval(numero, " x ", i, " = ", resultado)
   fimpara
fimalgoritmo
```

---

## Repetição com ENQUANTO

Use `enquanto` quando não souber exatamente quantas repetições serão necessárias.

Estrutura:

```visualg
enquanto condicao faca
   // comandos repetidos enquanto a condição for verdadeira
fimenquanto
```

Exemplo comentado:

```visualg
algoritmo "SenhaAteAcertar"
var
   senha: caractere
inicio
   senha <- ""

   // Enquanto a senha for diferente de "1234", o algoritmo continua pedindo.
   enquanto senha <> "1234" faca
      escreva("Digite a senha: ")
      leia(senha)
   fimenquanto

   escreval("Acesso liberado.")
fimalgoritmo
```

---

## Vetores

Vetores guardam vários valores do mesmo tipo em uma única estrutura.

Pense em um vetor como uma sequência de gavetas numeradas.

```visualg
nomes[1]
nomes[2]
nomes[3]
```

Exemplo comentado:

```visualg
algoritmo "VetorNomes"
var
   nomes: vetor[1..3] de caractere
   i: inteiro
inicio
   // Lemos três nomes e guardamos nas posições 1, 2 e 3 do vetor.
   para i de 1 ate 3 faca
      escreva("Digite o nome ", i, ": ")
      leia(nomes[i])
   fimpara

   escreval("Nomes cadastrados:")

   // Agora percorremos o vetor para mostrar os nomes.
   para i de 1 ate 3 faca
      escreval(nomes[i])
   fimpara
fimalgoritmo
```

---

## Matrizes

Matrizes guardam dados em linhas e colunas.

Exemplo real:

Uma sala de cinema pode ter linhas e poltronas.

```visualg
algoritmo "MatrizCinema"
var
   lugares: vetor[1..2,1..3] de caractere
   linha, coluna: inteiro
inicio
   // Preenche todos os lugares como "Livre".
   para linha de 1 ate 2 faca
      para coluna de 1 ate 3 faca
         lugares[linha,coluna] <- "Livre"
      fimpara
   fimpara

   // Marca um lugar como ocupado.
   lugares[1,2] <- "Ocupado"

   // Mostra a situação dos lugares.
   para linha de 1 ate 2 faca
      para coluna de 1 ate 3 faca
         escreva(lugares[linha,coluna], " ")
      fimpara
      escreval("")
   fimpara
fimalgoritmo
```

---

## Procedimentos

Procedimentos são blocos reutilizáveis que executam uma ação, mas não retornam um valor.

Exemplo:

```visualg
algoritmo "ExemploProcedimento"

procedimento mostrarCabecalho
inicio
   escreval("========================")
   escreval("   SISTEMA DE ESTUDOS")
   escreval("========================")
fimprocedimento

inicio
   // Chamamos o procedimento pelo nome.
   mostrarCabecalho

   escreval("Bem-vindo!")
fimalgoritmo
```

Use procedimentos quando uma sequência de comandos se repete ou representa uma ação clara.

---

## Funções

Funções também são blocos reutilizáveis, mas retornam um valor.

Exemplo:

```visualg
algoritmo "ExemploFuncao"

funcao calcularDobro(numero: inteiro): inteiro
inicio
   calcularDobro <- numero * 2
fimfuncao

var
   valor, resultado: inteiro
inicio
   valor <- 5

   // A função calcula e devolve um resultado.
   resultado <- calcularDobro(valor)

   escreval("Dobro: ", resultado)
fimalgoritmo
```

---

## Mini-projetos em VisuAlg

Depois de entender as estruturas, o estudante pode praticar com algoritmos mais completos.

### Mini-projeto 1 - Calculadora de média

```visualg
algoritmo "CalculadoraDeMedia"
var
   nota1, nota2, media: real
inicio
   escreva("Digite a primeira nota: ")
   leia(nota1)

   escreva("Digite a segunda nota: ")
   leia(nota2)

   media <- (nota1 + nota2) / 2

   escreval("Media final: ", media)

   se media >= 6 entao
      escreval("Aluno aprovado.")
   senao
      escreval("Aluno reprovado.")
   fimse
fimalgoritmo
```

### Mini-projeto 2 - Caixa eletrônico simples

```visualg
algoritmo "CaixaEletronico"
var
   saldo, valorSaque: real
inicio
   saldo <- 1000

   escreval("Saldo atual: R$ ", saldo)
   escreva("Digite o valor do saque: ")
   leia(valorSaque)

   se valorSaque <= 0 entao
      escreval("Valor invalido.")
   senao
      se valorSaque <= saldo entao
         saldo <- saldo - valorSaque
         escreval("Saque realizado.")
         escreval("Saldo final: R$ ", saldo)
      senao
         escreval("Saldo insuficiente.")
      fimse
   fimse
fimalgoritmo
```

### Mini-projeto 3 - Menu simples

```visualg
algoritmo "MenuSimples"
var
   opcao: inteiro
inicio
   opcao <- 0

   enquanto opcao <> 3 faca
      escreval("--- MENU ---")
      escreval("1 - Cadastrar")
      escreval("2 - Listar")
      escreval("3 - Sair")
      escreva("Escolha uma opcao: ")
      leia(opcao)

      se opcao = 1 entao
         escreval("Voce escolheu cadastrar.")
      senao
         se opcao = 2 entao
            escreval("Voce escolheu listar.")
         senao
            se opcao = 3 entao
               escreval("Encerrando...")
            senao
               escreval("Opcao invalida.")
            fimse
         fimse
      fimse
   fimenquanto
fimalgoritmo
```

---

## Erros Comuns no VisuAlg

### Esquecer o bloco `var`

Se usar uma variável sem declarar, o algoritmo terá erro.

### Usar `=` no lugar de `<-`

Atribuição usa `<-`.

```visualg
idade <- 20
```

Comparação usa `=`.

```visualg
se idade = 20 entao
   escreval("Tem 20 anos.")
fimse
```

### Esquecer de fechar blocos

Se abriu `se`, precisa fechar com `fimse`.

Se abriu `para`, precisa fechar com `fimpara`.

Se abriu `enquanto`, precisa fechar com `fimenquanto`.

### Esquecer aspas em textos

Errado:

```visualg
escreval(Ola)
```

Certo:

```visualg
escreval("Ola")
```

---

# Nível 2 - Desenvolvedor Java

## Ponte entre VisuAlg e Java

A lógica aprendida no VisuAlg será reaproveitada em Java.

Exemplo em VisuAlg:

```visualg
algoritmo "Maioridade"
var
   idade: inteiro
inicio
   escreva("Digite sua idade: ")
   leia(idade)

   se idade >= 18 entao
      escreval("Maior de idade")
   senao
      escreval("Menor de idade")
   fimse
fimalgoritmo
```

A mesma lógica em Java:

```java
import java.util.Scanner;

public class Maioridade {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.print("Digite sua idade: ");
        int idade = scanner.nextInt();

        if (idade >= 18) {
            System.out.println("Maior de idade");
        } else {
            System.out.println("Menor de idade");
        }

        scanner.close();
    }
}
```

A ideia é a mesma:

- Ler idade.
- Comparar com 18.
- Mostrar uma mensagem.

O que muda é a sintaxe.

---

## Preparando o Ambiente Java

Para programar em Java, o estudante precisará de:

- **JDK**: kit de desenvolvimento Java.
- **IDE ou editor**: ferramenta para escrever código.

Sugestões de IDE:

- IntelliJ IDEA Community.
- Eclipse.
- VS Code com extensões Java.

Conceitos importantes:

### JDK

É o conjunto de ferramentas necessário para compilar e executar programas Java.

### Compilação

Java transforma o código `.java` em bytecode `.class`.

### JVM

A JVM executa o bytecode Java.

Fluxo:

```text
Codigo Java (.java) -> Compilador -> Bytecode (.class) -> JVM -> Programa rodando
```

---

## Primeiro Programa em Java

```java
public class OlaMundo {
    public static void main(String[] args) {
        System.out.println("Ola, mundo!");
    }
}
```

Explicação:

### `public class OlaMundo`

Define uma classe chamada `OlaMundo`.

Em Java, todo código precisa estar dentro de uma classe.

### `public static void main(String[] args)`

É o ponto de entrada do programa. Quando o programa executa, ele começa por esse método.

### `{` e `}`

As chaves abrem e fecham blocos de código.

### `System.out.println`

Mostra uma mensagem na tela e pula linha.

### `;`

Em Java, a maioria dos comandos termina com ponto e vírgula.

---

## Tipos, Variáveis e Entrada de Dados em Java

### Tipos básicos

```java
int idade = 25;
double preco = 49.90;
String nome = "Maria";
boolean ativo = true;
```

Comparação com VisuAlg:

```text
inteiro   -> int
real      -> double
caractere -> String
logico    -> boolean
```

### Entrada de dados com Scanner

```java
import java.util.Scanner;

public class CadastroSimples {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.print("Digite seu nome: ");
        String nome = scanner.nextLine();

        System.out.print("Digite sua idade: ");
        int idade = scanner.nextInt();

        System.out.println("Nome: " + nome);
        System.out.println("Idade: " + idade);

        scanner.close();
    }
}
```

---

## Condicionais e Repetições em Java

### Condicional `if/else`

```java
if (idade >= 18) {
    System.out.println("Maior de idade");
} else {
    System.out.println("Menor de idade");
}
```

### Repetição `for`

```java
for (int i = 1; i <= 5; i++) {
    System.out.println("Valor: " + i);
}
```

### Repetição `while`

```java
int opcao = 0;

while (opcao != 3) {
    System.out.println("1 - Cadastrar");
    System.out.println("2 - Listar");
    System.out.println("3 - Sair");

    opcao = 3;
}
```

---

## Classes, Objetos e Métodos

### Classe

Uma classe é um molde.

Exemplo:

```java
public class Cliente {
    String nome;
    String email;
}
```

### Objeto

Um objeto é algo criado a partir de uma classe.

```java
Cliente cliente = new Cliente();
cliente.nome = "Ana";
cliente.email = "ana@email.com";
```

### Método

Método representa uma ação.

```java
public class Conta {
    double saldo;

    void depositar(double valor) {
        saldo = saldo + valor;
    }
}
```

Exemplo completo:

```java
public class ProgramaConta {
    public static void main(String[] args) {
        Conta conta = new Conta();

        conta.depositar(100);

        System.out.println("Saldo: " + conta.saldo);
    }
}

class Conta {
    double saldo;

    void depositar(double valor) {
        saldo = saldo + valor;
    }
}
```

---

## Encapsulamento

Encapsulamento protege os dados de uma classe.

Exemplo ruim:

```java
conta.saldo = -500;
```

Isso permitiria um saldo inválido.

Exemplo melhor:

```java
public class Conta {
    private double saldo;

    public void depositar(double valor) {
        if (valor <= 0) {
            throw new IllegalArgumentException("Valor deve ser positivo");
        }
        saldo += valor;
    }

    public boolean sacar(double valor) {
        if (valor <= 0) {
            return false;
        }

        if (valor > saldo) {
            return false;
        }

        saldo -= valor;
        return true;
    }

    public double getSaldo() {
        return saldo;
    }
}
```

Aqui, o saldo só muda por regras controladas.

---

## Collections

Collections são estruturas para trabalhar com grupos de objetos.

### List

Use quando a ordem importa e pode haver repetidos.

```java
import java.util.ArrayList;
import java.util.List;

public class ExemploList {
    public static void main(String[] args) {
        List<String> nomes = new ArrayList<>();

        nomes.add("Ana");
        nomes.add("Bruno");
        nomes.add("Ana");

        for (String nome : nomes) {
            System.out.println(nome);
        }
    }
}
```

### Set

Use quando não quiser repetidos.

```java
import java.util.HashSet;
import java.util.Set;

public class ExemploSet {
    public static void main(String[] args) {
        Set<String> cpfs = new HashSet<>();

        cpfs.add("111");
        cpfs.add("222");
        cpfs.add("111");

        System.out.println(cpfs);
    }
}
```

### Map

Use para chave e valor.

```java
import java.util.HashMap;
import java.util.Map;

public class ExemploMap {
    public static void main(String[] args) {
        Map<Integer, String> produtos = new HashMap<>();

        produtos.put(1, "Mouse");
        produtos.put(2, "Teclado");

        System.out.println(produtos.get(1));
    }
}
```

---

## Exceções

Exceções representam situações inválidas ou inesperadas.

Exemplo:

```java
public class Conta {
    private double saldo;

    public void sacar(double valor) {
        if (valor <= 0) {
            throw new IllegalArgumentException("Valor deve ser positivo");
        }

        if (valor > saldo) {
            throw new IllegalStateException("Saldo insuficiente");
        }

        saldo -= valor;
    }
}
```

Exemplo com `try/catch`:

```java
try {
    conta.sacar(500);
} catch (IllegalStateException erro) {
    System.out.println("Nao foi possivel sacar: " + erro.getMessage());
}
```

---

## Lambdas, Streams e Optional

### Lambda

Lambda é uma forma curta de representar uma ação.

```java
nomes.forEach(nome -> System.out.println(nome));
```

### Stream

Stream ajuda a processar coleções.

```java
List<String> nomesComA = nomes.stream()
    .filter(nome -> nome.startsWith("A"))
    .toList();
```

### Optional

Optional representa algo que pode existir ou não.

```java
Optional<String> nome = Optional.ofNullable(buscarNome());

nome.ifPresent(valor -> System.out.println(valor));
```

Exemplo real:

Buscar um cliente por CPF. Talvez exista, talvez não.

---

## Orientação a Objetos

Orientação a objetos organiza código a partir de conceitos como classes, objetos e responsabilidades.

### Abstração

Escolher apenas o que importa para o sistema.

Em um sistema bancário, um cliente pode ter:

- Nome.
- CPF.
- E-mail.

Talvez não precise ter:

- Altura.
- Cor favorita.
- Time de futebol.

### Herança

Uma classe pode reaproveitar características de outra.

```java
class Pessoa {
    String nome;
}

class Cliente extends Pessoa {
    String cpf;
}
```

### Polimorfismo

Objetos diferentes podem obedecer ao mesmo contrato.

```java
interface Pagamento {
    void pagar(double valor);
}

class PagamentoPix implements Pagamento {
    public void pagar(double valor) {
        System.out.println("Pagando com Pix: " + valor);
    }
}

class PagamentoCartao implements Pagamento {
    public void pagar(double valor) {
        System.out.println("Pagando com cartao: " + valor);
    }
}
```

---

## SOLID

SOLID é um conjunto de princípios para escrever código mais fácil de manter.

### SRP - Responsabilidade Única

Uma classe deve ter uma responsabilidade principal.

Ruim:

```java
class Pedido {
    void calcularTotal() {}
    void salvarNoBanco() {}
    void enviarEmail() {}
}
```

Melhor:

```java
class Pedido {}
class CalculadoraDePedido {}
class PedidoRepository {}
class ServicoDeEmail {}
```

### OCP - Aberto para extensão, fechado para modificação

Devemos conseguir adicionar comportamentos novos sem alterar tudo que já existe.

Exemplo: criar novas formas de pagamento usando a interface `Pagamento`.

### DIP - Inversão de Dependência

Dependa de abstrações.

```java
interface Notificador {
    void notificar(String mensagem);
}

class PedidoService {
    private final Notificador notificador;

    PedidoService(Notificador notificador) {
        this.notificador = notificador;
    }
}
```

---

# Nível 3 - Dados, Qualidade e Mercado

## Banco de Dados e SQL

Banco de dados armazena informações de forma persistente.

Exemplo real de e-commerce:

- Clientes.
- Produtos.
- Pedidos.
- Pagamentos.

### Tabela

Uma tabela organiza dados de um mesmo tipo.

Exemplo: tabela `clientes`.

```sql
CREATE TABLE clientes (
    id INT PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    email VARCHAR(100) NOT NULL
);
```

### Inserir dados

```sql
INSERT INTO clientes (id, nome, email)
VALUES (1, 'Ana', 'ana@email.com');
```

### Consultar dados

```sql
SELECT id, nome, email
FROM clientes;
```

### Atualizar dados

```sql
UPDATE clientes
SET email = 'ana.novo@email.com'
WHERE id = 1;
```

### Remover dados

```sql
DELETE FROM clientes
WHERE id = 1;
```

### Relacionamento entre tabelas

Um cliente pode ter vários pedidos.

```sql
CREATE TABLE pedidos (
    id INT PRIMARY KEY,
    cliente_id INT NOT NULL,
    valor_total DECIMAL(10, 2) NOT NULL,
    FOREIGN KEY (cliente_id) REFERENCES clientes(id)
);
```

### JOIN

JOIN junta dados de tabelas relacionadas.

```sql
SELECT clientes.nome, pedidos.valor_total
FROM clientes
JOIN pedidos ON pedidos.cliente_id = clientes.id;
```

---

## Testes Automatizados

Testes automatizados são códigos que verificam se outro código funciona como esperado.

### Exemplo de regra

Se um produto custa 100 e tem 10% de desconto, o valor final deve ser 90.

Classe:

```java
public class CalculadoraDesconto {
    public double aplicar(double valor, double percentual) {
        return valor - (valor * percentual / 100);
    }
}
```

Teste com JUnit:

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

### Mockito

Mockito é usado para simular dependências.

Exemplo real:

Um pedido aprovado envia e-mail. No teste, não queremos enviar e-mail de verdade. Então simulamos o serviço de e-mail.

---

## Git e GitHub

Git registra a evolução do código.

GitHub armazena o projeto online e ajuda a montar portfólio.

### Comandos básicos

```bash
git init
git status
git add .
git commit -m "Adiciona primeiros algoritmos em VisuAlg"
git branch -M main
git remote add origin URL_DO_REPOSITORIO
git push -u origin main
```

### Estrutura sugerida

```text
estudos-programacao
 ├── nivel-1-visualg
 │   ├── ola-mundo.alg
 │   ├── variaveis.alg
 │   ├── condicionais.alg
 │   └── mini-projeto-caixa-eletronico.alg
 ├── nivel-2-java
 └── nivel-3-projetos
```

### README

O arquivo `README.md` deve explicar:

- Objetivo do repositório.
- O que está sendo estudado.
- Como executar os exemplos.
- Quais projetos já foram concluídos.

---

## Java Efetivo

O livro **Java Efetivo**, de Joshua Bloch, deve ser estudado quando o estudante já souber criar classes, métodos, objetos, coleções e exceções.

### Static Factory Method

Método estático para criar objetos com nomes mais claros.

```java
public class Cliente {
    private String nome;
    private String email;

    private Cliente(String nome, String email) {
        this.nome = nome;
        this.email = email;
    }

    public static Cliente criarComEmail(String nome, String email) {
        return new Cliente(nome, email);
    }
}
```

Uso:

```java
Cliente cliente = Cliente.criarComEmail("Ana", "ana@email.com");
```

### Builder

Ajuda a criar objetos com muitos atributos.

```java
public class Pedido {
    private String cliente;
    private double total;
    private String cupom;

    private Pedido(Builder builder) {
        this.cliente = builder.cliente;
        this.total = builder.total;
        this.cupom = builder.cupom;
    }

    public static class Builder {
        private String cliente;
        private double total;
        private String cupom;

        public Builder cliente(String cliente) {
            this.cliente = cliente;
            return this;
        }

        public Builder total(double total) {
            this.total = total;
            return this;
        }

        public Builder cupom(String cupom) {
            this.cupom = cupom;
            return this;
        }

        public Pedido build() {
            return new Pedido(this);
        }
    }
}
```

Uso:

```java
Pedido pedido = new Pedido.Builder()
    .cliente("Ana")
    .total(150)
    .cupom("PROMO10")
    .build();
```

---

## Design Patterns

Design Patterns são soluções conhecidas para problemas recorrentes de design de software.

Eles não devem ser usados para complicar o código. Devem ser usados quando resolvem um problema real.

### Strategy

Permite trocar um algoritmo sem alterar o código principal.

Exemplo: formas de pagamento.

```java
interface EstrategiaPagamento {
    void pagar(double valor);
}

class PagamentoPix implements EstrategiaPagamento {
    public void pagar(double valor) {
        System.out.println("Pagamento Pix: " + valor);
    }
}

class PagamentoCartao implements EstrategiaPagamento {
    public void pagar(double valor) {
        System.out.println("Pagamento Cartao: " + valor);
    }
}

class Checkout {
    private EstrategiaPagamento pagamento;

    Checkout(EstrategiaPagamento pagamento) {
        this.pagamento = pagamento;
    }

    void finalizar(double valor) {
        pagamento.pagar(valor);
    }
}
```

### Factory Method

Centraliza a criação de objetos.

```java
class PagamentoFactory {
    static EstrategiaPagamento criar(String tipo) {
        if (tipo.equals("PIX")) {
            return new PagamentoPix();
        }

        if (tipo.equals("CARTAO")) {
            return new PagamentoCartao();
        }

        throw new IllegalArgumentException("Tipo invalido");
    }
}
```

### Facade

Cria uma interface simples para uma operação complexa.

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

Spring Boot é um framework Java muito usado para criar aplicações web e APIs.

Ele deve vir depois da base porque abstrai muita coisa. Com lógica, Java, OOP, banco e testes dominados, Spring Boot fica muito mais fácil.

### O que é uma API REST?

Uma API REST permite que sistemas conversem usando HTTP.

Exemplos de endpoints:

```text
GET /produtos
GET /produtos/1
POST /produtos
PUT /produtos/1
DELETE /produtos/1
```

### Controller

Recebe requisições.

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

### Service

Contém regras de negócio.

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

### Repository

Acessa o banco de dados.

```java
public interface ProdutoRepository extends JpaRepository<Produto, Long> {
}
```

### Entity

Representa uma tabela no banco.

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

Objeto usado para entrada ou saída de dados da API.

```java
public record ProdutoRequest(String nome, BigDecimal preco) {
}
```

---

## Projeto Final e Portfólio

O projeto final deve consolidar tudo.

### Projeto sugerido: Sistema de E-commerce

Funcionalidades:

- Cadastro de clientes.
- Cadastro de produtos.
- Controle de estoque.
- Carrinho de compras.
- Criação de pedidos.
- Simulação de pagamento.
- Banco de dados.
- Testes automatizados.
- API REST.
- README detalhado.

### Conceitos aplicados

- Java.
- Orientação a objetos.
- SOLID.
- Collections.
- Exceções.
- Streams.
- Banco de dados.
- Testes.
- Design Patterns.
- Spring Boot.
- Git e GitHub.

### Estrutura sugerida

```text
src
 └── main
     └── java
         └── br.com.estudos.ecommerce
             ├── controller
             ├── service
             ├── repository
             ├── domain
             ├── dto
             ├── exception
             └── config
```

### README do projeto final

O README deve conter:

- Objetivo do projeto.
- Tecnologias utilizadas.
- Como executar.
- Funcionalidades.
- Exemplos de requisição.
- Decisões técnicas.
- Próximos passos.

### Critérios de conclusão

O projeto está pronto quando o estudante conseguir:

- Explicar a arquitetura.
- Executar localmente.
- Demonstrar os endpoints.
- Mostrar testes passando.
- Explicar onde usou OOP, SOLID e patterns.
- Publicar o código no GitHub.

---

# Exercícios Progressivos Recomendados

## VisuAlg

- Olá Mundo.
- Apresentação pessoal.
- Calculadora simples.
- Média de notas.
- Maioridade.
- Desconto de compra.
- Tabuada.
- Login simples.
- Caixa eletrônico.
- Vetor de nomes.
- Média de turma.
- Menu interativo.

## Java

- Recriar a calculadora em Java.
- Recriar a média de notas em Java.
- Criar classe `Cliente`.
- Criar classe `Conta`.
- Criar sistema bancário simples.
- Criar agenda de contatos.
- Criar controle de estoque.

## SQL

- Criar tabela de clientes.
- Criar tabela de produtos.
- Criar tabela de pedidos.
- Inserir dados.
- Consultar dados.
- Atualizar registros.
- Remover registros.
- Fazer consultas com JOIN.

## Spring Boot

- Criar API de produtos.
- Criar API de clientes.
- Adicionar banco de dados.
- Adicionar validações.
- Adicionar tratamento global de erros.
- Adicionar testes.

---

# Livros Base do Roadmap

## Use a Cabeça Java

Ideal para o primeiro contato com Java, objetos, classes e fundamentos da linguagem.

## Java Efetivo

Ideal para melhorar a qualidade do código depois que a pessoa já sabe programar em Java.

## Padrões de Projeto

Ideal para entender soluções clássicas de design de software. Deve ser estudado depois de orientação a objetos estar bem compreendida.

---

# Fontes úteis para VisuAlg

- Página do projeto VisuAlg 3.0 no SourceForge: <https://sourceforge.net/projects/visualg30/>
- VisuAlg Online: <https://visualg.com.br/>

---

# Observação Final

Este roadmap não tenta formar alguém rapidamente de forma superficial.

Ele foi pensado para criar uma base sólida, com exemplos suficientes para o estudante praticar, errar, pesquisar e voltar com dúvidas melhores.

A jornada começa com VisuAlg porque ele torna a lógica mais acessível. Depois, a pessoa leva esse raciocínio para Java e, com o tempo, aprende ferramentas mais próximas do mercado, como banco de dados, testes, Git, Spring Boot e APIs REST.
