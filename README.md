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
		
	Contruir uma imagem através do Dockerfile
	
		docker image build -t toskeira:1.0 .
		
	Listar as imagens
	
		docker images ls
		
	Criando container com a nossa imagem
	
		docker container run -d toskeira:1.0

		
	
