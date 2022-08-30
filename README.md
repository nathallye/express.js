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

- Primeiramente, vamps criar o arquivo descritor do node `package.json` que armazena a declaração das dependências do projeto que evita a instalação novamente de todas as dependências/módulos manualmente se por ventura trocarmos de máquina ou enviarmos esse projeto para outra pessoa, podendo serem instaladas através desse arquivo usando o comando `npm install`. 
E para criarmos esse arquivo vamos rodar o comando seguinte no terminal:

```
npm init -y
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

- E para caso ele consiga realmente alorcar essa porta e ficar escutando ela vamos chamar uma função callback(arrow function) que irá exibir um `console` para identificarmos que deu tudo certo:

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

- Em seguida, para visualizarmos o retorno dessa rota(raiz) vamos abrir no navegador a URL `http://localhost:3000/`;

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
