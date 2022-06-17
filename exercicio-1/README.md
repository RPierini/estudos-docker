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
