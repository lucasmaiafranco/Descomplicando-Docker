# Descomplicando-Docker

## Sumário

<!-- TOC -->
- [Descomplicando-Docker](#descomplicando-Docker)
  - [Sumário](#sumário)
  - [Day 01](#day-01)
	  - [Comandos Vagrant](#comandos-vagrant)
		- [Para subir a VM](#para-subir-a-vm)
		- [Verficar Status da VM](#verificar-status-da-vm)
		- [Conectar SSH](#conectar-ssh)
	  - [Instalação do Docker](#instalação-do-docker)
		- [Instalação via Vagrantfile](#instalação-via-vagrantfile)
	  - [Comandos Docker](#comandos-docker)
		- [Versão](#versão)
		- [Visualizar containers em execução](#visualizar-containers-em-execução)
		- [Visualizar todos containers](#visualizar-todos-containers)
		- [Primeiro container](#primeiro-container)
		- [Criando containers](#criando-containers)
		- [Sair do container sem matar ele](#sair-do-container-sem-matar-ele)
		- [Conectar no container novamente](#conectar-no-container-novamente)
		- [Executar container como daemon](#executar-container-como-daemon)
		- [Executar um comando dentro do container](#executar-um-comando-dentro-do-container)
		- [start/stop/restart container](#startstoprestart-container)
		- [pause/unpause container](#pauseunpause-container)
		- [Informações do container](#informações-do-container)
		- [Log do container](#log-do-container)
		- [Remover container](#remover-container)
		- [Remover todos conatiners que estão stopados](#remover-todos-conatiners-que-estão-stopados)
		- [Ver o consumo de CPU e memória do container](#ver-o-consumo-de-cpu-e-memória-do-container)
		- [Ver os processos em execução no container](#ver-os-processos-em-execução-no-container)
		- [Criando container com o tamanho da memória](#criando-container-com-o-tamanho-da-memória)
		- [Criando container com o tamanho da memória e CPU](#criando-container-com-o-tamanho-da-memória-e-cpu)
		- [Atualizar o container - Aumentar memória, CPU, etc](#atualizar-o-container-aumentar-memória-cpu-etc)
		- [Consultar apenas o ID do container](#consultar-apenas-o-id-do-container)
		- [Remover todos containers pelo ID](#remover-todos-containers-pelo-id)
	- [Primeiro Dockerfile](#primeiro-dockerfile)
		- [Construir uma imagem através do Dockerfile](#construir-uma-imagem-através-do-dockerfile)
		- [Listar as imagens](#listar-as-imagens)
		- [Criando container com a nossa imagem](#criando-container-com-a-nossa-imagem)
  - [Day 02](#day-02)
  	- [Volumes](#volumes)
  		- [Bind](#bind)
  			- [Criando container com volume bind](#criando-container-com-volume-bind)
  			- [Testes realizados](#testes-realizados)
  				- [Criamos um diretório](#criamos-um-diretório)
  				- [Criamos um container passando o diretório criado](#criamos-um-container-passando-o-diretório-criado)
  				- [Dentro do container criamos um arquivo no diretório](#dentro-do-container-criamos-um-arquivo-no-diretório)
  				- [Saindo do container](#saindo-do-container)
  				- [Verificamos o conteúdo do arquivo criado dentro do container](#verificamos-o-conteúdo-do-arquivo-criado-dentro-do-container)
  		- [Volumes](#volumes)
  			- [Listar os volumes](#listar-os-volumes)
  			- [Criando volume](#criando-volume)
  			- [Verificar as config do volume](#verificar-as-config-do-volume)
  			- [Diretório do volume](#diretório-do-volume)
  			- [Criando container utilizando o volume criado](#criando-container-utilizando-o-volume-criado)
  			- [Deletar todos volumes que não estão sendo utilizados, por nenhum container](#deletar-todos-volumes-que-não-estão-sendo-utilizados-por-nenhum-container)
  			- [Testes realizados](#testes-realizados)
  				- [Criando um volume chamado fusca](#criando-um-volume-chamado-fusca)
  				- [Criando um arquivo dentro do diretório do volume](#criando-um-arquivo-dentro-do-diretório-do-volume)
  				- [Criando um container debian passando o volume criado](#criando-um-container-debian-passando-o-volume-criado)
  				- [Dentro do container adicionamos o texto "Fusca Branco" no arquivo criado anteriormente fusca_77](#dentro-do-container-adicionamos-o-texto-fusca-branco-no-arquivo-criado-anteriormente-fusca_77)
  				- [Saindo do container](#saindo-do-container)
  				- [Verificando o conteúdo do arquivo](#verificando-o-conteúdo-do-arquivo)
  				- [Criando outro container centos e verificando o conteudo do mesmo arquivo fusca_77](#criando-outro-container-centos-e-verificando-o-conteudo-do-mesmo-arquivo-fusca_77)
  		- [Data-Only](#data-only)
  			- [Criando o container para criar o volume](#criando-o-container-para-criar-o-volume)
  			- [Criando novo container utilizando o volume](#criando-novo-container-utilizando-o-volume)
  			- [Criando outro container utilizando o mesmo volume](#criando-outro-container-utilizando-o-mesmo-volume)
  			- [Exemplo do data only](#exemplo-do-data-only)
  				- [Criando o volume](#criando-o-volume)
				- [Criando o primeiro container utilizando o volume](#criando-o-primeiro-container-utilizando-o-volume)
				- [Criando o segundo container utilizando o volume](#criando-o-segundo-container-utilizando-o-volume)
		- [Backup volume](#backup-volume)
	- [Dockerfile](#dockerfile)
		- [Criando um docker file apache](#criando-um-docker-file-apache)
		- [Build do dockerfile](#build-do-dockerfile)
		- [Nova imagem do apache](#nova-imagem-do-apache)
		- [Build novo dockerfile](#build-novo-dockerfile)
		- [Subindo container com a imagem criada](#subindo-container-com-a-imagem-criada)
		- [Novo Dockerfile](#novo-dockerfile)
		- [Build e novo container](#build-e-novo-container)
	- [MultiStage](#multistage)
		- [O que é?](#O-que-é?)
		- [Criando uma aplicação em go](#criando-uma-aplicação-em-go)
		- [Criando dockerfile passando a aplicação e go](#criando-dockerfile-passando-a-aplicação-e-go)
		- [Criando o container com a imagem meugo](#criando-o-container-com-a-imagem-meugo)
		- [Tamanho da imagem golang](#tamanh-da-imagem-golang)
		- [Utilizando multistage](#utilizando-multistage)
		- [Tamanho da imagem utilizando multistage](#tamanho-da-imagem-utilizando-multistage)
	- [Healthcheck](#healthcheck)
	- [Docker commit](#docker-commit)
		- [Alterando Tag de uma imagem](#alterando-tag-de-uma-imagem)
	- [Dockerhub](#dockerhub)
		- [Login no Dockerhub](#login-no-dockerhub)
		- [Subir imagem no Dockerhub](#subir-imagem-no-dockerhub)
		- [Baixar uma imagem do Dockerhub](#baixar-uma-imagem-do-dockerhub)
	- [Docker Registry](#docker-registry)
		- [Docker Registry local](#docker-registry-local)
		- [Tag para registry local](#tag-para-registry-local)
		- [Subir imagem para o registry local](#subir-imagem-para-o-registry-local)
		- [Container com a imagem do registry local](#container-com-a-imagem-do-registry-local)
		- [Visualizar imagens do registry local](#visualizar-imagens-do-registry-local)
 - [Day 03](#day-03)
	- [Docker-machine](#docker-machine)
		- [O que é](#o-que-é)
		- [Instalação](#instalação)
		- [Criando VM HOSTS](#driando-vm-hosts)
		- [Listar as VMs](#listar-as-vms)
		- [IP da VM](#ip-da-vm)
		- [SSH na VM](#ssh-na-VM)
		- [Stop/Start na VM](#stop/start-na-VM)
		- [Detalhes da VM](#detalhes-da-vm)
		- [Variaveis da VM](#variaveis-da-vm)
		- [Remover a VM](#Remover-a-vm)
	- [Docker Swarm](#docker-swarm)
		- [Manager](#manager)
		- [Worker](#worker)
		- [Teoria](#teoria)
		- [Prática](#pratica)
			- [Criando 3 VMs](#criando-3-vms)
			- [Iniciando o Swarm](#iniciando-o-swarm)
			- [Adicionando server02 e server03 no cluster](#adicionando-server02-e-server03-no-cluster)
			- [Node como manager](#node-como-manager)
			- [Node como worker](#node-como-worker)
			- [Remover server do cluster](#remover-server-do-cluster)
			- [Visualizar a chave](#visualizar-a-chave)
				- [Worker](#worker)
				- [Manager](#manager)
			- [Gerar novas chaves](#gerar-novas-chaves)
				- [Worker](#worker)
				- [Manager](#manager)
	- [Docker Service](#docker-service)
		- [Criando serviço](#criando-serviço)
		- [Listar Serviços](#listar-serviços)
		- [Listar os container](#listar-os-container)
		- [Aumentar ou diminuir quantidade de containers](#aumentar-ou-diminuir-quantidade-de-containers)
		- [Ver o log do service](#ver-o-log-do-service)
		- [Adicionar rede no serviço](#dicionar-rede-no-serviço)
	- [LAB](#LAB)
		- [Criando o cluster Swarm](#criando-o-cluster-Swarm)
		- [Adicionando server02 e server03 no cluster](#adicionando-server02-e-server03-no-cluster)
		- [Status do cluster](#status-do-cluster)
		- [GlusterFS](#glusterFS)
			- [Instalando GLusterFS](#instalando-glusterfs)
			- [Serviço GlusterFS](#serviço-glusterfs)
			- [Em uma linha](#em-uma-linha)
			- [Editando o arquivo hosts](#editando-o-arquivo-hosts)
			- [Criando uma pool de armazenamento confiável](#criando-uma-pool-de-armazenamento-confiável)
			- [Listando os nós](#listando-os-nós)
			- [Criando volume](#criando-volume)
			- [Ativando o Volume](#ativando-o-volume)
			- [Verificando o volume](#verificando-o-volume)
			- [Montando o volume](#montando-o-volume)
			- [Criando index.html](#criando-index.html)
		- [Criando Volume no docker](#criando-volume-no-docker)
		- [Criando link simbolico](#criando-link-simbolico)
		- [Criando serviço no docker](#criando-serviço-no-docker)
  - [Day 04](#day-04)
	- [Secrets](#secrets)
		- [Criando um secrets](#criando-um-secrets)
		- [Listando secrets](#listando-secrets)
		- [Removendo um secrets](#removendo-um-secrets)
		- [Usando secrets criado](#usando-secrets-criado)
		- [Diretório no container onde ficam os secret](#diretório-no-container-onde-ficam-os-secret)
		- [Secrets com target, permissão](#secrets-com-target,-permissão)
		- [Removendo secrets de um serviço](#removendo-secrets-de-um-serviço)
	- [Compose](#compose)
		- [Primeiro docker compose](#primeiro-docker-compose)
		- [Deploy do docker compose](#deploy-do-docker-compose)
		- [Removendo deploy](#removendo-deploy)
		- [Rodando Wordpress](#rodando-wordpress)
			- [Docker compose](#docker-compose)
			- [Deploy do wordpress](#deploy-do-wordpress)
			- [Listando os serviços criados](#listando-os-serviços-criados)
			- [Visualizando Logs](#visualizando-logs)
		- [Visualizer](#visualizer)
			- [docker compose](#docker-compose)
			- [Adicionando tag no node](#adicionando-tag-no-node)
			- [Deploy](#deploy)
		- [Exemplo Voting App](#exemplo-voting-app)
			- [Docker compose](#docker-compose)
			- [Deploy](#deploy)
			- [Listar stack](#listar-stack)

<!-- TOC -->

## Day 01

- Um pouco de teoria

- Criado VM Debian, utilizei o Vagrant com o vagrantfile deste repo

### Comandos Vagrant

#### Para subir a VM
	
	vagrant up
		
#### Verificar status da VM
		
	vagrant status (Deve estar no diretório que está o Vagrantfile)
	vagrant global-status (Lista todas VMs da sua máquina)
		
#### Conectar SSH
	
	vagrant ssh (deve estar no diretório que está o Vagrantfile)
	vagrant ssh <id da vm>
		
	
### Instalação do Docker

	apt install curl -y && curl -fsSL https://get.docker.com -o get-docker.sh && sudo sh ./get-docker.sh
	
#### Instalação via Vagrantfile

Para instalar o Docker ao subir a VM devemos adicionar a linha abaixo no Vagrantfile

	docker.vm.provision :shell, inline: "apt install curl -y && curl -fsSL https://get.docker.com -o get-docker.sh && sudo sh ./get-docker.sh"

Segue como ficou o Vagrantfile

	Vagrant.configure("2") do |config|

	  config.vm.define "docker" do |docker|
	    docker.vm.box = "debian/buster64"
		docker.vm.hostname = "docker-server"
		docker.vm.network "forwarded_port", guest: 80, host: 8000
		docker.vm.network "forwarded_port", guest: 8080, host: 8080
		docker.vm.provision :shell, inline: "apt install curl -y && curl -fsSL https://get.docker.com -o get-docker.sh && sudo sh ./get-docker.sh"
	  end
	end

### Comandos docker

#### Versão
	
	docker version
		
#### Visualizar containers em execução
	
	docker container ls
		
#### Visualizar todos containers
	
	docker container ls -a
		
#### Primeiro container
	
	docker container run -ti hello-world
		
	-ti = terminal e interatividade
		
#### Criando containers
	
	docker container run -ti ubuntu
		
	docker container run -ti centos
		
#### Sair do container sem matar ele
	
	CTRL + p + q
		
#### Conectar no container novamente
	
	docker container attach <container ID ou nome do container>
		
#### Executar container como daemon
	
	docker container -d nginx
		
#### Executar um comando dentro do container
	
	docker container exec -ti <container ID> <comando>
	docker container exec -ti 86cd7182695f ls /usr/share/nginx/html
	docker container exec -ti 86cd7182695f bash
		
#### start/stop/restart container
	
	docker container start 86cd7182695f
	docker container stop 86cd7182695f
	docker container restart 86cd7182695f
		
#### pause/unpause container
	
	docker container pause <container ID>
	docker container unpause <container ID>

#### Informações do container

	docker container inspect <container ID>
		
#### Log do container

	docker container logs -f <container ID>
		
#### Remover container

	docker container rm <container ID>
		
	docker container rm -f <container ID>
		
#### Remover todos conatiners que estão stopados
	
	docker container prune
		
#### Ver o consumo de CPU e memória do container
	
	docker container stats <container ID>
		
#### Ver os processos em execução no container

	docker container top <container ID>
		
#### Criando container com o tamanho da memória
	
	docker container run -d -m 128M nginx

#### Criando container com o tamanho da memória e CPU

	docker container run -d -m 128M --cpus 0.5 nginx
		
#### Atualizar o container aumentar memória, CPU, etc
	
	docker container update --memory 64M --cpus 0.4 nginx

#### Consultar apenas o ID do container

	docker container ls -q
	docker container ls -aq
		
#### Remover todos containers pelo ID

	docker container rm $(docker container ls -aq)

### Primeiro Dockerfile
		
	# Imagem que será usada
	FROM debian
	# Label cria uma info, metadata
	LABEL app="Giropops"
	# ENV criar uma variavel de ambiente no container
	ENV LUCAS="LINDAO"
	# Executa comando no momento do build, quando está criando a imagem
	RUN apt-get update && apt-get install -y stress && apt-get clean
	# Executa comando quando inicia o container
	CMD stress --cpu 1 --vm-bytes 64M --vm 1
		
#### Construir uma imagem através do Dockerfile
	
	docker image build -t toskeira:1.0 .
		
#### Listar as imagens
	
	docker images ls
		
#### Criando container com a nossa imagem
	
	docker container run -d toskeira:1.0
		
----------------------------------------------------------------------------------------------------------------------	
## Day 02

### Volumes

#### Bind

É quando ja temos um diretório e queremos montar esse diretório dentro do container
	
##### Criando container com volume bind
	
	mkdir /opt/teste
	
	docker container run -ti --mount type=bind,src=/opt/teste,dst=/teste debian
		
	src -> diretório que deseja usar no container
	dst -> diretório que será criado no container
		
##### Testes realizados

		
###### Criamos um diretório
	
	root@docker-server:/opt# mkdir /opt/teste
		
###### Criamos um container passando o diretório criado
	
	root@docker-server:/opt# docker container run -ti --mount type=bind,src=/opt/teste,dst=/teste debian
		
###### Dentro do container criamos um arquivo no diretório
	
	root@eebf87b86a27:/# touch /teste/teste.txt
	root@eebf87b86a27:/# echo "Isso e um teste" > /teste/teste.txt
		
###### Saindo do container

Com isso ele é finalizado

	root@eebf87b86a27:/# exit
	exit

###### Verificamos o conteúdo do arquivo criado dentro do container.
		
	root@docker-server:/opt# cat /opt/teste/teste.txt
	Isso e um teste
	root@docker-server:/opt#
		
#### Volumes

##### Listar os volumes
	
	docker volume ls
		
##### Criando volume
	
	docker volume create <nome do volume>
		
##### Verificar as config do volume
	
	docker volume inspect <nome do volume>
		
##### Diretório do volume 

Onde ficam os files
	
	/var/lib/docker/volumes/<nome do volume>/_data
		
##### Criando container utilizando o volume criado
	
	docker container run -ti --mount type=volume,src=<nome do volume>,dst=/teste debian

	src -> nome do volume
	dst -> diretório que será criado no container
	
##### Deletar todos volumes que não estão sendo utilizados, por nenhum container

	docker volume prune
		
#### Testes realizados

##### Criando um volume chamado fusca

	root@docker-server:/# docker volume create fusca

##### Criando um arquivo dentro do diretório do volume

	root@docker-server:/# touch /var/lib/docker/volumes/fusca/_data/fusca_77

##### Criando um container debian passando o volume criado

	root@docker-server:/# docker container run -ti --mount type=volume,src=fusca,dst=/fusca debian

##### Dentro do container adicionamos o texto "Fusca Branco" no arquivo criado anteriormente fusca_77

	root@8609465c807e:/# ls /fusca/fusca_77
	/fusca/fusca_77
	root@8609465c807e:/# echo "Fusca Branco" > /fusca/fusca_77	

##### Saindo do container 

Com isso ele é finalizado
	
	root@8609465c807e:/# exit
	exit
	root@docker-server:/# docker container ls
	CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

##### Verificando o conteúdo do arquivo

	root@docker-server:/# cat /var/lib/docker/volumes/fusca/_data/fusca_77
	Fusca Branco

##### Criando outro container centos e verificando o conteudo do mesmo arquivo fusca_77

	root@docker-server:/var/lib/docker/volumes/fusca/_data# docker container run -ti --mount type=volume,src=fusca,dst=/fusca centos
	[root@1d46daf87440 /]# cat /fusca/fusca_77
	Fusca Branco
	[root@1d46daf87440 /]# exit
	exit
		

#### Data Only 

Utilizado antigamente quando não tinha volume

##### Criando o container para criar o volume

	docker container create -v /data --name dbdados centos

##### Criando novo container utilizando o volume

	docker container run -d -p 5432:5432 --name pgsql1 --volumes-from dbdados -e POSTGRESQL_USER=docker -e POSTGRESQL_PASS=docker -e POSTGRESQL_DB=docker kamui/postgresql

##### Criando outro container utilizando o mesmo volume

	docker container run -d -p 5433:5432 --name pgsql2 --volumes-from dbdados -e POSTGRESQL_USER=docker -e POSTGRESQL_PASS=docker -e POSTGRESQL_DB=docker kamui/postgresql
		
		
##### Exemplo do data only

###### Criando o volume

	docker volume create dbdados

###### Criando o primeiro container utilizando o volume

	docker container run -d -p 5432:5432 --name pgsql1 --mount type=volume,src=dbdados,dst=/data -e POSTGRESQL_USER=docker -e POSTGRESQL_PASS=docker -e POSTGRESQL_DB=docker kamui/postgresql

###### Criando o segundo container utilizando o volume

	docker container run -d -p 5433:5432 --name pgsql2 --mount type=volume,src=dbdados,dst=/data -e POSTGRESQL_USER=docker -e POSTGRESQL_PASS=docker -e POSTGRESQL_DB=docker kamui/postgresql


#### Backup volume 

Utilizamos volume e volume bind no mesmo container

Criamos um container passando o volume (dbdados) que queremos fazer backup, o volume será montado no diretório /data. 
Passamos um diretório para salvar o backup (/opt/backup) e passamos o comando tar para empacotar o diretório /data

	docker container run -ti --mount type=volume,src=dbdados,dst=/data --mount type=bind,src=/opt/backup,dst=/backup debian tar -cvf /backup/bkp-banco.tar /data

### Dockerfile

#### Criando um docker file apache

	FROM debian

	RUN apt-get update && apt-get install -y apache2 && apt-get clean
	ENV APACHE_LOCK_DIR="/var/lock"
	ENV APACHE_PID_FILE="/var/run/apache2.pid"
	ENV APACHE_RUN_USER="www-data"
	ENV APACHE_RUN_GROUP="www-data"
	ENV APACHE_LOG_DIR="/var/log/apache2"

	LABEL description="Webserver"

	VOLUME /var/www/html
	EXPOSE 80
	
#### Build do dockerfile

	docker image build -t meuapache:1.0 .
	
#### Nova imagem do apache

Agora colocamos o serviço do apache como o principal da imagem

	FROM debian

	RUN apt-get update && apt-get install -y apache2 && apt-get clean
	ENV APACHE_LOCK_DIR="/var/lock"
	ENV APACHE_PID_FILE="/var/run/apache2.pid"
	ENV APACHE_RUN_USER="www-data"
	ENV APACHE_RUN_GROUP="www-data"
	ENV APACHE_LOG_DIR="/var/log/apache2"

	LABEL description="Webserver"

	VOLUME /var/www/html
	EXPOSE 80

	ENTRYPOINT ["/usr/sbin/apachectl"]
	CMD ["-D", "FOREGROUND"]
	
#### Build novo dockerfile

	docker image build -t meuapache:2.0 .
	
#### Subindo container com a imagem criada

	docker container run -d -p 8080:80 meuapache:2.0
	
#### Novo Dockerfile 

Agora copiamos o arquvio index.html para dentro do container

	FROM debian

	RUN apt-get update && apt-get install -y apache2 && apt-get clean
	ENV APACHE_LOCK_DIR="/var/lock"
	ENV APACHE_PID_FILE="/var/run/apache2.pid"
	ENV APACHE_RUN_USER="www-data"
	ENV APACHE_RUN_GROUP="www-data"
	ENV APACHE_LOG_DIR="/var/log/apache2"

	COPY index.html /var/www/html/

	LABEL description="Webserver"

	VOLUME /var/www/html
	EXPOSE 80

	ENTRYPOINT ["/usr/sbin/apachectl"]
	CMD ["-D", "FOREGROUND"]
	
#### Build e novo container

	docker image build -t meuapache:3.0 .
	docker container run -d -p 8080:80 meuapache:3.0

### MultiStage

#### O que é?

Com o uso de Multi Stage, temos a possibilidade de utilizar imagens diferentes entre o processo de build e o processo de execução do binário. O resultado disso é uma imagem significativamente menor, já que as camadas anteriores são ‘descartadas’, restando somente o binário para execução em uma imagem enxuta.

No exemplo abaixo criamos uma aplicação em golang e depois executamos o binario da aplicação em uma imagem alpine, que é muito mais leve.

#### Criando uma aplicação em go

	package main
	import "fmt"
	func main() {
		fmt.Println("TESTE DOCKER")
	}

#### Criando dockerfile passando a aplicação e go

	FROM golang

	WORKDIR /app
	ADD . /app
	RUN go build meu_go.go
	ENTRYPOINT ./meu_go

#### Criando o container com a imagem meugo

	docker container run -ti meugo:1.0

Segue a saida

	root@docker-server:~/dockerfile/4# docker container run -ti meugo:1.0
	TESTE DOCKER
	root@docker-server:~/dockerfile/4#

#### Tamanho da imagem golang

	root@docker-server:~/dockerfile/4# docker image ls
	REPOSITORY   TAG       IMAGE ID       CREATED              SIZE
	meugo        1.0       1d00cb196b71   49 seconds ago       864MB
	golang       latest    0821480a2b48   2 days ago           862MB

#### Utilizando multistage

Dockerfile

	FROM golang AS buildando

	WORKDIR /app
	ADD . /app
	RUN go build meu_go.go


	FROM alpine
	WORKDIR /giropops
	COPY --from=buildando /app/meu_go /giropops

	ENTRYPOINT ./meu_go

Podemos observar que possuimos dois FROM, onde o primeiro gera o binario e o segundo executa a aplicação.
Ele considera a imagem do ultimo FROM para criar a imagem.

#### Tamanho da imagem utilizando multistage

REPOSITORY   TAG       IMAGE ID       CREATED              SIZE
meugo        2.0       b5a9aff72fb7   About a minute ago   7.53MB
alpine       latest    021b3423115f   2 days ago           5.6MB

Podemos observar que a imagem anterior tinha 864MB e agora possuimos 7.53MB, praticamente 99% de redução de espaço.

### Healthcheck

O healthcheck é quando você fala para o Docker testar sua aplicação, se ela esta integra. Se o seu container com apache ou nginx estiver atingindo o limite máximo, ou não respondendo requisição, o Docker coloca o contêiner como unhealthy e tomar uma ação evasiva (no modo swarm, o Docker substitui os contêineres não íntegros ativando substituições).

Nesse caso colocamos para o Docker realizar um curl no endereço localhost a cada um minuto e caso falhe ele finaliza o container

	FROM debian

	RUN apt-get update && apt-get install -y apache2 curl
	ENV APACHE_LOCK_DIR="/var/lock"
	ENV APACHE_PID_FILE="/var/run/apache2.pid"
	ENV APACHE_RUN_USER="www-data"
	ENV APACHE_RUN_GROUP="www-data"
	ENV APACHE_LOG_DIR="/var/log/apache2"

	ADD index.html /var/www/html/
	HEALTHCHECK --interval=1m --timeout=3s \
		CMD curl -f http://localhost/ || exit 1

	LABEL description="Webserver"

	WORKDIR /var/www/html/

	VOLUME /var/www/html
	EXPOSE 80

	ENTRYPOINT ["/usr/sbin/apachectl"]
	CMD ["-D", "FOREGROUND"]

Segue o status do container com o check

	CONTAINER ID   IMAGE           COMMAND                  CREATED          STATUS                    PORTS     NAMES
	f5eb993dfca6   meuapache:1.0   "/usr/sbin/apachectl…"   16 minutes ago   Up 16 minutes (healthy)   80/tcp    confident_chandrasekhar

Segue o status após apresentar um erro

	CONTAINER ID   IMAGE           COMMAND                  CREATED          STATUS                      PORTS     NAMES
	f5eb993dfca6   meuapache:1.0   "/usr/sbin/apachectl…"   28 minutes ago   Up 28 minutes (unhealthy)   80/tcp    confident_chandrasekhar


### Docker commit

Utilizamos docker commit quando queremos criar uma imagem através de um container em execução

	docker commit -m "ubuntu com vim e curl" <id container>

#### Alterando Tag de uma imagem

	docker image tag <id da imagem> <nova tag>
	docker image tag 2c85450e4df9 ubuntu_vi_curl:1.0

### Dockerhub

	https://hub.docker.com/

Alterando tag da imagem para subir no Dockerhub

	docker image tag 95f30ee9d8e7 lucasmaiafranco/meu_apache:1.0

#### login no Dockerhub

	docker login

#### Subir imagem no Dockerhub

	docker push lucasmaiafranco/meu_apache:1.0

#### Baixar uma imagem do Dockerhub

	docker pull lucasmaiafranco/meu_apache:1.0

### Docker Registry

O Docker Registry é uma forma padrão de armazenar e distribuir imagens do Docker.

#### Docker Registry local

Criando um container registry

	docker container run -d -p 5000:5000 --restart=always --name registry registry:2

#### Tag para registry local

	docker image tag 95f30ee9d8e7 localhost:5000/meu_apache:1.0

#### Subir imagem para o registry local

	docker image push localhost:5000/meu_apache:1.0

#### Container com a imagem do registry local

	docker container run -d localhost:5000/meu_apache:1.0

#### Visualizar imagens do registry local

	curl localhost:5000/v2/_catalog


## Day 03

### Docker-machine

#### O que é

Docker-machiner é uma ferramenta para criar as VMs com Docker, podemos usar tanto local, quanto na nuvem.
No nosso caso utilizaremos local com drivers do virtualbox.

Nesse momento deixaremos de usar o Vagrant e instalaremos o docker-machine para fazer o controle de nossas VMs no próprio Desktop.

#### Instalação

Acesse o site abaixo para instalar docker-machine

https://docs.docker.com/machine/install-machine/

#### Criando VM HOSTS

	docker-machine create --driver virtualbox giropops

A máquina apresentou o seguinte erro: 

	Error with pre-create check: "This computer doesn't have VT-X/AMD-v enabled. Enabling it in the BIOS is mandatory"

Com isso utilizamos a opção --virtualbox-no-vtx-check

	docker-machine create --driver virtualbox --virtualbox-no-vtx-check giropops

#### Listar as VMs

	docker-machine ls

#### IP da VM

	docker-machine ip <nome da vm>

#### SSH na VM

	docker-machine ssh <nome da vm>

#### Stop/Start na VM

	docker-machine stop <nome da vm>

	docker-machine start <nome da vm>

#### Detalhes da VM

	docker-machine inspect <nome da vm>

#### Variaveis da VM

	docker-machine env <nome da vm>

A saída deve ser parecida com essa:

	export DOCKER_TLS_VERIFY="1"
	export DOCKER_HOST="tcp://192.168.99.100:2376"
	export DOCKER_CERT_PATH="C:\Users\N6170144\.docker\machine\machines\giropops"
	export DOCKER_MACHINE_NAME="giropops"
	export COMPOSE_CONVERT_WINDOWS_PATHS="true"
	# Run this command to configure your shell:
	# eval $("C:\Users\N6170144\bin\docker-machine.exe" env giropops)

Podemos utilizar o comando eval para conectar nosso docker cliente no docker daemon da VM

	eval $("C:\Users\N6170144\bin\docker-machine.exe" env giropops)

Para remover o docker daemon da VM

	docker-machine env -u

	unset DOCKER_TLS_VERIFY
	unset DOCKER_HOST
	unset DOCKER_CERT_PATH
	unset DOCKER_MACHINE_NAME
	# Run this command to configure your shell:
	# eval $("C:\Users\N6170144\bin\docker-machine.exe" env -u)

Execute o eval novamente

	eval $("C:\Users\N6170144\bin\docker-machine.exe" env -u)

#### Remover a VM

	docker-machine rm <nome da vm>

### Docker Swarm

O Swarm é a ferramenta nativa de orquestração do docker. Ela permite que containers executem distribuídos em um cluster, controlando a quantidade de containers, registro, deploy e update de serviços.

#### Manager

Responsavel por administrar o cluster, ele sabe onde estão todos os containers de todos os serviços. Manager também executa containers.

#### Worker

Responsavel por executar os container, "Carregar o piano".

#### Teoria

Para o cluster swarm funcionar é necessário no minimo dois managers funcionando.

2 manager, um cai ficamos com 50% e o cluster cai.

3 manager, um cai ficamos com 66% e o cluster continua executando.

Quando perdemos o manager ativo outro manager é eleito como ativo, com isso não é vantajoso termos muitos managers pois ira demorar mais eleger um novo manager.

#### Prática

##### Criando 3 VMs
Vamos criar 3 hosts

	docker-machine create --driver virtualbox --virtualbox-no-vtx-check server01
	docker-machine create --driver virtualbox --virtualbox-no-vtx-check server02
	docker-machine create --driver virtualbox --virtualbox-no-vtx-check server03

As VMs 

	$ docker-machine ls
	NAME       ACTIVE   DRIVER       STATE     URL                         SWARM   DOCKER      ERRORS
	server01   -        virtualbox   Running   tcp://192.168.99.101:2376           v19.03.12
	server02   -        virtualbox   Running   tcp://192.168.99.102:2376           v19.03.12
	server03   -        virtualbox   Running   tcp://192.168.99.103:2376           v19.03.12


##### Iniciando o Swarm

Acessando o server01 vamos iniciar o docker swarm

	docker swarm init --advertise-addr 192.168.99.101

##### Adicionando server02 e server03 no cluster

Ao criarmos o cluster é mostrado um comando com uma chave para adicionar os outros server. Devemos armazenar com segurança essa chave

	docker@server01:~$ docker swarm init --advertise-addr 192.168.99.101
	Swarm initialized: current node (1l8vo1wu6ctjdlwjwkz46nlrq) is now a manager.

	To add a worker to this swarm, run the following command:

		docker swarm join --token SWMTKN-1-1fb7fnh3f7aokzpub761gitka1y4b3467gsf0dzg32by05yxze-bblflomyuffd6chc0x1x7hl90 192.168.99.101:2377

	To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.

Devemos executar esse comando no server02 e server03

##### Node como manager

Para promover o server como manager

	docker node promote <server>

	docker node promote server02

##### Node como worker

Para colocar node como worker

	docker node demote <server manager>

	docker node demote server02

##### Remover server do cluster

O node não pode ser manager, caso seja rode o demote e depois o rm

	docker node rm -f server02

##### Visualizar a chave

###### Worker

	docker swarm join-token worker

###### Manager

	docker swarm join-token manager

###### Gerar novas chaves

Para podermos gerar novas chaves do nosso cluster.

###### Worker

	docker swarm join-token --rotate worker

###### Manager

	docker swarm join-token --rotate manager

#### Docker Service

Ele é uma feature que foi incorporada pela engine Docker em sua última versão e que permite ao administrador criar e administrar sua stack de serviço dentro de um cluster Swarm, sem precisar utilizar uma segunda ferramenta para isso. Ele também faz o balanceamento entre os containers.

##### Criando serviço

	docker service create --name sitedaempresa --replicas 3 -p 8080:80 nginx

	--name -> nome do serviço
	--replicas -> quantos container serão usados
	-p -> bindar porta 8080 para 80 dos containers

##### Listar Serviços

	docker service ls

	docker@server01:~$ docker service ls
	ID                  NAME                MODE                REPLICAS            IMAGE               PORTS
	7899ij56ateg        sitedaempresa       replicated          3/3                 nginx:latest        *:8080->80/tcp

#### Listar os container 

	docker service ps <nome do serviço>

	docker@server01:~$ docker service ps sitedaempresa
	ID                  NAME                IMAGE               NODE                DESIRED STATE       CURRENT STATE           ERROR               PORTS
	pcu70w893xja        sitedaempresa.1     nginx:latest        server01            Running             Running 4 minutes ago
	p70qx2qi8o6h        sitedaempresa.2     nginx:latest        server03            Running             Running 4 minutes ago
	olepiqa3m178        sitedaempresa.3     nginx:latest        server01            Running             Running 4 minutes ago

#### Aumentar ou diminuir quantidade de containers

Aumentando para 10 containers

	docker service scale <nome do serviço>=10

	docker service scale sitedaempresa=10

Diminuindo para 1 container

	docker service scale <nome do serviço>=1

	docker service scale sitedaempresa=1


#### Ver o log do service

	docker service logs -f <nome do serviço>

#### Adicionar rede no serviço

	docker service update --network-add network service

### LAB

Vamos criar 3 máquinas virtuais com o Vagrant

	servers/
		server01/
			Vagrantfile
		server02/
			Vagrantfile
		server03
			Vagrantfile

No provisionamento das máquinas foi instalado o docker, vamos utilizar o GlusterFS para criar e compartilhar volume entre elas.

#### Criando o cluster Swarm

Criamos o cluster no server01

	root@server01:~# docker swarm init --advertise-addr 192.168.99.11

#### Adicionando server02 e server03 no cluster

	root@server02:~# docker swarm join --token <token> 192.168.99.11:2377

	root@server03:~# docker swarm join --token <token> 192.168.99.11:2377

#### Status do cluster

	root@server01:~# docker node ls
	ID                            HOSTNAME   STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
	pvazi4p9we4r1ks9gjnj08c9e *   server01   Ready     Active         Leader           20.10.8
	4wktgf4ilrae0m0sm7nyalnyl     server02   Ready     Active                          20.10.8
	xf9hkh219gbk3fiqhei3nvmhq     server03   Ready     Active                          20.10.8


#### GlusterFS

##### Instalando GLusterFS

	root@server01:~# apt update

	root@server01:~# apt install software-properties-common

	root@server01:~# add-apt-repository ppa:gluster/glusterfs-7

	root@server01:~# apt update

	root@server01:~# apt install glusterfs-server

##### Serviço GlusterFS

	root@server01:~# systemctl start glusterd

	root@server01:~# systemctl enable glusterd

##### Em uma linha

	apt update && apt install software-properties-common && add-apt-repository -y ppa:gluster/glusterfs-7 && apt update && apt install -y glusterfs-server && systemctl start glusterd && systemctl enable glusterd

##### Editando o arquivo hosts

Devemos adicionar o IP e nome de cada máquina

Server01

	/etc/hosts

	192.168.99.21 server02 server02
	192.168.99.31 server03 server03

Server02

	/etc/hosts

	192.168.99.11 server01 server01
	192.168.99.31 server03 server03

Server03

	/etc/hosts

	192.168.99.11 server01 server01
	192.168.99.21 server02 server02

##### Criando uma pool de armazenamento confiável

	root@server01:/# gluster peer probe server02

	root@server01:/# gluster peer probe server03

##### Listando os nós

	root@server01:/etc/glusterfs# gluster peer status
	Number of Peers: 2

	Hostname: server02
	Uuid: d6921e47-2c73-4c4f-9ee9-c5795f60d409
	State: Peer in Cluster (Connected)

	Hostname: server03
	Uuid: 2d5f2fe6-d4c9-49d1-bef8-6e09596398ee
	State: Peer in Cluster (Connected)

##### Criando volume

	gluster volume create volume_name replica number_of_servers domain1.com:/path/to/data/directory domain2.com:/path/to/data/directory force

	volume_name -> Nome do volume
	replica number_of_servers -> replica é o tipo de volume e o número de servidores 

	domain1.com:/… e domain2.com:/… -> Eles definem a localização das máquinas e do diretório do bricks — o termo do GlusterFS para sua unidade básica de armazenamento, que inclui todos os diretórios em todas as máquinas que sirvam como parte ou uma cópia de um volume maior — que formarão o volume1. O exemplo a seguir irá criar um diretório chamado gluster-storage no diretório root de ambos os servidores.

	force -> Essa opção irá sobrescrever quaisquer avisos ou opções que de outra forma apareceriam e interromperiam a criação do volume.

Segue o comando utilizado:

	gluster volume create volume01 replica 3 transport tcp server01:/gluster-volumes/brick1/ server02:/gluster-volumes/brick1/ server03:/gluster-volumes/brick1/ force

##### Ativando o Volume

	gluster volume start volume-site01

##### Verificando o volume

	gluster volume status

	root@server01:/# gluster volume status
	Status of volume: volume-site01
	Gluster process                             TCP Port  RDMA Port  Online  Pid
	------------------------------------------------------------------------------
	Brick server01:/gluster-volumes/brick1      49152     0          Y       14087
	Brick server02:/gluster-volumes/brick1      49152     0          Y       14072
	Brick server03:/gluster-volumes/brick1      49152     0          Y       13778
	Self-heal Daemon on localhost               N/A       N/A        Y       14108
	Self-heal Daemon on server03                N/A       N/A        Y       13806
	Self-heal Daemon on server02                N/A       N/A        Y       14093

	Task Status of Volume volume-site01
	------------------------------------------------------------------------------
	There are no active volume tasks

##### Montando o volume

	mkdir -p /storage-pool/site01

	mount -t glusterfs server01:/volume-site01 /storage-pool/site01/

##### Criando index.html

	echo "Teste volume com docker swarm" > /storage-pool/site01/index.html

#### Criando Volume no docker 

	docker volume create site01

#### Criando link simbolico

Vamos criar um link simbolico do nosso volume Glaster para o volume do docker

	ln -s /storage-pool/site01/_data/ /var/lib/docker/volumes/site01/

#### Criando serviço no docker

	docker service create --name site01 --replicas 6 -p 8080:80 --mount type=volume,src=site01,dst=/usr/share/nginx/html nginx


## Day 04

### Secrets

Docker Secrets funciona como um cofre, onde você pode colocar coisas sensíveis e só quem tem a chave do cofre consegue utilizar – no caso, essa chave é designada aos nós dos serviços que a chave for atribuída.

Dicas
Só funciona no Swarm Mode onde toda a comunicação entre os nós é por padrão encriptada.

#### Criando um secrets

Podemos criar de duas maneiras, utilizando o STDIN:

	echo "ConteudoDoSecret" | docker secret create um-secret -

Lendo um arquivo:

	docker secret create novo-secret $HOME/senhas.txt

#### Listando secrets

	docker secret ls

#### Removendo um secrets

	docker secret rm um-secret

OBS: O secret não pode estar vinculado a um service

#### Usando secrets criado

Vamos criar um serviço utilizando o secret criado

	docker service create --name site02 --secret um-secret alpine

#### Diretório no container onde ficam os secrets

	/run/secrets

#### Secrets com target, permissão

	docker service create --detach=false --name app --secret source=db_pass,target=password,uid=2000,gid=3000,mode=0400 minha_app:1.0

#### Removendo secrets de um serviço

	docker service update --secret-rm um-secret demo


### Compose

Docker compose é uma ferramenta para definição e execução de múltiplos containers Docker. Com ela é possível configurar todos os parâmetros necessários para executar cada container a partir de um arquivo de definição.

#### Primeiro docker compose

	version: "3.7"
	services:
	web:
		image: nginx
		deploy:
		replicas: 5
		resources:
			limits:
			cpus: "0.1"
			memory: 50M
		restart_policy:
			condition: on-failure
		ports:
		- "8080:80"
		networks:
		- webserver
	networks:
	webserver:

#### Deploy do docker compose

	docker stack -c docker-compose.yaml site01

#### Removendo deploy

	docker stack rm site01

#### Rodando Wordpress

##### Docker compose

Vamos criar dois serviços:

- Database
- Wordpress

O serviço wordpress só irá subir após o db subir (opção depends_on)

	version: "3.7"
	services:
	db:
		image: mysql:5.7
		volumes:
		- db_data:/var/lib/mysql
		environment:
		MYSQL_ROOT_PASSWORD: somewordpress
		MYSQL_DATABASE: wordpress
		MYSQL_USER: wordpress
		MYSQL_PASSWORD: wordpress

	wordpress:
		depends_on:
		- db
		image: wordpress:latest
		ports:
		- "8080:80"
		environment:
		WORDPRESS_DB_HOST: db:3306
		WORDPRESS_DB_USER: wordpress
		WORDPRESS_DB_PASSWORD: wordpress

	volumes:
	db_data:

##### Deploy do wordpress

	docker stack deploy -c docker-compose.yml wordpress

##### Listando os serviços criados

	root@server02:~/compose/2# docker service ls
	ID             NAME                  MODE         REPLICAS   IMAGE              PORTS
	vcqcv1ina4ei   wordpress_db          replicated   1/1        mysql:5.7
	lob7yolx9tyk   wordpress_wordpress   replicated   1/1        wordpress:latest   *:8080->80/tcp

##### Visualizando Logs

	docker service logs -f wordpress_db
    docker service logs -f wordpress_wordpress

#### Visualizer

O Visualizer é uma página web simples que mostra informações básicas sobre os nós e contêineres de um Docker swarm.

##### docker compose


	version: "3.7"
	services:
	web:
		image: nginx
		deploy:
		placemant:
			constrainsts:
			- node.labels.dc == UK
		replicas: 5
		resources:
			limits:
			cpus: "0.1"
			memory: 50M
		restart_policy:
			condition: on-failure
		ports:
		- "8080:80"
		networks:
		- webserver
	visualizer:
		image: dockersamples/visualizer:stable
		ports:
		-  "8888:8080"
		volumes:
		- "/var/run/docker.socke:/var/run/docker.sock"
		deploy:
		placement:
			constrainsts: [node.role == manager]
		networks:
		- webserver

	networks:
	webserver:


Podemos ver duas novas configurações:

1° Estamos passando para que o visualizer so execute em node manager

	placement:
		constrainsts: [node.role == manager]

2° Estamos passando a informação para que o nginx so rode em node com label UK

	placemant:
		constrainsts:
		- node.labels.dc == UK

Se nenhum noe tiver essa label os containers nginx ficam em status peending

	root@server02:~/compose/3# docker stack ps site03
	ID             NAME                  IMAGE                             NODE       DESIRED STATE   CURRENT STATE              ERROR                              PORTS
	lxs9dyi33jys   site03_visualizer.1   dockersamples/visualizer:stable   server03   Running         Preparing 14 seconds ago
	mcqmurclx3sr   site03_web.1          nginx:latest                                 Running         Pending 10 seconds ago     "no suitable node (scheduling …"
	nf6y3dl7gvv9   site03_web.2          nginx:latest                                 Running         Pending 10 seconds ago     "no suitable node (scheduling …"

##### Adicionando tag no node

	docker node update --label-add chave=valor node

	docker node update --label-add dc=UK server03

##### Deploy

	docker stack deploy -c docker-compose.yaml site03

#### Exemplo Voting App

https://github.com/dockersamples/example-voting-app

##### Docker compose

	version: "3.7"
	services:
	redis:
		image: redis:alpine
		ports:
		- "6379"
		networks:
		- frontend
		deploy:
		replicas: 2
		update_config:
			parallelism: 1
			delay: 10s
			order: start-first
		rollback_config:
			parallelism: 1
			delay: 10s
			failure_action: continue
			monitor: 60s
			order: stop-first
		restart_policy:
			condition: on-failure

	db:
		image: postgres:9.4
		volumes:
		- db-data:/var/lib/postgresql/data
		networks:
		- backend
		deploy:
		placement:
			constraints: [node.role == manager]

	vote:
		image: dockersamples/examplevotingapp_vote:before
		ports:
		- 5000:80
		networks:
		- frontend
		depends_on:
		- redis
		deploy:
		replicas: 2
		update_config:
			parallelism: 2
		restart_policy:
			condition: on-failure

	result:
		image: dockersamples/examplevotingapp_result:before
		ports:
		- 5001:80
		networks:
		- backend
		depends_on:
		- db
		deploy:
		replicas: 1
		update_config:
			parallelism: 1
			delay: 10s
		restart_policy:
			condition: on-failure

	worker:
		image: dockersamples/examplevotingapp_worker
		networks:
		- frontend
		- backend
		deploy:
		mode: replicated
		replicas: 1
		labels: [APP-VOTING]
		restart_policy:
			condition: on-failure
			delay: 10s
			max_attempts: 3
			window: 120s
		placement:
			constraints: [node.role == manager]

	visualizer:
		image: dockersamples/visualizer:stable
		ports:
		- 8080:8080
		stop_grace_period: 1m30s
		volumes:
		- "/var/run/docker.sock:/var/run/docker.sock"
		deploy:
		placement:
			constraints: [node.role == manager]

	networks:
	frontend:
	backend:

	volumes:
	db-data:


##### Deploy

	docker stack deploy -c docker-compose.yaml vote

##### Listar stack

	root@server02:~/compose/4# docker stack ps vote
	ID             NAME                IMAGE                                          NODE       DESIRED STATE   CURRENT STATE           ERROR                       PORTS
	wckbvoew3nvt   vote_db.1           postgres:9.4                                   server04   Running         Running 2 minutes ago
	urzdhddxfbl0   vote_redis.1        redis:alpine                                   server02   Running         Running 2 minutes ago
	uzju8tkjixij   vote_redis.2        redis:alpine                                   server03   Running         Running 2 minutes ago
	2i63jz6oevih   vote_result.1       dockersamples/examplevotingapp_result:before   server02   Running         Running 2 minutes ago
	ki7p35xk9zfp   vote_visualizer.1   dockersamples/visualizer:stable                server04   Running         Running 2 minutes ago
	fbbst2frop2z   vote_vote.1         dockersamples/examplevotingapp_vote:before     server03   Running         Running 2 minutes ago
	3wiiu8y0iptx   vote_vote.2         dockersamples/examplevotingapp_vote:before     server02   Running         Running 2 minutes ago
	tbwowpddrk1y   vote_worker.1       dockersamples/examplevotingapp_worker:latest   server03   Running         Running 2 minutes ago
