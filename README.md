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

- Comandos básicos docker

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


	   

		
	
