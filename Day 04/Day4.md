
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