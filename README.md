# Repositório com tutoriais para ensino do Docker

Esse repositório tem como objetivo auxiliar aos que querem iniciar os estudos com Containers, especialmente com a ferramenta "Docker".

O repositório é organizado em exercícios que vão abordar alguns aspectos do gerenciamento de containers, desde as funções básicas, até a orquestração em um ambiente de cluster Swarm (PENDENTE).

Os exercícios são descritos conforma abaixo:

* Exercício 1 - Criando um container
* Exercício 2 - Trabalhando com Volumes
* Exercício 3 - Trabalhando com Redes
* Exercício 4 - Criando sua própria imagem (Dockerfile)
* Exercício 5 - Orquestrando Containers em conjuntos com Docker-Compose
* Exercício 6 - Configurando um Cluster Docker Swarm mínimo

# Preparando o ambiente (Getting Started)

Para este repositório, assume-se um ambiente com Ubuntu Server 20.04.3 atualizado até a data atual (16/06/2022) e com os pacotes _docker.io_ e _docker-compose_ instalados. Para o ambiente, o estudante pode fazer a própria instalação em quaisquer virtualizador, cloud ou bare-metal que quiser, desde que tenha acesso ao terminal como Root, que não haja outros serviços pré-existentes no ambiente e que tenha acesso à internet com resolução de nomes DNS. Opcionalmente, também é possível baixar essa [Appliance Virtualbox](https://drive.google.com/u/0/uc?id=1w8l6t-55DKcYhg-cA9N1fcJFtyngiXAb&export=download) e utilizar o usuário "aluno" com senha "ifsp".

Após a instalação do S.O. ou importação da appliance, é necessário instalar os pacotes básicos para utilizar o docker e dar permissão ao usuário "aluno" para conseguir interagir com o socket do serviço:

> sudo apt update && sudo apt install docker.io docker-compose <br>
> sudo adduser aluno docker

Após executados esses comandos, reinicie o terminal (rode o comando "logout" e refaça o login) para atualizar as permissões do usuário e rode o comando "systemctl status docker", ele deve retornar uma linha verde como "Active (running)"

Feito isso, o ambiente já está preparado para iniciar os tutoriais.