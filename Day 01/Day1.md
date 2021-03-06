# Day 01

## Sumário

<!-- TOC -->
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






