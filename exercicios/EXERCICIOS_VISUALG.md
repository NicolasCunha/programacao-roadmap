# Folha de Exercícios - VisuAlg

> **Objetivo:** praticar lógica de programação em VisuAlg com exercícios progressivos. Não use IA para gerar o algoritmo final. Primeiro tente escrever os passos em português, depois transforme os passos em código. Fique à vontade para fazer pesquisas na internet, mas tente não encontrar respostas prontas.

## Como resolver cada exercício

Para cada atividade:

1. Leia o enunciado com calma — inclusive a "historinha", ela geralmente contém uma pista de que estrutura usar.
2. Escreva os passos da solução em português.
3. Identifique as variáveis necessárias.
4. Crie o algoritmo no VisuAlg.
5. Execute e teste com mais de um valor.
6. Comente o que o algoritmo faz.
7. Salve o arquivo `.alg` no repositório.

Os exercícios marcados com 🎁 têm um desafio bônus opcional — vale tentar depois de resolver a versão principal, sem pressa.

---

# Nível Fácil — 🏝️ Ilha Deserta

Um navio afundou perto de uma ilha desconhecida. Um grupo de náufragos sobreviveu e nadou até a praia — e agora precisa se organizar para sobreviver até o resgate chegar. Você é a pessoa encarregada de programar o rádio de emergência recuperado dos destroços, usado para registrar tudo: quem chegou, quanto de comida existe, quem pode fazer tarefas arriscadas. Cada exercício é uma tarefa real do acampamento.

## Exercício 1 - O rádio liga

Antes de confiar o rádio para qualquer tarefa séria, o grupo quer ter certeza de que ele ainda funciona depois do naufrágio.

Crie um algoritmo que imprima na tela a mensagem:

```text
Ola, mundo!
```

O objetivo é garantir que o estudante consegue criar, executar e visualizar a saída de um algoritmo no VisuAlg.

## Exercício 2 - Registro de sobreviventes

Toda pessoa que chega à praia precisa ser registrada no rádio, para o grupo saber exatamente quantos náufragos existem.

Crie um algoritmo que leia o nome de uma pessoa e mostre uma mensagem de saudação personalizada, confirmando o registro.

Exemplo de saída:

```text
Ola, Ana!
```

## Exercício 3 - Turno de escalada no penhasco

Escalar o penhasco para observar o horizonte em busca de navios é arriscado — só quem já é maior de idade pode assumir essa tarefa.

Crie um algoritmo que leia a idade de uma pessoa e informe se ela é maior ou menor de idade.

Regra:

- idade maior ou igual a 18: maior de idade;
- idade menor que 18: menor de idade.

## Exercício 4 - Divisão de provisões

O grupo encontrou uma caixa de suprimentos e precisa fazer as contas rapidamente antes de decidir o que fazer com ela.

Crie um algoritmo que leia dois números e mostre:

- soma;
- subtração;
- multiplicação;
- divisão.

## Exercício 5 - Resistência da corda

Dois membros do grupo testaram, cada um à sua maneira, o quanto uma corda trançada à mão aguenta de peso antes de arrebentar.

Crie um algoritmo que leia dois valores de resistência (em kg) e calcule a média entre eles.

A média deve ser calculada somando os dois valores e dividindo por 2.

## Exercício 6 - Escambo com o barco mercante

Um pequeno barco mercante passa perto da ilha de vez em quando e aceita trocar itens por conchas raras — hoje ele está com uma promoção de 10% de desconto em tudo.

Crie um algoritmo que leia o preço (em conchas) de um item e calcule o valor final com 10% de desconto.

Mostre:

- preço original;
- valor do desconto;
- preço final.

## Exercício 7 - Código de tambor

O grupo descobriu que consegue se comunicar com outro acampamento distante usando batidas de tambor em múltiplos de um número combinado.

Crie um algoritmo que leia um número inteiro e mostre sua tabuada de 1 até 10 — a sequência de batidas que será usada como código.

## Exercício 8 - A nascente mais forte

Foram encontradas três nascentes de água doce na ilha, cada uma com uma vazão diferente. O grupo precisa saber qual delas abastece melhor o acampamento.

Crie um algoritmo que leia três números (a vazão de cada nascente) e mostre qual deles é o maior.

## Exercício 9 - Divisão justa de conchas

Um grupo de sobreviventes encontrou uma quantidade de conchas e quer saber se dá para dividir igualmente entre duas pessoas, sem sobrar nenhuma.

Crie um algoritmo que leia um número inteiro e informe se ele é par ou ímpar.

## Exercício 10 - Créditos de resgate

O rádio conseguiu captar a central de resgate, que informa quantos "créditos de resgate" cada concha vale, numa taxa fixa.

Crie um algoritmo que leia uma quantidade de conchas e converta para créditos de resgate usando uma cotação fixa definida no próprio algoritmo.

Exemplo:

```text
cotacao <- 5.00
```

## Exercício 11 - A colheita rendeu mais

O grupo achou uma plantação de frutas nativas maior do que o esperado, e a ração diária de cada pessoa pode aumentar em 8%.

Crie um algoritmo que leia a ração diária atual (em gramas) e calcule a nova ração com aumento de 8%.

## Exercício 12 - Área do abrigo

Antes de começar a cortar galhos, o grupo precisa saber quanto de espaço o novo abrigo retangular vai ocupar.

Crie um algoritmo que leia a base e a altura do abrigo e calcule sua área.

Fórmula:

```text
area = base * altura
```

## Exercício 13 - Cofre de suprimentos 🎁

O grupo escondeu os suprimentos mais valiosos em um cofre de madeira trancado. Só quem sabe o código de acesso definido pelo líder pode abrir.

Crie um algoritmo que leia usuário e senha.

O acesso deve ser liberado apenas se:

- usuário for `admin`;
- senha for `1234`.

Caso contrário, mostre uma mensagem de acesso negado.

**Desafio bônus:** depois de resolver, pense (sem precisar implementar ainda) em como você limitaria as tentativas de digitar a senha errada — isso vai virar um exercício de verdade mais adiante.

## Exercício 14 - Contando os dias

Alguém sugeriu marcar cada dia de espera em um pedaço de madeira, como um calendário improvisado.

Crie um algoritmo que leia um número inteiro positivo e conte de 1 até esse número usando estrutura de repetição.

## Exercício 15 - Diário de bordo

O grupo tem exatamente cinco sobreviventes confirmados, e alguém sugeriu manter um diário de bordo com o nome de cada um.

Crie um algoritmo que leia cinco nomes usando vetor e depois mostre todos os nomes cadastrados.

---

# Nível Médio — ⚽ Campeonato de Futebol de Várzea

O campeonato de futebol de várzea do bairro está de volta, e você foi escalado (sem trocadilho intencional) para programar os sistemas usados pela organização: da tabela de classificação à barraquinha que vende água e salgado durante os jogos.

## Exercício 1 - Caixa do campeonato

A organização arrecada dinheiro com inscrições dos times e precisa controlar esse caixa junto de qualquer saque feito para comprar troféus, bolas e uniformes de árbitro.

Crie um algoritmo que simule um saque desse caixa.

Regras:

- o saldo inicial deve ser definido no algoritmo;
- o organizador informa o valor do saque;
- saque menor ou igual a zero deve ser recusado;
- saque maior que o saldo deve ser recusado;
- saque válido deve atualizar o saldo.

## Exercício 2 - Média de gols da rodada

Depois de cada rodada, a organização quer saber a média de gols marcados, olhando o placar de cada jogo.

Crie um algoritmo que leia a quantidade de jogos da rodada, leia o total de gols de cada jogo e calcule a média geral de gols da rodada.

## Exercício 3 - Painel da secretaria do campeonato

A secretaria do campeonato criou um pequeno painel para organizar os times inscritos.

Crie um algoritmo com menu contendo as opções:

1. Cadastrar time;
2. Listar times;
3. Sair.

O menu deve continuar aparecendo até o organizador escolher sair.

## Exercício 4 - Estatísticas da rodada

Um estagiário anotou o número de gols de dez partidas diferentes e quer um resumo rápido para o boletim do campeonato.

Crie um algoritmo que leia dez números em um vetor (gols de cada partida) e mostre:

- soma total de gols;
- média de gols por partida;
- maior número de gols em uma partida;
- menor número de gols em uma partida.

## Exercício 5 - Barraquinha do campeonato

A barraquinha que vende lanches durante os jogos tem cinco produtos no cardápio, cada um com seu preço.

Crie um algoritmo que leia cinco produtos e seus respectivos preços usando vetores.

Ao final, mostre:

- produto mais caro;
- produto mais barato.

## Exercício 6 - Transporte do time visitante

Times visitantes de outros bairros às vezes precisam de ônibus fretado — mas se a taxa de inscrição paga por eles for alta o bastante, a organização banca o transporte de graça.

Crie um algoritmo que calcule o custo do transporte de um time visitante.

Regras:

- se o valor pago na inscrição for maior ou igual a 300, o transporte é grátis;
- caso contrário, o custo do transporte é a distância (em km) multiplicada por 1,50.

## Exercício 7 - Apito final

Durante a partida, cada gol é anotado no rádio do bandeirinha, um de cada vez, até o apito final ser digitado como 0.

Crie um algoritmo que leia números (gols) até o usuário digitar 0.

Ao final, mostre:

- quantidade de gols marcados, sem contar o zero;
- soma total de gols da partida.

## Exercício 8 - Aposta do intervalo

No intervalo, a torcida faz uma brincadeira: alguém escolhe um número secreto (o número da camisa do artilheiro da rodada) e os torcedores tentam adivinhar.

Crie um algoritmo com um número secreto fixo.

O torcedor deve tentar adivinhar o número até acertar.

## Exercício 9 - Escalação titular

O técnico quer organizar a escalação titular em duas linhas de três jogadores cada, para visualizar melhor no papel.

Crie um algoritmo que leia valores para uma matriz 2x3 (nomes ou números de camisa) e depois mostre todos os valores cadastrados.

## Exercício 10 - Rodada em dobro

Em uma rodada especial de premiação, a organização decidiu que os pontos de qualquer time valem o dobro.

Crie uma função que receba um número inteiro (pontos) e retorne o dobro desse número.

Depois, use a função dentro do algoritmo principal.

## Exercício 11 - Desconto de sócio-torcedor

Times que são "sócio-torcedores" do campeonato há mais de um ano ganham desconto na taxa de inscrição da próxima temporada.

Crie uma função que receba o valor da inscrição e o percentual de desconto, calcule e retorne o valor do desconto.

## Exercício 12 - Boletim do campeonato

Antes de cada seção do boletim impresso (classificação, artilharia, próximos jogos), a organização sempre repete o mesmo cabeçalho com o nome e a edição do campeonato.

Crie um procedimento que mostre esse cabeçalho.

Use esse procedimento em pelo menos três momentos diferentes do algoritmo.

## Exercício 13 - Vestiário restrito 🎁

O vestiário da equipe técnica só pode ser acessado com uma senha, e o segurança do campo tem instruções de bloquear o acesso depois de tentativas demais.

Crie um algoritmo que peça uma senha ao usuário.

Regras:

- a senha correta é `1234`;
- o usuário tem no máximo três tentativas;
- se acertar, mostre acesso liberado;
- se errar três vezes, mostre acesso bloqueado.

**Desafio bônus:** faça o algoritmo mostrar quantas tentativas ainda restam a cada erro.

## Exercício 14 - Prêmio bola cheia

O prêmio "bola cheia" da rodada vai para todo jogador que pontuou acima da média geral da rodada.

Crie um algoritmo que leia um vetor de pontuações, calcule a média e mostre quantas pontuações ficaram acima da média.

## Exercício 15 - Estoque da barraquinha

A barraquinha cresceu e agora precisa de um controle melhor: nome, preço e quantidade em estoque de cada produto.

Crie um algoritmo que cadastre produtos usando vetores paralelos para:

- nome;
- preço;
- quantidade.

Ao final, liste todos os produtos cadastrados.

---

# Nível Difícil — 🎓 Campus Universitário

Você foi contratado como estagiário de TI do campus e ficou responsável por programar, um sistema de cada vez, as ferramentas que professores, alunos e a administração usam no dia a dia — da cantina até a eleição do diretório acadêmico.

## Exercício 1 - Estoque da cantina

A cantina do campus precisa de um sistema simples para controlar os itens vendidos entre uma aula e outra.

Crie um mini-sistema de controle de estoque com menu.

Funcionalidades:

- cadastrar produto;
- listar produtos;
- atualizar quantidade;
- sair.

Use vetores para armazenar os dados.

## Exercício 2 - Biblioteca do campus

A biblioteca central quer digitalizar o controle de empréstimos, que hoje é feito em um caderno de capa dura.

Crie um sistema de biblioteca usando vetores para armazenar:

- título do livro;
- autor;
- disponibilidade.

Funcionalidades:

- listar livros;
- emprestar livro;
- devolver livro.

## Exercício 3 - Boletim do semestre

A coordenação do curso precisa calcular o boletim de cada aluno a partir de três notas por disciplina.

Crie um algoritmo que cadastre alunos e três notas por aluno.

Ao final, mostre:

- média de cada aluno;
- situação aprovado ou reprovado;
- média geral da turma.

## Exercício 4 - Cartão do RU

O Restaurante Universitário (RU) usa um cartão com crédito pré-pago para as refeições dos alunos.

Crie um terminal de cartão do RU com menu:

1. consultar saldo;
2. depositar (recarregar cartão);
3. sacar (pagar uma refeição);
4. sair.

Valide valores inválidos em todas as operações.

## Exercício 5 - Auditório da aula magna

A aula magna do semestre é tão concorrida que a coordenação decidiu abrir reserva de assentos no auditório.

Crie um algoritmo que represente os assentos do auditório usando matriz.

Funcionalidades:

- listar assentos;
- reservar assento;
- impedir reserva duplicada.

## Exercício 6 - Combinatória do professor

O professor de Matemática Discreta pediu um algoritmo que calcule, para qualquer turma, de quantas formas os alunos podem ser organizados em fila — o clássico cálculo de fatorial.

Crie uma função recursiva para calcular o fatorial de um número.

O algoritmo principal deve ler o número e mostrar o resultado.

## Exercício 7 - Créditos até a formatura

Um aluno quer saber quantos créditos precisa somar, semestre a semestre, até chegar em N semestres — supondo que ele cursa exatamente `semestre` créditos a cada período.

Crie uma função recursiva que calcule a soma de 1 até N.

Exemplo:

```text
N = 5
resultado = 1 + 2 + 3 + 4 + 5
```

## Exercício 8 - Ranking do vestibular

A secretaria acadêmica recebeu as notas finais do vestibular e precisa publicar o ranking em ordem crescente antes da divulgação oficial.

Crie um algoritmo que leia dez números em um vetor e ordene os valores em ordem crescente.

Use lógica simples de comparação e troca.

## Exercício 9 - Testando o portal do aluno

Antes de liberar o novo portal do aluno para todo o campus, a equipe de TI quer testar o login com três contas fictícias.

Crie um sistema de login com três usuários e três senhas usando vetores.

O usuário deve ter no máximo três tentativas.

## Exercício 10 - Carrinho da livraria do campus

A livraria do campus quer um carrinho de compras simples para o balcão, enquanto o sistema completo não fica pronto.

Crie um simulador de carrinho de compras.

Funcionalidades:

- cadastrar produtos;
- adicionar produto ao carrinho;
- calcular total da compra.

## Exercício 11 - Folha de pagamento do departamento

O departamento precisa calcular o salário líquido de vários professores, cada um com valores diferentes de bônus e desconto.

Crie um algoritmo que calcule a folha de pagamento de vários professores.

Para cada professor, leia:

- nome;
- salário base;
- bônus;
- desconto.

Mostre o salário líquido.

## Exercício 12 - Eleição do Diretório Acadêmico

Três chapas concorrem à eleição do Diretório Acadêmico (DA) deste ano, e a comissão eleitoral precisa de um sistema de apuração simples.

Crie um sistema de votação com três candidatos (chapas).

Funcionalidades:

- registrar votos;
- encerrar votação;
- mostrar total de votos por chapa;
- mostrar vencedor.

## Exercício 13 - Easter egg da grade horária

Um aluno de exatas percebeu um padrão na grade de horários 3x3 do seu semestre e quer testar uma curiosidade matemática: a soma da diagonal principal.

Crie um algoritmo que leia uma matriz 3x3 e calcule a soma da diagonal principal.

## Exercício 14 - Analisador de título de TCC

O núcleo de pesquisa quer um analisador simples para contar vogais no título de trabalhos de conclusão de curso, como uma métrica (bem informal) de "facilidade de pronúncia".

Crie um algoritmo que leia uma palavra e conte quantas vogais ela possui.

Se sua versão do VisuAlg tiver limitação com manipulação de caracteres, descreva a lógica esperada em comentários.

## Exercício 15 - Pedidos da cantina 🎁

A cantina cresceu tanto que agora aceita pedidos com desconto para quem tem carteirinha de aluno.

Crie um mini-sistema de pedidos que leia:

- cliente;
- produto;
- quantidade;
- preço unitário.

O sistema deve calcular subtotal, aplicar desconto quando necessário e exibir um relatório final.

**Desafio bônus:** depois de resolver, pense em como esse mesmo problema mudaria se a cantina precisasse guardar os pedidos entre uma execução e outra do programa — essa é exatamente a pergunta que o [Nível 3](../NIVEL_3_BANCO_DE_DADOS.md) começa a responder.
