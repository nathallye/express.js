# Express.js

- Framework web que roda "em cima" do node.js; Minimalista, simples de enetender e muito eficiente;
- Nesse projeto, vamos usar o Express para implementar a API REST que o back-end vai prover para a aplicação front-end;
- Conceito principal: 
O Express é escrito em um padrão `Chain of Responsibility`(Cadeia de Responsabilidade), que é a mesma coisa que `Middleware`. Ou seja, qual chega uma requisição dentro do node.js no back-end e essa requisição está sendo interceptada pelo Express, a forma que configuramos a requisição no Express é adicionando vvários middlewares. Esses caras são funções que tem determinada assinatura padrão, e vamos encadeando uma função atrás da outra, quando finalizar o processamento ou ele retorna uma resposta para o browser ou passa para o próximo middleware.

![image](https://user-images.githubusercontent.com/86172286/187313568-f6ea1ce1-ed1f-4250-b9fd-658a8a8b0501.png)

- Assinatura de um Middleware dentro do Express:
Temos uma função que recebe requisição/request/`req`, resposta/response/`res` e próximo/`next`:

``` JS
const mid1 = function(req, resp, next) {

}
```

Uma observação quanto a Express é que ele já encapsula todas as "questões" relacionadas ao HTTP, como por exemplo, tipo de requisição, headers, métodos. Fortemente baseado no conceito de `Middleware`, sempre será chamado um middleware passando `req`, `res` e `next`. O papel do middleware é fazer algum tipo de processamento e no final ele tem duas possibilidades, ou ele chama o método `next`(chama o próximo middleware defino na cadeia):

``` JS
const mid1 = function(req, resp, next) {
  // faz algo...
  next();
}
```

Ou ele chama `resp.send` para enviar uma mensagem para o browser(ou status code):

``` JS
const mid1 = function(req, resp, next) {
  // faz algo...
  resp.send("...");
}
```

E uma das formas de colocar um middleware dentro da linha de excusão do request é chamar a função `use` do express:

``` JS
server.use(mid1);
```

Nesse caso, estamos informando que todas as requisições serão interceptadas pelo middleware informado.
Podemos também, passar uma URL base para ele ser chamado só quando ela for atendida:

``` JS
server.use("/api", mid1);
```

## Configuração 

- Primeiramente, vamos criar o arquivo descritor do node `package.json` que armazena a declaração das dependências do projeto que evita a instalação novamente de todas as dependências/módulos manualmente se por ventura trocarmos de máquina ou enviarmos esse projeto para outra pessoa, podendo serem instaladas através desse arquivo usando o comando `npm install`. 
E para criarmos esse arquivo vamos rodar o comando seguinte no terminal:

```
npm init
```

Ou

```
npm init -y
```

**Obs.:** `-y` - É para responder todas as perguntas dessa inicialização de forma padrão.

- Em seguida, vamos instalar o express:

```
npm install --save express
```

**Obs.:** `--save` - Para salvar a referência dessa dependência no arquivo `package.json`.

- Feito isso, vamos criar um arquivo chamado `ex01.js` e nele iremos fazer a importação do `express` através do `require` e armazenando em uma variável chamada `express`:

``` JS
const express = require("express");
```

- Em seguida, iremos criar uma const chamada `server`(normalmente essa constante é chamada de `app`) que irá receber uma instância de `express`:

``` JS
const express = require("express");
const server = express();
```

## Mapeando uma Rota(método GET)

- Agora, iremos mapear para a URL raiz da aplicação(/) para o método `get` uma funcionalidade, vamos associar uma função/um `middleware`, ou seja, vamos mapear essa rota, apontar para um middleware e dentro dele será feito um processamente.
Para isso, iremos chamar o método `get` do `server`(que armazena a instância do express) que irá apontar para a URL `/`(raiz da aplicação) e o segundo parâmetro é justamente uma função middleware que irá receber como parâmetros `req` e `res`:

``` JS
const express = require("express");
const server = express();

server.get("/", function(req, res) {

});
```

- E no corpo da função iremos mandar uma resposta simples para o browser:

``` JS
const express = require("express");
const server = express();

server.get("/", function(req, res) {
  res.send("<h1>Index</h1>");
});
```

- Para finalizar iremos criar uma porta, vamos informar ao servidor/`server` ficar escutando/`listen` essa porta que nesse caso será `3000`:

``` JS
const express = require("express");
const server = express();

server.get("/", function(req, res) {
  res.send("<h1>Index</h1>");
});

server.listen(3000);
```

- E caso ele consiga realmente alorcar essa porta e ficar escutando ela vamos chamar uma função callback(arrow function) que irá exibir um `console` para identificarmos que deu tudo certo:

``` JS
const express = require("express");
const server = express();

server.get("/", function(req, res) {
  res.send("<h1>Index</h1>");
});

server.listen(3000, () => console.log("Executando..."));
```

- Por fim, vamos rodar esse arquivo no servidor node, para isso, no terminal(no diretório do arquivo) iremos usar o comando seguinte:

``` JS
node ex01.js
     [name_file]
```

- Em seguida, para visualizarmos o retorno dessa rota(raiz) vamos abrir no navegador a URL `http://localhost:3000/`.

## Mapeando uma Rota(todos os métodos)

- Para mapear todos os métodos(GET, POST, PUT, DELETE) para um rota podemos usar o método `all`:

``` JS
const express = require("express");
const server = express();

server.get("/", function(req, res) {
  res.send("<h1>Index</h1>");
});

server.all("/test", function(req, res) {
  res.send("<h1>Test</h1>");
});

server.listen(3000, () => console.log("Executando..."));
```

## Cadeia de Middlewares

- Para entendermos como funciona na prática como funciona a questão do `Chain of Responsibility`, termos uma cadeia de de `Middlewares` para atender determinada requisição, iremos criar um arquivo chamado `ex02.js` e nele iremos fazer a importação do `express` através do `require` e armazenando em uma variável chamada `express`.
Em seguida, criar uma const chamada `server`(normalmente essa constante é chamada de `app`) que irá receber uma instância de `express`:

``` JS
const express = require("express");
const server = express();
```

- Em seguida, iremos criar uma porta, vamos informar ao servidor/`server` ficar escutando/`listen` essa porta que nesse caso será `3000`.
Caso ele consiga realmente alorcar essa porta e ficar escutando ela vamos chamar uma função callback(arrow function) que irá exibir um `console` para identificarmos que deu tudo certo:

``` JS
const express = require("express");
const server = express();

server.listen(3000, () => console.log("Executando..."));
```

- Agora, iremos mapear para a URL raiz da aplicação(/) para o método `get` uma funcionalidade, vamos associar uma função/um `middleware`, ou seja, vamos mapear essa rota, apontar para um middleware e dentro dele será feito um processamente.
Para isso, iremos chamar o método `get` do `server`(que armazena a instância do express) que irá apontar para a URL `/`(raiz da aplicação) e o segundo parâmetro é justamente uma função middleware que irá receber como parâmetros `req`, `res` e `next`:

``` JS
const express = require("express");
const server = express();

server.get("/", function(req, res, next) { // podemos suprimir esse último param `next`, mesmo que a função receba por padrão um determinado conjunto de parâmetros, não somos obrigados a passar todos; mas só podemos suprimir os últimos, não podemos suprimir um do meio

});

server.listen(3000, () => console.log("Executando..."));
```

- E no corpo da função iremos exibir um console "Início", quando essa função/esse `middleware` for executado.
Em seguida, vamos chamar o próximo/`next` middleware/função e exibir um console "Fim":

``` JS
const express = require("express");
const server = express();

server.get("/", function(req, res, next) {
  console.log("Início...");
  next(); // next é uma função que irá chamar o próximo middleware
  console.log("Fim...");
});

server.listen(3000, () => console.log("Executando..."));
```

- Feito isso, iremos mapear novamente o método `get` para a mesma URL("/" - raiz da aplicação) e dessa vez a função middleware receber como parâmetros apenas `req` e `res`:

``` JS
const express = require("express");
const server = express();

server.get("/", function(req, res, next) {
  console.log("Início...");
  next(); // next é uma função que irá chamar o próximo middleware
  console.log("Fim...");
});

server.get("/", function(req, res) {

});

server.listen(3000, () => console.log("Executando..."));
```

- E no corpo da função iremos exibir um console "Resposta", quando essa função/esse `middleware` for executado.
Em seguida, vamos enviar uma resposta para o browser chamando `res.send` e passando um `h1`:

``` JS
const express = require("express");
const server = express();

server.get("/", function(req, res, next) {
  console.log("Início...");
  next(); // next é uma função que irá chamar o próximo middleware
  console.log("Fim..."); 
});

server.get("/", function(req, res) {
  console.log("Resposta...");
  res.send("<h1>Olá Express!</h1>");
});

server.listen(3000, () => console.log("Executando..."));
```

- Por fim, vamos rodar esse arquivo no servidor node, para isso, no terminal(no diretório do arquivo) iremos usar o comando seguinte:

``` JS
node ex02.js
     [name_file]
```

- Em seguida, para visualizarmos o retorno dessa rota(raiz) vamos abrir no navegador a URL `http://localhost:3000/`; No browser termos título "Olá Express!" e no console podemos notar o retorno seguinte:

``` 
Executando...
Início...
Resposta...
Fim... // executou o próximo middleware da resposta e quando terminou continuou a excutar o primeiro middleware
``` 

- Com isso, podemos concluir que temos uma cadeia de responsabilidades, ou seja, uma cadeia de middlewares.

## Método USE

- Com o método use temos uma outra forma de mapear a URL e criar uma cadeia de middlewares no Express.
Para entendermos melhor vamos duplicar o exercício anterior(`ex02.js`) e renomear essa cópia para `ex03_use.js` e dentro desse arquivo substituir o método `get` por `use` e no lugar da URL raiz("/") vamos colocar `/api`:

``` JS
const express = require("express");
const server = express();

server.use("/api", function(req, res, next) {
  console.log("Início...");
  next(); 
  console.log("Fim...");
});

server.use("/api", function(req, res) {
  console.log("Resposta...");
  res.send("<h1>API!</h1>");
});

server.listen(3000, () => console.log("Executando..."));
```

- Por fim, vamos rodar esse arquivo no servidor node, para isso, no terminal(no diretório do arquivo) iremos usar o comando seguinte:

``` JS
node ex0.js
     [name_file]
```

- Em seguida, para visualizarmos o retorno dessa rota(raiz) vamos abrir no navegador a URL `http://localhost:3000/api`; No browser termos título "API!" e no console podemos notar o retorno seguinte:

``` 
Executando...
Início...
Resposta...
Fim... // executou o próximo middleware da resposta e quando terminou continuou a excutar o primeiro middleware
```

- Qual a diferença básica do `get` para o `use`? 
O primeiro fato é que usando o método `use` estamos informando que todas as requisições(todos os métodos HTTP - GET, POST, PUT, DELETE) serão interceptadas pelo middleware informado. 
O segundo, é que mesmo se colocarmos a URL assim `http://localhost:3000/api/blabla` ele vai continuar chamando o `/api`, isso acontece porque o `use` recebe esse parâmetro de URL como sendo o início da requisição(prefixo), então se começar com `/api` + algo ele vai continuar chamando o `/api`.

## Método Route

- Para entendermos melhor o método `route`, iremos criar um arquivo chamado `ex04_route.js` e nele iremos fazer a importação do `express` através do `require` e armazenando em uma variável chamada `express`.
Em seguida, criar uma const chamada `server`(normalmente essa constante é chamada de `app`) que irá receber uma instância de `express`:

``` JS
const express = require("express");
const server = express();
```

- Em seguida, iremos criar uma porta, vamos informar ao servidor/`server` ficar escutando/`listen` essa porta que nesse caso será `3000`.
Caso ele consiga realmente alorcar essa porta e ficar escutando ela vamos chamar uma função callback(arrow function) que irá exibir um `console` para identificarmos que deu tudo certo:

``` JS
const express = require("express");
const server = express();

server.listen(3000, () => console.log("Executando..."));
```

- Feito isso, vamos chamar o método `route` do `server`(que armazena a instância do express) passando a URL `/clients`:

``` JS
const express = require("express");
const server = express();

server.route("/clients")

server.listen(3000, () => console.log("Executando..."));
```

- A partir do método `route` podemos encadear/mapear todos os métodos para essa mesma URL informada:

``` JS
const express = require("express");
const server = express();

server.route("/clients")
  .get((req, res) => res.send("Lista de Clientes"))
  .post((req, res) => res.send("Novo Cliente"))
  .put((req, res) => res.send("Altera Cliente"))
  .delete((req, res) => res.send("Remove Cliente"))

server.listen(3000, () => console.log("Executando..."));
```

- Podemos testar a resposta a essas requisições via `Postman`.

## Express Router

- O express.Router nos ajuda a manipular nossas rotas em aplicativos NodeJS.
Para entendermos melhor o método, iremos criar um arquivo chamado `ex05_router.js` e nele iremos fazer a importação do `express` através do `require` e armazenando em uma variável chamada `express`.
Em seguida, criar uma const chamada `router` que irá receber uma instância de `express.Router()`:

``` JS
const express = require("express");
const router = express.Router();
```

- Agora, iremos mapear qualquer URL passada(para isso não iremos passar nenhuma URL como parâmetro para o use) com o método `use` que irá mapear para todos os método HTTP.
Para isso, iremos chamar o método `use` do `server`(que armazena a instância do express.Router()) que não irá receber nenhum URL como parâmetro e função middleware(arrow function) que irá receber como parâmetros `req`, `res` e `next`:

``` JS
const express = require("express");
const router = express.Router();

router.use((req, res, next) => {

});
```

- E no corpo desse middleware global(que irá ser chamado em todas as requisições) vamos criar um middleware que será chamado para qualquer rota dentro desse `router` e ele vai calcular o tempo de duração da requisição.
Para isso, primeiramente iremos criar uima const chamada `init` que irá receber a hora atual em milisegundos através do método `Date.now()`:

``` JS
const express = require("express");
const router = express.Router();

router.use((req, res, next) => {
  const init = Date.now();
});
```

- Depois de "pegarmos" essa hora inicial vamos chamar a função `next()` que irá invocar o resto dos middlewares da cadeia e quando finalizar a execução de todos esses middlewares seguintes, irá retornar para o middleware atual e exibir um console com o tempo de execução desses middlewares, que nada mais é que a hora atual(`Date.now()`) menos a hora inicial(armazenada em `init`):

``` JS
const express = require("express");
const router = express.Router();

router.use((req, res, next) => {
  const init = Date.now();
  next();
  console.log(`Tempo = ${Date.now() - init} ms.`);
});
```

- Em seguida, iremos mapear a URL `/produts/:id`(irá receber como parâmetro o id do produto) para o método `get` e quando for feita uma requisição para essa URL com esse método(get) o middleware(arrow function) será chamado e irá retornar uma resposta em objeto JSON(`res.json`) com o `id` passado via `params` na resposta/`res` e o nome do produto:

``` JS
const express = require("express");
const router = express.Router();

router.use((req, res, next) => {
  const init = Date.now();
  next();
  console.log(`Tempo = ${Date.now() - init} ms.`);
});

router.get("/produts/:id", (req, res) => {
  res.json({id: req.params.id, name: "Caneta"});
});
```

- O mesmo iremos fazer para a URL de clientes `/clients/:id`:

``` JS
const express = require("express");
const router = express.Router();

router.use((req, res, next) => {
  const init = Date.now();
  next();
  console.log(`Tempo = ${Date.now() - init} ms.`);
});

router.get("/produts/:id", (req, res) => {
  res.json({id: req.params.id, name: "Caneta"});
});

router.get("/clients/:id", (req, res) => {
  res.json({id: req.params.id, name: "João"});
});
```

- E para finalizar esse arquivo de routas precisamos exportar essa const `router` para conseguirmos usar ela em outro módulo node:

``` JS
const express = require("express");
const router = express.Router();

router.use((req, res, next) => {
  const init = Date.now();
  next();
  console.log(`Tempo = ${Date.now() - init} ms.`);
});

router.get("/produts/:id", (req, res) => {
  res.json({id: req.params.id, name: "Caneta"});
});

router.get("/clients/:id", (req, res) => {
  res.json({id: req.params.id, name: "João"});
});

module.exports = router;
```

- Feito isso, iremos criar um outro arquivo(chamado `ex05_app.js`) o qual iremos importar o módulo que contém o router com as rotas da aplicação mapeadas(`ex05_router.js`):

``` JS
const express = require("express");
const server = express();
const router = require("./ex05_router");
```
- Em seguida, iremos informar para o express(que está instânciado dentro da const `server`) usar essas rotas através do método `use` passando a URL `/api`, ou seja, sempre para o chamar o router(que é uma cadeia de middlewares) deve constar no início da rota o `/api`:

``` JS
const express = require("express");
const server = express();
const router = require("./ex05_router");

server.use("/api", router);
```

- Feito isso, iremos criar uma porta, vamos informar ao servidor/`server` ficar escutando/`listen` essa porta que nesse caso será `3000`:

``` JS
const express = require("express");
const server = express();
const router = require("./ex05_router");

server.use("/api", router);

server.listen(3000, () => console.log("Executando..."));
```

- Por fim, vamos rodar esse arquivo no servidor node, para isso, no terminal(no diretório do arquivo) iremos usar o comando seguinte:

``` JS
node ex05_app.js
     [name_file]
```

- Em seguida, para visualizarmos o retorno dessas rotas vamos chamar no navegador/ou postman as URL's `http://localhost:3000/api/produts/1` e `http://localhost:3000/api/clients/1`; no browser/postman teremos retornos assim:

``` JSON
// http://localhost:3000/api/produts/1

{
  "id": "1",
  "name": "Caneta"
}
```

``` JSON
// http://localhost:3000/api/clients/1

{
  "id": "1",
  "name": "João"
}
```

- E no console podemos notar o retorno do tempo de execução dessas requisições em ms:

``` 
Executando...
Tempo = 3 ms.
Tempo = 1 ms.
```
## Express e Router são Singletons?

- Quando trabalhamos com um `require` em cima de um modulo do node ele sempre vai retornar uma única instância(singleton):

``` JS
const express1 = require("express");
const express2 = require("express");
console.log(express1 === express2);

// retorno => 
// true
```

- Quando criamos instâncias do `express` estamos trabalhando com instâncias diferentes:

``` JS
const express1 = require("express");
const express2 = require("express");
console.log(express1 === express2);

// retorno => 
// true


const server1 = express1();
const server2 = express2();
console.log(server1 === server2);

// retorno => 
// false
```

- Da mesma forma quando criamos instâncias do `Router` estaremos trabalhando com instâncias diferentes:

``` JS
const express1 = require("express");
const express2 = require("express");
console.log(express1 === express2);

// retorno => 
// true


const server1 = express1();
const server2 = express2();
console.log(server1 === server2);

// retorno => 
// false


const router1 = express1.Router();
const router2 = express2.Router();
console.log(router1 === router2);

// retorno => 
// false
```
