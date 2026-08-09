# Nível 9 - Projeto Final e Portfólio

Todo nível anterior ensinou uma peça isolada: lógica, Java, banco de dados, testes, Git, APIs, Spring Boot, Design Patterns. Este nível não ensina nada tecnicamente novo — ele junta tudo em um projeto único, construído do zero, e trata de uma pergunta que nenhum nível técnico responde sozinho: **como transformar esse projeto em algo que convence alguém, de fora, de que você sabe o que está fazendo?**

---

## Índice de progresso

- [ ] [Objetivo do nível](#objetivo-do-nível)
- [ ] [Projeto sugerido: Sistema de E-commerce](#projeto-sugerido-sistema-de-e-commerce)
- [ ] [Modelando antes de codificar](#modelando-antes-de-codificar)
- [ ] [Funcionalidades esperadas](#funcionalidades-esperadas)
- [ ] [Estrutura de pastas sugerida](#estrutura-de-pastas-sugerida)
- [ ] [Ordem sugerida de construção](#ordem-sugerida-de-construção)
- [ ] [Checklist de pronto para portfólio](#checklist-de-pronto-para-portfólio)
- [ ] [Como escrever um README que um recrutador entenda em 30 segundos](#como-escrever-um-readme-que-um-recrutador-entenda-em-30-segundos)
- [ ] [Publicando no GitHub com boa apresentação](#publicando-no-github-com-boa-apresentação)
- [ ] [Preparando-se para falar sobre o projeto em entrevista](#preparando-se-para-falar-sobre-o-projeto-em-entrevista)
- [ ] [Erros comuns de quem monta o primeiro projeto de portfólio](#erros-comuns-de-quem-monta-o-primeiro-projeto-de-portfólio)
- [ ] [Próximos passos depois deste roadmap](#próximos-passos-depois-deste-roadmap)
- [ ] [Recapitulando o roadmap inteiro](#recapitulando-o-roadmap-inteiro)

---

## Objetivo do nível

Ao final deste nível, o estudante deve ter:

- Um projeto completo, funcionando de ponta a ponta, cobrindo banco de dados modelado, API REST, testes automatizados e pelo menos um Design Pattern aplicado com justificativa real.
- Um repositório no GitHub organizado, com histórico de commits legível e um README que qualquer pessoa de fora do projeto consiga entender em poucos minutos.
- Uma explicação preparada, em poucas frases, do que o projeto faz e de pelo menos uma decisão técnica relevante que foi tomada durante a construção dele.

## Projeto sugerido: Sistema de E-commerce

O domínio de e-commerce foi usado como exemplo em todos os níveis anteriores — clientes, produtos, categorias, pedidos, itens de pedido — de propósito: ao chegar aqui, você já modelou boa parte desse domínio em pedaços, nos exercícios do Nível 3 e Nível 4. Este projeto final é a oportunidade de juntar essas peças em um sistema coeso, em vez de exemplos isolados.

Não é obrigatório usar exatamente e-commerce — o mais importante é escolher um domínio que você entenda bem o suficiente para tomar decisões de modelagem sozinho, sem depender de outra pessoa explicar as regras de negócio. Um domínio familiar (uma biblioteca, uma agenda de estudos, um controle de gastos pessoal) funciona tão bem quanto, desde que tenha entidades relacionadas o bastante para exercitar tudo que o roadmap ensinou.

## Modelando antes de codificar

Antes de criar o primeiro `CREATE TABLE`, retome o processo do [Nível 4](./NIVEL_4_SQL_MODELAGEM.md#modelagem-conceitual-lógica-e-física): desenhe o DER do domínio inteiro primeiro, identificando entidades e a cardinalidade entre elas.

```text
[Cliente] 1 ---- N [Pedido] N ---- N [Produto]
                                        |
                                        N
                                        |
                                        1
                                  [Categoria]
```

Esse desenho não precisa ser bonito nem em uma ferramenta específica — papel, quadro branco ou uma ferramenta como dbdiagram.io servem igualmente. O que importa é que a decisão de "quantas tabelas, e como elas se conectam" aconteça antes do código, não durante — corrigir o modelo no papel custa um rabisco; corrigir depois de meio projeto construído em cima de um modelo errado custa uma refatoração inteira.

## Funcionalidades esperadas

Um escopo razoável para o projeto final, cobrindo as camadas ensinadas sem virar um projeto interminável:

- **Clientes**: cadastro, consulta, atualização.
- **Produtos e categorias**: cadastro, consulta com filtro por categoria, controle de estoque.
- **Carrinho e pedidos**: criação de pedido a partir de um carrinho de itens, cálculo de valor total.
- **Pagamento**: pelo menos uma forma simulada (não é necessário integrar com um gateway de pagamento real).
- **Banco de dados**: MySQL, modelado até a 3FN (Nível 4), com relacionamentos 1:N e N:N reais no domínio (cliente/pedido, pedido/produto).
- **API REST**: endpoints para cada recurso principal, seguindo as convenções do Nível 6 (verbos, status codes, paginação em listagens).
- **Testes**: unitários para regras de negócio (cálculo de total, validação de estoque) e pelo menos um teste de integração de Controller (Nível 5 e Nível 7).
- **Design Pattern**: pelo menos um aplicado com justificativa — por exemplo, Strategy para formas de pagamento, ou Builder para montar um pedido complexo (Nível 8).

Resistir à tentação de adicionar tudo que parece "legal" (autenticação completa, notificação por email, painel administrativo) é uma decisão válida — o objetivo deste projeto é demonstrar profundidade no que já foi ensinado, não amplitude sobre tecnologias novas não cobertas no roadmap.

## Estrutura de pastas sugerida

```text
src/main/java/br/com/estudos/ecommerce
├── controller
├── service
├── repository
├── domain
├── dto
├── exception
└── config
```

Essa estrutura já apareceu, camada por camada, no [Nível 7](./NIVEL_7_SPRING_BOOT.md#arquitetura-em-camadas): `controller` recebe requisições, `service` aplica regras de negócio, `repository` acessa dados, `domain` guarda as Entities, `dto` guarda os objetos de entrada/saída da API, `exception` guarda exceções customizadas (como as tratadas no `@RestControllerAdvice`), `config` guarda configurações específicas do Spring (como CORS, visto no Nível 6).

## Ordem sugerida de construção

Construir tudo ao mesmo tempo tende a gerar um projeto pela metade em várias frentes. Uma ordem que segue a mesma progressão do roadmap:

1. Modelar o domínio completo no papel (DER).
2. Criar o schema no MySQL, com todas as tabelas, chaves e constraints (Nível 3 e 4).
3. Popular o banco manualmente com alguns dados de teste, e validar o modelo com consultas SQL diretas (`JOIN`, `GROUP BY`) antes de escrever qualquer linha de Java.
4. Criar o projeto Spring Boot, com as Entities mapeadas para o schema já existente.
5. Construir um recurso de cada vez, de ponta a ponta (Repository → Service → Controller → teste), começando pelo mais simples (`Categoria`, sem dependências) antes dos mais complexos (`Pedido`, que depende de `Cliente` e `Produto`).
6. Adicionar validação e tratamento de erro centralizado.
7. Escrever os testes que ficaram pendentes durante a construção rápida dos recursos.
8. Revisar o projeto inteiro procurando por um lugar onde um Design Pattern resolveria um problema real já presente no código.
9. Escrever o README e organizar o repositório para publicação.

Um recurso construído de ponta a ponta (passo 5, repetido) antes de avançar para o próximo evita o cenário comum de ter todos os Controllers prontos, mas nenhum Service testado.

## Checklist de pronto para portfólio

- [ ] O projeto roda do zero seguindo só as instruções do próprio README, sem passos que só você sabe de cabeça.
- [ ] O banco de dados está modelado até a 3FN, com chaves primárias, estrangeiras e constraints coerentes.
- [ ] Existem testes automatizados, e eles passam (`mvn test`, sem falha).
- [ ] A API segue as convenções REST do Nível 6: verbos corretos, status codes corretos, JSON bem estruturado.
- [ ] O código está organizado em camadas (Controller, Service, Repository, DTO), sem regra de negócio dentro do Controller.
- [ ] Existe pelo menos um Design Pattern aplicado, com uma linha de comentário ou menção no README explicando por quê.
- [ ] O histórico de commits é legível, com mensagens descritivas (Nível 5), não uma sequência de "ajustes" e "fix".
- [ ] Não existe nenhuma credencial (senha de banco, token) commitada no repositório — tudo sensível está no `.gitignore` (Nível 5) ou em variáveis de ambiente.
- [ ] O README explica o que o projeto faz, como rodar, e quais decisões técnicas foram tomadas.

## Como escrever um README que um recrutador entenda em 30 segundos

Um README de portfólio tem uma audiência diferente da documentação técnica interna de um projeto de trabalho: quem lê, na maioria das vezes, está avaliando rapidamente vários candidatos, e não vai investir tempo tentando entender um projeto malexplicado. A estrutura recomendada, nessa ordem:

```markdown
# Nome do Projeto

Uma frase dizendo o que o sistema faz e para quem.

## Funcionalidades

- Lista curta das funcionalidades principais.

## Tecnologias

Java, Spring Boot, MySQL, JUnit, Maven.

## Como rodar

1. Clone o repositório.
2. Configure o banco (`application.properties`).
3. Rode `mvn spring-boot:run`.

## Decisões técnicas

Uma ou duas decisões relevantes, explicadas em poucas frases —
por exemplo, por que um Design Pattern específico foi usado, ou
como o modelo de dados foi pensado.

## Possíveis melhorias

O que você faria a seguir, se tivesse mais tempo.
```

A seção **"Decisões técnicas"** é a que mais diferencia um README de portfólio de um README genérico gerado automaticamente — ela demonstra que você entende o "porquê" por trás do código, não só o "o quê", que é exatamente o tipo de raciocínio que este roadmap tentou construir desde o Nível 3.

## Publicando no GitHub com boa apresentação

Além do README, alguns detalhes pequenos mudam a primeira impressão de um repositório:

- **Descrição do repositório**: o campo curto de descrição do GitHub (aparece ao lado do nome do repositório) deveria resumir o projeto em uma linha, sem precisar abrir o README.
- **Topics**: tags como `java`, `spring-boot`, `mysql`, `rest-api` ajudam o repositório a ser encontrado e comunicam a stack de imediato, antes mesmo de abrir qualquer arquivo.
- **Commits organizados**: um histórico com dezenas de commits chamados "ajustes" não passa a mesma confiança que um histórico com mensagens descritivas, seguindo o padrão do [Nível 5](./NIVEL_5_TESTES_E_GIT.md#commits-atômicos-e-mensagens).
- **Sem arquivos desnecessários versionados**: pastas de build (`target/`), configuração de IDE, e arquivos temporários não deveriam aparecer no repositório — sinal de um `.gitignore` ausente ou incompleto.
- **Licença**: adicionar uma licença open source simples (MIT, por exemplo) é opcional para um projeto de portfólio, mas comunica profissionalismo básico sobre como o código pode ser usado por outros.

## Preparando-se para falar sobre o projeto em entrevista

Um projeto de portfólio é, na prática, um roteiro de conversa em entrevistas técnicas. Vale preparar respostas curtas, não decoradas, para perguntas como:

- "Me conta sobre esse projeto." — uma resposta de 30 segundos, começando pelo problema que o sistema resolve, não pela lista de tecnologias usadas.
- "Por que você modelou o banco assim?" — uma decisão de modelagem específica (por que uma tabela associativa aqui, por que uma chave composta ali) que você consegue justificar, não só descrever.
- "Por que você usou esse Design Pattern?" — o problema concreto que motivou a escolha, não "porque parecia legal usar".
- "O que você faria diferente hoje?" — sinal de maturidade técnica é reconhecer limitações do próprio projeto, não apresentá-lo como perfeito. A seção "Possíveis melhorias" do README já é um rascunho dessa resposta.

Essas perguntas recompensam quem realmente entendeu as decisões tomadas ao longo do roadmap, e expõem rapidamente quem só copiou uma estrutura pronta sem entender o porquê — outro motivo, além do aprendizado em si, pelo qual as seções de "Uso responsável de IA" insistiram, nível após nível, para que o raciocínio fosse seu.

## Erros comuns de quem monta o primeiro projeto de portfólio

**Projeto grande demais, terminado pela metade**
Um projeto incompleto, com metade das funcionalidades quebradas, comunica menos confiança do que um projeto menor e completo. Prefira reduzir o escopo da seção [Funcionalidades esperadas](#funcionalidades-esperadas) a entregar algo inacabado.

**README ausente ou desatualizado**
Um projeto tecnicamente bom, sem README, obriga quem avalia a ler código para entender o que o sistema faz — a maioria simplesmente não vai fazer esse esforço. O README não é opcional para fins de portfólio.

**Credenciais reais commitadas no histórico**
Mesmo removidas depois, senhas e tokens commitados em algum momento do histórico Git continuam recuperáveis por qualquer pessoa (Nível 5). Sempre usar variáveis de ambiente ou arquivos fora do controle de versão para credenciais, desde o primeiro commit.

**Testes que não passam**
Um repositório onde `mvn test` falha na primeira tentativa passa a impressão (correta) de descuido. Rodar a suíte de testes completa antes de cada push é um hábito barato que evita esse problema.

**Copiar um tutorial sem adaptar o domínio**
Um projeto idêntico a um tutorial popular, sem nenhuma decisão própria de modelagem ou funcionalidade, não demonstra capacidade de resolver problemas novos — só capacidade de seguir instruções. Pequenas variações no domínio, nas regras de negócio ou nas funcionalidades já diferenciam um projeto autoral de uma cópia.

## Próximos passos depois deste roadmap

Este roadmap cobre uma base sólida, mas não é o fim da estrada. Alguns caminhos naturais de aprofundamento, sem obrigação de seguir nenhum específico:

- **Autenticação e autorização**: Spring Security, JWT — como proteger endpoints e identificar quem está fazendo cada requisição.
- **Containers**: Docker, para empacotar a aplicação e o banco de dados de forma reprodutível em qualquer máquina.
- **Deploy**: hospedar o projeto de verdade (Railway, Render, AWS, entre outros), para ter um link funcionando, não só um repositório.
- **Mensageria**: filas (RabbitMQ, Kafka) para comunicação assíncrona entre partes de um sistema maior.
- **Frontend**: para quem quiser construir a interface que consome a API construída aqui, em vez de testar só via Postman.
- **Outro SGBD**: experimentar PostgreSQL depois do MySQL, para sentir na prática que o raciocínio relacional do Nível 3 e 4 se transfere quase diretamente.

Nenhum desses é pré-requisito para considerar o roadmap concluído — são expansões naturais a partir da base que os nove níveis já construíram.

## Recapitulando o roadmap inteiro

Antes de considerar este roadmap concluído, vale revisar se cada nível deixou o que se propôs:

- [ ] **Nível 1**: lógica de programação, sem depender de sintaxe complexa.
- [ ] **Nível 2**: Java, orientação a objetos, SOLID, e o hábito de versionar código com Git desde o início.
- [ ] **Nível 3**: por que um banco de dados existe, e os fundamentos de uma única tabela bem modelada.
- [ ] **Nível 4**: modelagem de relacionamentos entre várias tabelas, normalização, e consultas ricas com `JOIN`.
- [ ] **Nível 5**: testes automatizados como rede de segurança, e Git como ferramenta de colaboração, não só de backup pessoal.
- [ ] **Nível 6**: o protocolo por trás de qualquer comunicação entre sistemas na web.
- [ ] **Nível 7**: um framework de verdade automatizando o que antes era manual, sem virar caixa preta.
- [ ] **Nível 8**: vocabulário para reconhecer e resolver problemas recorrentes de design.
- [ ] **Nível 9**: tudo isso junto, em um projeto que representa o que você é capaz de construir sozinho.

Se todos os itens fazem sentido sem precisar reabrir o nível correspondente, o roadmap cumpriu o que se propôs.
