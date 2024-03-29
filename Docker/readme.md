# Docker
## O que é o docker?
O Docker é uma ferramenta de criação de containers.  
### Mas o que é um container?
Container é um ambiente isolado dentro da nossa máquina.  
Com o container temos as nossas imagens que são instruções para criação de um container  

O Docker veio para resolver o famoso problema: Ahh na minha máquina funciona!!  
Ou seja, o Docker basicamente faz com que o que roda localmente também rode em produção  
O Docker permite que usemos o mesmo SO dentre os containers, portanto podemos usar os mesmos recursos que a máquina host   
![Docker](../img/Docker.png)  
## Aplicando o Docker
Podemo rodar nossa aplicação dentro do Docker, é bom rodarmos na nossa máquina, mas com o Docker não precisamos de muita coisa instalada para rodar a aplicação, portanto qualquer máquina pode rodar.  
Para rodar nossa aplicação dentro do Docker, iremos criar um Dockerfile na raiz do nosso projeto e dentro dele terá todo o nosso passo a passo que precisamos para rodar o nosso projeto.  
Na primeira linha iremos passar a imagem que iremos passar para esse container, no nosso caso é a imagem do node.  
O nosso código está assim:  
![Docker code](../img/dockerCode.png)  
Apenas troque a linha 3 a parte do ./usr/app por ./
- A primeira linha de código está relacionada a imagem que iremos passar para o container,   
- A segunda linha tem o WORKDIR que representa a pasta em que serão armazenadas as informações assim que o docker rodar o programa.
- Logo apoś vem o comando COPY que estará copiando o arquivi package.json para o nosso diretório de trabalho do docker
- Após copiar o arquivo para dentro do diretório, iremos instalar as dependências do package.json no diretório
- Na linha 5 mandamos ele copiar tudo do projeto, exceto os itens ignorados pelo dockerignore, para dentro do diretório.
- Agora com o EXPOSE iremos expor a porta que estamos utilizando dentro do nosso container
- E por fim iremos passar o cmd para rodar o script, por padrão é separado dentro de array, então iremos passar o npm com o comando pra rodar(run) e o dev que é o mesmo que yarn dev que utilizamos sempre

Esse é o nosso dockerignore:  
![Docker ignore](../img/dockerIgnore.png)  
Agora iremos criar a nossa imagem com o docker build -t nomedaimagem .  
O ponto é para dizer onde está o nosso dockerfile, no caso está na raiz do projeto  
Vamos ver agora os containers que estã rodando com o docker ps, porém não aparece o container que criamos, pois precisamos agora rodar ele de fato, nós apenas criamos ele com o build.  
Para rodar iremos utilizar o comando:

```console
docker run -p 3000:3000 carprjt
```

carprjt é o nome do nosso container  
Docker run -p(para mapear as portas) 3000:3000(quer dizer que toda vez que no localhost, for chamada a porta 3000, queremos que dentro do docker ele procure pela porta 3000 que é a porta do nosso container) carprjt.  
Para confirmar que estamos realmente utilizando o docker podemos o comando docker ps e pegar o nome do container ou o id, com o nome ou o id podemos verificar se está rodando realmente com o comando:

mystifying_bhabha é o nome do nosso container que foi dado pelo docker

```console
docker exec -it mystifying_bhabha /bin/bash
```

Com isso iremos ter acesso ao terminal do nosso container, se quisermos verificar os nossos arquivos podemos digitar ls e iremos ver no terminal.

## Docker-compose
O Docker-compose é um orquestrador de container  
Para colocar em prática o docker compose iremos criar um arquivo docker-compose.yml  
Lembrando que na instalação do docker já instalamos também o docker-compose, para linux é necessário instalar separado, já para windows e mac quando se instala o docker já se instala também o docker-compose  

**Até agora tinhamos 2 problemas, precisávamos escrever docker kill nomeDoContainer para parar o docker e o docker não atualizava junto com a nossa aplicação.  
O Docker-compose resolve esses nossos problemas!!!**  
Precisamos somente digitar docker-compose up no terminal para rodar a aplicação no docker e se quisermos parar basta apertar ctrl+c no terminal.  
Caso queiramos que ele funcione no background, basta digitar docker-compose up -d  
Para verificar se está funcionando basta digitar docker logs nomeDoContainer  -f  
![Docker-compose](../img/docker-compose.png)  
A versão é a versão que está disponível para nós no momento, os serviços são de fato todos os serviços que utilizaremos na nossa aplicação, então iremos colocar todos dentro dele, por enquanto temos apenas o nosso app que contém a nossa aplicação, mas ainda vamos adicionar o banco de dados que iremos utilizar, a nossa build é toda a nossa aplicação por isso contém o ponto, as portas estão a porta do host e do docker e os volumes primeiramente tem o . que significa toda nossa aplicação exceto o dockerignore e o que está dentro dele e o (:) significa passar para no caso para o diretório, então se lê: passar tudo dessa pasta para o diretório tal  
## Comandos Docker
- docker ps -> Mostra todos os containers rodando
- - docker ps -a -> Mostra todos os containers(inclusive os que estão off)
- docker rm (id ou nome)(container tem que estar parado) -> para remover o container
- docker run -> para criar e iniciar um container
- docker start (id ou nome) -> para iniciar
- docker stop (id ou nome) -> para parar o container
- docker exec -it (id ou nome) /bin/bash -> para acessar o terminal do container
- - ctrl+d para sair
- docker logs (nome ou id) -> mostra os logs
- - docker logs (nome ou id) -f -> para ficar assistindo os logs

Existem outros
- ## Comandos Docker-compose
- docker-compose up -> para criar o docker-compose
- - docker-compose up -d -> para criar no background
- docker-compose start -> para rodar o container
- docker-compose stop -> para o container
- docker-compose down -> remove tudo de dentro container

## Pontos sobre o Docker
- Não conseguimos modificar imagens, elas são read-only, mas temos uma camada de escrita que chamamos de layer onde podemos acessar o container e utilizar ele.  
- Containers são voláteis, iremos criar, remover, iniciar e parar containers a todo momento.
- Para salvar os dados do container e não perdemos os dados da aplicação, iremos utilizar os volumes.
- Os volumes ficam salvos dentro do docker host e não do container, assim não perdemos os dados da aplicação.
## Imagens
As imagens do docker hub são baseadas em versões e quando você cria uma imagem, ela é baseada em uma versão. Por padrão a versão é sempre a mais atualizada, mas se quisermos utilizar uma versão antiga, basta utilizar o comando:  
docker pull nomeDaImagem:versaoAntiga ex:  
docker pull nomeDaImagem:1.0.0
## Inputs
Se por acaso rodarmos uma aplicação que tenha um input no console, por padrão o docker não está configurado para aceitar inputs, então temos que configurar o docker para aceitar inputs. Na hora de rodarmos o container basta utilizar a tag -i para poder digitar no terminal e a tag -t para poder ver o que está sendo digitado e perguntado, podemos utilizar de forma mais fácil -it.  
## Volumes
Quando rodamos um container no docker, os dados que são colocados na execução são perdidos após o container ser parado. Para resolver isso, existem os volumes que pegam os dados e os salvam no docker host, então quando o container é parado, os dados são salvos no host, e quando o container é iniciado, os dados são recuperados do host e colocados no container.  
Isso também nos dá a possibilidade de compartilhar dados entre containers differentes, por exemplo, se quisermos compartilhar um banco de dados entre os containers, podemos utilizar volumes.
## Portas