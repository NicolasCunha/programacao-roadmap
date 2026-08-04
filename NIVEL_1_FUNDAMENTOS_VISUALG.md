# Nível 1 - Fundamentos com VisuAlg

## Índice

- [ ] [Uso responsável de IA](#uso-responsável-de-ia)
- [ ] [Instalação e primeiro programa](#instalação-e-primeiro-programa)
- [ ] [Estrutura do VisuAlg](#estrutura-do-visualg)
- [ ] [Variáveis, tipos, entrada e saída](#variáveis-tipos-entrada-e-saída)
- [ ] [Condições, repetições, vetores e funções](#condições-repetições-vetores-e-funções)
- [ ] [Recursão no VisuAlg](#recursão-no-visualg)

## Uso responsável de IA

Não use IA para gerar o código final dos exercícios. Use para entender erros, revisar uma tentativa ou pedir uma explicação alternativa.

## Instalação e primeiro programa

Use o VisuAlg desktop ou o VisuAlg Online. Primeiro programa para copiar e colar:

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

## Estrutura do VisuAlg

```visualg
algoritmo "Nome"
var
   // variáveis
inicio
   // comandos
fimalgoritmo
```

- `algoritmo`: nome do programa.
- `var`: bloco de variáveis.
- `inicio`: começo da execução.
- `fimalgoritmo`: fim da execução.
- `//`: comentário.

## Variáveis, tipos, entrada e saída

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

Atribuição usa `<-`. Texto usa aspas duplas.

Entrada:

```visualg
algoritmo "Entrada"
var nome: caractere
inicio
   escreva("Digite seu nome: ")
   leia(nome)
   escreval("Ola, ", nome)
fimalgoritmo
```

## Condições, repetições, vetores e funções

Condição:

```visualg
se idade >= 18 entao
   escreval("Maior")
senao
   escreval("Menor")
fimse
```

Para:

```visualg
para i de 1 ate 10 faca
   escreval(i)
fimpara
```

Enquanto:

```visualg
enquanto senha <> "1234" faca
   leia(senha)
fimenquanto
```

Vetor:

```visualg
nomes: vetor[1..5] de caractere
```

Função:

```visualg
funcao dobro(n: inteiro): inteiro
inicio
   dobro <- n * 2
fimfuncao
```

## Recursão no VisuAlg

Recursão é quando uma função chama a si mesma. Precisa de caso base.

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
