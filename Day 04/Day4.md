
  - [Day 04](#day-04)
	- [Secrets](#secrets)
		- [Criando um secrets](#criando-um-secrets)
		- [Listando secrets](#listando-secrets)
		- [Removendo um secrets](#removendo-um-secrets)
		- [Usando secrets criado](#usando-secrets-criado)
		- [Diretório no container onde ficam os secret](#diretório-no-container-onde-ficam-os-secret)
		- [Secrets com target, permissão](#secrets-com-target,-permissão)
		- [Removendo secrets de um serviço](#removendo-secrets-de-um-serviço)

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