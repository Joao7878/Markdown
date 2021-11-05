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