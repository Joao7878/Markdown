# SOLID
## S
**S => SRP (SINGLE RESPONSABILITY PRINCIPLE) [Princípio da Responsabilidade Única]**  

**O conceito de SRP define que uma classe deve ter um, e somente um, motivo para mudar.**  

**Uma classe deve ter SOMENTE UMA FINALIDADE**  

As classes não podem ser classes Deus, ou seja, classes que sabem demais ou fazem demais. Não se limite somente a classes, mas também para funções e afins. Exemplo de SRP:

```javascript
class UserRepository{
  create()
  read()
  update()
  delete()
}
class UserSchema{
  name:String,
  age: Number,
  createdAt: new Date()
}
```

Perceba que as classes não fazem tudo, cada uma faz o seu papel.  
**OBS: SE APLICA PARA FUNÇÃO TAMBÉM**

## O
**O=> OCP (OPEN-CLOSED-PRINCIPLE) [Princípio aberto-fechado]**

**Objetos ou entidades devem estar abertos para extensão, mas fechados para modificação**  

Exemplo errado:
```php
class ContratoClt
{
    public function salario()
    {
        //...
    }
}

class Estagio
{
    public function bolsaAuxilio()
    {
        //...
    }
}

class FolhaDePagamento
{
    protected $saldo;
    
    public function calcular($funcionario)
    {
        if ( $funcionario instanceof ContratoClt ) {
            $this->saldo = $funcionario->salario();
        } else if ( $funcionario instanceof Estagio) {
            $this->saldo = $funcionario->bolsaAuxilio();
        }
    }
}

```

A classe FolhaDePagamento precisa verificar o funcionário para aplicar a regra de negócio correta na hora do pagamento. Supondo que a empresa cresceu e resolveu trabalhar com funcionários PJ, obviamente seria necessário modificar essa classe! Sendo assim, estaríamos quebrando o princípio Open-Closed do SOLID.  

Exemplo correto:

```php
interface Remuneravel
{
    public function remuneracao();
}

class ContratoClt implements Remuneravel
{
    public function remuneracao()
    {
        //...
    }
}

class Estagio implements Remuneravel
{
    public function remuneracao()
    {
        //...
    }
}

class FolhaDePagamento
{
    protected $saldo;
    
    public function calcular(Remuneravel $funcionario)
    {
        $this->saldo = $funcionario->remuneracao();
    }
}
```

## L
**L => (LISKOV SUBSTITUTION PRINCIPLE) [Princípio da substituição de liskov]**  

**Uma classe derivada deve ser substituível por sua classe base**

Exemplo errado:

```php
# - Sobrescrevendo um método que não faz nada...
class Voluntario extends ContratoDeTrabalho
{
    public function remuneracao()
    {
        // não faz nada
    }
}


# - Lançando uma exceção inesperada...
class MusicPlay
{
    public function play($file)
    {
        // toca a música   
    }
}

class Mp3MusicPlay extends MusicPlay
{
    public function play($file)
    {
        if (pathinfo($file, PATHINFO_EXTENSION) !== 'mp3') {
            throw new Exception;
        }
        
        // toca a música
    }
}


# - Retornando valores de tipos diferentes...
class Auth
{
    public function checkCredentials($login, $password)
    {
        // faz alguma coisa
        
        return true;
    }
}

class AuthApi extends Auth
{
    public function checkCredentials($login, $password)
    {
        // faz alguma coisa
        
        return ['auth' => true, 'status' => 200];
    }
}

```

Exemplo certo:

```php
class A 
{
    public function getNome()
    {
        echo 'Meu nome é A';
    }
}

class B extends A 
{ 
    public function getNome()
    {
        echo 'Meu nome é B';
    }
}

$objeto1 = new A;
$objeto2 = new B;

function imprimeNome(A $objeto)
{
    return $objeto->getNome();
}

imprimeNome($objeto1); // Meu nome é A
imprimeNome($objeto2); // Meu nome é B
```

## I
**I => (Interface Segregation Principle)[Princípio da Segregação da interface]**  

**Uma classe não deve ser forçada a implementar interfaces e métodos que não irá utilizar**  
Esse princípio basicamente diz que é melhor criar interfaces mais específicas ao invés de termos uma única interface genérica.  

Exemplo errado:
```php
interface Aves
{
    public function setLocalizacao($longitude, $latitude);
    public function setAltitude($altitude);
    public function renderizar();
}

class Papagaio implements Aves
{
    public function setLocalizacao($longitude, $latitude)
    {
        //Faz alguma coisa
    }
    
    public function setAltitude($altitude)
    {
        //Faz alguma coisa   
    }
    
    public function renderizar()
    {
        //Faz alguma coisa
    }
}

class Pinguim implements Aves
{
    public function setLocalizacao($longitude, $latitude)
    {
        //Faz alguma coisa
    }
    
    // A Interface Aves está forçando a Classe Pinguim a implementar esse método.
    // Isso viola o príncipio ISP
    public function setAltitude($altitude)
    {
        //Não faz nada...  Pinguins são aves que não voam!
    }
    
    public function renderizar()
    {
        //Faz alguma coisa
    }
}

```

Exemplo certo:

```php
interface Aves
{
    public function setLocalizacao($longitude, $latitude);
    public function renderizar();
}

interface AvesQueVoam extends Aves
{
    public function setAltitude($altitude);
}

class Papagaio implements AvesQueVoam
{
    public function setLocalizacao($longitude, $latitude)
    {
        //Faz alguma coisa
    }
    
    public function setAltitude($altitude)
    {
        //Faz alguma coisa   
    }
    
    public function renderizar()
    {
        //Faz alguma coisa
    }
}

class Pinguim implements Aves
{
    public function setLocalizacao($longitude, $latitude)
    {
        //Faz alguma coisa
    }
    
    public function renderizar()
    {
        //Faz alguma coisa
    }
}
```

## D
**D => DIP (Dependency Inversion Principle) [Princípio da inversão de dependência]**


**Dependa de abstrações e não de implementações**  

**NÃO É INJEÇÃO DE DEPENDÊNCIA**  

Exemplo errado:
```php
use MySQLConnection;

class PasswordReminder
{
    private $dbConnection;
    
    public function __construct()
    {       
        $this->dbConnection = new MySQLConnection();           
    }
    
    // Faz alguma coisa
}
```

Para recuperar a senha, a classe PasswordReminder, precisa conectar na base de dados, por tanto, ela cria um instancia da classe MySQLConnection em seu método construtor para realizar as respectivas operações.
  
Nesse pequeno trecho de código temos um alto nível de acoplamento, isso acontece porque a classe PasswordReminder tem a responsabilidade de criar uma instância da classe MySQLConnection! Se quiséssemos reaproveitar essa classe em outro sistema, teriamos obrigatoriamente de levar a classe MySQLConnection junto, portanto, temos um forte acoplamento aqui.  

Por que nosso exemplo refatorado viola o Dependency Inversion Principle?
Porque estamos dependendo de uma implementação e não de uma abstração, simples assim.
De acordo com a definição do DIP, um módulo de alto nível não deve depender de módulos de baixo nível, ambos devem depender da abstração. Então, a primeira coisa que precisamos fazer é identificar no nosso código qual é o módulo de alto nível e qual é o módulo de baixo nível. Módulo de alto nível é um módulo que depende de outros módulos.


Exemplo certo, mas desrespeita outros princípios do SOLID:

```php
use MySQLConnection;

class PasswordReminder
{
    private $dbConnection;
    
    public function __construct(MySQLConnection $dbConnection)
    {       
        $this->dbConnection = $dbConnection;           
    }
    
    // Faz alguma coisa
}
```

Exemplo que respeita todo o SOLID:

```php
interface DBConnectionInterface
{
    public function connect();
}


class MySQLConnection implements DBConnectionInterface
{
    public function connect()
    {
        // ...
    }
}

class OracleConnection implements DBConnectionInterface
{
    public function connect()
    {
        // ...
    }
}

class PasswordReminder
{
    private $dbConnection;

    public function __construct(DBConnectionInterface $dbConnection) {
        $this->dbConnection = $dbConnection;
    }
  
    // Faz alguma coisa
}
```