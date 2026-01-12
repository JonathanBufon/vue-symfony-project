# Backend API – Histórico de Criação do Projeto

Este documento descreve, de forma objetiva, como o projeto foi criado até o estado atual, **exclusivamente por meio de comandos executados no terminal**, sem alterações manuais relevantes de código fora dos geradores padrão do Symfony.

---

## 1. Estrutura inicial

O projeto foi iniciado dentro do diretório `backend/`.

```bash
cd backend/
````

---

## 2. Criação do projeto Symfony

Foi criado um novo projeto Symfony utilizando a versão **6.4**, com o preset **webapp**, que inclui os componentes essenciais (HTTP Kernel, Twig, Security, etc.).

```bash
symfony new api --version=6.4 --webapp
```

Após a criação:

```bash
composer update
composer install
```

Esses comandos garantiram que todas as dependências fossem resolvidas e instaladas corretamente.

---

## 3. Verificação de requisitos do ambiente

Foi executada a checagem de compatibilidade do ambiente local com o Symfony:

```bash
symfony check:requirements
```

---

## 4. Dependências adicionais

### HTTP Client

O componente HTTP Client do Symfony foi adicionado manualmente via Composer:

```bash
composer require symfony/http-client
```

### API Platform

Posteriormente, o API Platform foi instalado para transformar a aplicação em uma API REST completa:

```bash
symfony composer req api
```

Isso adicionou:

* API Platform
* Serialização JSON-LD / Hydra
* Rotas automáticas para recursos
* Documentação OpenAPI / Swagger

---

## 5. Ambiente Docker

O projeto utiliza Docker Compose para orquestração de serviços.

Comandos executados durante o processo:

```bash
docker compose restart
docker compose ps
docker compose up -d
docker compose build --no-cache
```

Esses comandos indicam:

* Reinicialização dos serviços
* Verificação do estado dos containers
* Subida dos containers em background
* Rebuild completo das imagens sem cache

---

## 6. Criação de entidades (API Resources)

Dentro do diretório do projeto (`backend/api/`), foram criadas entidades utilizando o maker do Symfony já integradas ao API Platform.

```bash
cd backend/api/
```

Criação de entidade como recurso de API:

```bash
symfony console make:entity --api-resource
```

(O comando foi executado mais de uma vez durante o processo.)

Esse passo gerou:

* Classe de entidade Doctrine
* Anotações / atributos de API Resource
* Integração automática com o API Platform

---

## 7. Migrations e banco de dados

Após a criação das entidades, o fluxo padrão do Doctrine foi seguido:

```bash
symfony console make:migration
symfony console doctrine:migrations:migrate
```

Isso resultou em:

* Geração dos arquivos de migration
* Criação/atualização do schema do banco de dados

---

## 8. Servidor de desenvolvimento

O servidor embutido do Symfony foi utilizado para rodar a aplicação localmente.

```bash
symfony server:start
```

Posteriormente, em modo daemon:

```bash
symfony server:start -d
```

---

## 9. Estado atual do projeto

Até este ponto, o projeto possui:

* Symfony 6.4
* API Platform configurado
* Docker Compose ativo
* Entidades expostas automaticamente como endpoints REST
* Migrations aplicadas no banco
* Servidor Symfony rodando localmente

Todo o estado atual do projeto foi alcançado **exclusivamente via comandos**, sem configuração manual relevante fora do fluxo padrão das ferramentas.

# Configurações adicionais

Foi definido o formato de resposta como **JSON** no API Platform.  
Por padrão, o API Platform utiliza **JSON-LD**.

---

### Caminho do arquivo `backend/api/config/packages/api_platform.yaml`


Este trecho foi adicionado:

```yaml
    defaults:
      stateless: true
      formats:
        json: ['application/json']
      cache_headers:
        vary: ['Content-Type', 'Authorization', 'Origin']

    formats:
      json:
        mime_types: ['application/json']
```

### Caminho do arquivo `backend/api/config/packages/framework.yaml`


Este trecho foi adicionado:

```yaml
    request:
      formats:
        json: ['application/json']
```

## Acesso aos serviços após subir o Docker

Com o ambiente Docker em execução, os seguintes serviços ficam disponíveis localmente:

---

### Aplicação Symfony
Interface principal da aplicação Symfony.

http://127.0.0.1:8000/

---

### API Platform
Interface da API REST gerada automaticamente pelo API Platform, incluindo documentação e testes de endpoints.

http://127.0.0.1:8000/api

---

### Adminer (Gerenciador de Banco de Dados)
Ferramenta web para administração do banco de dados MariaDB/MySQL utilizado pelo projeto.

http://127.0.0.1:8080/

#### Credenciais de acesso do Adminer

- Sistema: MySQL / MariaDB
- Servidor: `database`
- Usuário: `root`
- Senha: `password`
- Base de dados: `main`
