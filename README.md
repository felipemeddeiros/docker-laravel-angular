## Instalação

Para permitir a instalação em qualquer sistema operacional foi utilizado o [Docker](https://www.docker.com/products/docker-desktop).

Apoś instalar o docker, crie sua pasta do projeto Incentivador e clone esse repositório dentro.

Clone os repositórios dos outros projetos ao lado.

- [Backoffice](https://bitbucket.org/Incentivadorvendas/backoffice/src/master/)

- [API](https://bitbucket.org/Incentivadorvendas/api/src/master/)

- [Webv2](https://bitbucket.org/Incentivadorvendas/webv2/src/master/)


Abra o terminal de comandos e vá ao diretório do docker. Dentro execute o seguinte comando para subir os containers.
```bash
$ docker-compose up -d
```

Isso deve levar alguns minutos. Enquanto isso vá as configurações de hosts do seu sistema operacional e inclua alguns dominios.
> **Windows**: C:/Windows/System32/Drivers/etc/hosts

> **Linux**: /etc/hosts
```text
127.0.0.1	backoffice.local
127.0.0.1	api.local
127.0.0.1	webv2.local
127.0.0.1	webv2.prod
```

## Backoffice

Após os containers subirem, execute o comando para atualizar os packages
```bash
$ docker container exec -it php-fpm composer install -d /var/www/backoffice
```

No diretório do backoffice execute o comando para gerar o arquivo .env de configuração
```bash
$ cp .env.example .env
```
> Abra o arquivo e confirme as configurações abaixo. Caso algo esteja diferente altere para o que está abaixo.
DB_CONNECTION=mysql
DB_HOST=mysql
DB_PORT=3306
DB_DATABASE=incentivador
DB_USERNAME=root
DB_PASSWORD=root

Cacheando as configurações
```bash
$ docker container exec php-fpm php ./backoffice/artisan config:cache
```

Executando as migrations e seeders
```bash
$ docker container exec php-fpm php ./backoffice/artisan migrate --seed
```

Liberando o acesso público as imagens incluídas no sistema.
```bash
$ docker container exec php-fpm php ./backoffice/artisan storage:link
```

Pronto o backoffice deve estar disponível no seguinte endereço: http://backoffice.local

## API

Instalando os packages da API
```bash
$ docker container exec php-fpm composer install -d /var/www/api 
```

No diretório da API, execute o comando para gerar o arquivo .env de configuração 
```bash
$ cp .env.example .env
```
> Novamente, abra o arquivo e confirme as configurações abaixo. Caso algo esteja diferente altere para o que está abaixo.
DB_CONNECTION=mysql
DB_HOST=mysql
DB_PORT=3306
DB_DATABASE=incentivador
DB_USERNAME=root
DB_PASSWORD=root

Cacheando as configurações
```bash
$ docker container exec php-fpm php ./api/artisan config:cache
```

Executando as migrations da API
```bash
$ docker container exec php-fpm php ./api/artisan migrate
```

Instalando o Laravel passport
```bash
$ docker container exec php-fpm php ./api/artisan passport:install
```

Pronto a api deve estar disponível no seguinte endereço: http://api.local

## Webv2

A aplicação angular está configurada para instalar tudo automaticamente. Então apoś os containers subirem
ela deve estar disponível no seguinte endereço: http://webv2.local

## Instruções de uso

Caso precise executar algum comando *php artisan* ou *composer* você só precisa entrar no container **php-fpm** escolher o projeto(backoffice ou API), entrar na pasta e executar o comando desejado. Abaixo o comando para entrar no container
```bash
$ docker container exec -it php-fpm bash
```

Caso precise executar algum comando *npm* na aplicação angular você pode utilizar o container **npm-angular**. 
Para acessar execute o seguinte comando:
```bash
$ docker container exec -it npm-angular bash
```

Caso também precise executar algum comando *npm* no backoffice. Entre no container **npm-angular** e vá para o diretório do backoffice. Como mostra abaixo.
```bash
$ docker container exec -it npm-angular bash
$ cd ..
$ cd backoffice
```