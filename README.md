# Descomplicando-Docker

## Day 01

- Um pouco de teoria

- Criado VM Debian, utilizei o Vagrant com o vagrantfile deste repo

Comandos Vagrant

	Para subir a VM
	
		vagrant up
		
	Verificar status da VM
		
		vagrant status (Deve estar no diretório que está o Vagrantfile)
		vagrant global-status (Lista todas VMs da sua máquina)
		
	Conectar SSH
	
		vagrant ssh (deve estar no diretório que está o Vagrantfile)
		vagrant ssh <id da vm>
		
	
- Instalação do Docker

		apt install curl -y && curl -fsSL https://get.docker.com -o get-docker.sh && sudo sh ./get-docker.sh

- Comandos docker

	Versão
	
		docker version
		
	Visualizar containers em execução
	
		docker container ls
		
	Visualizar todos containers
	
		docker container ls -a
		
	Primeiro container
	
		docker container run -ti hello-world
		
		-ti = terminal e interatividade
		
	Criando containers
	
		docker container run -ti ubuntu
		
		docker container run -ti centos
		
	Para sair do container sem matar ele(bash)
	
		CTRL + p + q
		
	Conectar no container novamente
	
		docker container attach <container ID ou nome do container>
		
	Executar container como daemon
	
		docker container -d nginx
		
	Executar um comando dentro do container
	
		docker container exec -ti <container ID> <comando>
		docker container exec -ti 86cd7182695f ls /usr/share/nginx/html
		docker container exec -ti 86cd7182695f bash
		
	start/stop/restart container
	
		docker container start 86cd7182695f
		docker container stop 86cd7182695f
		docker container restart 86cd7182695f
		
	pause/unpause container
	
		docker container pause <container ID>
		docker container unpause <container ID>

	Informações do container

		docker container inspect <container ID>
		
	Log do container

		docker container logs -f <container ID>
		
	Remover container

		docker container rm <container ID>
		
		docker container rm -f <container ID>
		
	Remover todos conatiner que estão stopados
	
		docker container prune
		
	Ver o consumo de CPU e memória do container
	
		docker container stats <container ID>
		
	Ver os processos em execução no container

		docker container top <container ID>
		
	Criando container com o tamanho da memória (128 M)
	
		docker container run -d -m 128M nginx

	Criando container com o tamanho da memória (128 M) e CPU (0.5 é a metade de um core)

		docker container run -d -m 128M --cpus 0.5 nginx
		
	Atualizar o container (aumentar memória, CPU, etc)
	
		docker container update --memory 64M --cpus 0.4 nginx
		
- Primeiro Dockerfile
		
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
		
	Construir uma imagem através do Dockerfile
	
		docker image build -t toskeira:1.0 .
		
	Listar as imagens
	
		docker images ls
		
	Criando container com a nossa imagem
	
		docker container run -d toskeira:1.0

		
	
## Day 02

- Volumes Bind

	É quando ja temos um diretório e queremos montar esse diretório dentro do container
	
	Criando container com volume bind
	
		mkdir /opt/teste
		
		docker container run -ti --mount type=bind,src=/opt/teste,dst=/teste debian
		
		src -> diretório que deseja usar no container
		dst -> diretório que será criado no container
		
	Segue os testes realizados:
		
		- Criamos um diretório
		- Criamos um conteiner passando o diretório criado
		- Dentro do container criamos um arquivo no diretório
		- Saimos do container (com isso ele é finalizado)
		- Verificamos o conteúdo do arquivo criado dentro do container.
		
	segue os comandos 
	
		root@docker-server:/opt# mkdir /opt/teste
		root@docker-server:/opt# docker container run -ti --mount type=bind,src=/opt/teste,dst=/teste debian
		root@eebf87b86a27:/# touch /teste/teste.txt
		root@eebf87b86a27:/# echo "Isso e um teste" > /teste/teste.txt
		root@eebf87b86a27:/# exit
		exit
		root@docker-server:/opt# docker container ls
		CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
		root@docker-server:/opt# cat /opt/teste/teste.txt
		Isso e um teste
		root@docker-server:/opt#
		
- Volumes

	Listar os volumes
	
		docker volume ls
		
	Criando volume
	
		docker volume create <nome do volume>
		
	Verificar as config do volume
	
		docker volume inspect <nome do volume>
		
	Diretório do volume (onde ficam os files)
	
		/var/lib/docker/volumes/<nome do volume>/_data
		
	Criando container utilizando o volume criado
	
		docker container run -ti --mount type=volume,src=<nome do volume>,dst=/teste debian
		
		src -> nome do volume
		dst -> diretório que será criado no container
		
	- Segue os testes realizados
	
	Criamos um volume chamado fusca
	
		root@docker-server:/# docker volume create fusca
	
	Criamos um arquivo dentro do diretório do volume
		
		root@docker-server:/# touch /var/lib/docker/volumes/fusca/_data/fusca_77
		
	Criamos um container debian passando o volume criado
	
		root@docker-server:/# docker container run -ti --mount type=volume,src=fusca,dst=/fusca debian
		
	Dentro do container adicionamos o texto "Fusca Branco" no arquivo criado anteriormente fusca_77
	
		root@8609465c807e:/# ls /fusca/fusca_77
		/fusca/fusca_77
		root@8609465c807e:/# echo "Fusca Branco" > /fusca/fusca_77	
		
	Saimos do container (com isso ele é finalizado)
	
		root@8609465c807e:/# exit
		exit
		root@docker-server:/# docker container ls
		CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
		
	Verificamos o conteúdo do arquivo
	
		root@docker-server:/# cat /var/lib/docker/volumes/fusca/_data/fusca_77
		Fusca Branco
		
	Criamos outro container centos e verificamos o conteudo do mesmo arquivo (fusca_77)

		root@docker-server:/var/lib/docker/volumes/fusca/_data# docker container run -ti --mount type=volume,src=fusca,dst=/fusca centos
		[root@1d46daf87440 /]# cat /fusca/fusca_77
		Fusca Branco
		[root@1d46daf87440 /]# exit
		exit
<<<<<<< HEAD
		
	Comando para apagar todos volumes que não estão sendo utilizados, por nenhum container
	
		docker volume prune
		
		
	Data Only (utilizado antigamente quando não tinha volume)
	
		Criamos o container
	
=======
	
>>>>>>> 01f562524e6f67dd02b00323413dc39ae213b02e
