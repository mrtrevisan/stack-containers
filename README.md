# Stack Local de Containers

![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Traefik](https://img.shields.io/badge/Traefik-24A1C1?style=for-the-badge&logo=traefikproxy&logoColor=white)
![Portainer](https://img.shields.io/badge/Portainer-13BEF9?style=for-the-badge&logo=portainer&logoColor=white)
![mkcert](https://img.shields.io/badge/mkcert-555555?style=for-the-badge)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white)
![phpMyAdmin](https://img.shields.io/badge/phpMyAdmin-6C78AF?style=for-the-badge&logo=phpmyadmin&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=for-the-badge&logo=postgresql&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-FF4438?style=for-the-badge&logo=redis&logoColor=white)
![Memcached](https://img.shields.io/badge/Memcached-2E7D32?style=for-the-badge)
![RabbitMQ](https://img.shields.io/badge/RabbitMQ-FF6600?style=for-the-badge&logo=rabbitmq&logoColor=white)
![Jenkins](https://img.shields.io/badge/Jenkins-D24939?style=for-the-badge&logo=jenkins&logoColor=white)
![Prometheus](https://img.shields.io/badge/Prometheus-E6522C?style=for-the-badge&logo=prometheus&logoColor=white)
![Grafana](https://img.shields.io/badge/Grafana-F46800?style=for-the-badge&logo=grafana&logoColor=white)

Provisionamento de containers de Infra gerenciado pelo Portainer + Traefik, usando TLS.

## Tecnlologias e Requisitos:
- Docker - https://www.docker.com/
- Docker-compose - https://docs.docker.com/compose/
- Portainer - https://www.portainer.io/
- Traefik - https://github.com/traefik/traefik
- Mkcert - ferramenta para criar certificados de desenvolvimento localmente confiáveis.  https://github.com/FiloSottile/mkcert
- edição do arquivo `hosts` do sistema (simulando um DNS local)

Basicamente criamos um deploy do traefik para gerenciar as coenxoes com um dominio local. (*.local.test)

![](./docs/images/traefik-architecture.webp)

Passos a seguir consideram já o Docker e o Docker-Compose instalados no sistema. 

###  Gerando Certificado Local
Instalar o mkcert no sistema operacional utilizado:
Windows (utilizando winget): abra o prompt de comando do Windows e digite:
```
winget install mkcert -s winget 
```

Instalando autoridade certificadora local
```
mkcert -install
```
Gerando certificado para os domínios (exemplo utilizado: local.test).
Gerar o o certifcado a partir da pasta `core`. Os certificados vao ser armazenados na pasta `certs`.

```
cd core
mkcert -cert-file certs/local-cert.pem -key-file certs/local-key.pem "local.test" "*.local.test" "cloud.local" "*.cloud.local"
```

###  Iniciando o Traefik e o Portainer
A partir da posta `core` digitar os seguintes comandos:

Criar uma rede do tipo bridge (padrão do Docker), porém com um nome. (exemplo utilizado: traefik-proxy)
```
docker network create traefik-proxy
```

Verificar se o arquivo de manifesto está ok (a saida é o próprio manifesto yml.):
```
docker-compose config
```

Iniciar a stack core com o parametro -d (detached mode) rodando em background. A partir da pasta core, executar:
```
docker-compose up -d
```

Editar o arquivo hosts e inserir os dominios locais (template pode ser encontrado na poasta docs):

Windows - abrir o Notepad como administrador
```
C:\Windows\System32\drivers\etc\hosts
```
```
Start-Process notepad -Argument C:\Windows\System32\drivers\etc\hosts -Verb RunAs
```

Linux e Mac - editar usando sudo
```
\etc\hosts
```

Abrir o navegador e digitar a url:
```
http://portainer.cloud.local
```

