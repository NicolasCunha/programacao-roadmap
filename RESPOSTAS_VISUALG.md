# Respostas - Exercícios VisuAlg

> Use este arquivo apenas depois de tentar. As respostas são referências, não a única forma correta.

## Fácil

### 1
```visualg
algoritmo "OlaMundo"
inicio
   escreval("Ola, mundo!")
fimalgoritmo
```

### 2
```visualg
algoritmo "Nome"
var nome: caractere
inicio
   escreva("Nome: ")
   leia(nome)
   escreval("Ola, ", nome)
fimalgoritmo
```

### 3
```visualg
algoritmo "Maioridade"
var idade: inteiro
inicio
   leia(idade)
   se idade >= 18 entao escreval("Maior") senao escreval("Menor") fimse
fimalgoritmo
```

### 4 a 15
- 4: declare `a`, `b`; leia os dois; imprima `a+b`, `a-b`, `a*b`, `a/b`.
- 5: leia `nota1`, `nota2`; calcule `media <- (nota1 + nota2) / 2`.
- 6: `desconto <- preco * 0.10`; `final <- preco - desconto`.
- 7: `para i de 1 ate 10 faca escreval(numero, " x ", i, " = ", numero*i) fimpara`.
- 8: compare `a`, `b`, `c` com `se` aninhado ou variável `maior`.
- 9: use `numero mod 2 = 0` para par.
- 10: `dolar <- reais / cotacao`.
- 11: `novoSalario <- salario * 1.08`.
- 12: `area <- base * altura`.
- 13: `se (usuario = "admin") e (senha = "1234") entao`.
- 14: `para i de 1 ate numero faca escreval(i) fimpara`.
- 15: `nomes: vetor[1..5] de caractere`; dois laços `para`, um para ler e outro para mostrar.

## Médio

### 1 Caixa eletrônico
```visualg
algoritmo "Caixa"
var saldo, saque: real
inicio
   saldo <- 1000
   leia(saque)
   se saque <= 0 entao
      escreval("Valor invalido")
   senao
      se saque <= saldo entao
         saldo <- saldo - saque
         escreval("Saldo: ", saldo)
      senao
         escreval("Saldo insuficiente")
      fimse
   fimse
fimalgoritmo
```

### 2 a 15
- 2: use `quantidade`, `soma <- 0`, laço `para`, leia notas, some e divida.
- 3: use `enquanto opcao <> 3` com opções por `se`.
- 4: vetor de 10 inteiros; inicialize `soma`, `maior`, `menor`.
- 5: vetores paralelos `produtos` e `precos`; compare preços.
- 6: se compra >= 300 frete 0, senão `distancia * 1.50`.
- 7: `enquanto numero <> 0`; conte e some.
- 8: número secreto fixo; `enquanto tentativa <> secreto`.
- 9: matriz `vetor[1..2,1..3]`; dois laços aninhados.
- 10: `funcao dobro(n: inteiro): inteiro` retorna `n * 2`.
- 11: `funcao desconto(valor, percentual: real): real` retorna `valor * percentual / 100`.
- 12: crie `procedimento cabecalho` e chame em vários pontos.
- 13: conte tentativas com `enquanto tentativa < 3`.
- 14: calcule média, depois conte notas acima dela.
- 15: use vetores `nomes`, `precos`, `quantidades` e menu simples.

## Difícil

As respostas difíceis são projetos. Estrutura recomendada:

1. Estoque: vetores `nomes`, `precos`, `quantidades`; variável `totalCadastrados`; menu com cadastrar, listar, atualizar e sair.
2. Biblioteca: vetores `livros`, `autores`, `disponivel`; emprestar muda `disponivel[pos] <- falso`; devolver muda para verdadeiro.
3. Notas: matriz `notas[aluno, nota]`; vetor `medias`; classifique média >= 6.
4. Caixa completo: menu com saldo, depósito, saque e sair; valide todos os valores.
5. Cinema: matriz de `caractere`; use `Livre` e `Ocupado`; valide antes de reservar.
6. Fatorial recursivo: `se n <= 1 entao 1 senao n * fatorial(n-1)`.
7. Soma recursiva: `se n = 0 entao 0 senao n + soma(n-1)`.
8. Ordenação: dois laços; se `v[j] > v[j+1]`, troque.
9. Login: vetores `usuarios`, `senhas`; laço para buscar; limite tentativas.
10. Carrinho: produtos cadastrados + vetor de quantidades no carrinho.
11. Folha: vetores para nomes, salários, descontos, bônus e líquido.
12. Votação: contadores para candidatos; menu até encerrar; compare maior.
13. Diagonal: matriz 3x3; some `matriz[i,i]`.
14. Vogais: percorra caracteres se sua versão suportar; conte `a`, `e`, `i`, `o`, `u`.
15. Pedidos: leia cliente, produto, quantidade; calcule subtotal, desconto e total.
