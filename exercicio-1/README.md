# Exercicio 1 - Meu primeiro container

## Introdução

Docker é uma ferramenta maravilhosa para gerenciar ambientes heterogêneos com diferentes dependendências e diferentes necessidades de alocação, além de permitir a otimização de recursos.

Nesse primeiro exercício, iremos estudar como buscar uma imagem de um container, criá-lo, vê-lo em execução e depois removê-lo. Para isso, siga os passos e tente compreender cada ocorrência do terminal. Haverá algumas perguntas ao longo dos exercícios com as respostas ocultas logo em seguida, portanto tente pensar a resposta à pergunta primeiro e apenas depois tente ver a resposta descrita.

>PERGUNTA: Você entendeu a parte anterior?
><details>
>  <summary> Resposta </summary>
>  Sim, entendi! Devo pensar minha resposta antes de clicar em "Resposta" após a pergunta para poder comparar o que eu pensei com a resposta correta. É sempre melhor >aprender ativamente do que passivamente!
></details>

Tendo isso em mente, vamos ao que interessa! Para começarmos o exercício, eu estou assumindo que você já fez as configurações básicas na tela principal do repositório e que já está com o seu docker instalado e seu serviço rodando. Caso não tenha feito, volte para a [tela principal do repositorio](/README.md) e faça os passos!

## Baixando minha primeira imagem

Iremos começar baixando uma imagem bem simples: um servidor web HTTP para hospedar uma página de teste. Todas as imagens devem ser baixadas de um repositório de imagens, seja ele um repositório público como o [DockerHub](https://hub.docker.com) ou seja ele um servidor Registry privativo de uma organização. Para esse caso, iremos baixar uma imagem que já está no DockerHub e que é mantida pela organização que a criou: o [httpd](https://hub.docker.com/_/httpd). Assim como o GitHub, o DockerHub é um repositório público em que qualquer pessoa pode subir qualquer imagem, então muito cuidado com as imagens que for baixar para não ganhar um presente de grego!! Priorize sempre baixar imagens dos mantenedores originais e que tenham boa documentação descrita no DockerHub!

Para fazer o download de uma imagem na nossa máquina virtual, iremos usar o comando abaixo:
> docker pull httpd

após fazer o download, tente responder à pergunta:

> PERGUNTA: o que significa as linhas de retorno geradas pelo comando? e o conteúdo enigmático na "digest"?
> <details>
  >  <summary> Resposta </summary>
  > Using default tag: latest significa que por padrão, estamos baixando a última versão disponível dessa imagem no nosso repositório. Poderíamos especificar outras versões, como por exemplo "docker pull httpd:v2.4.53". Isso garante que tenhamos exatamente o ambiente que queremos para nossa aplicação. <br>
  > latest: Pulling from library/httpd: indica qual repositório e imagem está sendo puxada do dockerhub <br>
  > 42c077c10790: Pull complete (e outras parecidas): essas são as camadas (layers) que compoem nossa imagem, iremos entender melhor essas camadas em exercícios posteriores! <br>
  > Digest: sha256:f899....: esse é o Hash da nossa imagem baixada, ele identifica unicamente essa imagem no nosso sistema. <br>
</details>

Para verificarmos as imagens que temos disponíveis no nosso sistema, podemos rodar o comando:

> docker images

ele irá mostrar um resumo das imagens que temos, demonstrando também as tags a que essas imagens se referem e um ID dessa imagem, quando ela foi criada (ou seja, foi feito seu Build) e o tamanho que ela ocupa em disco. 

## Criando meu primeiro container

Como temos a imagem _httpd_ disponível localmente no nosso ambiente, agora podemos _criar_ um container para nós utilizando o comando abaixo:

> docker create --name apache2-teste httpd

Nesse comando, estamos criando um container no nosso ambiente com o nome "apache2-teste" e utilizando a imagem _httpd:latest_ (sendo a tag "_latest_" assumida como padrão se não for especificada uma tag). O nome é apenas uma forma mais fácil de identificarmos o nosso container à frente. Caso não tivéssemos especificado um nome, o Docker iria gerar um nome aleatório para ele, geralmente algo composto por Adjetivo_Substantivo ("trusty_tahr", "bionic_beaver", "xenial_xerus" são exemplos). O retorno do comando é o ID único do container criado

Depois de criado, tente rodar o comando abaixo:

> docker ps

Esse comando deveria listar os containers do nosso ambiente, mas ele não mostrou o que acabamos de criar. Isso porquê por padrão esse comando mostra apenas os containers que estejam no estado "Running" (em execução). Nós apenas criamos nosso container, mas não o iniciamos. Para listar todos os containers, mesmos os que não estão em execução, tente rodar o comando abaixo:

> docker ps -a

Agora sim temos nosso container listado, não é? Para iniciarmos nosso container, rode o comando abaixo:

> docker start apache2-teste

Se rodarmos novamente o "docker ps", agora veremos o nosso container com estado "Up"!

> PERGUNTA: Tente rodar o comando "wget 127.0.0.1" para baixar a página "index.html" padrão do container. O que aconteceu?
> <details>
  >  <summary> Resposta </summary>
  >  O wget retornou uma mensagem de conexão recusada, pois não há um servidor web disponível na porta 80 para atender nossa solicitação. Contraditório, não é? Quando rodamos o "docker ps", verificamos que há uma informação da porta "80/TCP" na linha do nosso container, mas na verdade essa informação é apenas uma documentação indicando que aquele container utiliza essa porta. Veremos mais à frente sobre como expor essa porta do container para ser acessível pelo nosso host e outros computadores da rede!
</details>

De forma intuitiva, se com o _start_ nós iniciamos um container, então com um _stop_ nós paramos um container. Faça a parada do container e depois verifique o resultado com o "ps -a":

> docker stop apache2-teste <br>
> docker ps -a

Feito isso, nós também podemos remover o nosso container para liberar o espaço ocupado por ele (muito menos espaço do que você imagina) com o comando abaixo:

> docker rm apache2-teste

Com isso nós já aprendemos a _listar_, _criar_, _iniciar_, _parar_ e _remover_ nossos containers!

## Expondo meu container para o mundo

Um container que roda de maneira isolada nem sempre é muito útil, principalmente se esse container tiver a função de prestar um serviço para clientes, não é mesmo? Quando criamos nosso container anteriormente, não conseguimos fazer o download a página padrão index.html do httpd para confirmarmos que nosso serviço estava rodando, pois ficou faltando uma configuração a ser feita: publicarmos a porta 80 (HTTP) do nosso container para ser acessível.

Antes de fazermos a publicação, se você tiver um pouco de afinidade com o Firewall IPTables, esse é um recurso interessante a ser visualizado:

> sudo iptables -S DOCKER

Esse comando irá exibir as entradas da Chain DOCKER do IPtables em formato de comandos IPTables. Nessa primeira execução, provavelmente você verá apenas a linha "-N DOCKER" indicando que é o início da Chain na travessia do Firewall. Nós usaremos esse comando novamente para vermos as alterações que o Docker faz para expor um container.

Para expor um container, nós devemos adicionar um parâmetro no momento da criação do nosso container: o _-p host:container_, ou _--publish host:container_ na notação extendida. Esse comando serve para mapear uma porta do nosso _host_ em uma porta do _container_. Por exemplo, vamos supor que a porta 80 do nosso host onde o Docker está instalado já está com outro serviço rodando nela, portanto não poderíamos utilizá-la. Ao invés disso, poderíamos utilizar outra porta, como por exemplo a _8080_ para acessar a porta _80_ dentro do container, utilizando o parâmetro _-p 8080:80_. Por enquanto, ligaremos a porta 80 do nosso host na porta 80 do container, portanto usaremos o seguinte comando:

> docker run -d --name apache2-teste -p 80:80 httpd

Ué, mas pera aí, "Docker Run"? Sim, o comando "docker run" é um canivete suíço para facilitar a criação de containers: Ele faz o _pull_ da imagem caso não a tenhamos localmente, além de fazer a _criação_ e o _início_ do container. os parâmetros _--name_ e _-p_ foram explicados anteriormente, já o parâmetro _-d_ serve para indicarmos que o container deve ser rodado de forma _detached_, isso é, ele irá rodar em segundo plano. Se não indicarmos o parâmetro _-d_, o container será iniciado e o processo irá tomar o terminal para exibir os logs de execução, aí precisaremos usar o _CRTL+C_ para sair de sua execução.

> PERGUNTA: Tente rodar o comando "iptables -S DOCKER" novamente. O que apareceu de diferente?
> <details>
  >  <summary> Resposta </summary>
  >  Foi adicionada uma entrada na Chain DOCKER que faz a exposição da porta 80 do container (com IP alocado em 172.2.0.0/24) para a _bridge virtual_ (docker0) criada pelo serviço do Docker.
</details>

<br>

> PERGUNTA: Tente rodar o comando "wget 127.0.0.1" para baixar a página "index.html" padrão do container. O que aconteceu?
> <details>
  >  <summary> Resposta </summary>
  >  O wget baixou o conteúdo do index.html do nosso container. Agora sabemos que ele está funcionando e está alcançável!
</details>

## Executando comandos e explorando um container