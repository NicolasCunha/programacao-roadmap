# Respostas - Exercícios VisuAlg

> Use este arquivo apenas depois de tentar. As respostas são referências, não a única forma correta — nomes de variáveis e pequenos detalhes podem mudar sem problema.

## Nível Fácil — Ilha Deserta

### 1 - O rádio liga

```visualg
algoritmo "RadioLiga"
inicio
   escreval("Ola, mundo!")
fimalgoritmo
```

Só confirma que o ambiente e o `escreval` funcionam — a base de qualquer exercício seguinte.

### 2 - Registro de sobreviventes

```visualg
algoritmo "RegistroSobreviventes"
var nome: caractere
inicio
   escreva("Nome do sobrevivente: ")
   leia(nome)
   escreval("Ola, ", nome, "!")
fimalgoritmo
```

`leia` pausa esperando o valor; `escreval` concatena partes com vírgula, sem precisar montar a string manualmente.

### 3 - Turno de escalada no penhasco

```visualg
algoritmo "TurnoEscalada"
var idade: inteiro
inicio
   escreva("Idade: ")
   leia(idade)
   se idade >= 18 entao
      escreval("Maior de idade - pode escalar")
   senao
      escreval("Menor de idade - nao pode escalar")
   fimse
fimalgoritmo
```

`>=` inclui o próprio 18 como maioridade, conforme a regra do enunciado.

### 4 - Divisão de provisões

```visualg
algoritmo "DivisaoProvisoes"
var a, b: real
inicio
   leia(a, b)
   escreval("Soma: ", a + b)
   escreval("Subtracao: ", a - b)
   escreval("Multiplicacao: ", a * b)
   escreval("Divisao: ", a / b)
fimalgoritmo
```

Usar `real` em vez de `inteiro` evita perder a parte decimal da divisão.

### 5 - Resistência da corda

```visualg
algoritmo "ResistenciaCorda"
var r1, r2, media: real
inicio
   leia(r1, r2)
   media <- (r1 + r2) / 2
   escreval("Resistencia media: ", media, " kg")
fimalgoritmo
```

### 6 - Escambo com o barco mercante

```visualg
algoritmo "EscamboBarco"
var preco, desconto, final: real
inicio
   leia(preco)
   desconto <- preco * 0.10
   final <- preco - desconto
   escreval("Preco original: ", preco)
   escreval("Desconto: ", desconto)
   escreval("Preco final: ", final)
fimalgoritmo
```

### 7 - Código de tambor

```visualg
algoritmo "CodigoTambor"
var numero, i: inteiro
inicio
   leia(numero)
   para i de 1 ate 10 faca
      escreval(numero, " x ", i, " = ", numero * i)
   fimpara
fimalgoritmo
```

O `para` é a estrutura certa aqui porque o número de repetições (10) já é conhecido de antemão.

### 8 - A nascente mais forte

```visualg
algoritmo "NascenteMaisForte"
var a, b, c, maior: real
inicio
   leia(a, b, c)
   maior <- a
   se b > maior entao
      maior <- b
   fimse
   se c > maior entao
      maior <- c
   fimse
   escreval("Maior vazao: ", maior)
fimalgoritmo
```

Comparar contra uma variável `maior`, atualizando-a a cada teste, evita `se` aninhados e escala para qualquer quantidade de valores.

### 9 - Divisão justa de conchas

```visualg
algoritmo "DivisaoConchas"
var conchas: inteiro
inicio
   leia(conchas)
   se conchas mod 2 = 0 entao
      escreval("Par - da para dividir igualmente")
   senao
      escreval("Impar - sobra uma concha")
   fimse
fimalgoritmo
```

`mod 2 = 0` é o teste padrão de paridade — o resto da divisão por 2 só é zero em números pares.

### 10 - Créditos de resgate

```visualg
algoritmo "CreditosResgate"
var conchas, cotacao, creditos: real
inicio
   cotacao <- 5.00
   leia(conchas)
   creditos <- conchas / cotacao
   escreval("Creditos de resgate: ", creditos)
fimalgoritmo
```

### 11 - A colheita rendeu mais

```visualg
algoritmo "ColheitaMaior"
var racaoAtual, novaRacao: real
inicio
   leia(racaoAtual)
   novaRacao <- racaoAtual * 1.08
   escreval("Nova racao diaria: ", novaRacao, "g")
fimalgoritmo
```

Multiplicar por `1.08` já soma o aumento de 8% em uma única operação, sem precisar calcular o valor do aumento separadamente.

### 12 - Área do abrigo

```visualg
algoritmo "AreaAbrigo"
var base, altura, area: real
inicio
   leia(base, altura)
   area <- base * altura
   escreval("Area do abrigo: ", area)
fimalgoritmo
```

### 13 - Cofre de suprimentos

```visualg
algoritmo "CofreSuprimentos"
var usuario, senha: caractere
inicio
   escreva("Usuario: ")
   leia(usuario)
   escreva("Senha: ")
   leia(senha)
   se (usuario = "admin") e (senha = "1234") entao
      escreval("Acesso liberado")
   senao
      escreval("Acesso negado")
   fimse
fimalgoritmo
```

O `e` lógico exige que as duas condições sejam verdadeiras ao mesmo tempo — errar só o usuário, ou só a senha, já é suficiente para negar o acesso.

### 14 - Contando os dias

```visualg
algoritmo "ContandoDias"
var n, i: inteiro
inicio
   leia(n)
   para i de 1 ate n faca
      escreval("Dia ", i)
   fimpara
fimalgoritmo
```

### 15 - Diário de bordo

```visualg
algoritmo "DiarioDeBordo"
var nomes: vetor[1..5] de caractere
var i: inteiro
inicio
   para i de 1 ate 5 faca
      escreva("Nome do sobrevivente ", i, ": ")
      leia(nomes[i])
   fimpara

   escreval("--- Diario de bordo ---")
   para i de 1 ate 5 faca
      escreval(i, " - ", nomes[i])
   fimpara
fimalgoritmo
```

Dois laços separados: um para preencher o vetor, outro para exibi-lo — misturar os dois em um só funcionaria aqui, mas separar deixa cada laço com uma única responsabilidade.

---

## Nível Médio — Campeonato de Futebol de Várzea

### 1 - Caixa do campeonato

```visualg
algoritmo "CaixaCampeonato"
var saldo, saque: real
inicio
   saldo <- 1000
   escreva("Valor do saque: ")
   leia(saque)
   se saque <= 0 entao
      escreval("Valor invalido")
   senao
      se saque <= saldo entao
         saldo <- saldo - saque
         escreval("Saque realizado. Saldo: ", saldo)
      senao
         escreval("Saldo insuficiente")
      fimse
   fimse
fimalgoritmo
```

Duas validações encadeadas: primeiro se o valor faz sentido (`> 0`), depois se o saldo cobre o saque — nessa ordem, porque não faz sentido checar saldo para um valor já inválido.

### 2 - Média de gols da rodada

```visualg
algoritmo "MediaGolsRodada"
var qtdJogos, i: inteiro
var golsJogo, somaGols, media: real
inicio
   escreva("Quantidade de jogos: ")
   leia(qtdJogos)
   somaGols <- 0
   para i de 1 ate qtdJogos faca
      escreva("Gols do jogo ", i, ": ")
      leia(golsJogo)
      somaGols <- somaGols + golsJogo
   fimpara
   media <- somaGols / qtdJogos
   escreval("Media de gols da rodada: ", media)
fimalgoritmo
```

`qtdJogos` não é fixo, então o `para` usa o valor lido como limite — a mesma lógica do Exercício 15 do nível fácil, agora com um total definido em tempo de execução.

### 3 - Painel da secretaria do campeonato

```visualg
algoritmo "PainelSecretaria"
var opcao: inteiro
var nomeTime: caractere
inicio
   opcao <- 0
   enquanto opcao <> 3 faca
      escreval("1. Cadastrar time")
      escreval("2. Listar times")
      escreval("3. Sair")
      leia(opcao)

      escolha opcao
         caso 1
            escreva("Nome do time: ")
            leia(nomeTime)
            escreval("Time cadastrado: ", nomeTime)
         caso 2
            escreval("(listagem de times cadastrados)")
         caso 3
            escreval("Encerrando painel...")
         outrocaso
            escreval("Opcao invalida")
      fimescolha
   fimenquanto
fimalgoritmo
```

O `enquanto opcao <> 3` mantém o menu voltando até a saída ser escolhida — repare que essa versão simplificada não guarda os times em vetor; isso vira exigência a partir do Exercício 15 deste mesmo nível.

### 4 - Estatísticas da rodada

```visualg
algoritmo "EstatisticasRodada"
var gols: vetor[1..10] de inteiro
var i, soma, maior, menor: inteiro
var media: real
inicio
   para i de 1 ate 10 faca
      escreva("Gols da partida ", i, ": ")
      leia(gols[i])
   fimpara

   soma <- 0
   maior <- gols[1]
   menor <- gols[1]
   para i de 1 ate 10 faca
      soma <- soma + gols[i]
      se gols[i] > maior entao
         maior <- gols[i]
      fimse
      se gols[i] < menor entao
         menor <- gols[i]
      fimse
   fimpara
   media <- soma / 10

   escreval("Soma: ", soma)
   escreval("Media: ", media)
   escreval("Maior: ", maior)
   escreval("Menor: ", menor)
fimalgoritmo
```

`maior` e `menor` começam iguais a `gols[1]`, não a zero — se todos os valores fossem menores que zero, iniciar em zero daria um "maior" errado.

### 5 - Barraquinha do campeonato

```visualg
algoritmo "BarraquinhaCampeonato"
var produtos: vetor[1..5] de caractere
var precos: vetor[1..5] de real
var i, posMaisCaro, posMaisBarato: inteiro
inicio
   para i de 1 ate 5 faca
      escreva("Produto ", i, ": ")
      leia(produtos[i])
      escreva("Preco: ")
      leia(precos[i])
   fimpara

   posMaisCaro <- 1
   posMaisBarato <- 1
   para i de 2 ate 5 faca
      se precos[i] > precos[posMaisCaro] entao
         posMaisCaro <- i
      fimse
      se precos[i] < precos[posMaisBarato] entao
         posMaisBarato <- i
      fimse
   fimpara

   escreval("Mais caro: ", produtos[posMaisCaro])
   escreval("Mais barato: ", produtos[posMaisBarato])
fimalgoritmo
```

Guardar a **posição** do maior/menor (não só o preço) é o que permite recuperar o nome do produto correspondente depois — dois vetores paralelos, mesma posição, mesmo produto.

### 6 - Transporte do time visitante

```visualg
algoritmo "TransporteVisitante"
var valorInscricao, distancia, custoTransporte: real
inicio
   leia(valorInscricao, distancia)
   se valorInscricao >= 300 entao
      custoTransporte <- 0
   senao
      custoTransporte <- distancia * 1.50
   fimse
   escreval("Custo do transporte: ", custoTransporte)
fimalgoritmo
```

### 7 - Apito final

```visualg
algoritmo "ApitoFinal"
var gol, quantidade, soma: inteiro
inicio
   quantidade <- 0
   soma <- 0
   leia(gol)
   enquanto gol <> 0 faca
      quantidade <- quantidade + 1
      soma <- soma + gol
      leia(gol)
   fimenquanto
   escreval("Gols marcados: ", quantidade)
   escreval("Soma: ", soma)
fimalgoritmo
```

A leitura acontece duas vezes no código (antes do laço e dentro dele) porque o `enquanto` testa a condição **antes** de cada repetição — precisa de um valor já lido para decidir se entra no laço pela primeira vez.

### 8 - Aposta do intervalo

```visualg
algoritmo "ApostaIntervalo"
var secreto, tentativa: inteiro
inicio
   secreto <- 7
   tentativa <- 0
   enquanto tentativa <> secreto faca
      escreva("Chute o numero da camisa: ")
      leia(tentativa)
      se tentativa <> secreto entao
         escreval("Errou, tente de novo!")
      fimse
   fimenquanto
   escreval("Acertou! Era o numero ", secreto)
fimalgoritmo
```

### 9 - Escalação titular

```visualg
algoritmo "EscalacaoTitular"
var escalacao: vetor[1..2, 1..3] de caractere
var linha, coluna: inteiro
inicio
   para linha de 1 ate 2 faca
      para coluna de 1 ate 3 faca
         escreva("Jogador [linha ", linha, ", coluna ", coluna, "]: ")
         leia(escalacao[linha, coluna])
      fimpara
   fimpara

   para linha de 1 ate 2 faca
      para coluna de 1 ate 3 faca
         escreval(escalacao[linha, coluna])
      fimpara
   fimpara
fimalgoritmo
```

Uma matriz sempre pede dois laços aninhados: o de fora percorre linhas, o de dentro percorre colunas dentro de cada linha.

### 10 - Rodada em dobro

```visualg
algoritmo "RodadaEmDobro"

funcao dobroPontos(pontos: inteiro): inteiro
inicio
   dobroPontos <- pontos * 2
fimfuncao

var pontosTime: inteiro
inicio
   leia(pontosTime)
   escreval("Pontos em dobro: ", dobroPontos(pontosTime))
fimalgoritmo
```

### 11 - Desconto de sócio-torcedor

```visualg
algoritmo "DescontoSocio"

funcao calcularDesconto(valor, percentual: real): real
inicio
   calcularDesconto <- valor * percentual / 100
fimfuncao

var inscricao, desconto: real
inicio
   leia(inscricao)
   desconto <- calcularDesconto(inscricao, 15)
   escreval("Desconto: ", desconto)
fimalgoritmo
```

### 12 - Boletim do campeonato

```visualg
algoritmo "BoletimCampeonato"

procedimento cabecalho
inicio
   escreval("=== Boletim do Campeonato de Varzea ===")
fimprocedimento

inicio
   cabecalho
   escreval("Classificacao geral...")

   cabecalho
   escreval("Artilharia da rodada...")

   cabecalho
   escreval("Proximos jogos...")
fimalgoritmo
```

O procedimento é chamado três vezes, exatamente porque ele não devolve nenhum valor a ser reaproveitado — só executa a mesma ação (imprimir o cabeçalho) sempre que for chamado.

### 13 - Vestiário restrito

```visualg
algoritmo "VestiarioRestrito"
var senha: caractere
var tentativas: inteiro
inicio
   tentativas <- 0
   repita
      escreva("Senha do vestiario: ")
      leia(senha)
      tentativas <- tentativas + 1
      se senha <> "1234" entao
         escreval("Senha incorreta. Tentativas restantes: ", 3 - tentativas)
      fimse
   ate (senha = "1234") ou (tentativas = 3)

   se senha = "1234" entao
      escreval("Acesso liberado")
   senao
      escreval("Acesso bloqueado")
   fimse
fimalgoritmo
```

`repita...ate` encaixa melhor que `enquanto` aqui, porque a senha precisa ser pedida pelo menos uma vez antes de qualquer verificação existir.

### 14 - Prêmio bola cheia

```visualg
algoritmo "PremioBolaCheia"
var pontuacoes: vetor[1..11] de real
var i: inteiro
var soma, media: real
var acimaDaMedia: inteiro
inicio
   soma <- 0
   para i de 1 ate 11 faca
      escreva("Pontuacao do jogador ", i, ": ")
      leia(pontuacoes[i])
      soma <- soma + pontuacoes[i]
   fimpara
   media <- soma / 11

   acimaDaMedia <- 0
   para i de 1 ate 11 faca
      se pontuacoes[i] > media entao
         acimaDaMedia <- acimaDaMedia + 1
      fimse
   fimpara

   escreval("Media da rodada: ", media)
   escreval("Jogadores acima da media: ", acimaDaMedia)
fimalgoritmo
```

É preciso terminar de calcular a média (primeiro laço inteiro) antes de começar a comparar qualquer pontuação contra ela — por isso são dois laços separados, não um só.

### 15 - Estoque da barraquinha

```visualg
algoritmo "EstoqueBarraquinha"
var nomes: vetor[1..5] de caractere
var precos: vetor[1..5] de real
var quantidades: vetor[1..5] de inteiro
var i: inteiro
inicio
   para i de 1 ate 5 faca
      escreva("Produto ", i, ": ")
      leia(nomes[i])
      escreva("Preco: ")
      leia(precos[i])
      escreva("Quantidade: ")
      leia(quantidades[i])
   fimpara

   escreval("--- Estoque da barraquinha ---")
   para i de 1 ate 5 faca
      escreval(nomes[i], " - R$", precos[i], " - ", quantidades[i], " unidades")
   fimpara
fimalgoritmo
```

---

## Nível Difícil — Campus Universitário

### 1 - Estoque da cantina

```visualg
algoritmo "EstoqueCantina"
var nomes: vetor[1..20] de caractere
var quantidades: vetor[1..20] de inteiro
var totalCadastrados, opcao, pos: inteiro
inicio
   totalCadastrados <- 0
   opcao <- 0
   enquanto opcao <> 4 faca
      escreval("1.Cadastrar 2.Listar 3.Atualizar 4.Sair")
      leia(opcao)

      escolha opcao
         caso 1
            totalCadastrados <- totalCadastrados + 1
            escreva("Nome do produto: ")
            leia(nomes[totalCadastrados])
            escreva("Quantidade: ")
            leia(quantidades[totalCadastrados])
         caso 2
            para pos de 1 ate totalCadastrados faca
               escreval(nomes[pos], " - ", quantidades[pos])
            fimpara
         caso 3
            escreva("Posicao a atualizar: ")
            leia(pos)
            escreva("Nova quantidade: ")
            leia(quantidades[pos])
         caso 4
            escreval("Encerrando...")
      fimescolha
   fimenquanto
fimalgoritmo
```

`totalCadastrados` funciona como um "ponteiro" de quantas posições do vetor já estão realmente em uso — sem ele, listar percorreria posições vazias do vetor.

### 2 - Biblioteca do campus

```visualg
algoritmo "BibliotecaCampus"
var titulos: vetor[1..10] de caractere
var autores: vetor[1..10] de caractere
var disponivel: vetor[1..10] de logico
var total, opcao, pos: inteiro
inicio
   total <- 3
   titulos[1] <- "Clean Code"
   autores[1] <- "Robert Martin"
   disponivel[1] <- verdadeiro
   titulos[2] <- "Estruturas de Dados"
   autores[2] <- "Anonimo"
   disponivel[2] <- verdadeiro
   titulos[3] <- "Calculo I"
   autores[3] <- "Anonimo"
   disponivel[3] <- verdadeiro

   opcao <- 0
   enquanto opcao <> 4 faca
      escreval("1.Listar 2.Emprestar 3.Devolver 4.Sair")
      leia(opcao)
      escolha opcao
         caso 1
            para pos de 1 ate total faca
               se disponivel[pos] entao
                  escreval(titulos[pos], " - ", autores[pos], " - Disponivel")
               senao
                  escreval(titulos[pos], " - ", autores[pos], " - Emprestado")
               fimse
            fimpara
         caso 2
            escreva("Posicao do livro: ")
            leia(pos)
            disponivel[pos] <- falso
         caso 3
            escreva("Posicao do livro: ")
            leia(pos)
            disponivel[pos] <- verdadeiro
         caso 4
            escreval("Encerrando...")
      fimescolha
   fimenquanto
fimalgoritmo
```

`disponivel` é um vetor `logico` paralelo aos outros dois — a posição `3` de `disponivel` descreve o mesmo livro que a posição `3` de `titulos`.

### 3 - Boletim do semestre

```visualg
algoritmo "BoletimSemestre"
var qtdAlunos, aluno, notaN: inteiro
var nomes: vetor[1..30] de caractere
var medias: vetor[1..30] de real
var n1, n2, n3, somaTurma: real
inicio
   escreva("Quantidade de alunos: ")
   leia(qtdAlunos)
   somaTurma <- 0

   para aluno de 1 ate qtdAlunos faca
      escreva("Nome do aluno ", aluno, ": ")
      leia(nomes[aluno])
      escreva("Notas (3): ")
      leia(n1, n2, n3)
      medias[aluno] <- (n1 + n2 + n3) / 3
      somaTurma <- somaTurma + medias[aluno]

      escreva(nomes[aluno], " - Media: ", medias[aluno], " - ")
      se medias[aluno] >= 6 entao
         escreval("Aprovado")
      senao
         escreval("Reprovado")
      fimse
   fimpara

   escreval("Media geral da turma: ", somaTurma / qtdAlunos)
fimalgoritmo
```

### 4 - Cartão do RU

```visualg
algoritmo "CartaoRU"
var saldo, valor: real
var opcao: inteiro
inicio
   saldo <- 0
   opcao <- 0
   enquanto opcao <> 4 faca
      escreval("1.Saldo 2.Depositar 3.Sacar 4.Sair")
      leia(opcao)
      escolha opcao
         caso 1
            escreval("Saldo atual: ", saldo)
         caso 2
            escreva("Valor a depositar: ")
            leia(valor)
            se valor <= 0 entao
               escreval("Valor invalido")
            senao
               saldo <- saldo + valor
            fimse
         caso 3
            escreva("Valor da refeicao: ")
            leia(valor)
            se (valor <= 0) ou (valor > saldo) entao
               escreval("Operacao invalida")
            senao
               saldo <- saldo - valor
            fimse
         caso 4
            escreval("Encerrando...")
      fimescolha
   fimenquanto
fimalgoritmo
```

### 5 - Auditório da aula magna

```visualg
algoritmo "AuditorioAulaMagna"
var assentos: vetor[1..5, 1..5] de logico
var linha, coluna, opcao: inteiro
inicio
   para linha de 1 ate 5 faca
      para coluna de 1 ate 5 faca
         assentos[linha, coluna] <- verdadeiro // verdadeiro = livre
      fimpara
   fimpara

   opcao <- 0
   enquanto opcao <> 2 faca
      escreval("1.Reservar 2.Sair")
      leia(opcao)
      se opcao = 1 entao
         escreva("Linha e coluna: ")
         leia(linha, coluna)
         se assentos[linha, coluna] entao
            assentos[linha, coluna] <- falso
            escreval("Assento reservado!")
         senao
            escreval("Assento ja ocupado")
         fimse
      fimse
   fimenquanto
fimalgoritmo
```

Guardar `verdadeiro`/`falso` na própria matriz (livre/ocupado) evita precisar de uma segunda estrutura só para controlar disponibilidade.

### 6 - Combinatória do professor

```visualg
algoritmo "CombinatoriaProfessor"

funcao fatorial(n: inteiro): inteiro
inicio
   se n <= 1 entao
      fatorial <- 1
   senao
      fatorial <- n * fatorial(n - 1)
   fimse
fimfuncao

var alunos: inteiro
inicio
   escreva("Numero de alunos na turma: ")
   leia(alunos)
   escreval("Formas de organizar a fila: ", fatorial(alunos))
fimalgoritmo
```

### 7 - Créditos até a formatura

```visualg
algoritmo "CreditosAteFormatura"

funcao somaCreditos(n: inteiro): inteiro
inicio
   se n = 0 entao
      somaCreditos <- 0
   senao
      somaCreditos <- n + somaCreditos(n - 1)
   fimse
fimfuncao

var semestres: inteiro
inicio
   leia(semestres)
   escreval("Total acumulado: ", somaCreditos(semestres))
fimalgoritmo
```

Repare no paralelo com o Exercício 6: os dois são recursão, mudando só a operação (`*` vira `+`) e o valor do caso base (`1` vira `0`) — o caso base sempre precisa devolver o "elemento neutro" daquela operação.

### 8 - Ranking do vestibular

```visualg
algoritmo "RankingVestibular"
var notas: vetor[1..10] de real
var i, j: inteiro
var aux: real
inicio
   para i de 1 ate 10 faca
      escreva("Nota ", i, ": ")
      leia(notas[i])
   fimpara

   para i de 1 ate 9 faca
      para j de 1 ate 10 - i faca
         se notas[j] > notas[j + 1] entao
            aux <- notas[j]
            notas[j] <- notas[j + 1]
            notas[j + 1] <- aux
         fimse
      fimpara
   fimpara

   escreval("--- Ranking (crescente) ---")
   para i de 1 ate 10 faca
      escreval(notas[i])
   fimpara
fimalgoritmo
```

Esse algoritmo de ordenação (bubble sort) compara vizinhos e troca quando estão fora de ordem, repetindo passadas até que nenhuma troca a mais seja necessária — `aux` existe só para não perder um dos dois valores no meio da troca.

### 9 - Testando o portal do aluno

```visualg
algoritmo "TestandoPortal"
var usuarios: vetor[1..3] de caractere
var senhas: vetor[1..3] de caractere
var usuarioDigitado, senhaDigitada: caractere
var i, tentativas: inteiro
var encontrado: logico
inicio
   usuarios[1] <- "ana"
   senhas[1] <- "123"
   usuarios[2] <- "bruno"
   senhas[2] <- "456"
   usuarios[3] <- "carla"
   senhas[3] <- "789"

   tentativas <- 0
   encontrado <- falso
   enquanto (nao encontrado) e (tentativas < 3) faca
      escreva("Usuario: ")
      leia(usuarioDigitado)
      escreva("Senha: ")
      leia(senhaDigitada)
      tentativas <- tentativas + 1

      para i de 1 ate 3 faca
         se (usuarios[i] = usuarioDigitado) e (senhas[i] = senhaDigitada) entao
            encontrado <- verdadeiro
         fimse
      fimpara

      se nao encontrado entao
         escreval("Login invalido")
      fimse
   fimenquanto

   se encontrado entao
      escreval("Login liberado")
   senao
      escreval("Acesso bloqueado")
   fimse
fimalgoritmo
```

### 10 - Carrinho da livraria do campus

```visualg
algoritmo "CarrinhoLivraria"
var nomes: vetor[1..10] de caractere
var precos: vetor[1..10] de real
var totalProdutos, opcao, pos: inteiro
var carrinho: vetor[1..10] de inteiro
var totalCarrinho, total: real
inicio
   totalProdutos <- 2
   nomes[1] <- "Caderno"
   precos[1] <- 15.00
   nomes[2] <- "Caneta"
   precos[2] <- 3.50

   totalCarrinho <- 0
   total <- 0
   opcao <- 0
   enquanto opcao <> 2 faca
      escreval("1.Adicionar ao carrinho 2.Fechar carrinho")
      leia(opcao)
      se opcao = 1 entao
         escreva("Posicao do produto: ")
         leia(pos)
         total <- total + precos[pos]
         escreval("Adicionado! Total parcial: ", total)
      fimse
   fimenquanto
   escreval("Total da compra: ", total)
fimalgoritmo
```

### 11 - Folha de pagamento do departamento

```visualg
algoritmo "FolhaDepartamento"
var qtdProfessores, i: inteiro
var nome: caractere
var salarioBase, bonus, desconto, liquido: real
inicio
   escreva("Quantidade de professores: ")
   leia(qtdProfessores)

   para i de 1 ate qtdProfessores faca
      escreva("Nome: ")
      leia(nome)
      escreva("Salario base, bonus, desconto: ")
      leia(salarioBase, bonus, desconto)
      liquido <- salarioBase + bonus - desconto
      escreval(nome, " - Liquido: ", liquido)
   fimpara
fimalgoritmo
```

### 12 - Eleição do Diretório Acadêmico

```visualg
algoritmo "EleicaoDA"
var votosChapa1, votosChapa2, votosChapa3, opcao: inteiro
inicio
   votosChapa1 <- 0
   votosChapa2 <- 0
   votosChapa3 <- 0
   opcao <- 0

   enquanto opcao <> 4 faca
      escreval("1.Chapa A 2.Chapa B 3.Chapa C 4.Encerrar votacao")
      leia(opcao)
      escolha opcao
         caso 1
            votosChapa1 <- votosChapa1 + 1
         caso 2
            votosChapa2 <- votosChapa2 + 1
         caso 3
            votosChapa3 <- votosChapa3 + 1
      fimescolha
   fimenquanto

   escreval("Chapa A: ", votosChapa1)
   escreval("Chapa B: ", votosChapa2)
   escreval("Chapa C: ", votosChapa3)

   se (votosChapa1 > votosChapa2) e (votosChapa1 > votosChapa3) entao
      escreval("Vencedora: Chapa A")
   senao
      se (votosChapa2 > votosChapa1) e (votosChapa2 > votosChapa3) entao
         escreval("Vencedora: Chapa B")
      senao
         escreval("Vencedora: Chapa C")
      fimse
   fimse
fimalgoritmo
```

### 13 - Easter egg da grade horária

```visualg
algoritmo "DiagonalGradeHoraria"
var matriz: vetor[1..3, 1..3] de inteiro
var linha, coluna, somaDiagonal: inteiro
inicio
   somaDiagonal <- 0
   para linha de 1 ate 3 faca
      para coluna de 1 ate 3 faca
         leia(matriz[linha, coluna])
         se linha = coluna entao
            somaDiagonal <- somaDiagonal + matriz[linha, coluna]
         fimse
      fimpara
   fimpara
   escreval("Soma da diagonal principal: ", somaDiagonal)
fimalgoritmo
```

Na diagonal principal, o número da linha é sempre igual ao número da coluna (`[1,1]`, `[2,2]`, `[3,3]`) — é esse padrão que a condição `linha = coluna` captura.

### 14 - Analisador de título de TCC

```visualg
algoritmo "AnalisadorTitulo"
// Descricao da logica esperada, caso a versao do VisuAlg
// nao tenha manipulacao de caracteres individual:
//
// 1. Percorrer a palavra caractere por caractere.
// 2. Para cada caractere, comparar (ignorando maiusculas/minusculas)
//    contra "a", "e", "i", "o", "u".
// 3. Incrementar um contador a cada vogal encontrada.
// 4. Ao final do laco, mostrar o contador.
inicio
   escreval("Descreva a logica em comentarios se faltar suporte a caracteres.")
fimalgoritmo
```

### 15 - Pedidos da cantina

```visualg
algoritmo "PedidosCantina"
var cliente, produto: caractere
var quantidade: inteiro
var precoUnitario, subtotal, desconto, total: real
var temCarteirinha: logico
inicio
   escreva("Cliente: ")
   leia(cliente)
   escreva("Produto: ")
   leia(produto)
   escreva("Quantidade: ")
   leia(quantidade)
   escreva("Preco unitario: ")
   leia(precoUnitario)
   escreva("Tem carteirinha de aluno? (verdadeiro/falso): ")
   leia(temCarteirinha)

   subtotal <- quantidade * precoUnitario
   se temCarteirinha entao
      desconto <- subtotal * 0.10
   senao
      desconto <- 0
   fimse
   total <- subtotal - desconto

   escreval("--- Relatorio do pedido ---")
   escreval("Cliente: ", cliente)
   escreval("Produto: ", produto, " x", quantidade)
   escreval("Subtotal: ", subtotal)
   escreval("Desconto: ", desconto)
   escreval("Total: ", total)
fimalgoritmo
```
