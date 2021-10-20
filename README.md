# Docker Compose ambiente Laravel

Este é um projeto que você pode usar em projetos existentes ou novos projetos. 

[Download](https://github.com/Repox/laravel-docker/archive/master.zip) o conteúdo deste repositório e adicione os arquivos à raiz do seu projeto Laravel. Descarte este arquivo `README.md` para evitar sobrescrever o seu próprio arquivo ou um existente.

## Stack

A Stack é configurada por padrão da seguinte forma:

- PHP 8.0 ([image](https://hub.docker.com/r/repox/laravel-dev-php))
- MySQL 8.0 ([image](https://hub.docker.com/_/mysql))
- Nginx ([image](https://hub.docker.com/_/nginx), latest)
- Composer v2 (built into PHP image)

Você pode usar [PHP 7.4 and PHP 7.3](hhttps://hub.docker.com/r/repox/laravel-dev-php) especificando a tag de versão em `docker-compose.yml` onde o `repox/laravel-dev-php` image é referenciado; i.e.:

```
repox/laravel-dev-php:7.3
repox/laravel-dev-php:7.4
repox/laravel-dev-php:8.0
repox/laravel-dev-php:latest # Fornece a versão mais recente do PHP
```

As imagens PHP usadas para este repositório são feitas especificamente para o desenvolvimento com o Laravel neste contexto.

### Comece o ambiente local

Iniciar o ambiente é muito fácil.

```bash
$ docker-compose up -d
```

Isso terá um ambiente de trabalho disponível em [http://localhost](http://localhost)

Você já deve ter coisas em execução localmente na porta 80, o que pode gerar alguns erros. Nesse caso, pode ser benéfico alterar a porta exposta para algo como `8080` ou` 8000`. Eu escolhi `80 'para minha própria conveniência. Expor o serviço `nginx` na porta` 8000`, em vez disso, significaria que você configurou o `docker-compose.yml` e alterou a configuração da` port` para algo como o seguinte:

```yaml
  nginx:
    image: nginx
    ports:
      - "8080:80"
```

O lado esquerdo da especificação da porta é a porta do host Docker que mapeia para a porta do contêiner especificada no lado direito.

Os nomes de host para MySQL (e outros serviços adicionados) são acessados por meio do nome do serviço. <br>
O contêiner do MySQL foi rotulado como `db` em` docker-compose.yml` e, portanto, o parâmetro `DB_HOST` em seu arquivo` .env` no Laravel deve refletir isso como o nome do host:

```ìni
DB_CONNECTION=mysql
DB_HOST=db
DB_PORT=3306
DB_DATABASE=homestead
DB_USERNAME=homestead
DB_PASSWORD=secret
```

### Pare o ambiente local

Para interromper o ambiente, mas mantendo os dados de volume, execute o seguinte:

```bash
$ docker-compose stop
```

### Removendo o ambiente local

Para interromper o ambiente e remover dados de volume e rede de contêineres (por exemplo, para reiniciar):

```bash
$ docker-compose down -v
```

## Comandos de ajuda

Foram adicionados alguns contêineres que podem interagir diretamente com o ambiente para utilizar o Composer e o Artisan na rede docker.

Exemplos:

```bash
$ docker-compose run artisan migrate:refresh
$ docker-compose run artisan queue:work --once
$ docker-compose run composer require laravel/sanctum
$ docker-compose run composer dump
```



## Acessando MySQL

Este repositório contém uma configuração feita especificamente para permitir que você acesse-o com seu cliente favorito diretamente de seu host, conectando-se à porta exposta via `127.0.0.1`. Como um exemplo:

```bash
$ mysql -h 127.0.0.1 -p 3306 -u root -proot
```

O mesmo procedimento pode ser usado em ie. DbWeaver, Sequel Ace, MySQL Workbench, HeidiSQL ou qualquer ferramenta que você preferir.
