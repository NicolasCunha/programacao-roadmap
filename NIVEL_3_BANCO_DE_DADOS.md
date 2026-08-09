# Nível 3 - Fundamentos de Banco de Dados

Nos níveis anteriores, todo dado que seu programa usava vivia dentro da memória do próprio programa: variáveis, listas, mapas. Este nível resolve uma pergunta que todo sistema real precisa responder: **onde os dados ficam guardados quando o programa fecha, e como garantimos que eles continuem confiáveis conforme o sistema cresce?**

Este é um nível longo de propósito. Banco de dados é uma das poucas áreas onde um iniciante que pula os fundamentos "básicos" (tipo de dado errado, ausência de chave, falta de `WHERE`) carrega esse erro pelo resto da carreira, porque banco de dados guarda estado — diferente de um bug de lógica, um erro de modelagem de dados corrompe informação real e é caro de corrigir depois. Vale a pena ir devagar aqui.

---

## Índice de progresso

- [ ] [Objetivo do nível](#objetivo-do-nível)
- [ ] [Uso responsável de IA](#uso-responsável-de-ia)
- [ ] [Dado x informação](#dado-x-informação)
- [ ] [O problema que o banco de dados resolve](#o-problema-que-o-banco-de-dados-resolve)
- [ ] [O que é um banco de dados](#o-que-é-um-banco-de-dados)
- [ ] [Uma breve história: como chegamos ao modelo relacional](#uma-breve-história-como-chegamos-ao-modelo-relacional)
- [ ] [SQL: um padrão, várias implementações](#sql-um-padrão-várias-implementações)
- [ ] [Famílias de banco de dados](#famílias-de-banco-de-dados)
- [ ] [SGBD: quem gerencia o banco](#sgbd-quem-gerencia-o-banco)
- [ ] [O caminho de uma consulta dentro do SGBD](#o-caminho-de-uma-consulta-dentro-do-sgbd)
- [ ] [Propriedades ACID](#propriedades-acid)
- [ ] [Transações: uma prévia](#transações-uma-prévia)
- [ ] [Banco relacional](#banco-relacional)
- [ ] [Tabela, coluna e linha](#tabela-coluna-e-linha)
- [ ] [Entidade x tabela](#entidade-x-tabela)
- [ ] [Modelagem conceitual, lógica e física](#modelagem-conceitual-lógica-e-física)
- [ ] [Convenções de nomenclatura](#convenções-de-nomenclatura)
- [ ] [Tipos de dado](#tipos-de-dado)
- [ ] [O valor NULL](#o-valor-null)
- [ ] [Constraints (restrições)](#constraints-restrições)
- [ ] [Chave primária](#chave-primária)
- [ ] [Chave estrangeira](#chave-estrangeira)
- [ ] [Índices](#índices)
- [ ] [Instalando o MySQL](#instalando-o-mysql)
- [ ] [Criando seu primeiro banco e tabela](#criando-seu-primeiro-banco-e-tabela)
- [ ] [Charset e collation: por que acentos quebram](#charset-e-collation-por-que-acentos-quebram)
- [ ] [Inserindo dados](#inserindo-dados)
- [ ] [Consultando dados](#consultando-dados)
- [ ] [Atualizando dados](#atualizando-dados)
- [ ] [Removendo dados](#removendo-dados)
- [ ] [Erros comuns e como interpretá-los](#erros-comuns-e-como-interpretá-los)
- [ ] [Segurança básica: SQL Injection](#segurança-básica-sql-injection)
- [ ] [Conectando o Java ao MySQL: JDBC](#conectando-o-java-ao-mysql-jdbc)
- [ ] [Backup: por que pensar nisso desde já](#backup-por-que-pensar-nisso-desde-já)
- [ ] [Glossário rápido](#glossário-rápido)
- [ ] [Recapitulando](#recapitulando)
- [ ] [O que vem no próximo nível](#o-que-vem-no-próximo-nível)

---

## Objetivo do nível

Ao final deste nível, o estudante deve conseguir:

- Explicar, com as próprias palavras, por que aplicações reais não guardam dados só em memória ou em arquivos soltos.
- Explicar o que é um banco de dados, o que é um SGBD e o que significa ACID, sem recorrer só à definição de "linhas e colunas".
- Escolher o tipo de dado correto para cada coluna e justificar a escolha (por exemplo, por que dinheiro não deve ser `FLOAT`).
- Explicar o que é `NULL` e por que ele se comporta de forma diferente de outros valores.
- Explicar o que é uma chave primária, uma chave estrangeira e um índice, e por que cada um existe.
- Instalar o MySQL e criar um banco e uma tabela do zero, com constraints apropriadas.
- Escrever comandos SQL de criação, inserção, consulta, atualização e remoção de dados em uma única tabela, incluindo filtros mais ricos que um `WHERE id = 1`.
- Ler uma mensagem de erro do MySQL e identificar a causa mais provável.

Relacionamentos entre tabelas, `JOIN` e normalização ficam para o [Nível 4](./NIVEL_4_SQL_MODELAGEM.md). Aqui o foco é entender uma tabela até o osso antes de trabalhar com várias ao mesmo tempo.

## Uso responsável de IA

Nesta fase, a IA pode ajudar a explicar um erro do MySQL ou comparar duas formas de escrever a mesma consulta. Evite pedir para ela criar a tabela ou o comando SQL do exercício por você. O objetivo é que a lógica de "que colunas eu preciso", "que tipo cada uma deve ter" e "o que pode ou não ser vazio" vire raciocínio seu, não um resultado colado.

---

## Dado x informação

Antes de falar de banco de dados, vale separar dois termos que são usados como sinônimos no dia a dia, mas que têm papéis diferentes:

- **Dado** é um valor bruto, sem contexto: `250.00`, `"Ana"`, `18`.
- **Informação** é o dado interpretado dentro de um contexto que faz ele significar algo: "o pedido 250.00 pertence à cliente Ana, que tem 18 anos".

Um banco de dados guarda dados. A estrutura que você desenha — quais tabelas existem, quais colunas cada uma tem, como elas se conectam — é o que permite transformar esses dados de volta em informação quando alguém consulta. Uma modelagem ruim não perde os dados, mas perde a capacidade de reconstruir a informação de forma confiável. Isso vai ficar mais concreto ao longo deste nível.

## O problema que o banco de dados resolve

### Por que memória RAM não basta

Imagine que você está construindo o cadastro de clientes de um e-commerce em Java, do jeito que aprendeu no Nível 2:

```java
List<Cliente> clientes = new ArrayList<>();
clientes.add(new Cliente("Ana", "ana@email.com"));
```

Isso funciona enquanto o programa está rodando. Mas assim que o programa é fechado — ou trava, ou o servidor reinicia, ou dá queda de energia — a lista `clientes` desaparece. Ela vive na memória RAM, e memória RAM é **volátil**: existe apenas enquanto há energia e o processo está ativo. Não existe "salvar sem querer" em RAM; ao fechar o processo, o conteúdo simplesmente deixa de existir.

Isso não é um detalhe técnico menor. É o motivo central pelo qual todo sistema que precisa lembrar de algo depois de reiniciar precisa de um lugar para persistir dados fora da memória do processo.

### Por que um arquivo de texto quase basta, mas não basta

Uma primeira ideia seria salvar os clientes em um arquivo de texto:

```text
1;Ana;ana@email.com
2;Bruno;bruno@email.com
```

Isso já resolve o problema de perder os dados ao fechar o programa. Só que resolver "os dados sobrevivem" não é o mesmo que resolver "os dados continuam confiáveis e utilizáveis". Veja o que ainda falta:

**1. Não há como consultar de forma eficiente.** "Me dê todos os clientes cadastrados este mês" exige ler o arquivo inteiro, linha por linha, interpretar cada campo manualmente e filtrar na aplicação. Com 200 linhas isso é rápido. Com 20 milhões de linhas, ler o arquivo inteiro para cada consulta se torna inviável.

**2. Não há controle de acesso simultâneo.** Imagine dois processos do seu sistema tentando gravar no mesmo arquivo ao mesmo tempo — um processando um novo cadastro, outro atualizando um email. Sem coordenação, um processo pode começar a escrever no meio da escrita do outro, corrompendo o arquivo inteiro (não só a linha em questão). Esse tipo de corrupção costuma aparecer em produção, sob carga, exatamente quando é mais caro de resolver.

**3. Não há garantia de integridade.** Nada no arquivo impede que o `id` `1` apareça duas vezes, com dois nomes diferentes. Nada impede que um "pedido" salvo em outro arquivo referencie um `cliente_id` que nunca existiu. A responsabilidade de manter tudo consistente cai inteiramente sobre o código da aplicação, e qualquer bug — ou qualquer edição manual do arquivo — quebra essa consistência silenciosamente.

**4. Não há um formato padrão de pergunta.** Cada nova necessidade de consulta ("clientes sem pedido", "produtos com estoque zerado", "total vendido por categoria") exige escrever mais código Java para ler, filtrar e cruzar arquivos manualmente. Isso é lento de desenvolver e fácil de fazer errado.

**5. Não sobrevive a falhas no meio da escrita.** Se o processo cair exatamente no meio da gravação de uma linha, o arquivo pode ficar com uma linha incompleta ou truncada, e não existe um mecanismo automático para detectar ou desfazer essa escrita parcial.

Um banco de dados existe para resolver essas cinco lacunas ao mesmo tempo: persistir os dados de forma confiável, permitir consultas rápidas e flexíveis através de uma linguagem padronizada, controlar acesso simultâneo, garantir integridade e se recuperar de falhas no meio de uma operação.

## O que é um banco de dados

Um banco de dados é um conjunto organizado de dados persistentes — ou seja, que sobrevivem ao fechamento do programa que os criou — guardado de um jeito que permite consultar, alterar e confiar nessas informações mesmo com muitos usuários e processos mexendo neles ao mesmo tempo.

Repare que a definição não fala de "linhas e colunas" em nenhum momento. Linhas e colunas são a forma como um tipo específico de banco de dados (o **relacional**, foco deste roadmap) organiza a informação. O banco de dados em si resolve um problema mais fundamental: **fazer dados sobreviverem e continuarem confiáveis e consultáveis ao longo do tempo**, independente da forma escolhida para organizá-los.

Uma analogia útil: pense em um arquivo físico de uma empresa, com armários e pastas etiquetadas, comparado a uma pilha solta de papéis em cima de uma mesa. As duas guardam a mesma informação, mas só uma delas permite que qualquer pessoa da empresa encontre o que precisa em segundos, sem depender de quem organizou a pilha e sem risco de dois funcionários rasurarem o mesmo papel ao mesmo tempo. Um banco de dados é o armário organizado, com um funcionário (o SGBD, que vem a seguir) responsável por ele. O arquivo de texto do exemplo anterior é a pilha de papéis: os dados estão ali, mas ninguém garante nada sobre eles.

### Schema x dados x instância

Três termos que costumam ser confundidos por quem está começando:

- **Schema (esquema)**: a estrutura — quais tabelas existem, quais colunas cada uma tem, quais tipos, quais chaves. É a "planta baixa" do banco. `CREATE TABLE` define schema.
- **Dados**: o conteúdo propriamente dito, guardado dentro dessa estrutura. `INSERT` grava dados.
- **Instância**: o estado do banco em um momento específico no tempo — o schema mais os dados que existem agora. Se você inserir uma linha, a instância muda, mas o schema continua o mesmo.

Essa distinção importa porque a maior parte dos erros de modelagem acontece no schema (a decisão de estrutura), não nos dados (o conteúdo). Um schema mal desenhado permite dados inconsistentes de entrar; um schema bem desenhado impede isso na origem, antes mesmo de qualquer linha de código da aplicação rodar.

## Uma breve história: como chegamos ao modelo relacional

Não é preciso decorar isso, mas ajuda a entender por que o modelo relacional é o padrão até hoje.

Nos anos 1960, os primeiros sistemas de banco de dados usavam modelos **hierárquico** (dados organizados como uma árvore, um "pai" com vários "filhos") e **em rede** (parecido, mas um registro podia ter vários "pais"). Os dois funcionavam, mas exigiam que quem fosse consultar os dados já soubesse de antemão o caminho exato dentro da estrutura — como navegar pastas e subpastas manualmente. Mudar a estrutura depois era caro, porque todo código que navegava aquele caminho quebrava.

Em 1970, o pesquisador **Edgar F. Codd**, da IBM, publicou o artigo que propôs o **modelo relacional**: em vez de o programador navegar caminhos fixos, os dados seriam organizados em tabelas (matematicamente, "relações") e consultados através de uma linguagem declarativa — você diz **o que** quer, não **como** navegar até lá. Essa ideia deu origem ao SQL alguns anos depois, e se tornou o padrão dominante de banco de dados pelas décadas seguintes, motivo pelo qual este roadmap foca nele.

## SQL: um padrão, várias implementações

SQL significa **Structured Query Language**. É a linguagem usada para criar estruturas, inserir, consultar, atualizar e remover dados em bancos relacionais — todos os comandos que você viu até aqui (`CREATE TABLE`, `INSERT`, `SELECT`, `UPDATE`, `DELETE`) são SQL.

SQL possui padrões internacionais definidos por comitês (ANSI/ISO), que evoluíram ao longo do tempo: SQL-86, SQL-89, SQL-92, SQL:1999, SQL:2003, SQL:2008, SQL:2011, SQL:2016 e SQL:2023. Cada revisão adicionou recursos novos à linguagem.

Na prática, isso importa por um motivo simples: **cada SGBD implementa o padrão com variações próprias**. A base — `SELECT`, `WHERE`, `JOIN`, tipos como `INT` e `VARCHAR` — é praticamente idêntica entre MySQL, PostgreSQL e SQL Server. Mas detalhes como `AUTO_INCREMENT` (MySQL) versus `SERIAL` (PostgreSQL), ou pequenas diferenças de sintaxe em funções de data, são específicos de cada implementação.

Ao estudar MySQL neste roadmap, você está aprendendo SQL padrão com algumas particularidades do MySQL. Trocar de SGBD no futuro significa reaprender essas particularidades pontuais — não reaprender o modelo relacional inteiro, que continua sendo o mesmo raciocínio em qualquer um deles.

## Famílias de banco de dados

O modelo relacional não é a única forma de organizar um banco de dados — só é a mais comum como ponto de partida, e a mais adequada para dados estruturados com relações claras (como um e-commerce). Vale saber que outras famílias existem, mesmo sem aprofundar nelas agora:

| Família | Como organiza os dados | Exemplo de uso típico |
|---|---|---|
| Relacional | Tabelas com linhas e colunas, conectadas por chaves | Sistemas de cadastro, financeiro, ERPs |
| Documento | Documentos flexíveis, tipo JSON, sem schema fixo | Catálogos com atributos variáveis por item |
| Chave-valor | Um identificador aponta direto para um valor | Cache, sessão de usuário |
| Colunar | Otimizado para ler colunas inteiras rapidamente | Análise de grandes volumes de dados |
| Grafo | Nós e conexões entre eles como estrutura central | Redes sociais, sistemas de recomendação |

Este roadmap foca 100% em relacional porque é o modelo mais usado em sistemas de negócio tradicionais e porque os conceitos que você aprende aqui (chave, integridade, consulta declarativa) formam a base para entender qualquer um dos outros modelos depois, por comparação.

## SGBD: quem gerencia o banco

Um armário de arquivos não se organiza sozinho. Alguém precisa decidir onde cada pasta fica, impedir que duas pessoas peguem a mesma gaveta ao mesmo tempo de forma desordenada, e garantir que ninguém guarde um papel fora do padrão esperado.

Esse "alguém", no mundo dos bancos de dados, é o **SGBD**: Sistema Gerenciador de Banco de Dados.

O SGBD é o software responsável por:

- gravar os dados em disco de forma durável;
- criar e manter a estrutura de bancos e tabelas (o schema);
- interpretar e executar comandos SQL de forma eficiente, mesmo com muitos dados;
- controlar acesso simultâneo, para dois processos não corromperem os mesmos dados ao mesmo tempo;
- aplicar regras de integridade (por exemplo, impedir um pedido sem cliente válido);
- controlar permissões de quem pode ler ou alterar o quê;
- oferecer mecanismos de backup e recuperação em caso de falha.

Exemplos de SGBD relacional:

| SGBD | Licença | Observação |
|---|---|---|
| MySQL | Gratuito (Community) / paga (Enterprise) | Muito usado no mercado, boa documentação |
| PostgreSQL | Gratuito e open source | Muito completo em recursos avançados |
| SQL Server | Paga (com edição gratuita limitada) | Comum em ambientes Microsoft/.NET |
| Oracle Database | Paga | Comum em grandes empresas legadas |
| MariaDB | Gratuito e open source | Derivado do MySQL, compatível na maior parte |
| SQLite | Gratuito e open source | Banco embutido, sem servidor separado, usado em apps mobile |

Todos resolvem o mesmo problema central e compartilham os mesmos conceitos fundamentais deste nível (tabela, chave, `SELECT`), mas cada um tem particularidades próprias de sintaxe e recursos.

Neste roadmap usamos **MySQL**, por ser gratuito na edição Community, popular no mercado brasileiro e bem documentado para quem está começando. O raciocínio que você constrói aqui — o que é uma tabela, uma chave, uma consulta — se transfere quase diretamente para PostgreSQL ou qualquer outro SGBD relacional que você usar no futuro. O que muda entre eles é sintaxe pontual (por exemplo, `AUTO_INCREMENT` no MySQL vira `SERIAL` no PostgreSQL), não o modelo mental.

## O caminho de uma consulta dentro do SGBD

Vale entender, em linhas gerais, o que acontece entre você digitar `SELECT * FROM clientes;` e a resposta aparecer na tela. Não é mágica, e entender esse caminho ajuda a explicar por que alguns comandos são mais lentos que outros — assunto que retomamos na seção de [Índices](#índices).

1. **Parsing**: o SGBD lê o texto do comando SQL e verifica se ele é sintaticamente válido (parênteses fechados, palavras-chave corretas).
2. **Validação**: verifica se a tabela e as colunas mencionadas realmente existem, e se você tem permissão para acessá-las.
3. **Planejamento e otimização**: o SGBD decide o **como** — por exemplo, se existe um índice que torna a busca mais rápida do que ler a tabela inteira, ele usa esse caminho.
4. **Execução**: o plano escolhido roda de fato, lendo os dados do disco (ou de memória cache, se já tiverem sido lidos recentemente).
5. **Retorno**: o resultado é devolvido para quem fez a consulta — o MySQL Workbench, ou o código Java da sua aplicação através de um driver de conexão.

Isso é o mesmo tanto para um `SELECT` simples quanto para uma consulta complexa com `JOIN` (Nível 4). A diferença é que consultas mais complexas dão mais trabalho para o otimizador decidir o melhor caminho de execução.

## Propriedades ACID

Quando dissemos, na seção anterior, que o SGBD "controla acesso simultâneo" e "garante integridade", isso tem nome: são as propriedades **ACID**, um conjunto de garantias que um SGBD relacional oferece para cada operação (ou conjunto de operações) que ele executa.

Para entender por que ACID importa, pense em uma transferência bancária: tirar R$100 da conta de Ana e colocar na conta de Bruno. Isso são, na prática, duas operações: um `UPDATE` que subtrai da Ana e um `UPDATE` que soma no Bruno. O que pode dar errado?

- **Atomicidade (Atomicity)**: as duas operações acontecem como um bloco único — ou as duas são aplicadas, ou nenhuma é. Se o sistema falhar exatamente entre debitar da Ana e creditar no Bruno, a atomicidade garante que a operação inteira é desfeita, e o dinheiro não desaparece no meio do caminho.
- **Consistência (Consistency)**: o banco só transita entre estados válidos. Se existe uma regra de que o saldo não pode ficar negativo, o SGBD não permite que uma operação deixe o banco em um estado que viole essa regra.
- **Isolamento (Isolation)**: operações acontecendo ao mesmo tempo não enxergam resultados parciais umas das outras. Se, no meio da transferência, outra pessoa consultar o saldo da Ana, ela não deve ver um estado intermediário estranho (por exemplo, já debitado da Ana mas ainda não creditado no Bruno).
- **Durabilidade (Durability)**: uma vez que a operação é confirmada, ela sobrevive mesmo que o sistema caia um segundo depois. Não existe "confirmado, mas ainda não gravado de verdade".

Por enquanto, o que importa é entender que ACID é a resposta técnica para o problema 2 e 3 listados em [Por que um arquivo de texto quase basta, mas não basta](#por-que-um-arquivo-de-texto-quase-basta-mas-não-basta) — é isso que um arquivo de texto nunca vai te dar de graça.

## Transações: uma prévia

Uma **transação** é como o SGBD materializa a atomicidade explicada acima: um grupo de comandos SQL que deve ser tratado como uma unidade só, tudo ou nada. A sintaxe completa, com cenários de falha e concorrência, é assunto do [Nível 4](./NIVEL_4_SQL_MODELAGEM.md) — mas vale ver a forma básica agora, para a ideia de ACID sair do papel:

```sql
START TRANSACTION;

UPDATE contas SET saldo = saldo - 100 WHERE id = 1;
UPDATE contas SET saldo = saldo + 100 WHERE id = 2;

COMMIT;
```

- `START TRANSACTION`: marca o início do bloco. Nenhuma das alterações seguintes é definitiva ainda.
- Os dois `UPDATE`: as operações que, juntas, formam a transferência completa.
- `COMMIT`: confirma o bloco inteiro de uma vez. Só a partir daqui as alterações se tornam permanentes e visíveis para outras conexões.

Se algo der errado no meio do caminho — por exemplo, uma regra de negócio detectar que o saldo ficaria negativo — o comando `ROLLBACK` desfaz tudo que foi feito desde o `START TRANSACTION`, como se nada tivesse acontecido:

```sql
ROLLBACK;
```

Sem transação, cada `UPDATE` seria confirmado individualmente e de forma independente — e uma falha entre os dois comandos deixaria o dinheiro descontado de uma conta sem nunca ter sido creditado na outra, exatamente o cenário que a atomicidade existe para impedir.

## Banco relacional

Existem várias famílias de banco de dados, como vimos, mas este roadmap foca no **banco relacional**, que organiza os dados em tabelas conectadas entre si por identificadores, evitando duplicar a mesma informação em vários lugares.

Para entender por que isso importa, veja o que aconteceria se guardássemos tudo em uma tabela só:

```text
pedido_id | cliente_nome | cliente_email      | valor_total
1         | Ana          | ana@email.com      | 250.00
2         | Ana          | ana@email.com      | 89.90
3         | Bruno        | bruno@email.com    | 120.00
```

O nome e o email da Ana aparecem duplicados em cada pedido dela. Isso cria três problemas concretos, cada um com nome próprio na literatura de banco de dados (você vai reencontrar esses nomes ao estudar normalização, no Nível 4):

- **Anomalia de atualização**: se a Ana atualizar o email, é preciso lembrar de atualizar em todas as linhas onde ele aparece. Esquecer uma linha faz o mesmo cliente ter dois emails diferentes registrados no sistema, sem que nada acuse esse erro.
- **Anomalia de inserção**: e se a Ana se cadastrar no site mas ainda não fizer nenhum pedido? Nessa tabela, não existe como registrar a cliente sem inventar um pedido fictício para ela caber em uma linha.
- **Anomalia de remoção**: se o único pedido do Bruno for cancelado e a linha for removida, o cadastro do Bruno como cliente desaparece junto — mesmo que ele continue sendo um cliente da loja.

A solução relacional é separar isso em duas tabelas, uma para clientes e outra para pedidos, conectadas por um identificador:

```text
clientes
id | nome | email
1  | Ana  | ana@email.com
2  | Bruno| bruno@email.com

pedidos
id | cliente_id | valor_total
1  | 1          | 250.00
2  | 1          | 89.90
3  | 2          | 120.00
```

Agora o nome e o email da Ana existem em um único lugar. Cada pedido apenas aponta para o cliente dono dele através do `cliente_id`. Atualizar o email da Ana altera uma única linha. A Ana pode existir na tabela `clientes` com zero pedidos. E remover um pedido do Bruno não apaga o cadastro dele. As três anomalias desaparecem.

> **Pare e pense:** se a tabela `pedidos` guardasse também uma coluna `cliente_endereco`, copiada em cada pedido, que anomalia (de atualização, inserção ou remoção) isso criaria se o cliente se mudasse de casa?
>
> <details><summary>Ver resposta</summary>
>
> Anomalia de atualização: o endereço do cliente estaria duplicado em todos os pedidos antigos dele. Ao atualizar o endereço no cadastro do cliente, seria preciso lembrar de atualizar também em cada pedido — e esquecer um deixaria o histórico com endereços divergentes para a mesma pessoa. A solução, de novo, é o pedido referenciar o cliente por `cliente_id` e o endereço atual viver só na tabela `clientes` (ou em uma tabela própria de endereços, tema do Nível 4).
> </details>

## Tabela, coluna e linha

### Tabela

Uma tabela representa um tipo de coisa que o sistema precisa lembrar: um cliente, um produto, um pedido. Cada tabela guarda apenas registros daquele mesmo tipo — misturar tipos diferentes na mesma tabela (por exemplo, clientes e produtos juntos) é sinal de que falta pelo menos uma tabela no modelo.

### Coluna

Uma coluna é uma característica que toda linha da tabela possui, sempre do mesmo tipo de dado. Na tabela `clientes`, toda linha vai ter um `id`, um `nome` e um `email` — não faz sentido um cliente ter email e outro não ter essa coluna, porque a coluna descreve a estrutura da tabela inteira, não de um registro específico. (Um cliente específico *não ter um email* é diferente disso — isso é resolvido com `NULL`, explicado mais adiante, não com "essa coluna não existe para essa linha".)

### Linha

Uma linha é uma ocorrência real daquele tipo de coisa: um cliente específico, com valores preenchidos em cada coluna. Em teoria relacional, uma linha também é chamada de **tupla** e uma coluna de **atributo** — esses termos aparecem bastante em material mais formal, vale reconhecer mesmo usando "linha" e "coluna" no dia a dia.

```text
clientes
id | nome | email
1  | Ana  | ana@email.com
```

Aqui `clientes` é a tabela, `id`/`nome`/`email` são as colunas, e a linha `1 | Ana | ana@email.com` é um registro específico daquela tabela.

## Entidade x tabela

Antes de existir a tabela `clientes` no MySQL, existe o **conceito** de cliente no seu domínio de problema — algo que existe independente de qualquer banco de dados. Esse conceito é chamado de **entidade** na fase de modelagem.

A diferença importa porque modelar um sistema bem começa antes de abrir o MySQL Workbench: primeiro você identifica quais entidades o seu domínio tem (cliente, produto, pedido, categoria), o que caracteriza cada uma (quais atributos ela precisa) e como elas se relacionam — só depois disso você traduz cada entidade em uma tabela física, com tipos de dado e chaves definidos.

Pular essa etapa e sair criando tabelas direto no banco, sem pensar antes no papel, costuma gerar modelos confusos, porque você fica resolvendo dois problemas ao mesmo tempo (o que o sistema precisa guardar + como o MySQL guarda isso) em vez de um de cada vez.

## Modelagem conceitual, lógica e física

A tradução de "conceito do mundo real" para "tabela no MySQL" (seção anterior) normalmente passa por três etapas, mesmo que informalmente:

1. **Modelagem conceitual**: identifica as entidades do domínio (Cliente, Produto, Pedido) e como elas se relacionam entre si, sem se preocupar ainda com tipos de dado, chaves ou qual SGBD será usado. O resultado costuma ser desenhado como um **DER** — Diagrama Entidade-Relacionamento.
2. **Modelagem lógica**: refina o DER definindo atributos de cada entidade, quais são obrigatórios, quais identificam o registro (futuras chaves primárias) e como os relacionamentos vão se traduzir em chaves estrangeiras — ainda de forma independente de SGBD específico.
3. **Modelagem física**: traduz o modelo lógico para SQL de um SGBD específico — os `CREATE TABLE` que você escreveu nas seções anteriores, com os tipos exatos do MySQL (`VARCHAR(100)`, `DECIMAL(10,2)`) e sintaxe específica dele.

Um DER simples, em notação textual, para o que já vimos:

```text
[Cliente] 1 ---- N [Pedido]
```

Isso se lê: "um Cliente pode ter vários Pedidos, mas cada Pedido pertence a exatamente um Cliente." A notação gráfica completa (losangos, cardinalidade nas duas pontas) e os tipos de relacionamento (1:1, 1:N, N:N) são o foco do [Nível 4](./NIVEL_4_SQL_MODELAGEM.md) — aqui, o importante é internalizar que **pensar no modelo antes de abrir o Workbench economiza retrabalho**. Corrigir uma entidade esquecida no papel custa um rabisco a mais. Corrigir uma tabela esquecida depois que o sistema já está em produção, com dados reais, é uma migração de banco de dados — bem mais caro.

## Convenções de nomenclatura

Não existe uma regra universal obrigatória, mas existe um padrão amplamente adotado no mercado, e seguir um padrão consistente facilita a leitura de qualquer pessoa (inclusive você, seis meses depois) que abrir o banco:

- Nomes de tabela no **plural** e em **snake_case**: `clientes`, `itens_pedido`, não `Cliente` nem `itemPedido`.
- Nomes de coluna também em **snake_case**: `data_nascimento`, não `dataNascimento` nem `DataNascimento`.
- Chave primária, por convenção, chamada de `id`.
- Chave estrangeira nomeada como `<tabela_no_singular>_id`: `cliente_id` dentro da tabela `pedidos`, apontando para `clientes.id`.
- Evite nomes de coluna que sejam palavras reservadas do SQL, como `order` ou `group` — MySQL até aceita usando aspas invertidas (`` `order` ``), mas é mais simples só evitar.
- Evite acentos e espaços em nomes de tabela e coluna. `preco`, não `preço`; `data_criacao`, não `data criacao`.

Esse é o padrão usado no restante deste roadmap, incluindo os exemplos de Spring Boot no Nível 7, onde o Hibernate/JPA converte automaticamente nomes de classe Java em `PascalCase` (`ItemPedido`) para nomes de tabela em `snake_case` (`item_pedido`) seguindo essa mesma convenção.

## Tipos de dado

Cada coluna precisa declarar que tipo de dado ela guarda. Isso não é burocracia: o tipo define quanto espaço em disco a coluna ocupa, que operações fazem sentido nela (dá para somar um `VARCHAR`?) e que valores o SGBD aceita ou rejeita automaticamente, protegendo a integridade dos dados sem depender de validação manual no código Java.

### Números inteiros

| Tipo | Faixa aproximada | Uso típico |
|---|---|---|
| `TINYINT` | -128 a 127 (ou 0 a 255 sem sinal) | Flags pequenas, idade |
| `INT` | ±2,1 bilhões | Chave primária, quantidade em estoque |
| `BIGINT` | ±9,2 quintilhões | Volumes muito grandes, IDs de sistemas distribuídos |

Para a maioria dos casos deste roadmap, `INT` é suficiente.

### Números decimais: o erro mais comum de iniciante

```sql
preco FLOAT
```

Parece razoável, mas é um erro clássico para valores monetários. `FLOAT` e `DOUBLE` guardam números de ponto flutuante em binário, e certas frações decimais (como 0.10) não têm representação exata em binário — o mesmo motivo pelo qual, em Java, `0.1 + 0.2` não resulta exatamente em `0.3`. Em um sistema financeiro, esse tipo de erro de arredondamento pode acumular e gerar diferenças de centavos entre o que foi cobrado e o que foi registrado.

Para dinheiro, o tipo correto é `DECIMAL(precisão, escala)`, que guarda o número de forma exata, como decimal mesmo, sem conversão para binário:

```sql
preco DECIMAL(10,2)
```

Isso significa até 10 dígitos no total, sendo 2 depois da vírgula — suficiente para valores até 99.999.999,99. Use `FLOAT`/`DOUBLE` apenas para valores onde uma pequena imprecisão é aceitável (por exemplo, coordenadas geográficas), nunca para dinheiro.

### Texto

| Tipo | Comportamento | Uso típico |
|---|---|---|
| `CHAR(n)` | Tamanho fixo, sempre ocupa `n` caracteres, completando com espaço | Códigos de tamanho fixo, como sigla de estado (`UF CHAR(2)`) |
| `VARCHAR(n)` | Tamanho variável, até `n` caracteres | Nome, email, título — a maioria dos textos curtos |
| `TEXT` | Tamanho variável, sem limite prático definido na coluna | Descrição longa, comentário |

Na dúvida entre `CHAR` e `VARCHAR`, use `VARCHAR`. `CHAR` só compensa quando o tamanho é sempre exatamente o mesmo.

### Datas e horários

| Tipo | Guarda | Uso típico |
|---|---|---|
| `DATE` | Só data (`2026-08-09`) | Data de nascimento |
| `DATETIME` | Data e hora, sem fuso horário embutido | Data de um pedido, dentro de uma única aplicação/fuso |
| `TIMESTAMP` | Data e hora, com conversão automática de fuso horário pelo servidor | Registro de criação/atualização (`created_at`) em sistemas que podem rodar em fusos diferentes |

### Booleano

MySQL não tem um tipo booleano nativo separado — `BOOLEAN` é, internamente, um apelido para `TINYINT(1)`, onde `0` é falso e qualquer valor diferente de zero é tratado como verdadeiro. Na prática, você escreve `ativo BOOLEAN` e usa `true`/`false` normalmente; só vale saber que por baixo é um número.

### ENUM

```sql
status ENUM('PENDENTE', 'PAGO', 'CANCELADO')
```

Restringe a coluna a um conjunto fixo de valores de texto, definidos na criação da tabela. Útil para status e categorias fechadas, mas tem uma desvantagem real: adicionar um novo valor exige alterar a estrutura da tabela (`ALTER TABLE`). Em muitos sistemas profissionais, prefere-se uma tabela separada de status (assunto de relacionamento 1:N, no Nível 4) exatamente para evitar essa rigidez.

### Outros tipos que você vai encontrar

- **`JSON`**: guarda um documento JSON dentro de uma coluna. Útil para dados semiestruturados que variam de registro para registro (por exemplo, atributos específicos de cada categoria de produto), mas usar demais é sinal de que talvez faltem tabelas no modelo relacional — vale usar com moderação, não como atalho para não modelar.
- **`BLOB`**: guarda dados binários (uma imagem, um arquivo) diretamente no banco. Na prática, a maioria dos sistemas prefere guardar o arquivo em um serviço de armazenamento próprio (disco, S3, etc.) e salvar só a URL ou caminho no banco, porque `BLOB` deixa o banco pesado e mais lento de fazer backup.

### Resumo de decisão rápida

| Preciso guardar... | Use |
|---|---|
| Um identificador numérico | `INT` |
| Um valor monetário | `DECIMAL(10,2)`, nunca `FLOAT` |
| Um texto curto (nome, email) | `VARCHAR(n)` |
| Um texto longo (descrição) | `TEXT` |
| Uma data sem horário | `DATE` |
| Um instante no tempo | `DATETIME` ou `TIMESTAMP` |
| Verdadeiro/falso | `BOOLEAN` |
| Um valor de uma lista fixa e pequena | `ENUM`, ou uma tabela separada se a lista crescer |

## O valor NULL

`NULL` não é o número zero, não é uma string vazia `""`, não é `false`. `NULL` representa **ausência de valor** — a informação simplesmente não existe ou não foi preenchida para aquela linha.

```text
clientes
id | nome | telefone
1  | Ana  | 11999998888
2  | Bruno| NULL
```

O Bruno não tem `NULL` como telefone — ele não tem telefone registrado. Essa diferença parece sutil, mas tem consequências práticas importantes:

**Comparações com `NULL` nunca são verdadeiras nem falsas — são desconhecidas.**

```sql
SELECT * FROM clientes WHERE telefone = NULL;
```

Essa consulta **não** retorna as linhas com telefone nulo, mesmo que pareça que deveria. Em SQL, `NULL = NULL` não resulta em `true`, resulta em "desconhecido" (esse é o comportamento de uma lógica de três valores: verdadeiro, falso, desconhecido). A forma correta é:

```sql
SELECT * FROM clientes WHERE telefone IS NULL;
SELECT * FROM clientes WHERE telefone IS NOT NULL;
```

**`NULL` também afeta cálculos.** Qualquer operação aritmética envolvendo `NULL` resulta em `NULL`: `10 + NULL` é `NULL`, não `10`. Isso importa em somas e médias — uma coluna com valores nulos pode fazer um `SUM` ou `AVG` se comportar de forma diferente do esperado se você não tratar isso explicitamente (o MySQL tem funções como `COALESCE` e `IFNULL` para dar um valor padrão no lugar de `NULL`, mas isso fica para quando você precisar na prática).

A pergunta que você deve se fazer ao desenhar cada coluna é: **"faz sentido esse campo não ter valor para algumas linhas?"** Se sim, a coluna pode aceitar `NULL`. Se não — todo cliente precisa ter um nome, por exemplo — a coluna deve ser declarada como `NOT NULL`, tema da próxima seção.

> **Pare e pense:** por que `SELECT * FROM clientes WHERE telefone <> '11999998888';` pode não trazer de volta o Bruno, mesmo ele claramente tendo um telefone diferente daquele número?
>
> <details><summary>Ver resposta</summary>
>
> Se o telefone do Bruno for `NULL`, a comparação `NULL <> '11999998888'` não resulta em verdadeiro — resulta em desconhecido, como qualquer comparação envolvendo `NULL`. Por isso a linha do Bruno não entra no resultado, mesmo intuitivamente parecendo que deveria. Para incluir também quem não tem telefone cadastrado, seria preciso um filtro explícito: `WHERE telefone <> '11999998888' OR telefone IS NULL`.
> </details>

## Constraints (restrições)

Constraint é uma regra que o SGBD aplica automaticamente sobre uma coluna ou tabela, rejeitando qualquer operação que a viole. É a forma de codificar, na estrutura do banco, regras que de outra forma dependeriam inteiramente do código da aplicação lembrar de validar — e código da aplicação esquece, tem bugs, ou é contornado por alguém rodando SQL direto no banco.

```sql
CREATE TABLE clientes (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100) NOT NULL,
    email VARCHAR(150) NOT NULL UNIQUE,
    idade INT DEFAULT 0,
    CHECK (idade >= 0)
);
```

- **`NOT NULL`**: obriga a coluna a ter sempre um valor. Sem essa constraint, `nome` poderia ficar `NULL`, e um cliente "sem nome" provavelmente é um bug em algum lugar do sistema, não um caso legítimo.
- **`UNIQUE`**: garante que nenhum valor se repete entre as linhas daquela coluna. Aqui, `email UNIQUE` impede dois clientes cadastrados com o mesmo email — uma regra de negócio real, aplicada no nível do banco, não só validada em Java.
- **`DEFAULT`**: define um valor automático quando a coluna não é informada no `INSERT`. Se você inserir um cliente sem passar `idade`, o banco grava `0` sozinho.
- **`CHECK`**: valida uma condição arbitrária sobre o valor. Aqui, impede uma idade negativa de ser gravada, não importa de onde veio o `INSERT`.

A ideia central por trás de constraints: **regras importantes de integridade devem estar no banco, não só na aplicação.** Se amanhã existir uma segunda aplicação, um script de importação em massa, ou alguém rodando SQL manualmente no Workbench, as constraints continuam protegendo os dados, porque elas vivem no schema, não em um `if` espalhado pelo código Java.

## Chave primária

Nomes se repetem. Podem existir duas clientes chamadas Ana no mesmo sistema. Se o sistema tentasse identificar clientes pelo nome, um pedido da "Ana errada" poderia ser associado à pessoa errada.

A **chave primária** é a coluna (ou conjunto de colunas) que identifica uma linha de forma única e estável dentro da tabela. Estável significa que ela não muda mesmo que outros dados do registro mudem — o email pode ser atualizado, o nome pode ser corrigido, mas o identificador continua o mesmo. Duas garantias vêm embutidas em toda chave primária, automaticamente: ela nunca aceita `NULL` e nunca aceita valores repetidos.

### Chave natural x chave substituta (surrogate key)

Existem duas estratégias para escolher uma chave primária:

- **Chave natural**: usar um dado que já existe no mundo real e que naturalmente identifica o registro, como CPF para uma pessoa.
- **Chave substituta (surrogate key)**: criar um identificador artificial, sem nenhum significado no mundo real, só para servir de identificador — normalmente um número que o próprio banco gera.

Chaves naturais parecem mais simples à primeira vista, mas trazem riscos: e se o dado "único" na verdade não for tão único assim (CNPJs de filiais, códigos reaproveitados por sistemas legados)? E se ele puder ser corrigido depois (um CPF digitado errado no cadastro)? Alterar uma chave primária é problemático justamente porque outras tabelas podem estar apontando para ela através de chave estrangeira.

Por isso, a prática mais comum — e a adotada neste roadmap — é usar uma **chave substituta**: uma coluna `id` numérica, sem nenhum significado de negócio, gerada automaticamente pelo banco:

```sql
CREATE TABLE clientes (
    id INT PRIMARY KEY AUTO_INCREMENT,
    cpf VARCHAR(11) NOT NULL UNIQUE,
    nome VARCHAR(100) NOT NULL
);
```

Aqui, `id` é a chave primária (estável, sem significado de negócio, nunca muda), e `cpf` continua guardado e protegido por uma constraint `UNIQUE` — a unicidade do CPF como regra de negócio é preservada, só não é ela quem identifica a linha internamente no banco.

`AUTO_INCREMENT` diz ao MySQL para gerar esse número sozinho, incrementando a cada novo `INSERT`, então você normalmente nunca informa o `id` manualmente.

### Chave primária composta

Às vezes, a identidade única de um registro só faz sentido como a combinação de duas colunas, não uma sozinha. Isso aparece com mais força em tabelas de relacionamento N:N, assunto do Nível 4, mas a sintaxe básica é:

```sql
CREATE TABLE matriculas (
    aluno_id INT NOT NULL,
    curso_id INT NOT NULL,
    PRIMARY KEY (aluno_id, curso_id)
);
```

Aqui, nenhuma das duas colunas sozinha identifica a linha, mas a combinação das duas sim — o mesmo aluno pode aparecer em várias matrículas (com cursos diferentes), e o mesmo curso pode aparecer em várias matrículas (com alunos diferentes), mas o par `(aluno_id, curso_id)` não se repete.

## Chave estrangeira

Uma **chave estrangeira** (foreign key) é uma coluna que aponta para a chave primária de outra tabela, criando uma conexão entre os dois registros e, mais importante, uma regra que o SGBD passa a impor automaticamente: **integridade referencial**.

No exemplo já visto, `pedidos.cliente_id` é uma chave estrangeira: ela aponta para `clientes.id`, dizendo "este pedido pertence a este cliente".

```sql
CREATE TABLE pedidos (
    id INT PRIMARY KEY AUTO_INCREMENT,
    cliente_id INT NOT NULL,
    valor_total DECIMAL(10,2) NOT NULL,
    FOREIGN KEY (cliente_id) REFERENCES clientes(id)
);
```

Com essa constraint declarada, o MySQL passa a rejeitar automaticamente:

- um `INSERT` em `pedidos` com um `cliente_id` que não existe em `clientes`;
- um `DELETE` em `clientes` que deixaria pedidos "órfãos", apontando para um cliente que não existe mais — a menos que você diga explicitamente o que fazer nesse caso, como na próxima seção.

### O que fazer quando o "pai" é removido

Por padrão, o MySQL bloqueia a remoção de um cliente que ainda tem pedidos vinculados. Mas é possível configurar um comportamento diferente:

```sql
FOREIGN KEY (cliente_id) REFERENCES clientes(id) ON DELETE CASCADE
```

- **`RESTRICT`** (padrão): impede a remoção do cliente enquanto existir qualquer pedido vinculado a ele.
- **`CASCADE`**: remove automaticamente todos os pedidos vinculados quando o cliente é removido.
- **`SET NULL`**: mantém os pedidos, mas zera o `cliente_id` deles para `NULL` (exige que a coluna aceite `NULL`, ou seja, não pode ser `NOT NULL` nesse caso).

A escolha depende da regra de negócio real. Para pedidos de um e-commerce, `RESTRICT` costuma ser o mais seguro — você provavelmente não quer que apagar um cliente apague o histórico de vendas da empresa por acidente. `CASCADE` é mais comum em relações onde o "filho" não faz sentido sem o "pai" (por exemplo, os itens de um pedido não fazem sentido sem o pedido).

Isso é só a base de chave estrangeira. No [Nível 4](./NIVEL_4_SQL_MODELAGEM.md) você vai aprofundar em tipos de relacionamento (1:1, 1:N, N:N) e em como consultar dados de várias tabelas ao mesmo tempo com `JOIN`.

> **Pare e pense:** um `DELETE FROM clientes WHERE id = 5;` falha com erro de chave estrangeira, porque o cliente 5 tem pedidos vinculados. Quais são as duas formas de resolver isso, e qual delas apaga dados de pedidos junto?
>
> <details><summary>Ver resposta</summary>
>
> A primeira forma é remover (ou reatribuir) manualmente os pedidos vinculados antes de remover o cliente — nenhum dado é apagado sem você decidir explicitamente. A segunda é ter configurado a chave estrangeira com `ON DELETE CASCADE`: nesse caso, remover o cliente remove automaticamente todos os pedidos vinculados a ele, sem confirmação adicional — por isso `CASCADE` deve ser usado com cautela, só quando o "filho" realmente não faz sentido sem o "pai".
> </details>

## Índices

Voltando à seção [O caminho de uma consulta dentro do SGBD](#o-caminho-de-uma-consulta-dentro-do-sgbd): sem nenhuma ajuda extra, para encontrar um cliente pelo email o MySQL precisaria ler a tabela `clientes` inteira, linha por linha, comparando o email de cada uma — o que se chama de **table scan**. Com 50 linhas isso é instantâneo. Com 50 milhões, não.

Um **índice** é uma estrutura auxiliar que o SGBD mantém para acelerar buscas, funcionando de forma parecida com o índice remissivo no final de um livro técnico: em vez de ler o livro inteiro procurando por um termo, você consulta o índice, que já está ordenado, e vai direto à página certa.

```sql
CREATE INDEX idx_clientes_email ON clientes(email);
```

Uma chave primária já ganha um índice automaticamente, criado pelo próprio MySQL — é por isso que buscar por `id` é sempre rápido, mesmo sem você fazer nada extra. Colunas com `UNIQUE` também ganham índice automático, pela mesma razão: o SGBD precisa de uma forma rápida de checar se um valor já existe antes de aceitar um `INSERT`.

Índices têm um custo, que é importante entender para não sair criando índice em tudo: toda vez que uma linha é inserida, atualizada ou removida, o SGBD também precisa atualizar cada índice daquela tabela. Ou seja, índices aceleram leitura (`SELECT`) e desaceleram escrita (`INSERT`/`UPDATE`/`DELETE`). A prática comum é criar índices em colunas frequentemente usadas em filtros (`WHERE`) ou em `JOIN` (Nível 4), e não em colunas que raramente são consultadas.

### Índice composto

Assim como uma chave primária pode ser composta por mais de uma coluna, um índice também pode:

```sql
CREATE INDEX idx_pedidos_cliente_data ON pedidos(cliente_id, data_pedido);
```

Isso acelera consultas que filtram pelas duas colunas juntas (ou só pela primeira, `cliente_id`), mas não ajuda uma consulta que filtra só pela segunda (`data_pedido`) isoladamente — a ordem das colunas no índice importa. Esse é um ajuste fino que vai fazer mais sentido revisitar quando você já estiver escrevendo `JOIN`s no Nível 4.

### Descobrindo se uma consulta está usando um índice

```sql
EXPLAIN SELECT * FROM clientes WHERE email = 'ana@email.com';
```

`EXPLAIN` antes de qualquer `SELECT` mostra o plano que o MySQL escolheu para executar aquela consulta, incluindo se um índice foi usado ou se a tabela inteira precisou ser varrida (`type: ALL`, o table scan mencionado no início desta seção). Não é necessário dominar a leitura completa da saída agora — só vale saber que essa ferramenta existe, para quando "por que essa consulta está lenta?" virar uma pergunta real no seu dia a dia.

Por enquanto, o objetivo é só reconhecer o conceito. Decisões mais finas de quando indexar o quê fazem mais sentido depois que você já estiver escrevendo consultas mais complexas, no Nível 4.

## Instalando o MySQL

### Windows

1. Acesse <https://www.mysql.com/downloads/>.
2. Baixe o **MySQL Community Server** ou o **MySQL Installer**.
3. Durante a instalação, escolha uma opção que inclua:
   - MySQL Server;
   - MySQL Workbench;
   - MySQL Shell, se disponível.
4. Defina uma senha para o usuário `root`. Guarde essa senha — ela será pedida toda vez que você conectar.
5. Abra o MySQL Workbench.
6. Crie uma conexão local (normalmente `127.0.0.1`, porta `3306`, que já vêm preenchidos por padrão).
7. Teste com:

```sql
SELECT 1;
```

Se o resultado voltar sem erro, o servidor está rodando e a conexão está funcionando.

### macOS

1. Instale via [Homebrew](https://brew.sh/): `brew install mysql`.
2. Inicie o serviço: `brew services start mysql`.
3. Defina a senha do usuário `root`: `mysql_secure_installation`.
4. Instale o MySQL Workbench separadamente, baixando em <https://www.mysql.com/downloads/> (ou `brew install --cask mysqlworkbench`).

### Linux (Debian/Ubuntu)

1. Atualize os pacotes: `sudo apt update`.
2. Instale o servidor: `sudo apt install mysql-server`.
3. Rode a configuração inicial de segurança: `sudo mysql_secure_installation`.
4. Instale o Workbench, se preferir interface gráfica: `sudo apt install mysql-workbench`, ou use o cliente de terminal (`mysql -u root -p`) diretamente.

Independente do sistema operacional, o conceito é o mesmo: existe um **servidor MySQL** rodando em segundo plano (a próxima seção explica exatamente o papel dele), e um **cliente** — gráfico (Workbench) ou de terminal — usado só para enviar comandos até ele.

### Alternativas ao Workbench

O MySQL Workbench não é a única forma de conversar com o servidor. Vale saber que existem outras, porque é comum encontrá-las em times diferentes:

- **Cliente de terminal** (`mysql -u root -p`): vem junto da instalação, sem interface gráfica, útil para scripts e para quando não há acesso a uma interface visual (por exemplo, conectado a um servidor remoto por SSH).
- **DBeaver**: cliente gráfico gratuito e multi-SGBD — o mesmo programa conecta em MySQL, PostgreSQL, SQL Server, entre outros, útil se você acabar trabalhando com mais de um banco diferente.
- **Extensões de IDE**: VS Code e IntelliJ têm plugins que permitem rodar SQL e navegar tabelas sem sair do editor onde você já escreve o código Java.

Nenhuma dessas alternativas muda o que você aprendeu até aqui — todas conversam com o mesmo servidor MySQL, usando o mesmo SQL. A escolha da ferramenta é só sobre conforto de uso, não sobre conceito.

### O que é o Workbench, exatamente

O **MySQL Server** é o processo que efetivamente guarda e gerencia os dados — o SGBD em si, rodando em segundo plano. O **MySQL Workbench** é apenas um cliente gráfico que se conecta a esse servidor para você digitar comandos SQL e ver resultados de forma visual. É a mesma relação que existe entre um servidor web e um navegador: o servidor guarda e processa, o cliente só exibe e envia comandos. Sua aplicação Java, mais adiante no roadmap, vai se conectar ao mesmo MySQL Server através de um driver, sem precisar do Workbench no meio.

## Criando seu primeiro banco e tabela

```sql
CREATE DATABASE estudos_java;
USE estudos_java;

CREATE TABLE clientes (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100) NOT NULL,
    email VARCHAR(150) NOT NULL UNIQUE,
    data_nascimento DATE,
    ativo BOOLEAN NOT NULL DEFAULT TRUE
);
```

- `CREATE DATABASE`: cria um novo banco, um espaço isolado para guardar suas tabelas. Um mesmo servidor MySQL pode hospedar vários bancos ao mesmo tempo, completamente isolados uns dos outros.
- `USE`: define qual banco os próximos comandos vão afetar, dentro da sessão atual do Workbench.
- `CREATE TABLE`: cria a estrutura da tabela, definindo colunas, tipos e constraints.

Repare que cada decisão de coluna, nesse exemplo, reflete algo que este nível já explicou: `id` como chave substituta com `AUTO_INCREMENT`; `nome` e `email` como `NOT NULL` porque todo cliente precisa dos dois; `email` como `UNIQUE` porque é uma regra de negócio real; `data_nascimento` sem `NOT NULL` porque é aceitável o cliente não informar; `ativo` com `DEFAULT TRUE` porque todo cliente novo começa ativo, salvo indicação contrária.

## Charset e collation: por que acentos quebram

Um problema clássico para quem estuda banco de dados em português: cadastrar `"José"` e, ao consultar depois, ver `"JosÃ©"` ou um símbolo estranho no lugar do "é". Isso não é um bug misterioso — é um problema de **charset** (conjunto de caracteres) mal configurado.

Internamente, todo texto é guardado como números (cada caractere tem um código). O charset define qual tabela de códigos está sendo usada. Se o banco grava o texto usando um charset e alguma etapa no meio do caminho (conexão, aplicação, terminal) lê usando outro charset diferente, cada caractere acentuado é interpretado errado — esse fenômeno tem até nome, **mojibake**.

A prática recomendada é padronizar tudo em **UTF-8**, mais especificamente `utf8mb4` no MySQL (o `utf8` "puro" do MySQL é, por razões históricas, uma variação incompleta que não suporta todos os caracteres Unicode, incluindo emojis). Isso pode ser definido na criação do banco:

```sql
CREATE DATABASE estudos_java
    CHARACTER SET utf8mb4
    COLLATE utf8mb4_general_ci;
```

- **Charset**: qual conjunto de caracteres é aceito e como cada um é codificado em bytes.
- **Collation**: as regras de comparação e ordenação de texto sobre esse charset — por exemplo, se `'ana'` e `'Ana'` são tratadas como iguais em uma busca (`_ci` significa *case insensitive*, ou seja, não diferencia maiúsculas de minúsculas).

Se seu MySQL Workbench já mostra acentos corretamente na tela e você não está integrando com sistemas externos ainda, não precisa se aprofundar mais que isso agora — só é importante saber que esse ajuste existe e por que ele existe, para não perder horas depurando um "bug" que na verdade é configuração de charset quando ele aparecer.

## Inserindo dados

```sql
INSERT INTO clientes (nome, email, data_nascimento)
VALUES ('Ana', 'ana@email.com', '2005-03-14');
```

Repare que `id` não foi informado — o `AUTO_INCREMENT` cuida disso — e `ativo` também não foi informado, então assume o `DEFAULT TRUE` declarado na tabela.

Também é possível inserir várias linhas em um único comando:

```sql
INSERT INTO clientes (nome, email, data_nascimento) VALUES
    ('Bruno', 'bruno@email.com', '1998-07-22'),
    ('Carla', 'carla@email.com', '2001-11-02');
```

### Cuidados comuns

- Textos e datas usam aspas simples: `'Ana'`, `'2005-03-14'`. Números não usam aspas: `18`, `250.00`.
- O formato de data padrão do MySQL é `AAAA-MM-DD`, não `DD/MM/AAAA`.
- Se uma coluna `NOT NULL` não for informada e não tiver `DEFAULT`, o `INSERT` falha — de propósito, porque essa é exatamente a constraint fazendo seu trabalho.

## Consultando dados

```sql
SELECT id, nome, email
FROM clientes;
```

Consulta as linhas existentes. `SELECT *` traz todas as colunas; listar as colunas explicitamente deixa claro o que você espera ler, e evita trazer dados desnecessários (ou sensíveis) sem querer.

### Filtrando com WHERE

```sql
SELECT * FROM clientes WHERE ativo = TRUE;
SELECT * FROM clientes WHERE data_nascimento < '2005-01-01';
SELECT * FROM clientes WHERE nome <> 'Ana';
```

| Operador | Significado |
|---|---|
| `=` | igual |
| `<>` ou `!=` | diferente |
| `>`, `<`, `>=`, `<=` | comparação numérica ou de data |
| `BETWEEN a AND b` | dentro de um intervalo, incluindo as pontas |
| `IN (a, b, c)` | igual a qualquer um dos valores da lista |
| `LIKE 'padrão'` | comparação de texto com padrão (`%` para qualquer sequência, `_` para um caractere) |
| `IS NULL` / `IS NOT NULL` | ausência ou presença de valor (nunca use `=` com `NULL`, como visto na seção sobre NULL) |

```sql
SELECT * FROM clientes WHERE nome LIKE 'A%';
-- nomes que começam com "A"

SELECT * FROM clientes WHERE id IN (1, 3, 5);
-- clientes com esses ids específicos

SELECT * FROM clientes WHERE data_nascimento BETWEEN '2000-01-01' AND '2005-12-31';
```

### Ordenando e limitando resultados

```sql
SELECT * FROM clientes ORDER BY nome ASC;
SELECT * FROM clientes ORDER BY data_nascimento DESC LIMIT 5;
```

- `ORDER BY`: define a ordem do resultado. `ASC` (crescente) é o padrão; `DESC` inverte.
- `LIMIT`: restringe quantas linhas voltam — útil para não trazer uma tabela inteira quando só os primeiros resultados interessam.

### Funções de agregação básicas

Além de trazer linhas, o SQL consegue calcular um resumo sobre elas diretamente no banco, sem precisar trazer tudo para o Java e somar manualmente:

```sql
SELECT COUNT(*) FROM clientes;
-- quantos clientes existem no total

SELECT COUNT(*) FROM clientes WHERE ativo = TRUE;
-- quantos clientes ativos existem

SELECT MIN(data_nascimento), MAX(data_nascimento) FROM clientes;
-- a data de nascimento mais antiga e a mais recente

SELECT AVG(valor_total) FROM pedidos;
-- o valor médio dos pedidos

SELECT SUM(valor_total) FROM pedidos;
-- o valor total somado de todos os pedidos
```

| Função | O que calcula |
|---|---|
| `COUNT(*)` | Quantidade de linhas |
| `MIN(coluna)` | Menor valor da coluna |
| `MAX(coluna)` | Maior valor da coluna |
| `AVG(coluna)` | Média dos valores da coluna |
| `SUM(coluna)` | Soma dos valores da coluna |

Essas funções, combinadas com `GROUP BY` para calcular resultados por grupo (por exemplo, "total vendido por cliente"), são um dos assuntos centrais do [Nível 4](./NIVEL_4_SQL_MODELAGEM.md). Aqui, o objetivo é só conhecer as funções em sua forma mais simples, aplicadas à tabela inteira ou a um filtro de `WHERE`.

Vale lembrar da seção sobre [NULL](#o-valor-null): `AVG` e `SUM` ignoram automaticamente linhas com `NULL` na coluna agregada, o que pode fazer uma média "parecer certa" mesmo havendo dados faltando — outro motivo para pensar com cuidado em quais colunas devem ou não aceitar `NULL`.

## Atualizando dados

```sql
UPDATE clientes
SET email = 'ana.silva@email.com'
WHERE id = 1;
```

O `WHERE` é o que garante que você altera só o registro certo. Isso merece destaque em letras maiúsculas mentais: **um `UPDATE` sem `WHERE` atualiza a tabela inteira, todas as linhas, de uma vez.**

```sql
-- PERIGO: isso atualiza o email de TODOS os clientes para o mesmo valor
UPDATE clientes SET email = 'ana.silva@email.com';
```

Uma prática recomendada, especialmente enquanto você ainda está aprendendo: antes de rodar um `UPDATE` ou `DELETE`, rode um `SELECT` com o mesmo `WHERE` primeiro, para conferir exatamente quais linhas vão ser afetadas.

```sql
-- Primeiro confirme quais linhas serão afetadas:
SELECT * FROM clientes WHERE id = 1;

-- Só depois rode o UPDATE com o mesmo WHERE:
UPDATE clientes SET email = 'ana.silva@email.com' WHERE id = 1;
```

## Removendo dados

```sql
DELETE FROM clientes
WHERE id = 1;
```

Assim como no `UPDATE`, o `WHERE` evita apagar a tabela inteira por engano. `DELETE FROM clientes;`, sem `WHERE`, apaga todas as linhas da tabela, uma por uma.

Existe também o comando `TRUNCATE TABLE clientes;`, que remove todas as linhas de uma vez, de forma mais rápida que um `DELETE` sem filtro, mas sem possibilidade de filtro nenhum — é tudo ou nada. Por ser irreversível e sem `WHERE`, use com cautela redobrada, e praticamente nunca em uma tabela com dados reais.

## Erros comuns e como interpretá-los

Alguns erros que você provavelmente vai encontrar no MySQL Workbench, e o que eles normalmente significam:

**`Error Code: 1048. Column 'nome' cannot be null`**
Você tentou inserir ou atualizar uma linha sem valor em uma coluna `NOT NULL`. Confira se esqueceu de informar essa coluna no `INSERT`.

**`Error Code: 1062. Duplicate entry '...' for key 'email'`**
Você tentou inserir um valor que já existe em uma coluna `UNIQUE` (ou na chave primária). Confira se o registro já não existe antes de inserir de novo.

**`Error Code: 1452. Cannot add or update a child row: a foreign key constraint fails`**
Você tentou inserir (ou atualizar) uma linha cuja chave estrangeira aponta para um valor que não existe na tabela referenciada — por exemplo, um pedido com `cliente_id = 99`, mas não existe cliente com `id = 99`.

**`Error Code: 1451. Cannot delete or update a parent row: a foreign key constraint fails`**
O inverso do erro anterior: você tentou remover (ou alterar a chave primária de) um registro que ainda tem outras linhas apontando para ele por chave estrangeira, e a constraint está configurada como `RESTRICT` (o padrão).

**`Error Code: 1146. Table '...' doesn't exist`**
Normalmente é erro de digitação no nome da tabela, ou você esqueceu de rodar `USE nome_do_banco;` antes, e está tentando acessar uma tabela de outro banco.

**`Error Code: 1064. You have an error in your SQL syntax`**
Erro de sintaxe genérico — vírgula sobrando ou faltando, parêntese não fechado, palavra-chave escrita errada. O MySQL costuma indicar a posição aproximada do erro na mensagem; comece a procurar exatamente ali.

**`Error Code: 1366. Incorrect string value`**
Você tentou gravar um caractere que o charset da coluna não suporta — o problema descrito na seção sobre [charset e collation](#charset-e-collation-por-que-acentos-quebram). Normalmente resolve-se recriando a tabela (ou a coluna) com `utf8mb4`.

**`Error Code: 1264. Out of range value for column`**
Você tentou gravar um número maior do que o tipo da coluna suporta — por exemplo, um valor acima do limite de um `TINYINT` em uma coluna que deveria ser `INT`. Revise a tabela em [Números inteiros](#números-inteiros) para o tipo adequado.

**`Error Code: 1054. Unknown column`**
Você referenciou uma coluna que não existe na tabela, geralmente por erro de digitação ou porque a tabela foi criada sem aquela coluna. Confira a estrutura real da tabela com `DESCRIBE nome_da_tabela;`.

O padrão em todos esses erros: eles não são obstáculos aleatórios, são as constraints e o schema que você mesmo desenhou fazendo exatamente o que deveriam fazer — impedir que um dado inconsistente entre no banco.

## Segurança básica: SQL Injection

Esse é um assunto que costuma ser deixado de fora de material introdutório, mas é crucial mesmo neste nível básico, porque o erro que causa a vulnerabilidade acontece exatamente na forma mais "óbvia" de conectar Java com SQL — a de concatenar texto.

Imagine um sistema de login que monta a consulta assim, colando o que o usuário digitou diretamente no texto do comando SQL:

```java
String sql = "SELECT * FROM clientes WHERE email = '" + emailDigitado + "'";
```

Se `emailDigitado` for realmente um email, funciona. Mas nada impede que alguém digite, no campo de email, o seguinte texto:

```text
' OR '1'='1
```

O comando SQL montado vira:

```sql
SELECT * FROM clientes WHERE email = '' OR '1'='1'
```

`'1'='1'` é sempre verdadeiro, então essa consulta retorna **todos os clientes da tabela**, não porque o SGBD tenha um bug, mas porque o comando SQL foi construído com um texto que o usuário controlava. Isso é **SQL Injection**: injetar SQL malicioso através de um campo de entrada que a aplicação trata como se fosse só um dado, quando na verdade ele é interpretado como parte do comando. Em casos piores, o mesmo princípio permite `DROP TABLE`, exclusão em massa, ou vazamento de dados de outras tabelas.

A defesa, em Java, é nunca concatenar valores de entrada dentro do texto do SQL — usar `PreparedStatement` no lugar de `Statement`, passando os valores como parâmetros separados:

```java
String sql = "SELECT * FROM clientes WHERE email = ?";
PreparedStatement stmt = conexao.prepareStatement(sql);
stmt.setString(1, emailDigitado);
```

Com `PreparedStatement`, o valor digitado é sempre tratado como dado puro, nunca como parte do comando SQL, não importa o que o usuário digite. No [Nível 7](./NIVEL_7_SPRING_BOOT.md), quando você usar Spring Data JPA, essa proteção já vem embutida no framework — mas é importante entender o problema real por trás dela, em vez de só saber "usar JPA é seguro" sem saber contra o quê.

## Conectando o Java ao MySQL: JDBC

Até aqui, todo comando SQL deste nível foi digitado direto no MySQL Workbench. Mas o objetivo final é que sua aplicação Java converse com o banco sozinha. Essa ponte é feita pelo **JDBC** (Java Database Connectivity), uma API padrão do Java para conectar a bancos relacionais.

```java
String url = "jdbc:mysql://localhost:3306/estudos_java";
String usuario = "root";
String senha = "sua_senha";

try (Connection conexao = DriverManager.getConnection(url, usuario, senha)) {
    String sql = "SELECT id, nome, email FROM clientes WHERE ativo = ?";
    PreparedStatement stmt = conexao.prepareStatement(sql);
    stmt.setBoolean(1, true);

    ResultSet resultado = stmt.executeQuery();
    while (resultado.next()) {
        System.out.println(resultado.getString("nome"));
    }
}
```

Peça por peça:

- **`Connection`**: representa a conexão aberta com o servidor MySQL, identificada pela `url` (host, porta e nome do banco), usuário e senha.
- **`PreparedStatement`**: o mesmo mecanismo apresentado na seção sobre SQL Injection — os `?` são preenchidos depois, de forma segura, nunca por concatenação de texto.
- **`ResultSet`**: o conjunto de linhas retornado pela consulta, percorrido linha a linha com `.next()`, de forma parecida com o `Iterator` que você já viu no Nível 2.

Você não vai usar JDBC diretamente na maior parte deste roadmap — a partir do [Nível 7](./NIVEL_7_SPRING_BOOT.md), o Spring Data JPA passa a gerar esse código por trás dos panos. Mas entender essa camada manual primeiro é o que torna o "por trás dos panos" do Spring Boot compreensível, em vez de mágico — o mesmo raciocínio pedagógico já usado com JAX-RS antes do Spring Boot no Nível 6.

## Backup: por que pensar nisso desde já

Tudo que este nível ensinou até aqui protege contra erros de estrutura e de lógica (dado errado, tipo errado, referência quebrada). Nenhuma dessas proteções ajuda se o disco onde o banco está falhar fisicamente, ou se alguém rodar um `DELETE` sem `WHERE` em produção por engano.

**Backup** é uma cópia dos dados, guardada separadamente, que permite restaurar o banco para um estado anterior caso algo dê errado. O MySQL oferece uma ferramenta de linha de comando para isso, `mysqldump`:

```bash
mysqldump -u root -p estudos_java > backup_estudos_java.sql
```

Isso gera um arquivo `.sql` com todos os comandos necessários para recriar o banco do zero, exatamente como estava no momento do backup. Para restaurar:

```bash
mysql -u root -p estudos_java < backup_estudos_java.sql
```

Você não precisa automatizar backups para os exercícios deste roadmap — mas vale rodar um `mysqldump` manual antes de testes que envolvam apagar dados em massa, como hábito. Em um ambiente profissional, backup automatizado e testado (backup que nunca foi restaurado para verificar se funciona não é backup confiável) é considerado parte básica de operar qualquer banco de dados sério.

## Glossário rápido

| Termo | Definição curta |
|---|---|
| Banco de dados | Conjunto organizado de dados persistentes e consultáveis |
| SGBD | Software que gerencia o banco de dados (ex.: MySQL) |
| Schema | Estrutura do banco: tabelas, colunas, tipos, chaves |
| Tabela | Estrutura que guarda registros de um mesmo tipo de entidade |
| Coluna / atributo | Característica presente em toda linha da tabela |
| Linha / tupla | Um registro específico de uma tabela |
| Chave primária | Coluna que identifica uma linha de forma única e estável |
| Chave estrangeira | Coluna que aponta para a chave primária de outra tabela |
| Chave substituta (surrogate) | Identificador artificial, sem significado de negócio, gerado pelo banco |
| Constraint | Regra aplicada automaticamente pelo SGBD sobre uma coluna ou tabela |
| `NULL` | Ausência de valor, diferente de zero ou texto vazio |
| Índice | Estrutura que acelera buscas, com custo em operações de escrita |
| ACID | Atomicidade, Consistência, Isolamento, Durabilidade |
| Integridade referencial | Garantia de que uma chave estrangeira sempre aponta para um registro que existe |
| Transação | Bloco de comandos SQL tratado como uma unidade só, tudo ou nada |
| Charset | Conjunto de caracteres e como cada um é codificado em bytes |
| Collation | Regras de comparação e ordenação de texto sobre um charset |
| JDBC | API do Java para conectar e executar comandos em um banco relacional |
| SQL Injection | Ataque que injeta SQL malicioso através de uma entrada tratada incorretamente como dado |
| Backup | Cópia dos dados guardada separadamente, usada para restaurar o banco em caso de falha |
| DER | Diagrama Entidade-Relacionamento, representação visual do modelo conceitual |
| Table scan | Leitura da tabela inteira, linha por linha, quando não há índice aproveitável |

## Recapitulando

Antes de seguir para o próximo nível, confira se você consegue explicar cada item abaixo em voz alta, sem consultar o texto:

- [ ] Por que memória RAM e arquivos de texto não bastam para um sistema real.
- [ ] A diferença entre banco de dados, SGBD e schema.
- [ ] Por que o modelo relacional separa dados em tabelas em vez de duplicar tudo em uma só.
- [ ] O que são tabela, coluna, linha e entidade, e como cada um se relaciona com os outros.
- [ ] Por que `DECIMAL` é o tipo correto para dinheiro, e `FLOAT` não é.
- [ ] Por que `NULL` não é o mesmo que zero ou texto vazio, e por que `= NULL` nunca funciona.
- [ ] O que uma constraint (`NOT NULL`, `UNIQUE`, `DEFAULT`, `CHECK`) protege, e por que essa proteção vive no banco, não só no código Java.
- [ ] Por que se usa uma chave substituta (`id` com `AUTO_INCREMENT`) em vez de uma chave natural na maioria dos casos.
- [ ] O que uma chave estrangeira impede, e a diferença entre `RESTRICT`, `CASCADE` e `SET NULL`.
- [ ] Por que um índice acelera leitura e desacelera escrita.
- [ ] O que é SQL Injection e por que `PreparedStatement` resolve o problema.
- [ ] As quatro garantias de ACID, com um exemplo prático de cada uma.

Se algum item ainda não estiver claro, vale reler a seção correspondente antes de avançar — o [Nível 4](./NIVEL_4_SQL_MODELAGEM.md) assume que todo esse vocabulário já é natural para você.

## O que vem no próximo nível

Este nível te deu o vocabulário e o raciocínio fundamentais de banco de dados usando uma única tabela. No [Nível 4 - SQL Avançado e Modelagem](./NIVEL_4_SQL_MODELAGEM.md), você vai aprender a modelar relacionamentos entre várias tabelas (1:1, 1:N, N:N), normalizar um modelo de dados, consultar informações combinando tabelas com `JOIN`, e usar transações (`COMMIT`/`ROLLBACK`) para aplicar na prática as garantias ACID vistas aqui.
