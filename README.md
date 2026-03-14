# 🐳 Docker Registry Privado (com autenticação `htpasswd`)

[![Docker
Registry](https://img.shields.io/badge/Docker-Registry%203-2496ED?logo=docker&logoColor=white)](https://hub.docker.com/_/registry)
[![Compose](https://img.shields.io/badge/Orquestra%C3%A7%C3%A3o-Docker%20Compose-1D63ED)](https://docs.docker.com/compose/)
[![Auth](https://img.shields.io/badge/Auth-htpasswd%20%2B%20bcrypt-4C1)](#segurança)
[![Status](https://img.shields.io/badge/Status-Pronto%20para%20dev-success)](#início-rápido)

📦 **Registry Docker self-hosted** para ambiente local/laboratório, com
autenticação básica via `htpasswd` e persistência em volume.

------------------------------------------------------------------------

# 📚 Sumário

-   📌 Visão geral
-   🏗️ Arquitetura
-   📁 Estrutura do projeto
-   ⚙️ Pré-requisitos
-   🚀 Início rápido
-   🔧 Operação
-   🔐 Segurança
-   🧯 Troubleshooting
-   🧹 Limpeza

------------------------------------------------------------------------

# 📌 Visão geral

Este projeto publica um **Docker Registry privado** em:

    localhost:5000

Usando a imagem oficial:

    registry:3

Recursos habilitados:

-   🔐 autenticação por usuário/senha (`htpasswd`)
-   🔑 senha protegida com **bcrypt**
-   💾 dados persistidos em `./registry/data`
-   📡 API Docker Registry v2

------------------------------------------------------------------------

# 🏗️ Arquitetura

Padrão arquitetural:

> Monolito conteinerizado de serviço único

Fluxo:

docker client → docker login → registry → valida htpasswd → push/pull →
volume persistente

------------------------------------------------------------------------

# 📁 Estrutura do projeto

    .
    ├── docker-compose.yaml
    ├── README.md
    └── registry/
        ├── auth/
        │   └── htpasswd
        └── data/

------------------------------------------------------------------------

# ⚙️ Pré-requisitos

-   Docker Engine 24+
-   Docker Compose v2+

------------------------------------------------------------------------

# 🚀 Início rápido

## Criar diretórios e usuário

``` bash
mkdir -p registry/auth registry/data

docker run --rm --entrypoint htpasswd httpd:2 -Bbn usuario MinhaSenhaForte123 > registry/auth/htpasswd
```

## Subir registry

``` bash
docker compose up -d
docker compose ps
```

## Login

``` bash
docker login localhost:5000
```

------------------------------------------------------------------------

# 🔧 Operação

## Push

``` bash
docker pull alpine:latest
docker tag alpine:latest localhost:5000/alpine:latest
docker push localhost:5000/alpine:latest
```

## Pull

``` bash
docker pull localhost:5000/alpine:latest
```

------------------------------------------------------------------------

# 🔐 Segurança

Situação atual:

-   autenticação ativa
-   sem TLS

Recomendado para produção:

-   usar HTTPS
-   proteger acesso de rede
-   não versionar htpasswd

------------------------------------------------------------------------

# 🧯 Troubleshooting

## 401 Unauthorized

``` bash
docker logout localhost:5000
docker login localhost:5000
```

## Porta ocupada

Altere no compose:

    5001:5000

------------------------------------------------------------------------

# 🧹 Limpeza

Parar container:

``` bash
docker compose down
```

Remover imagens armazenadas:

``` bash
rm -rf registry/data
```

------------------------------------------------------------------------

# 📜 Licença

Uso interno para laboratório ou desenvolvimento.
