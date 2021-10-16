# DataBase
A relational database is used everyday by the most people   
A NoSQL database is used into facebook and others enterprises like twitter in the BIGDATA and data science.   
A database can have huge storage, it's not fixed. That's why there are DBMS (Database Management System) such as MySQL, Oracle, etc. These software allow you to define, insert, delete and consult the data.
## DBMS
* Definition - Specify types, structures and restrictions of stored data (Metadata).
* Construction - Process of storing data in some medium(meio).
* Handling - Query functions, retrieve data, update data and delete data.
* Sharing - Allow users and programs to access the database simultaneously.  
![Database](../img/database.png)
![Database](../img/implementDB.png)  
## Entity
Entity is something or someone that represents something in real life  
Every entity has simple or compound attributes  
![Entity](../img/EntidadeAtributos.png)  
![Atributtes](../img/atributos.png) 
![NULL](../img/atributonull.png)
![Ilustration](../img/representation.png)  
### **Um atributo chave é um atributo único para cada entidade, EX: Não é possível ter 2 carros com o mesmo registro**
A combinação do estado e do número formam um único registro para cada carro exclusivamente
![Relationships](../img/relacionamentos.png)  
The relationship between 2 entities is represented by a diamond
![Relationships ilustration](../img/relacionamentoilustração.png)
Recursive relationship 
![Recursive Relationship](../img/relacionamentorecursivo.png)
Cardinalidade tem haver com por exemplo quantos funcionários podem trabalhar para um departamento, aí a cardinalidade é definida em razões: 1:N, N:1, 1:1, N:M. Onde o N no exemplo seria o funcionário e M representa o departamento.
![Cardinalidade](../img/cardinalidade.png)
A cardinalidade mínima é quando uma entidade depende da outra para existir naquele mini-mundo, exemplo: Em uma empresa onde todo funcionário precisa estar em um departamento, o funcionário só existe caso esteja vinculado a algum departamento e o departamento só existe se houver funcionários nele.  
![Cardinalidade Mínima](../img/CardinalidadeMinima.png)  
Quando a linha é dupla significa que a cardinalidade é total, pois uma entidade não existe sem a outra.  
![Cardinalidade Parcial](../img/CardinalidadeParcial.png)
Uma cardinalidade é parcial quando uma entidade não depende da outra para existir. No exemplo o funcionário não necessita do projeto para existir, no entanto o projeto precisa de funcionário gerente para existir.
![Outra Maneira de ilustrar](../img/OutroManeira.png)  
Aí está uma outra maneira de ilustrar as mesmas relações de cardinalidade apresentadas acima. Perceba que entre parênteses quando o primeiro número é 0 significa que a cardinalidade é parcial, ou seja, não precisa da outra entidade para existir, mas perceba também que não é algo mútuo, pois a outra entidade tem como o primeiro número o 1 e por isso a cardinalidade é total, logo necessita da outra entidade para existir.  
Essa forma de ilustrar é melhor e mais utilizada.
![Relação funcionário projeto](../img/RelacaoFuncionarioProjeto.png)  
![Relação funcionário departamento](../img/RelacaoFuncionarioDepartamento.png)
![Atributo de Relacionamento](../img/AtributoRelacionamento.png)
![Migrando atributos](../img/Migrando.png)
![Regras para Migrar](../img/RegrasMigracao.png)
Em relacionamentos N:M não é possível migrar.  
![Relação entre entidades fracas e fortes](../img/RelacaoEntidadesFracaseFortes.png)
A relação entre entidades fracas e fortes é feita por um losango duplo.  
Com isso sublinhamos com pontilhado o atributo que faz essa relação ser única.
![Nomenclatura](../img/Nomenclatura.png)  
![Nomenclatura](../img/Nomenclatura2.png)  
