# Aplicando o SOLID em uma aplicação Typescript
## Relembrando o SOLID
S => SRP => Princípio da responsabilidade única  
Uma classe só deve ter uma única função.  
O => OCP => Princípio Aberto-Fechado  
Uma classe ou função pode ser estendida porém não modificada  
L => LSP => Princípio da Substituição de Liskov
Uma classe que herda ou implementa outra classe deve poder ser substituída pela classe pai.  
I => ISP => Princípio de Segregação de Interfaces  
Uma classe não deve ser obrigada a implementar uma interface que contenha atributos ou métodos que a classe não irá utilizar  
OBS: Basicamente não deve-se criar interfaces muito grandes e genéricas.  
D => DIP => Princípio da Inversão de Dependência  
**Anda lado a lado com o LSP**  
Módulos de alto nível não devem depender de módulos de baixo nível.  
Módulos de alto nível: são as rotinas mais fáceis de entender, mais próximas da realidade de qualquer programador. Tendem a exigir menos carga mental para serem usadas;  
Módulos de baixo nível: são as rotinas mais complexas e difíceis de entender. Geralmente são compostas de implementações de cálculos ou comportamentos específicos.  
![DIP](../img/DIP.png)
## Iniciando o projeto
Para iniciar o projeto iremos fazer todas as configurações que estão presentes no módulo de typescript do eslint, prettier e instalar o express com sua dependência para typescript.  
Agora então iremos criar o nosso server.ts, o nosso projeto se chamará carProject.  
Até então o nosso server.ts está assim:

```javascript
import express from "express";

import categoriesRoutes from "./routes/categories.routes";

const app = express();
app.use(express.json());
app.use(categoriesRoutes);
app.listen(3000, () => console.log("listening on 3000"));
```

Já está importando a nossa rota de categorias da nossa pasta de rotas.
## Situação atual do projeto
Atualmente o projeto está organizado assim:  
![Organização](../img/orgCarProject.png)  
A nossa pasta de models contém toda a modelagem, todo o schema da nossa aplicação. Lá tem como os nossos dados devem ser, ou seja, o modelo deles. Assim como o mongoose.Schema(), nela possui somente os atributos da nossa classe.  

A nossa pasta de repositórios possui todas as operações CRUDs da nossa classe que foi importada do model. Além dos repositórios, ela também possui as interfaces que serão implementadas pelas classes para manter o SOLID.  
![Repositórios](../img/repositorios.png)  
Contém 2 repositórios diferente para mostrar como o princípio da substituição de Liskov, as 2 classes(Postgres e Categories) são filhas da interface e na hora de implementar basta passar a interface como parâmetro. 


A nossa pasta de service possui a execução de cada um dos comandos CRUD do repositório: 


Esse é um serviço para a execução do create, perceba:
```javascript
// Será responsável por criar uma categoria

import {
  ICategoryRepositories,
  ICategoryRepositoriesDTO,
} from "../repositories/ICategoryRepositories";

class CreateCategoryService {
  // Inversão de dependência, o private é necessário
  constructor(private category: ICategoryRepositories) {
    this.category = category;
  }

  execute({ name, description }: ICategoryRepositoriesDTO) {
    const categoryAlreadyExists = this.category.findByName(name);
    if (categoryAlreadyExists) {
      throw new Error("Category already exists");
    }
    const newCategory = this.category.create({ name, description });
    return newCategory;
  }
}

export default CreateCategoryService;
```

Essas são as funções que estão sendo importadas:

```javascript
import Category from "../models/CategoryModel";

interface ICategoryRepositoriesDTO {
  // A interface é responsável por definir os valores dos atributos
  name: string;
  description: string;
}

interface ICategoryRepositories {
  create({ name, description }: ICategoryRepositoriesDTO);

  findByName(name: string): Category | undefined;
  list(): Category[];
}
export { ICategoryRepositories, ICategoryRepositoriesDTO };

```

A nossa pasta de rotas contém atualmente somente a nossa rota de categoria:

```javascript
import { Router } from "express";

import CategoriesRepositories from "../repositories/CategoriesRepositories";
import { PostgresCategoryRepositories } from "../repositories/PostgresRepositories";
import CreateCategoryService from "../services/CreateCategoryService";

const categoriesRoutes = Router();
const categoryRepo = new CategoriesRepositories();
const category = new CreateCategoryService(categoryRepo);
// Não deixaremos a rota ter acesso ao banco de dados
categoriesRoutes.post("/categories", (req, res) => {
  try {
    const { name, description } = req.body;
    const newCategory = category.execute({ name, description });
    return res.status(201).json(newCategory);
  } catch (err) {
    return res.status(400).json({ error: err.message });
  }
});
categoriesRoutes.get("/categories", (req, res) => {
  try {
    const categories = categoryRepo.list();
    return res.status(200).json(categories);
  } catch (err) {
    return res.status(400).json({ error: err.message });
  }
});

export default categoriesRoutes;
```
## Criando as especificações
Vamos dar continuidade ao nosso projeto criando as especificações do nosso carro, com base no nosso diagrama:
![Diagrama do Projeto](../img/diagrama.png)  
Para criar a nossa entidade especificações iremos utilizar os mesmos conceitos feitos para as categorias, porém se fizermos isso o repositório e os serviços ficarão muito grandes, então iremos englobar as pastas em módulos e iremos separar os módulos com base no tema:
![Projeto](../img/org.png)