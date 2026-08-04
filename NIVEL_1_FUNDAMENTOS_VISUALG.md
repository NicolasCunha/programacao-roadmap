# Nível 1 - Fundamentos com VisuAlg

Neste nível, o objetivo é aprender lógica de programação usando VisuAlg e Portugol. O VisuAlg é uma boa primeira linguagem prática porque deixa o estudante focar no raciocínio antes de lidar com a sintaxe mais rígida de linguagens profissionais.

---

## Índice de progresso

- [ ] [Objetivo do nível](#objetivo-do-nível)
- [ ] [Uso responsável de IA](#uso-responsável-de-ia)
- [ ] [Como baixar e executar o VisuAlg](#como-baixar-e-executar-o-visualg)
- [ ] [Primeiro programa: Olá Mundo](#primeiro-programa-olá-mundo)
- [ ] [O que é um algoritmo](#o-que-é-um-algoritmo)
- [ ] [Estrutura básica do VisuAlg](#estrutura-básica-do-visualg)
- [ ] [Comentários](#comentários)
- [ ] [Bloco VAR](#bloco-var)
- [ ] [Tipos de dados](#tipos-de-dados)
- [ ] [Entrada, saída e atribuição](#entrada-saída-e-atribuição)
- [ ] [Condições e repetições](#condições-e-repetições)
- [ ] [Vetores e matrizes](#vetores-e-matrizes)
- [ ] [Procedimentos e funções](#procedimentos-e-funções)
- [ ] [Recursão no VisuAlg](#recursão-no-visualg)
- [ ] [Mini-projetos](#mini-projetos)
- [ ] [Checklist de conclusão](#checklist-de-conclusão)

---

## Objetivo do nível

Ao final deste nível, o estudante deve conseguir:

- Executar algoritmos no VisuAlg.
- Entender variáveis, entrada, saída, condições, repetições, vetores e funções.
- Resolver problemas simples com lógica.
- Ler e alterar exemplos de código com segurança.
- Criar pequenos algoritmos, como calculadora, média, tabuada, login e caixa eletrônico.

---

## Uso responsável de IA

Neste nível, evite pedir para uma IA gerar o algoritmo completo.

A lógica de programação é como musculação: o ganho vem da repetição ativa. Se outra ferramenta escreve por você, o cérebro acompanha a solução, mas não treina a construção.

Use IA apenas para:

- Explicar uma palavra ou conceito.
- Ajudar a entender um erro.
- Criar um exercício parecido.
- Revisar uma solução que você já tentou.

Evite usar IA para copiar e entregar o código pronto.

---

## Como baixar e executar o VisuAlg

### Opção 1 - Instalar no computador

1. Acesse a página do VisuAlg 3.0 no SourceForge: <https://sourceforge.net/projects/visualg30/>
2. Clique em **Download**.
3. Extraia o arquivo baixado.
4. Abra a pasta extraída.
5. Execute `visualg.exe` ou `VisuAlg3.exe`.
6. Cole o exemplo de Olá Mundo abaixo e execute.

### Opção 2 - Usar online

Se não puder instalar nada, use o VisuAlg Online: <https://visualg.com.br/>

---

## Primeiro programa: Olá Mundo

Copie, cole e execute:

```visualg
algoritmo "OlaMundo"
// Define o nome do algoritmo.

inicio
   // Início do bloco de comandos.

   escreval("Ola, mundo!")
   // Mostra a mensagem na tela.

fimalgoritmo
// Fim do algoritmo.
```

Saída esperada:

```text
Ola, mundo!
```

---

## O que é um algoritmo

Um algoritmo é uma sequência de passos para resolver um problema.

Exemplo:

```text
Problema: calcular a média de duas notas.
1. Ler a primeira nota.
2. Ler a segunda nota.
3. Somar as duas notas.
4. Dividir por 2.
5. Mostrar a média.
```

Antes de codar, escreva os passos em português.

---

## Estrutura básica do VisuAlg

```visualg
algoritmo "NomeDoAlgoritmo"
// Nome do algoritmo.

var
   // Declaração das variáveis.

inicio
   // Comandos executados.

fimalgoritmo
// Fim do algoritmo.
```

---

## Comentários

Comentários são ignorados pelo computador e servem para explicar o código.

```visualg
algoritmo "Comentarios"
inicio
   // Esta linha imprime uma mensagem.
   escreval("Comentarios ajudam no aprendizado.")
fimalgoritmo
```

---

## Bloco VAR

O bloco `var` declara variáveis.

```visualg
algoritmo "ExemploVar"
var
   idade: inteiro
   nome: caractere
inicio
   idade <- 25
   nome <- "Maria"

   escreval("Nome: ", nome)
   escreval("Idade: ", idade)
fimalgoritmo
```

---

## Tipos de dados

- `inteiro`: números sem casas decimais.
- `real`: números com casas decimais.
- `caractere`: textos.
- `logico`: verdadeiro ou falso.

```visualg
algoritmo "Tipos"
var
   idade: inteiro
   preco: real
   nome: caractere
   ativo: logico
inicio
   idade <- 30
   preco <- 49.90
   nome <- "Ana"
   ativo <- verdadeiro

   escreval(idade)
   escreval(preco)
   escreval(nome)
   escreval(ativo)
fimalgoritmo
```

Textos devem ficar entre aspas duplas:

```visualg
nome <- "Ana"
```

---

## Entrada, saída e atribuição

Atribuição usa `<-`.

```visualg
idade <- 20
```

Saída usa `escreva` ou `escreval`.

Entrada usa `leia`.

```visualg
algoritmo "EntradaSaida"
var
   nome: caractere
inicio
   escreva("Digite seu nome: ")
   leia(nome)

   escreval("Ola, ", nome)
fimalgoritmo
```

---

## Condições e repetições

### Condição

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

### Repetição com PARA

```visualg
algoritmo "Contar"
var
   i: inteiro
inicio
   para i de 1 ate 5 faca
      escreval(i)
   fimpara
fimalgoritmo
```

### Repetição com ENQUANTO

```visualg
algoritmo "Senha"
var
   senha: caractere
inicio
   senha <- ""

   enquanto senha <> "1234" faca
      escreva("Digite a senha: ")
      leia(senha)
   fimenquanto

   escreval("Acesso liberado")
fimalgoritmo
```

---

## Vetores e matrizes

### Vetor

```visualg
algoritmo "VetorNomes"
var
   nomes: vetor[1..3] de caractere
   i: inteiro
inicio
   para i de 1 ate 3 faca
      escreva("Digite o nome ", i, ": ")
      leia(nomes[i])
   fimpara

   para i de 1 ate 3 faca
      escreval(nomes[i])
   fimpara
fimalgoritmo
```

### Matriz

```visualg
algoritmo "MatrizNotas"
var
   notas: vetor[1..2,1..2] de real
inicio
   notas[1,1] <- 8
   notas[1,2] <- 7
   notas[2,1] <- 6
   notas[2,2] <- 9

   escreval(notas[1,1])
   escreval(notas[2,2])
fimalgoritmo
```

---

## Procedimentos e funções

### Procedimento

Procedimento executa uma ação e não retorna valor.

```visualg
algoritmo "Procedimento"

procedimento cabecalho
inicio
   escreval("Sistema de Estudos")
fimprocedimento

inicio
   cabecalho
fimalgoritmo
```

### Função

Função executa uma lógica e retorna valor.

```visualg
algoritmo "FuncaoDobro"

funcao dobro(numero: inteiro): inteiro
inicio
   dobro <- numero * 2
fimfuncao

var
   resultado: inteiro
inicio
   resultado <- dobro(5)
   escreval(resultado)
fimalgoritmo
```

---

## Recursão no VisuAlg

Recursão acontece quando uma função chama a si mesma.

Ela precisa de duas partes:

1. **Caso base**: condição que para a recursão.
2. **Chamada recursiva**: a função chamando ela mesma com um problema menor.

Exemplo clássico: fatorial.

```text
5! = 5 * 4 * 3 * 2 * 1
```

Código em VisuAlg:

```visualg
algoritmo "FatorialRecursivo"

funcao fatorial(n: inteiro): inteiro
inicio
   // Caso base: se n for 0 ou 1, o resultado é 1.
   se n <= 1 entao
      fatorial <- 1
   senao
      // Chamada recursiva: n vezes o fatorial de n - 1.
      fatorial <- n * fatorial(n - 1)
   fimse
fimfuncao

var
   numero, resultado: inteiro
inicio
   escreva("Digite um numero: ")
   leia(numero)

   resultado <- fatorial(numero)

   escreval("Fatorial: ", resultado)
fimalgoritmo
```

Atenção: recursão sem caso base pode causar repetição infinita.

---

## Mini-projetos

- Calculadora de média.
- Caixa eletrônico simples.
- Menu interativo.
- Cadastro de nomes com vetor.
- Tabuada.
- Jogo de adivinhação.

---

## Checklist de conclusão

- [ ] Executei o Olá Mundo no VisuAlg.
- [ ] Entendo `algoritmo`, `var`, `inicio` e `fimalgoritmo`.
- [ ] Sei declarar variáveis.
- [ ] Sei usar entrada e saída.
- [ ] Sei usar condições.
- [ ] Sei usar repetições.
- [ ] Sei usar vetores.
- [ ] Entendo o conceito de função.
- [ ] Entendo a ideia básica de recursão.
- [ ] Fiz pelo menos três mini-projetos.
