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

<!-- TOC -->

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