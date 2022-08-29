# Express.js

- Framework web que roda "em cima" do node.js; Minimalista, simples de enetender e muito eficiente;
- Nesse projeto, vamos usar o Express para implementar a API REST que o back-end vai prover para a aplicação front-end;
- Conceito principal: O Express é escrito em um padrão `Chain of Responsibility`(Cadeia de Responsabilidade), que é a mesma coisa que `Middleware`. Ou seja, qual chega uma requisição dentro do node.js no back-end e essa requisição está sendo interceptada pelo Express, a forma que configuramos a requisição no Express é adicionando vvários middlewares. Esses caras são funções que tem determinada assinatura padrão, e vamos encadeando uma função atrás da outra, quando finalizar o processamento ou ele retorna uma resposta para o browser ou passa para o próximo middleware.

![image](https://user-images.githubusercontent.com/86172286/187313568-f6ea1ce1-ed1f-4250-b9fd-658a8a8b0501.png)
