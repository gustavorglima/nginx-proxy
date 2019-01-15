# Docker Nginx Proxy
O Nginx Proxy permite que tenha múltiplos containers com nginx usando a porta 80.

## Como usar?
```bash
docker network create nginx-proxy
git clone https://github.com/gustavorglima/nginx-proxy.git
cd nginx-proxy
docker-compose up -d
```

## Configurando em seu Projeto

Adicione a seguinte network no seu docker-compose:
```bash
nginx-proxy:
  external:
    name: nginx-proxy
```

#### No container do Nginx:

Configure a porta:
```bash
ports:
  - 81:80
```
Adicione a network:
```
networks:
  - nginx-proxy
```

Adicione o environment com domínio desejado:
```bash
environment:
  - VIRTUAL_HOST=site.test
 ```
 
 #### Hosts:
 Adicione o VIRTUAL_HOST criado acima no arquivo hosts:
 
 ```bash
sudo sh -c 'echo "127.0.0.1   site.test" >> /etc/hosts'
```

#### Pronto! Basta subir seu docker. 

Caso queira adicionar em outros docker basta mudar a porta do nginx, por exemplo:

```
ports:
  - 82:80
```
#### Exemplo do que fizemos acima:

```bash
version: "3"

networks:
  network:
  nginx-proxy:
    external:
      name: nginx-proxy

services:
  php:
    build:
      context: ./docker/php
    expose:
      - 9000
    networks:
      - network

  nginx:
    build:
      context: ./docker/nginx
    ports:
      - 81:80
    networks:
      - network
      - nginx-proxy
    environment:
      - VIRTUAL_HOST=site.test
```