# Express with node
**PARTES EM PORTUGUÊS SÃO MAIS IMPORTANTES CASO ESTEJAM EM NEGRITO SE NÃO ESTIVER FOI PREGUIÇA MSM DPS LEMBRA DE TROCAR PRA INGLÊS**  
For development better use linux, as the vast majority of servers are linux servers
## Nodemon
To start nodemon in package.json scripts let's add start and put nodemon server.js
To start the server we will use npm start
## Express introduction
Import express into the project, it's a node package so use require   
By convention create a variable called app   
Application routes are associations between an http method, a url, and a controller method   

```
Ex: method:'GET', url:'url', action:'listusers()'
```

If we have the site http://mysite.com/, the route gets the page /   
If we have the site http://mysite.com/sobre, the route gets the page /sobre      
CRUD - CREATE READ UPDATE DELETE   
Basic API Operations:   
CREATE->> POST, READ->> GET, UPDATE->> PUT, DELETE->>DELETE      
The first parameter of get is the route, in this case the initial /   
The second parameter receives the request and the response (req,res) by default   
The request (require) is what the user is asking for and the response is what will be sent   
```js
app.get('/route',(req,res)=>{
    res.send('something')
})
```
The internet works in request/response mode   
The client sends the request and the server has to send the response   
To send a very basic answer we will send a hello world   
```js
res.send("Hello world"); 
```
Let's send an HTML form to the server   
For this we will use template strings ``   
The post is usually used to receive (create) items from the customer   

```js
app.get("/", (req, res) => {
  res.send(`<form action="/" method="POST">
  Nome: <input type="text" name="nome">
  <button>Enviar formulário</button>
  </form>`);
});
app.post("/", (req, res) => {
  res.send("Recebi o formulário");
});
```
For example uploading files or forms   
In order to receive the form, the post is extremely necessary, without it it would give the error that it cannot be received without the post   
However, it is necessary to ask the server to listen for any request that arrives at the port   
A port refers to a process running on the server   
Some ports have standard things running on them, so we usually use ports that are not as common as 3000 or 3333   
To ask the express to listen use the method listen   
The first parameter is the port   
The second parameter we will pass here a console.log to show that the server is running   
The localhost or 127.0.0.1 is in reference to our machine   

```js
app.listen(3000, () => {
  console.log("Acessar http://localhost:3000");
  console.log("i am running!");
});
```

It is better to start the server by the node server.js method as it is easier to stop it just by typing Ctrl+C   
With each change it is necessary to stop the server and run again, but this is not very productive   
Nodemon restarts the server automatically   
#### Routes
Routes also have parameters   
For example http://mysite.com/profiles/5 the profiles route has for itself the parameter 5  
http://site.com/rota/parametro?querystring   
THE ? serves to demonstrate that it is a query string  
The & serves to separate a query string from another   

## Req params
Request parameters are accessed by req.params   
To access a parameter, it must be passed in the get method  
To say that is a param you need to put the : after the /

```js
app.get("/testes/:id_profiles", (req, res) => {
  console.log(req.params);
});
```

```js
Output:
Ex:"/testes/profiles"
{id_profiles: 'profile'}
```
Let's make the parameters optional with:

```js
"/testes/:id_profiles?"
```

The ? is the symbol for optional

## Req query
To access the require query we use req.query  

```js
app.get("/testes/:id_profiles?", (req, res) => {
  console.log(req.query);
  res.send(req.query);
});

```
## Req Body
To access the require body we use req.body  
The POST will appear in the body of the request  
If we put a console log for req.body in get will appear undefined

```js
app.get("/testes/:id_profiles?", (req, res) => {
  console.log(req.body);
});
Output: Undefined
```

The right way is:

```js

app.post("/", (req, res) => {
  console.log(req.body)
});
```
However, express by default returns us undefined as it doesn't handle  
So we have to tell it to handle req.body

```js
app.use(express.urlencoded({extended:true}))

```

And now will appear the input in the console  
To send that:

```js
app.post("/", (req, res) => {
  res.send(`Você me enviou ${req.body.nome}`);
});
```

Remember:

```js
<form action="/" method="POST">
  Nome: <input type="text" name="nome">
  <button>Enviar formulário</button>
  </form>
```

# MVC AND EXPRESS
![MVC](../img/diagramaMVC.png)

## Controllers
We will use a folder only for routes and we will use the MVC pattern  
We are going to create a routes.js file and we are going to put our routes in it.  
```js
Router.js
const express = require("express");
const route = express.Router();

```

Let's move our routes to the routes.js 

```js
const express = require("express");
const route = express.Router();

route.get("/", (req, res) => {
  res.send(`<form action="/" method="POST">
  Nome: <input type="text" name="nome">
  <button>Enviar formulário</button>
  </form>`);
});

```

But the functions of our routes will be stored by the controllers, so we will create the controllers folder  
We will create a controller for each feature eg a controller for the home page, a controller for creating users, etc  

```js
Controllers/homeController.js
exports.homePage = (req, res) => {
  res.send(`<form action="/" method="POST">
  Nome: <input type="text" name="nome">
  <button>Enviar formulário</button>
  </form>`);
};

```

Our file routes.js
```js

const express = require("express");
const route = express.Router();
const homeController = require("./controllers/homeController");
route.get("/", homeController.homePage);
//This is not homePage() is homePage
module.exports = route;

```
Our file server.js
```js
const express = require("express");
const app = express();
const routes = require("./routes");
app.use(express.urlencoded({ extended: true }));
app.use(routes);
app.listen(3000, () => {
  console.log("Acessar http://localhost:3000");
  console.log("i am running!");
});
```

We need to say to the express to use our routes with:

```js
app.use(routes);
```

Now we will create a post route:

```js
routes.js
const express = require("express");
const route = express.Router();
const homeController = require("./controllers/homeController.js");
//Rotas da Home
route.get("/", homeController.homePage);
route.post("/", homeController.trataPost);

module.exports = route;

```

```js
homeController.js
exports.homePage = (req, res) => {
  res.send(`<form action="/" method="POST">
  Nome: <input type="text" name="nome">
  <button>Enviar formulário</button>
  </form>`);
};
exports.trataPost = (req, res) => {
  res.send("Ei sou sua nova rota de post");
};

```

Now we will gonna create the contact routes
```js
contatoController.js
exports.contato = (req, res) => {
  res.send("Bem vindo a sua rota de contato");
};
```

```js
Routes.js
... código anterior
//Rotas do Contato
route.get("/contato", contatoController.contato);
module.exports = route;
```
Our server.js stay the same  
## Views 
At now our project have back-end and front-end together, but we will gonna create folders for back and front end  
We will create a folder src for the backend and will move the controllers for the src folder  
Now we will create a folder for the Views   
At now our project is like:  
![Project](./../img/project.png)  
We need to tell express that we are going to use the views folder as views and what engine will it use to render the views  
To do that:
```js
server.js
...
const path = require("path");
app.use(express.urlencoded({ extended: true }));
app.set("views", path.resolve(__dirname, "src", "views"));
app.set("view engine", "ejs");
...
```
Let's pass as a parameter that we want to set the views and after that we'll pass the folder path  
Let's pass a second set function to say the view engine, we gonna use the ejs engine to render our views because its similar to the HTML 
<br> 
<br> 
We need to use these engines to render because we need to use operators like for, if and others and we know that pure HTML doesn't do that  
For do that we need to install the ejs engine in the terminal: 

```sh
npm i ejs
```

Now we will create a index.ejs in the views folder to render the views
```html
index.ejs

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Testes</title>
</head>
<body>
    <h1>Eu fui renderizado</h1>
</body>
</html>
```

And in the controllers/homeController we will render ouw view that:
```js
exports.homePage = (req, res) => {
  res.render("index");
};
exports.trataPost = (req, res) => {
  res.send("Ei sou sua nova rota de post");
};

```
## Static content vs Dinamic content
Now let's create a folder for our application's static content, the static content is for example the bundle.js that will be loaded by our application. Static content is content that does not change with user interaction(video is static), it is always the same. Dynamic content is content that responds to user interaction. Sites that contain pure HTML and CSS are considered static, to be considered must have scripts.  
The folder name's Public  
And in the server.js we wil put that:

```js
app.use(express.static(path.resolve(__dirname, "Public")));
```

Now we wil create any file in the Public folder to test:

```txt
Alguma coisa
```

In the url let's put http://localhost:3000/teste.txt to see the text file  
## Express with Webpack
### **Briefly, what's in the frontend folder goes to the browser, what's in the src folder goes to the terminal.**
We will gonna create now a folder with the name Frontend and the folder src will stay with the backend  
Now let's copy the webpackconfig and our front end model  
![Folder Structrure](../img/folders.png)  
Important points: In the server.js file we are saying that our static files are in the Public folder, in our index.ejs it is referring to this folder and it is not necessary to reference the /Public only the /assets/js/bundle.js  
![Important point](../img/important.png)  
We brought the settings from our webpackmodel, then we added the assets from the static files and from our frontend folder, some things were with the wrong path but we've already configured
## Middlewares
In our route.metodo('/',middleware) every parameter after the route itself is middleware, which could be a function in the middle of the path or at the end of answering the client

```javascript
route.get('/',middleware)
```

Our homeController have 2 middlewares

```javascript
exports.homePage = (req, res) => {
  res.render("index");
};
exports.trataPost = (req, res) => {
  res.send("Ei sou sua nova rota de post");
};

```

![Middleware](../img/middleware.png)

![Middlewares in action](../img/cadeiademiddlewares.png)

### Sessions are used to save temporary data like login and password when the client is logged

To use session we can put req.session, like:

```js
function meuMiddleware(req, res, next) {
  req.session = {
    nome: "Luiz",
    sobrenome: "Miranda",
  };
  console.log("passei no seu middleware");
  next();
}
```
We can do that with homeController.js

```javascript
exports.homePage = (req, res) => {
  res.render("index");
  console.log(`Bem vindo cliente ${req.session.nome} ${req.session.sobrenome}`);
};
exports.trataPost = (req, res) => {
  res.send("Ei sou sua nova rota de post");
};
```

And do that with the route.get('/')

```javascript
route.get("/", meuMiddleware, homeController.homePage);
```

Now we will use the middlewares in our server.js with all the routes, like:
```javascript
app.use(meuMiddleware);
```

We can only send a message if the form you want has the proper name.

```javascript
const meuMiddleware = (req, res, next) => {
  if (req.body.cliente) {
    console.log();
    console.log(`Vi que você postou ${req.body.cliente}`);
    console.log();
  }
  next();
};
```

In the index.ejs 

```html
  <form action="/" method="post">
            <label >Cliente</label>
            <input type="text" name="cliente">
            <button>submit</button>
        </form>
```

But if we only want the first name, we can do:

```javascript
if (req.body.cliente) {
    var primeiroNome = req.body.cliente.split(" ");
    console.log();
    console.log(`Vi que você postou ${primeiroNome[0]}`);
    console.log();
  }
  input: João Pedro
  output: Vi que você postou João
```

## **Models and MongoDB**
Now let's use Models and MongoDB to configure our application. We did the account in the mongoDB atlas and did too our db server. Now let's install the packages, the mongoose  
 Não queremos que nossa senha fique exposta, portanto iremos deixá-la fora do repositório do curso com o pacote dotenv.  
Para conectar a nossa base de dados iremos por no server.js  

```javascript
const connectionString =
  "mongodb+srv://Joao7878:VjawD8LfpKmzJmMU@database.bxhp7.mongodb.net/myFirstDatabase?retryWrites=true&w=majority";
//Conectado a base de dados
mongoose.connect(connectionString).then(() => {
  console.log("conectado ao bd");
});
```

Mas o nosso banco de dados só está se conectando após o servidor ser iniciado, isso pode gerar alguns erros, então faremos:

```javascript
mongoose
  .connect(connectionString)
  .then(() => {
    console.log("conectando ao bd");
    app.emit("pronto");
  })
  .catch((e) => {
    console.log("Ocorreu um erro");
  });
```

Quando o sinal for emitido, o app.on() irá dizer ao servidor que ele está pronto para iniciar

```javascript
app.on("pronto", () => {
  app.listen(3000, () => {
    console.log("Acessar http://localhost:3000");
    console.log("i am running!");
  });
});
```
Agora para protegermos nossa senha e nossa url iremos criar na raiz do nosso projeto o arquivo .env para salvar essas informações da seguinta maneira

```
CONNECTIONSTRING = mongodb+srv://Joao7878:VjawD8LfpKmzJmMU@database.bxhp7.mongodb.net/myFirstDatabase?retryWrites=true&w=majority
```

E no nosso server.js iremos deixar o código da seguinta maneira:

```javascript
const express = require("express");
require("dotenv").config();
const app = express();
const mongoose = require("mongoose");
//Conectado a base de dados
mongoose
  //Para esconder a nossa senha
  .connect(process.env.CONNECTIONSTRING)
  .then(() => {
    console.log("conectado ao bd");
    app.emit("pronto");
  })
  .catch((e) => {
    console.log("erro de conexão ao banco");
  });
const routes = require("./routes.js");
const path = require("path");
app.use(express.urlencoded({ extended: true }));
app.use(express.static(path.resolve(__dirname, "Public")));
app.set("views", path.resolve(__dirname, "src", "views"));
app.set("view engine", "ejs");
app.use(routes);
app.on("pronto", () => {
  app.listen(3000, () => {
    console.log("Acessar http://localhost:3000");
    console.log("i am running!");
  });
});
```

Now let's create a model folder in the src folder:
![Model](../img/Model.png)
In the file we will create again a mongoose require and a homeSchema const to be the model for our data.
```javascript
const mongoose = require("mongoose");
//O esquema/modelagem dos nossos dados
const homeSchema = new mongoose.Schema({
  //Modelando nosso banco iremos passar alguns objetos com atributos
  titulo: {
    //Passando o tipo do objeto
    type: String,
    //Dizendo que o titulo é importante e não iremos cadastrar caso não tenha
    required: true,
  },
  descricao: String,
});
//Criando o model e passando ele como Home
const homeModel = mongoose.model("Home", homeSchema);
//Agora iremos exportar o nosso model para ser utilizado
module.exports = homeModel;
```

And now let's import the homeModel in the homeController to use and we need to create the homeModel "user" an that function returns a promisse like:
```javascript
homeController.js
const homeModel = require("../models/homeModel");
//Vamos pedir ao mongo para que ele crie o nosso esquema de dados
//Isso nos retornará uma promisse
homeModel
  .create({
    titulo: "um titulo qualquer",
    descricao: "uma descrição qualquer",
  })
  .then((dados) => {
    console.log(dados);
  })
  .catch((e) => {
    console.log(e);
  });
```

At now we are create data in the controller but the responsable for create, validate, delete,... is the model. Let's remove the import in the controller 
## Session and Flash Messages
## **HAVE 1 MORE SECTION FOR SESSIONS AND COOKIES BELLOW**  
Sessions are used for save somehing about the client's browser  
For example when you log in for the first time to facebook and it asks you if you want to save your username and password in the browser  
Every time the client accesses the server, if it does not clear the cookies and history, the server will send this cookie to log in automatically  
The flash messages is not too used because have some libs and frameworks like react for doing the same thing  
Seria como quando você erra sua senha no facebook e aparece a mensagem falando que você errou, essa mensagem aparece e precisa sumir, a flash message faz isso  
Para instalar iremos digitar no terminal:

```console
npm i express-session connect-mongo connect-flash
```

Now we gonna create a session const and a flash const:

```javascript
const session = require("express-session");
const flash = require("express-flash");
```

And let's connect the session with the database:

```javascript
const MongoStore = require("connect-mongo")(session);
```

Now let's configure the session options with:

```javascript
const sessionOptions = session({
  //Algo secreto que previne de adulteração por parte de terceiros
  secret: "algo secreto",
  //Onde a sessão ficará salva
  //Passamos somente uma conexão que é o cliente que conecta com o mongodb: mongoose
  store: new MongoStore({
    mongooseConnection: mongoose.connection,
  }),
  //Faz a sessão ser salva novamente na store
  resave: false,
  //Força uma sessão "não inicializada" a ser salva na loja
  saveUninitialized: false,
  //Vamos dizer quanto tempo nossa sessão/cookie vai durar
  cookie: {
    //maxAge: diz quanto tempo em milesimos de segundos a sessão vai durar
    //Vamos colocar 7 dias
    maxAge: 1000 * 60 * 60 * 24 * 7,
    //Vamos colocar somente para http
    httpOnly: true,
  },
```

Now let's use that with the flash:

```javascript
app.use(sessionOptions);
app.use(flash());
```

When we create a session the express automatically create a req.session and we can use this req.session like: 

```javascript
  homeController.js
  exports.homePage = (req, res) => {
  req.session.user = { nome: luiz, logado: true };
  res.render("index");
};
```

This will stay in the db for 7 days because we said. After start the server with this req.session we can delete that line of code but will stay in the database, we can print in the console like: 

```javascript
  homeController.js
  exports.homePage = (req, res) => {
  console.log(req.session.user);
  res.render("index");
};
output in console: 
{nome: 'luiz', logado:true}
```

In the mongodb will appear:
![Session](../img/session.png)
To use the flash messages we will put in the controller like:

```javascript
homeController.js 
exports.homePage = (req, res) => {
  console.log(req.session.user);
  req.flash("info", "Hello world");
  req.flash("error", "error");
  req.flash("success", "success");
  res.render("index");
};
```

After we flash this message we can catch then and do something like:

```javascript
console.log(req.flash("info"), req.flash("error"), req.flash("success"));
```

But that will work only 1 time and after will be deleted.
## Inject content into views 
We will use the ejs to do that:
![Ejs inject](../img/ejsinject.png)
![Head ejs](../img/head.ejs.png)
We can do for and if and others controls with the ejs.
## CSRF AND HELMET
A cada post iremos enviar um token para verificar se o post é próprio do nosso servidor  
Csrf é uma prevenção de segurança  
```javascript
server.js
const express = require("express");
require("dotenv").config();
const app = express();
const mongoose = require("mongoose");
const {
  middlewareGlobal,
  checkCSRFError,
  CSRFMiddleware,
} = require("./src/middlewares/middleware");

//Conectado a base de dados
mongoose
  //Para esconder a nossa senha
  .connect(process.env.CONNECTIONSTRING)
  .then(() => {
    console.log("conectado ao bd");
    app.emit("pronto");
  })
  .catch((e) => {
    console.log("erro de conexão ao banco");
  });
const session = require("express-session");
const MongoStore = require("connect-mongo");
const flash = require("connect-flash");
const routes = require("./routes.js");
const path = require("path");
const helmet = require("helmet");
const csrf = require("csurf");
const sessionOptions = session({
  secret: "algo secreto",
  store: MongoStore.create({
    mongoUrl: process.env.CONNECTIONSTRING,
  }),
  resave: false,
  saveUninitialized: false,
  cookie: {
    maxAge: 1000 * 60 * 60 * 24 * 7,
    httpOnly: true,
  },
});
//Bom desativar enquanto o sistema estiver em desenvolvimento
//app.use(helmet());
app.use(sessionOptions);
app.use(flash());
app.use(
  express.urlencoded({
    extended: true,
  })
);
//Colocar antes das rotas
app.use(csrf());
//Injetar o token em todas as paǵinas
app.use(express.static(path.resolve(__dirname, "Public")));
app.set("views", path.resolve(__dirname, "src", "views"));
app.set("view engine", "ejs");
app.use(checkCSRFError);
//Todo formulario das paǵinasprecisará ter o token
app.use(CSRFMiddleware);
app.use(middlewareGlobal);
app.use(routes);
app.on("pronto", () => {
  app.listen(3000, () => {
    console.log("Acessar http://localhost:3000");
    console.log("i am running!");
  });
});
```

```html
<form action="/" method="post">
                <input type="hidden" name="_csrf" value="<%= csrfToken %>" />
                <label>Cliente</label>
                <input type="text" name="cliente">
                <button>submit</button>
            </form>
```  

# **Sessões e Cookies**
## *Se tiver dúvida leia tudo*
## *Cookies*
**O protocolo HTTP é um método stateless ou seja toda informação enviada por uma resposta ou requisição é perdida após ser entregue para o servidor ou o cliente. No caso de dados fixos como a criação de um usuário, a informação será salva em um banco de dados e não será perdida. No entanto, por exemplo, se um usuário está ou não logado no site, essa informação não será salva no banco de dados, pois não tem relevância e gastaria memória para algo que é temporário. No entanto, é necessário saber quando o usuário está logado para poder tomar determinadas decisões.**   
**Temos então um problema já que é preciso salvar informação, não podemos salvar no banco de dados e o protocolo HTTP é stateless. Então surgem os cookies, que são nada mais do que pedaços de memória que são enviados entre requisições e respostas para salvar algo, ou seja, quando uma requisição é feita ela envia um cookie(que é um arquivo contendo os dados) e é armazenado no servidor até que uma resposta seja dada, quando a resposta é dada o cookie é enviado novamente para o cliente.**  
**Por exemplo, quando um usuário está logado na sua rede social e clica no botão de amigos, com isso é feita a requisição GET que acionará uma função para listar os amigos do usuário, nessa requisição GET é enviado um cookie com por exemplo o ID do usuário para garantir que ele está logado, no lado do servidor é verificado se esse ID existe, caso não exista a resposta enviada será provavelmente um erro ou uma página falando que precisa estar logado, mas se o ID existir no cookie, ele será verificado, validado e enviado com a resposta e a listagem dos amigos do usuário. Isso é um cookie.**  
<br>
<br>
## *Sessões*
**Sessões, diferentemente dos cookies, são espaços de memória presentes no servidor que enviaremos informação e resgataremos informação, sendo um espaço de memória no servidor é importante lembrar que não podemos armazenar dados muito grandes nas sessões, pois é uma memória RAM!**  
**Porém, como dito anteriormente, o protocolo HTTP é stateless, então como ele sabe que a nossa sessão existe, se toda informação é perdida após o envio da requisição ou da resposta? Aí entram os cookies, eles são enviados, por respostas e requisições, com o ID da sessão e a cada envio a sessão é verificada e validada.**  
**Com isso voltamos ao exemplo do usuário logado e querendo ver a lista de amigos, com as sessões, seriam enviados e recebidos cookies com o ID da determinada sessão para comprovar que o usuário está logado, quando o usuário deslogar a sessão será encerrada e apagada e os cookies não serão mais enviados.**
## *Cookies vs Sessões*
**Vimos os dois casos de utilização de cookies e de sessões, mas quando devemos utilizar um cookie e uma sessão? Um cookie deve ser utilizado quando o servidor precisa de uma resposta básica e simples do cliente, como quando aparece um *pop-up* e o usuário aperta a opção que não quer mais ver aquele *pop-up*, já as sessões são utilizadas em momentos mais importantes como para verificar se um usuário está ou não logado. Cookies podem ser acessados por qualquer usuário do computador e estão sujeitos a serem interceptados por algum ataque malicioso, por isso não se deve transportar informações importantes neles, além de que eles expiram na data que for especificada. Já as sessões não são expostas e ainda expiram por padrão 20 minutos após o *timeout* do usuário.**  
**Portanto, no exemplo do login de usuário utilize sessões e não cookies!**