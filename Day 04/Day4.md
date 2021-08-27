
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
			- [Deploy do docker compose](#deploy-do-docker-compose)
		- [Exemplo Voting App](#exemplo-voting-app)
			- [Docker compose](#docker-compose)
			- [Deploy](#deploy)
			- [Listar stack](#listar-stack)


<!-- TOC -->

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

##### Deploy do docker compose

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