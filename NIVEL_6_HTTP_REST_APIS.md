# Nível 6 - HTTP, REST e APIs

Até aqui, todo programa que você escreveu rodava sozinho, no seu computador, começando e terminando dentro de um único processo. Este nível responde a uma pergunta diferente: **como um programa conversa com outro programa, rodando em outra máquina, possivelmente do outro lado do mundo?**

---

## Índice de progresso

- [ ] [Objetivo do nível](#objetivo-do-nível)
- [ ] [Uso responsável de IA](#uso-responsável-de-ia)
- [ ] [O problema: como dois programas em máquinas diferentes conversam](#o-problema-como-dois-programas-em-máquinas-diferentes-conversam)
- [ ] [Cliente e servidor](#cliente-e-servidor)
- [ ] [O que é HTTP](#o-que-é-http)
- [ ] [Anatomia de uma requisição HTTP](#anatomia-de-uma-requisição-http)
- [ ] [Anatomia de uma resposta HTTP](#anatomia-de-uma-resposta-http)
- [ ] [Métodos HTTP](#métodos-http)
- [ ] [Idempotência: um detalhe que importa](#idempotência-um-detalhe-que-importa)
- [ ] [Status codes](#status-codes)
- [ ] [O que é um recurso](#o-que-é-um-recurso)
- [ ] [O que é um endpoint](#o-que-é-um-endpoint)
- [ ] [O que é REST, de verdade](#o-que-é-rest-de-verdade)
- [ ] [Desenhando URLs de recurso](#desenhando-urls-de-recurso)
- [ ] [Paginação, filtros e ordenação em coleções](#paginação-filtros-e-ordenação-em-coleções)
- [ ] [Versionamento de API](#versionamento-de-api)
- [ ] [O modelo de maturidade de Richardson](#o-modelo-de-maturidade-de-richardson)
- [ ] [JSON como formato de troca de dados](#json-como-formato-de-troca-de-dados)
- [ ] [Headers HTTP comuns](#headers-http-comuns)
- [ ] [HTTPS: por que o S importa](#https-por-que-o-s-importa)
- [ ] [CORS: quando o navegador bloqueia sua própria API](#cors-quando-o-navegador-bloqueia-sua-própria-api)
- [ ] [JAX-RS antes do Spring Boot](#jax-rs-antes-do-spring-boot)
- [ ] [Criando um recurso com JAX-RS](#criando-um-recurso-com-jax-rs)
- [ ] [Testando uma API sem escrever cliente nenhum](#testando-uma-api-sem-escrever-cliente-nenhum)
- [ ] [Erros comuns neste nível](#erros-comuns-neste-nível)
- [ ] [Glossário rápido](#glossário-rápido)
- [ ] [Recapitulando](#recapitulando)
- [ ] [O que vem no próximo nível](#o-que-vem-no-próximo-nível)

---

## Objetivo do nível

Ao final deste nível, o estudante deve conseguir:

- Explicar o modelo cliente-servidor e por que HTTP é um protocolo de pedido/resposta sem estado.
- Ler e montar uma requisição e uma resposta HTTP, identificando método, URL, headers, corpo e status code.
- Escolher o método HTTP e o status code corretos para uma operação, e justificar a escolha.
- Explicar o que é um recurso, um endpoint, e o que realmente torna uma API "RESTful" (além de "usa JSON").
- Criar um recurso simples com JAX-RS, incluindo `GET`, `POST` e `PUT`.
- Testar uma API manualmente sem escrever nenhum código cliente.

## Uso responsável de IA

Use a IA para entender uma mensagem de erro de conexão ou para comparar dois designs de URL, mas desenhe você mesmo os endpoints do exercício antes de pedir uma opinião. Pensar em "que recurso é esse, que verbo HTTP faz sentido, que status code eu devo devolver" é exatamente o raciocínio que este nível quer que você treine.

## O problema: como dois programas em máquinas diferentes conversam

O aplicativo de celular de um e-commerce mostra uma lista de produtos. Esses produtos não vivem no celular — eles vivem em um banco de dados, em um servidor, em algum lugar. O app precisa, de alguma forma, pedir "me dê a lista de produtos" para esse servidor, através da internet, e receber a resposta de volta, em um formato que ele consiga entender e exibir.

Isso é um problema de comunicação entre dois programas independentes, que não compartilham memória, não rodam no mesmo processo, e podem estar em qualquer lugar do mundo. Sem um protocolo comum, cada par de programas precisaria inventar sua própria forma de conversar — e um app não conseguiria falar com um servidor escrito em outra linguagem, por outra equipe, seguindo outra convenção. **HTTP** é o protocolo que resolve isso, definindo um formato padrão de pedido e resposta que qualquer sistema, em qualquer linguagem, consegue implementar e entender.

## Cliente e servidor

- **Cliente**: quem inicia a conversa, fazendo um pedido. O aplicativo do celular, o navegador, o `MySQL Workbench` do Nível 3 (cliente do MySQL), o Postman.
- **Servidor**: quem espera por pedidos e responde a eles. O programa Java rodando em algum computador, esperando conexões em uma porta específica.

Essa relação já apareceu no Nível 3, na distinção entre o MySQL Server e o Workbench — é o mesmo padrão arquitetural, aplicado agora à comunicação entre uma aplicação cliente (como um app ou navegador) e uma aplicação servidor que você mesmo vai construir a partir deste nível.

## O que é HTTP

**HTTP** (HyperText Transfer Protocol) é um protocolo de comunicação baseado em **pedido e resposta**: o cliente envia uma requisição, o servidor processa e devolve uma resposta. Depois disso, a conexão daquele pedido específico termina.

Duas características centrais de HTTP, que valem a pena internalizar cedo:

- **Sem estado (stateless)**: o servidor não guarda, por padrão, nenhuma memória de requisições anteriores do mesmo cliente. Cada requisição precisa carregar sozinha tudo que o servidor precisa saber para respondê-la (por exemplo, um token de autenticação, se a operação exigir login) — o servidor não "lembra" que você já se identificou na requisição anterior.
- **Baseado em texto**: uma requisição e uma resposta HTTP são, na essência, texto estruturado, legível por humanos — o que torna HTTP fácil de depurar, mesmo sem ferramentas especiais (dá para literalmente ler uma requisição em texto puro e entender o que ela está pedindo).

## Anatomia de uma requisição HTTP

```http
GET /produtos/10 HTTP/1.1
Host: api.loja.com
Accept: application/json
Authorization: Bearer eyJhbGciOiJIUzI1NiIs...

```

- **Linha inicial**: método (`GET`), caminho do recurso (`/produtos/10`) e versão do protocolo.
- **Headers**: metadados sobre a requisição — `Host` (para qual servidor), `Accept` (que formato de resposta o cliente aceita), `Authorization` (credencial de acesso).
- **Corpo (body)**: dados enviados junto com a requisição — presente em `POST`/`PUT`, tipicamente ausente em `GET`.

## Anatomia de uma resposta HTTP

```http
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 58

{"id": 10, "nome": "Teclado", "preco": 120.00}
```

- **Linha inicial**: versão do protocolo, status code (`200`) e uma descrição curta (`OK`).
- **Headers**: metadados sobre a resposta — `Content-Type` (formato do corpo), `Content-Length` (tamanho em bytes).
- **Corpo**: os dados retornados, no formato indicado por `Content-Type`.

## Métodos HTTP

O método indica a **intenção** do cliente sobre o recurso — o que ele quer fazer, não como fazer.

| Método | Intenção |
|---|---|
| `GET` | Ler um recurso, sem alterá-lo |
| `POST` | Criar um novo recurso |
| `PUT` | Atualizar um recurso existente por completo |
| `PATCH` | Atualizar parcialmente um recurso existente |
| `DELETE` | Remover um recurso |

```text
GET    /produtos       → lista todos os produtos
GET    /produtos/10    → busca o produto de id 10
POST   /produtos       → cria um novo produto
PUT    /produtos/10    → substitui o produto de id 10 por completo
PATCH  /produtos/10    → atualiza só alguns campos do produto de id 10
DELETE /produtos/10    → remove o produto de id 10
```

A diferença entre `PUT` e `PATCH` é um ponto comum de confusão: `PUT` espera o recurso completo no corpo da requisição (campos omitidos são tratados como removidos ou zerados); `PATCH` espera só os campos que devem mudar, deixando o resto do recurso intocado.

## Idempotência: um detalhe que importa

Um método é **idempotente** quando repeti-lo várias vezes, com os mesmos dados, produz o mesmo resultado final que executá-lo uma única vez — sem efeito colateral acumulado a cada repetição.

- `GET`, `PUT` e `DELETE` são idempotentes: buscar o mesmo produto dez vezes não muda nada; substituir o produto pelo mesmo valor dez vezes deixa o produto exatamente igual; remover o mesmo produto duas vezes deixa o resultado igual a remover uma vez (na segunda vez, ele simplesmente já não existe mais).
- `POST` **não** é idempotente: enviar a mesma requisição de criação duas vezes cria dois recursos novos, não um.

Isso não é só teoria: importa na prática quando uma requisição falha por problema de rede e o cliente decide tentar de novo automaticamente. Reenviar um `GET` ou um `PUT` é seguro. Reenviar um `POST` sem cuidado pode duplicar um pedido inteiro — um bug real e comum em sistemas de e-commerce, exatamente o domínio deste roadmap.

## Status codes

O status code, na linha inicial da resposta, comunica o resultado da requisição, agrupado em faixas com significado próprio:

| Faixa | Significado geral |
|---|---|
| `1xx` | Informacional (raramente visto diretamente) |
| `2xx` | Sucesso |
| `3xx` | Redirecionamento |
| `4xx` | Erro do cliente (o pedido está errado) |
| `5xx` | Erro do servidor (o servidor falhou ao processar um pedido válido) |

Os mais usados no dia a dia de uma API REST:

- `200 OK`: sucesso genérico, com corpo de resposta.
- `201 Created`: um recurso novo foi criado com sucesso (resposta típica de `POST`).
- `204 No Content`: sucesso, mas sem corpo de resposta (comum em `DELETE`).
- `400 Bad Request`: a requisição está malformada ou os dados enviados são inválidos.
- `401 Unauthorized`: o cliente não está autenticado (não disse quem é, ou disse errado).
- `403 Forbidden`: o cliente está autenticado, mas não tem permissão para essa ação.
- `404 Not Found`: o recurso pedido não existe.
- `409 Conflict`: a requisição conflita com o estado atual do recurso (por exemplo, criar um cliente com um email que já existe).
- `500 Internal Server Error`: algo deu errado no servidor, de forma inesperada — geralmente sinal de um bug não tratado.

> **Pare e pense:** um cliente tenta atualizar um pedido que já foi cancelado, e a regra de negócio não permite alterar pedidos cancelados. Qual status code é mais apropriado: `400`, `403` ou `409`?
>
> <details><summary>Ver resposta</summary>
>
> `409 Conflict` é o mais apropriado. A requisição em si está bem formada (não é `400`) e o cliente tem permissão para editar pedidos em geral (não é `403`) — o problema é que o **estado atual** do recurso (cancelado) é incompatível com a operação pedida (atualizar). Isso é exatamente o que `409` comunica: a ação é válida em princípio, mas não pode ser executada no estado atual do recurso.
> </details>

## O que é um recurso

Um **recurso** é qualquer coisa do domínio que a API expõe e que pode ser identificada por uma URL: um cliente, um produto, um pedido, a lista de pedidos de um cliente específico. Pensar em REST é, antes de tudo, pensar em quais substantivos do seu domínio merecem virar recursos — o mesmo raciocínio de identificar entidades, já treinado na modelagem de banco de dados do Nível 4.

## O que é um endpoint

Um **endpoint** é uma URL específica, combinada com um método HTTP, que permite interagir com um recurso.

```text
GET    /produtos
GET    /produtos/10
POST   /produtos
PUT    /produtos/10
DELETE /produtos/10
```

Repare que a mesma URL (`/produtos/10`) forma endpoints diferentes dependendo do método — `GET /produtos/10` e `DELETE /produtos/10` são dois endpoints distintos, com efeitos completamente diferentes, mesmo apontando para o mesmo recurso.

## O que é REST, de verdade

É comum ouvir "API REST" como sinônimo de "API que usa JSON e HTTP", mas isso é impreciso. **REST** (Representational State Transfer) é um conjunto de restrições arquiteturais, descrito por Roy Fielding em 2000, que uma API precisa seguir para ser chamada de RESTful de fato:

- **Cliente-servidor**: cliente e servidor são independentes; um pode evoluir sem forçar mudança imediata no outro, desde que o contrato da API se mantenha.
- **Sem estado (stateless)**: cada requisição contém tudo que o servidor precisa para processá-la — já visto na seção sobre HTTP.
- **Cacheável**: respostas podem indicar se e por quanto tempo podem ser reaproveitadas sem uma nova requisição ao servidor.
- **Interface uniforme**: recursos identificados por URL, manipulados através dos métodos HTTP padrão, com representações padronizadas (como JSON) — a base de tudo que este nível já mostrou.
- **Sistema em camadas**: o cliente não precisa saber se está falando direto com o servidor final ou com um intermediário (um balanceador de carga, um cache, um gateway) no meio do caminho.

Na prática do mercado, a maioria das APIs chamadas de "REST" segue a interface uniforme com bastante rigor e trata as outras restrições de forma mais frouxa — e tudo bem, desde que isso seja uma escolha consciente, não desconhecimento do que REST originalmente propõe. O ponto essencial para este roadmap: **REST é sobre modelar recursos e usar o protocolo HTTP como ele foi desenhado para ser usado** (métodos com a intenção certa, status codes com o significado certo), não é só "qualquer API que devolve JSON".

> **Pare e pense:** `GET /clientes/5/pedidos` lista os pedidos do cliente 5. Que status code esse endpoint deveria devolver se o cliente 5 existir, mas não tiver nenhum pedido ainda?
>
> <details><summary>Ver resposta</summary>
>
> `200 OK`, com uma lista vazia (`[]`) no corpo — não `404`. O recurso pedido é "a coleção de pedidos do cliente 5", e essa coleção existe, só está vazia; isso é diferente de `GET /clientes/999/pedidos` para um cliente que não existe, onde `404` seria correto porque o próprio cliente 5 (o recurso pai) não existe. Confundir "coleção vazia" com "recurso inexistente" é um erro comum de quem está aprendendo a desenhar status codes.
> </details>

## Desenhando URLs de recurso

Algumas convenções amplamente adotadas para desenhar URLs de API, que valem a pena seguir por padrão:

- Use substantivos no plural para coleções: `/produtos`, não `/produto` nem `/listarProdutos`.
- Recurso específico dentro de uma coleção: `/produtos/10`, não `/produtos/buscar?id=10`.
- Relacionamento entre recursos, aninhando na URL: `/clientes/5/pedidos` para "os pedidos do cliente 5".
- Não coloque o verbo na URL — o verbo já é o método HTTP. `POST /produtos`, não `POST /criarProduto`.
- Use filtros como parâmetros de query, não como parte do caminho: `/produtos?categoria=eletronicos`, não `/produtos/categoria/eletronicos` (a não ser que "categoria" seja, ela mesma, um recurso navegável).

```text
GET /clientes/5/pedidos       → lista os pedidos do cliente 5
GET /produtos?categoria=1     → lista produtos filtrados por categoria
```

## Paginação, filtros e ordenação em coleções

`GET /pedidos` parece simples até a tabela `pedidos` ter 2 milhões de linhas — devolver tudo de uma vez numa única resposta seria lento para o servidor montar, pesado para trafegar pela rede, e a maior parte disso o cliente nem vai exibir de uma vez. O mesmo problema de escala que o Nível 3 resolveu com `LIMIT` no banco aparece de novo aqui, um nível acima, na API.

A convenção comum é aceitar parâmetros de query para controlar o tamanho e o recorte da resposta:

```text
GET /pedidos?pagina=1&tamanho=20
GET /pedidos?status=PAGO&ordenarPor=data_pedido&direcao=desc
```

Uma resposta paginada costuma incluir, além dos dados, metadados sobre a paginação:

```json
{
  "dados": [ { "id": 1, "valorTotal": 250.00 }, { "id": 2, "valorTotal": 89.90 } ],
  "pagina": 1,
  "tamanho": 20,
  "totalElementos": 347,
  "totalPaginas": 18
}
```

Por trás dos panos, esses parâmetros de query normalmente se traduzem direto em `LIMIT`/`OFFSET` e `ORDER BY` na consulta SQL — a API é, em boa parte, uma tradução do que o Nível 4 já ensinou para o vocabulário HTTP. O [Nível 7](./NIVEL_7_SPRING_BOOT.md) mostra isso pronto, através do suporte a paginação do Spring Data JPA.

## Versionamento de API

Uma API evolui: campos são adicionados, removidos, o formato de uma resposta muda. O problema é que, diferente do código do seu próprio projeto, você não controla quem está consumindo uma API pública — outros times, outros aplicativos, às vezes clientes externos. Mudar um endpoint existente de forma incompatível pode quebrar sistemas que você nem sabe que existem.

**Versionamento** é a prática de sinalizar explicitamente qual "contrato" da API está sendo usado, permitindo que uma versão nova conviva com uma versão antiga até que todo mundo migre. As duas formas mais comuns:

```text
GET /v1/produtos       (versão na URL, mais simples e mais visível)
GET /produtos
Accept: application/vnd.loja.v1+json   (versão no header, mais "correto" segundo REST, menos comum na prática)
```

Não existe necessidade de versionar uma API deste roadmap, com um único consumidor (você mesmo). O ponto de aprender isso agora é reconhecer que uma API pública, usada por terceiros, não pode simplesmente mudar de forma incompatível sem aviso — o mesmo cuidado que se tem ao alterar uma tabela de banco de dados já em uso por outro sistema.

## O modelo de maturidade de Richardson

A seção sobre [o que é REST, de verdade](#o-que-é-rest-de-verdade) descreveu as restrições formais de Fielding. Uma forma mais prática de enxergar o quão "REST" uma API realmente é vem do **Modelo de Maturidade de Richardson**, que descreve níveis progressivos:

- **Nível 0**: um único endpoint, tudo via `POST`, o método HTTP não comunica nada — parecido com um túnel RPC disfarçado de HTTP.
- **Nível 1**: existem recursos com URLs próprias (`/produtos`, `/produtos/10`), mas ainda usando poucos métodos HTTP de forma correta.
- **Nível 2**: os métodos HTTP são usados com o significado correto (`GET` para ler, `POST` para criar...) e os status codes comunicam o resultado real — é o nível que este roadmap ensina, e onde a maioria das APIs consideradas "REST" no mercado se encontra.
- **Nível 3 (HATEOAS)**: além de tudo isso, cada resposta inclui links para as ações e recursos relacionados disponíveis a partir dali, permitindo ao cliente "navegar" pela API sem precisar conhecer todas as URLs de antemão — parecido com clicar em links dentro de uma página web, mas para uma API.

```json
{
  "id": 10,
  "nome": "Teclado",
  "preco": 120.00,
  "links": [
    { "rel": "self", "href": "/produtos/10" },
    { "rel": "categoria", "href": "/categorias/3" },
    { "rel": "adicionar-ao-carrinho", "href": "/carrinho/itens", "method": "POST" }
  ]
}
```

HATEOAS (Hypermedia as the Engine of Application State) é, tecnicamente, parte da definição original de REST de Fielding — mas é raro na prática, porque exige mais trabalho de design e nem todo cliente de API sabe aproveitar esses links. Vale reconhecer que existe e o que ele resolve (desacoplar o cliente de precisar conhecer toda a estrutura de URLs de antemão), sem se cobrar para implementá-lo nos exercícios deste roadmap — nível 2 de maturidade já é uma API bem desenhada para os padrões do mercado.

## JSON como formato de troca de dados

**JSON** (JavaScript Object Notation) é o formato mais comum de corpo de requisição/resposta em APIs REST modernas — texto estruturado, legível por humanos, com suporte nativo ou por biblioteca em praticamente toda linguagem de programação.

```json
{
  "id": 10,
  "nome": "Teclado",
  "preco": 120.00,
  "categoria": {
    "id": 3,
    "nome": "Periféricos"
  },
  "tags": ["gamer", "mecanico"]
}
```

Os tipos de dado do JSON são deliberadamente poucos: texto (entre aspas), número, booleano (`true`/`false`), `null`, objeto (entre `{}`) e lista (entre `[]`). Repare no paralelo direto com o que você já viu: um objeto JSON descrevendo um produto se parece, propositalmente, com uma linha de uma tabela `produtos` combinada com sua categoria — a "tradução" entre uma linha de banco de dados e um JSON de API é um dos papéis centrais do Controller e do DTO, no [Nível 7](./NIVEL_7_SPRING_BOOT.md).

## Headers HTTP comuns

Além de `Authorization`, `Content-Type` e `Accept`, já vistos, alguns headers aparecem com frequência no dia a dia de uma API:

| Header | Papel |
|---|---|
| `Content-Type` | Formato do corpo enviado (`application/json`, `text/html`) |
| `Accept` | Formato de resposta que o cliente aceita receber |
| `Authorization` | Credencial de acesso (ex.: `Bearer <token>`) |
| `Cache-Control` | Regras de cache para a resposta |
| `Location` | Em uma resposta `201 Created`, a URL do recurso recém-criado |

```http
HTTP/1.1 201 Created
Content-Type: application/json
Location: /produtos/57

{"id": 57, "nome": "Mouse sem fio", "preco": 89.90}
```

O header `Location`, especificamente, é parte do que torna uma resposta de criação bem desenhada: além de confirmar que o recurso foi criado, ela já informa exatamente onde ele pode ser encontrado depois.

## HTTPS: por que o S importa

Tudo até aqui descreveu HTTP puro, que trafega como texto plano pela rede — qualquer pessoa capaz de interceptar o tráfego (por exemplo, na mesma rede Wi-Fi pública) consegue ler o conteúdo, incluindo senhas e tokens de autenticação enviados em texto puro.

**HTTPS** é HTTP rodando sobre uma camada de criptografia (TLS), que embaralha o conteúdo da comunicação de forma que só o cliente e o servidor legítimos conseguem lê-lo. Toda API que lida com dados reais de usuários — praticamente qualquer sistema além de um exercício local — deve rodar exclusivamente sobre HTTPS. Isso volta a ser relevante no [Nível 7](./NIVEL_7_SPRING_BOOT.md), quando a aplicação sair do seu computador e for hospedada de verdade.

## CORS: quando o navegador bloqueia sua própria API

Um problema clássico ao ligar um frontend (rodando, digamos, em `http://localhost:3000`) a uma API (rodando em `http://localhost:8080`): a requisição funciona perfeitamente pelo Postman ou cURL, mas falha silenciosamente quando feita a partir do navegador, com um erro de "CORS" no console.

**CORS** (Cross-Origin Resource Sharing) é uma política de segurança que os navegadores aplicam por padrão: por segurança, uma página carregada de uma origem (`localhost:3000`) não pode, por padrão, fazer requisições para uma origem diferente (`localhost:8080`) e ler a resposta — mesmo que as duas estejam no mesmo computador. "Origem diferente" aqui significa domínio, porta ou protocolo diferentes; qualquer um dos três já conta.

Essa restrição existe para impedir que um site malicioso, aberto em uma aba, faça requisições silenciosas para outro site onde você está autenticado (por exemplo, seu banco), usando suas credenciais sem você saber — o navegador bloqueia por padrão qualquer comunicação entre origens diferentes, a não ser que o **servidor de destino explicitamente permita**.

A permissão é dada pelo servidor, através de um header na resposta:

```http
Access-Control-Allow-Origin: http://localhost:3000
```

Isso não é configurado no navegador nem no cliente — é o servidor (sua API JAX-RS ou, mais adiante, Spring Boot) que precisa declarar quais origens têm permissão de consumi-la. Esse é o motivo pelo qual "funciona no Postman, não funciona no navegador" não é contraditório: o Postman não é um navegador e não aplica política de CORS nenhuma — só o navegador aplica essa restrição.

## JAX-RS antes do Spring Boot

**JAX-RS**, hoje formalmente **Jakarta RESTful Web Services**, é uma especificação Java para criar serviços REST usando anotações. Este roadmap ensina JAX-RS antes do Spring Boot de propósito: construir um endpoint "na mão", vendo exatamente o que cada anotação faz, torna o Spring Boot (Nível 7) compreensível como uma automação de um processo que você já entende — não uma caixa preta mágica que "simplesmente funciona".

## Criando um recurso com JAX-RS

```java
@Path("/produtos")
public class ProdutoResource {

    @GET
    @Produces(MediaType.APPLICATION_JSON)
    public List<Produto> listar() {
        return List.of(new Produto("Mouse", 50.0), new Produto("Teclado", 120.0));
    }

    @GET
    @Path("/{id}")
    @Produces(MediaType.APPLICATION_JSON)
    public Produto buscarPorId(@PathParam("id") Long id) {
        return new Produto("Teclado", 120.0);
    }

    @POST
    @Consumes(MediaType.APPLICATION_JSON)
    @Produces(MediaType.APPLICATION_JSON)
    public Response criar(Produto produto) {
        // em um sistema real, aqui entraria a lógica de salvar no banco
        return Response.status(Response.Status.CREATED).entity(produto).build();
    }

    @PUT
    @Path("/{id}")
    @Consumes(MediaType.APPLICATION_JSON)
    public Response atualizar(@PathParam("id") Long id, Produto produto) {
        return Response.ok(produto).build();
    }

    @DELETE
    @Path("/{id}")
    public Response remover(@PathParam("id") Long id) {
        return Response.noContent().build();
    }
}
```

Ligando cada anotação ao vocabulário deste nível:

- `@Path("/produtos")`: define a URL base do recurso, na classe inteira.
- `@GET`, `@POST`, `@PUT`, `@DELETE`: ligam um método Java a um método HTTP, a mesma tabela vista na seção [Métodos HTTP](#métodos-http).
- `@Path("/{id}")`, em um método: complementa o caminho da classe, formando `/produtos/{id}`.
- `@PathParam("id")`: captura o valor `{id}` da URL e injeta como parâmetro do método Java.
- `@Produces`: declara o `Content-Type` da resposta — o que o método "produz".
- `@Consumes`: declara o `Content-Type` esperado no corpo da requisição — o que o método "consome".
- `Response.status(...)`, `Response.ok(...)`, `Response.noContent()`: constroem a resposta com o status code correto — `201` para criação, `200` para atualização, `204` para remoção, seguindo exatamente a tabela de [Status codes](#status-codes).

## Testando uma API sem escrever cliente nenhum

Você não precisa escrever um aplicativo cliente completo para testar os endpoints acima. Ferramentas dedicadas enviam requisições HTTP manualmente, permitindo inspecionar a resposta diretamente:

- **Postman** ou **Insomnia**: interfaces gráficas para montar requisições (método, URL, headers, corpo) e ver a resposta formatada.
- **cURL**: ferramenta de linha de comando, disponível na maioria dos sistemas.

```bash
curl -X GET http://localhost:8080/produtos

curl -X POST http://localhost:8080/produtos \
  -H "Content-Type: application/json" \
  -d '{"nome": "Mouse sem fio", "preco": 89.90}'
```

Testar manualmente dessa forma, antes de qualquer interface visual existir, é uma prática comum e valiosa: ela isola completamente a API do resto do sistema (banco de dados à parte, frontend à parte), permitindo confirmar que cada endpoint se comporta como esperado — método certo, status code certo, corpo certo — antes de conectar qualquer outra peça a ele.

## Erros comuns neste nível

**`404 Not Found` em um endpoint que "deveria existir"**
Geralmente é erro de digitação na URL, um `@Path` faltando ou escrito diferente do esperado, ou o servidor não estar rodando na porta esperada. Confira a URL completa, incluindo a base (`@Path` da classe) mais o caminho do método.

**`415 Unsupported Media Type`**
O corpo da requisição foi enviado com um `Content-Type` diferente do que o método espera em `@Consumes`. Confira se a requisição está declarando `Content-Type: application/json` quando o servidor espera JSON.

**Confundir `PUT` idempotente com `POST`**
Chamar `POST /produtos/10` esperando atualizar o produto 10 não funciona como esperado — `POST`, pela convenção deste nível, é para criar um recurso novo dentro da coleção `/produtos`, não para atualizar um específico. O endpoint correto para atualização é `PUT /produtos/10` (ou `PATCH`, para atualização parcial).

**Corpo vazio em `DELETE`, mas retornando `200` em vez de `204`**
Não é um "erro" que quebra a aplicação, mas é uma inconsistência semântica: se a resposta não tem corpo, `204 No Content` comunica isso com mais precisão do que `200 OK`, que sugere a presença de um corpo de resposta.

**Erro de CORS no console do navegador, requisição funciona no Postman**
Como visto na seção sobre [CORS](#cors-quando-o-navegador-bloqueia-sua-própria-api), isso não é um bug no cliente — é o servidor não declarando a origem do frontend como permitida. A correção acontece no servidor, não tentando configurar algo no navegador ou no código do frontend.

## Glossário rápido

| Termo | Definição curta |
|---|---|
| HTTP | Protocolo de comunicação cliente-servidor baseado em pedido/resposta |
| Stateless | O servidor não guarda memória de requisições anteriores do mesmo cliente |
| Cliente | Quem inicia a requisição |
| Servidor | Quem recebe e responde à requisição |
| Método HTTP | Verbo que indica a intenção sobre um recurso (`GET`, `POST`, `PUT`, `PATCH`, `DELETE`) |
| Idempotência | Repetir a operação com os mesmos dados produz o mesmo resultado final |
| Status code | Código numérico que comunica o resultado de uma requisição |
| Recurso | Algo do domínio exposto pela API, identificável por uma URL |
| Endpoint | URL específica combinada com um método HTTP |
| REST | Conjunto de restrições arquiteturais para APIs baseadas em recursos e HTTP |
| JSON | Formato de texto estruturado usado para troca de dados |
| Header HTTP | Metadado de uma requisição ou resposta |
| HTTPS | HTTP com criptografia (TLS) |
| JAX-RS | Especificação Java para criar serviços REST com anotações |
| Paginação | Dividir uma coleção grande em páginas menores na resposta da API |
| Versionamento de API | Sinalizar explicitamente qual contrato de uma API está sendo usado |
| HATEOAS | Respostas incluindo links de navegação para ações e recursos relacionados |
| CORS | Política do navegador que restringe requisições entre origens diferentes |
| Origem (origin) | Combinação de domínio, porta e protocolo que define de onde uma página foi carregada |

## Recapitulando

- [ ] Explicar o modelo cliente-servidor com um exemplo próprio.
- [ ] Explicar por que HTTP é stateless, e o que isso implica para uma requisição.
- [ ] Escolher o método HTTP e o status code corretos para pelo menos cinco cenários diferentes.
- [ ] Explicar a diferença entre `PUT` e `PATCH`, e entre `POST` idempotente e não idempotente.
- [ ] Explicar o que torna uma API "RESTful" além de usar JSON.
- [ ] Criar um recurso completo (`GET`, `POST`, `PUT`, `DELETE`) com JAX-RS.
- [ ] Testar um endpoint manualmente com cURL ou Postman, sem escrever nenhum código cliente.
- [ ] Explicar o que é CORS e por que a correção acontece no servidor, não no navegador.
- [ ] Explicar o que HATEOAS adiciona ao nível 2 de maturidade REST, e por que a maioria das APIs não implementa isso.

## O que vem no próximo nível

Você agora sabe expor um recurso pela web e entende o protocolo por trás de qualquer API. No [Nível 7 - Spring Boot](./NIVEL_7_SPRING_BOOT.md), você vai ver o que um framework de verdade automatiza a partir daqui — configuração, injeção de dependência, persistência — construindo o mesmo tipo de recurso de forma mais produtiva, mas sabendo exatamente o que está acontecendo por trás de cada anotação nova.
