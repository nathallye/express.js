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