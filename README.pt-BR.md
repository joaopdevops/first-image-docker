# 🐳 Meu Primeiro Docker 🇧🇷

[Read in English](README.md) | **Leia em Português**

Projeto simples para aprender Docker criando uma aplicação web com Nginx.

## 📋 O que é esse projeto?

Uma página HTML servida pelo servidor web Nginx rodando dentro de um container Docker.

## 🛠️ Tecnologias

- **Docker** - Containerização
- **Nginx** - Servidor web
- **HTML/CSS** - Interface

## 🚀 Como usar

### 1. Construir a imagem Docker

```bash
docker build -t my-first-docker .
```

**Explicação:**
- `docker build` - comando para construir uma imagem
- `-t my-first-docker` - dá o nome "my-first-docker" para a imagem
- `.` - usa o Dockerfile do diretório atual

### 2. Rodar o container

```bash
docker run -d -p 8080:80 --name my-container my-first-docker
```

**Explicação:**
- `docker run` - cria e inicia um container
- `-d` - roda em background (detached)
- `-p 8080:80` - mapeia porta 8080 do host para porta 80 do container
- `--name my-container` - dá um nome ao container
- `my-first-docker` - usa a imagem que criamos

### 3. Acessar a aplicação

Abra o navegador em: **http://localhost:8080**

## 📚 Comandos úteis Docker

### Ver containers rodando
```bash
docker ps
```

### Ver todas as imagens
```bash
docker images
```

### Ver logs do container
```bash
docker logs my-container
```

### Parar o container
```bash
docker stop my-container
```

### Iniciar o container novamente
```bash
docker start my-container
```

### Remover o container
```bash
docker rm my-container
```

### Remover a imagem
```bash
docker rmi my-first-docker
```

### Entrar dentro do container (shell)
```bash
docker exec -it my-container sh
```

## 🧹 Limpeza completa

Para parar e remover tudo:

```bash
docker stop my-container
docker rm my-container
docker rmi my-first-docker
```

Ou em um comando só:

```bash
docker stop my-container && docker rm my-container && docker rmi my-first-docker
```

## 📖 Conceitos Docker aprendidos

1. **Dockerfile** - Arquivo de instruções para criar uma imagem
2. **Imagem** - Template imutável com a aplicação e dependências
3. **Container** - Instância rodando de uma imagem
4. **Port mapping** - Mapear portas do host para o container
5. **Base image** - Usar imagem existente como base (nginx:alpine)

## 🎯 Próximos passos

- Adicionar mais páginas HTML
- Usar Docker Compose para múltiplos serviços
- Criar multi-stage builds
- Adicionar variáveis de ambiente
- Implementar health checks
