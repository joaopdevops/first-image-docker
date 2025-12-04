# 🐳 GUIA COMPLETO - MEU PRIMEIRO DOCKER

> **Objetivo:** Criar uma aplicação web simples rodando em um container Docker  
> **Tempo estimado:** 20-30 minutos  
> **Nível:** Iniciante

---

## 📚 ÍNDICE

1. [Conceitos Básicos](#-conceitos-básicos)
2. [Passo 1: Criar o HTML](#-passo-1-criar-o-arquivo-html)
3. [Passo 2: Criar o Dockerfile](#-passo-2-criar-o-dockerfile)
4. [Passo 3: Construir a Imagem](#-passo-3-construir-a-imagem-docker)
5. [Passo 4: Rodar o Container](#-passo-4-rodar-o-container)
6. [Passo 5: Acessar a Aplicação](#-passo-5-acessar-no-navegador)
7. [Passo 6: Comandos Úteis](#-passo-6-comandos-úteis-explicados)
8. [Passo 7: Limpeza](#-passo-7-limpar-tudo)
9. [Dicas para Apresentação](#-dicas-para-apresentar-ao-mentor)

---

## 🎓 CONCEITOS BÁSICOS

### O que é Docker?
Docker é uma plataforma que permite criar, testar e rodar aplicações em **containers**.

### Container vs Imagem
```
┌─────────────────────────────────────────┐
│  RECEITA DE BOLO  →  IMAGEM             │
│  (arquivo estático)                     │
│                                         │
│  BOLO PRONTO      →  CONTAINER          │
│  (processo rodando)                     │
└─────────────────────────────────────────┘
```

- **Imagem:** Template/molde imutável (arquivo)
- **Container:** Instância rodando da imagem (processo vivo)
- De **1 imagem** você pode criar **vários containers**

### Por que usar Docker?

| Vantagem | Explicação |
|----------|-----------|
| ✅ **Portabilidade** | Roda igual em qualquer sistema operacional |
| ✅ **Isolamento** | Não bagunça seu sistema com instalações |
| ✅ **Reprodutibilidade** | Sempre funciona do mesmo jeito |
| ✅ **Leveza** | Muito mais leve que uma VM completa |
| ✅ **Versionamento** | Controle de versões das aplicações |

---

## 📝 PASSO 1: Criar o arquivo HTML

### O que fazer:
Crie um arquivo chamado: **`index.html`**

### Por quê?
Este será o site que o Nginx vai servir. É a nossa aplicação web.

### Código:
```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Meu Primeiro Docker</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            color: white;
            margin: 0;
        }
        
        .container {
            text-align: center;
            padding: 40px;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 20px;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
        }
        
        h1 {
            font-size: 3em;
            margin-bottom: 20px;
        }
    </style>
</head>
<body>
    <div class="container">
        <div style="font-size: 4em;">🐳</div>
        <h1>Meu Primeiro Docker!</h1>
        <p>Aplicação rodando em container com Nginx</p>
    </div>
</body>
</html>
```

### ✅ Checklist:
- [ ] Arquivo criado com nome exato: `index.html`
- [ ] Código copiado completamente
- [ ] Salvo no diretório do projeto

---

## 🐳 PASSO 2: Criar o Dockerfile

### O que fazer:
Crie um arquivo chamado: **`Dockerfile`** (sem extensão, exatamente assim)

### Por quê?
O Dockerfile é como uma **receita de bolo**. Ele diz ao Docker:
- Qual sistema base usar
- Quais arquivos copiar
- Como configurar a aplicação

### Código:
```dockerfile
# Usar imagem oficial do Nginx como base
FROM nginx:alpine

# Copiar o HTML para o diretório padrão do Nginx
COPY index.html /usr/share/nginx/html/

# Expor a porta 80
EXPOSE 80
```

### 📖 Explicação linha por linha:

#### Linha 1: `FROM nginx:alpine`
```
┌──────────────────────────────────────────┐
│ FROM nginx:alpine                        │
│      │      └─ versão Alpine Linux      │
│      └─ imagem do Nginx (servidor web)  │
└──────────────────────────────────────────┘
```
- **`FROM`**: Começa com uma imagem base pronta
- **`nginx`**: Servidor web popular e rápido
- **`alpine`**: Versão mínima do Linux (~5MB vs ~100MB)
- **Por que nginx?** Já vem com tudo configurado para servir sites
- **Por que alpine?** Leve, rápido, seguro

#### Linha 2: `COPY index.html /usr/share/nginx/html/`
```
┌─────────────────────────────────────────────────────┐
│ COPY index.html /usr/share/nginx/html/             │
│      │           └─ destino DENTRO do container    │
│      └─ arquivo no SEU computador                  │
└─────────────────────────────────────────────────────┘
```
- **`COPY`**: Copia arquivo do host para o container
- **`index.html`**: Nosso arquivo (origem)
- **`/usr/share/nginx/html/`**: Pasta padrão onde o Nginx procura arquivos
- **Resultado:** Seu HTML fica disponível para o Nginx servir

#### Linha 3: `EXPOSE 80`
```
┌──────────────────────────────────────────┐
│ EXPOSE 80                                │
│        └─ porta HTTP padrão              │
└──────────────────────────────────────────┘
```
- **`EXPOSE`**: Documenta qual porta o container usa
- **`80`**: Porta padrão do HTTP (web)
- **Importante:** Isso NÃO abre a porta automaticamente, só documenta
- A porta será mapeada depois com `-p` no `docker run`

### ✅ Checklist:
- [ ] Arquivo criado com nome exato: `Dockerfile` (sem extensão)
- [ ] Todas as 3 linhas presentes
- [ ] Salvo no mesmo diretório do `index.html`

---

## 🔨 PASSO 3: Construir a Imagem Docker

### O que fazer:
No terminal, execute:
```bash
docker build -t my-first-docker .
```

### 📖 Explicação do comando:

```
docker build -t my-first-docker .
│      │     │  │                   └─ contexto (diretório atual)
│      │     │  └─ nome da imagem (tag)
│      │     └─ flag para nomear/taguear
│      └─ comando para construir
└─ Docker CLI
```

#### Parte por parte:

| Parte | O que faz | Por quê |
|-------|-----------|---------|
| `docker build` | Comando para construir uma imagem | Transforma o Dockerfile em imagem executável |
| `-t` | Flag de "tag" (nome) | Permite dar um nome à imagem em vez de usar um ID aleatório |
| `my-first-docker` | Nome da sua imagem | Facilita referenciar depois (melhor que `image-a3f7b2c9`) |
| `.` | Ponto = diretório atual | Diz ao Docker onde está o Dockerfile e arquivos |

### O que acontece internamente:

```
┌─────────────────────────────────────────────────────┐
│ 1. Docker lê o Dockerfile                          │
│ 2. Baixa nginx:alpine (se não existir)             │
│ 3. Cria uma camada com o nginx                     │
│ 4. Copia seu index.html para dentro                │
│ 5. Marca a porta 80 como exposta                   │
│ 6. Salva tudo como imagem "my-first-docker"    │
└─────────────────────────────────────────────────────┘
```

### Saída esperada:
```
[+] Building 2.3s (7/7) FINISHED
 => [internal] load build definition from Dockerfile
 => => transferring dockerfile: 187B
 => [internal] load .dockerignore
 => => transferring context: 2B
 => [internal] load metadata for docker.io/library/nginx:alpine
 => [1/2] FROM docker.io/library/nginx:alpine
 => [internal] load build context
 => => transferring context: 1.2kB
 => [2/2] COPY index.html /usr/share/nginx/html/
 => exporting to image
 => => exporting layers
 => => writing image sha256:abc123...
 => => naming to docker.io/library/my-first-docker
```

### ✅ Checklist:
- [ ] Comando executado sem erros
- [ ] Mensagem "Successfully tagged my-first-docker:latest"
- [ ] Imagem criada (verifique com `docker images`)

---

## 🚀 PASSO 4: Rodar o Container

### O que fazer:
No terminal, execute:
```bash
docker run -d -p 8080:80 --name my-container my-first-docker
```

### 📖 Explicação COMPLETA do comando:

```
docker run -d -p 8080:80 --name my-container my-first-docker
│      │   │  │  │    │   │      │             └─ imagem a usar
│      │   │  │  │    │   │      └─ nome do container
│      │   │  │  │    │   └─ flag para nomear
│      │   │  │  │    └─ porta no container
│      │   │  │  └─ porta no seu PC
│      │   │  └─ flag de port mapping
│      │   └─ detached mode (background)
│      └─ comando para criar e rodar container
└─ Docker CLI
```

#### Detalhamento de cada flag:

##### `-d` (detached mode)
```
SEM -d: Terminal fica travado mostrando logs
COM -d: Container roda em background, terminal livre
```
- **Por quê?** Permite continuar usando o terminal
- **Alternativa:** Sem `-d`, você veria os logs ao vivo (útil para debug)

##### `-p 8080:80` (port mapping)
```
┌──────────────────────────────────────────┐
│  Seu PC          Docker          Nginx   │
│                                           │
│  localhost:8080  →  :80  →  Nginx :80   │
│                                           │
│  Navegador         Túnel       Servidor  │
└──────────────────────────────────────────┘
```
- **`8080`**: Porta no SEU computador (host)
- **`80`**: Porta DENTRO do container
- **Por quê 8080?** Porta 80 pode estar ocupada, 8080 é comum para desenvolvimento
- **Pode mudar?** Sim! Pode usar `-p 3000:80`, `-p 9999:80`, etc.

##### `--name my-container`
```
SEM --name: Container recebe nome aleatório (ex: "hungry_einstein")
COM --name: Você escolhe o nome (ex: "my-container")
```
- **Por quê?** Muito mais fácil referenciar depois
- **Exemplo:** `docker stop my-container` vs `docker stop 7a3f9b2c`

##### `my-first-docker`
- Esta é a **imagem** que você criou no Passo 3
- Docker cria um **container** a partir dessa imagem
- Lembre: Imagem = molde, Container = instância rodando

### O que acontece nos bastidores:

```
1. Docker procura a imagem "my-first-docker"
2. Cria um container a partir dela
3. Inicia o Nginx dentro do container
4. Mapeia a porta 8080 do seu PC para 80 do container
5. Roda em background
6. Retorna o ID do container (ex: a3f7b2c9...)
```

### Saída esperada:
```
a3f7b2c9d8e1f2a3b4c5d6e7f8a9b0c1d2e3f4a5b6c7d8e9f0a1b2c3d4e5f6a7
```
☝️ Esse é o ID único do container (hash)

### ✅ Checklist:
- [ ] Comando executado sem erros
- [ ] ID do container retornado (hash longo)
- [ ] Container rodando (verifique com `docker ps`)

---

## 🌐 PASSO 5: Acessar no Navegador

### O que fazer:
1. Abra seu navegador (Chrome, Firefox, Edge, etc.)
2. Digite na barra de endereço:
```
http://localhost:8080
```

### O que você deve ver:
- 🐳 Ícone de baleia
- Título "Meu Primeiro Docker!"
- Fundo roxo degradê
- Texto "Aplicação rodando em container com Nginx"

### Fluxo completo da requisição:

```
┌────────────────────────────────────────────────────────────┐
│                                                            │
│  1. Você digita: http://localhost:8080                    │
│                                                            │
│  2. Navegador faz requisição HTTP para localhost:8080     │
│                                                            │
│  3. Docker escuta na porta 8080 e redireciona para :80    │
│                                                            │
│  4. Nginx no container recebe na porta 80                 │
│                                                            │
│  5. Nginx serve o arquivo /usr/share/nginx/html/index.html│
│                                                            │
│  6. Docker devolve a resposta para porta 8080             │
│                                                            │
│  7. Navegador renderiza o HTML                            │
│                                                            │
└────────────────────────────────────────────────────────────┘
```

### Testando outras portas:
Se você mudou a porta no `-p`, ajuste:
- `-p 3000:80` → acesse `http://localhost:3000`
- `-p 9999:80` → acesse `http://localhost:9999`

### ✅ Checklist:
- [ ] Navegador abriu a página
- [ ] Vê a baleia 🐳 e o texto
- [ ] Sem mensagens de erro

### ❌ Troubleshooting:

| Problema | Solução |
|----------|---------|
| "Não foi possível conectar" | Verifique se o container está rodando: `docker ps` |
| Porta 8080 ocupada | Use outra porta: `docker run -d -p 9090:80 ...` |
| Página em branco | Verifique logs: `docker logs my-container` |

---

## 🛠️ PASSO 6: Comandos Úteis Explicados

### 1️⃣ Ver containers RODANDO

```bash
docker ps
```

**O que faz:** Lista todos os containers ativos  
**Por quê usar:** Verificar se seu container está rodando  
**Informações mostradas:**
- `CONTAINER ID`: ID único curto
- `COMMAND`: Comando executado
- `CREATED`: Quando foi criado
- `STATUS`: Há quanto tempo está rodando
- `PORTS`: Mapeamento de portas
- `NAMES`: Nome do container

**Exemplo de saída:**
```
CONTAINER ID   IMAGE                  COMMAND                  CREATED         STATUS         PORTS                  NAMES
a3f7b2c9d8e1   my-first-docker   "/docker-entrypoint.…"   2 minutes ago   Up 2 minutes   0.0.0.0:8080->80/tcp   my-container
```

**Variações:**
```bash
docker ps -a         # Mostra TODOS (incluindo parados)
docker ps --format "table {{.Names}}\t{{.Status}}"  # Customizado
```

---

### 2️⃣ Ver TODAS as imagens

```bash
docker images
```

**O que faz:** Lista todas as imagens Docker no seu sistema  
**Por quê usar:** Verificar se a imagem foi criada, ver tamanho  
**Informações mostradas:**
- `REPOSITORY`: Nome da imagem
- `TAG`: Versão (default: latest)
- `IMAGE ID`: ID único
- `CREATED`: Quando foi criada
- `SIZE`: Tamanho da imagem

**Exemplo de saída:**
```
REPOSITORY              TAG       IMAGE ID       CREATED          SIZE
my-first-docker     latest    f9e8d7c6b5a4   10 minutes ago   23.5MB
nginx                   alpine    a1b2c3d4e5f6   2 weeks ago      23.4MB
```

---

### 3️⃣ Ver LOGS do container

```bash
docker logs my-container
```

**O que faz:** Mostra os logs (saída do console) do container  
**Por quê usar:** Debug, ver requisições, encontrar erros  
**Quando usar:** Quando algo não funciona ou quer ver atividade

**Exemplo de saída:**
```
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
...
2025/12/04 10:30:15 [notice] 1#1: start worker processes
```

**Variações:**
```bash
docker logs my-container -f          # Follow (acompanha em tempo real)
docker logs my-container --tail 50   # Últimas 50 linhas
docker logs my-container --since 5m  # Últimos 5 minutos
```

---

### 4️⃣ Entrar DENTRO do container

```bash
docker exec -it my-container sh
```

**O que faz:** Abre um terminal dentro do container rodando  
**Por quê usar:** Explorar, debugar, ver arquivos  
**É como:** Fazer SSH em um servidor

**Explicação do comando:**
```
docker exec -it my-container sh
│      │    │  │              └─ shell (sh para alpine, bash para outros)
│      │    │  └─ nome do container
│      │    └─ flags: -i (interactive) -t (terminal)
│      └─ execute comando em container rodando
└─ Docker CLI
```

**Quando estiver dentro:**
```sh
# Ver arquivos do Nginx
ls /usr/share/nginx/html/

# Ver conteúdo do HTML
cat /usr/share/nginx/html/index.html

# Ver processos rodando
ps aux

# Sair do container
exit
```

**⚠️ Importante:** Alpine usa `sh`, não `bash`

---

### 5️⃣ PARAR o container

```bash
docker stop my-container
```

**O que faz:** Para o container graciosamente (dá 10s para finalizar)  
**Por quê usar:** Quando não precisa mais do container rodando  
**O container:** Ainda existe, só não está rodando  
**Analogia:** Desligar o computador (não deletar)

**Como verificar:**
```bash
docker ps        # Não aparece
docker ps -a     # Aparece com STATUS "Exited"
```

**Para reiniciar depois:**
```bash
docker start my-container
```

---

### 6️⃣ REMOVER o container

```bash
docker rm my-container
```

**O que faz:** Deleta o container permanentemente  
**Por quê usar:** Limpar containers não usados  
**⚠️ Atenção:** Precisa estar parado primeiro  
**Analogia:** Deletar o arquivo da VM

**Se estiver rodando:**
```bash
docker rm -f my-container    # Força remoção (para + remove)
```

**Verificar:**
```bash
docker ps -a    # Container sumiu
```

---

### 7️⃣ REMOVER a imagem

```bash
docker rmi my-first-docker
```

**O que faz:** Deleta a imagem permanentemente  
**Por quê usar:** Limpar espaço, remover versões antigas  
**⚠️ Atenção:** Não pode ter containers usando essa imagem  
**Analogia:** Deletar o instalador/ISO

**Se tiver containers:**
```bash
# Primeiro remover todos os containers que usam a imagem
docker rm $(docker ps -a -q --filter ancestor=my-first-docker)
# Depois remover a imagem
docker rmi my-first-docker
```

---

### 8️⃣ Inspecionar container

```bash
docker inspect my-container
```

**O que faz:** Mostra TODOS os detalhes do container em JSON  
**Por quê usar:** Ver configurações, IPs, volumes, etc  
**Saída:** Arquivo JSON enorme com todas as informações

**Filtrar informação específica:**
```bash
docker inspect my-container | grep IPAddress
docker inspect --format='{{.NetworkSettings.IPAddress}}' my-container
```

---

### 9️⃣ Ver uso de recursos

```bash
docker stats my-container
```

**O que faz:** Mostra uso de CPU, memória, rede em tempo real  
**Por quê usar:** Monitorar performance  
**É como:** Task Manager do Windows

**Saída:**
```
CONTAINER      CPU %     MEM USAGE / LIMIT     MEM %     NET I/O
my-container  0.01%     3.5MiB / 7.775GiB    0.04%     1.2kB / 0B
```

---

## 🧹 PASSO 7: Limpar Tudo

### Opção 1: Passo a passo (recomendado)

```bash
# 1. Parar o container
docker stop my-container

# 2. Remover o container
docker rm my-container

# 3. Remover a imagem
docker rmi my-first-docker
```

**Por quê essa ordem?**
1. Container precisa estar parado para ser removido
2. Imagem não pode ser removida se tiver containers usando

---

### Opção 2: Comando único (força bruta)

```bash
docker stop my-container && docker rm my-container && docker rmi my-first-docker
```

**O que faz:** Executa os 3 comandos em sequência  
**`&&`**: Só executa o próximo se o anterior der certo

---

### Opção 3: Forçar remoção

```bash
docker rm -f my-container    # Para + remove
docker rmi -f my-first-docker
```

**`-f`**: Force (força)  
**⚠️ Cuidado:** Não espera finalização graceful

---

### Limpeza geral do Docker

```bash
# Remover TODOS os containers parados
docker container prune

# Remover TODAS as imagens não usadas
docker image prune

# Remover tudo (containers, imagens, volumes, networks)
docker system prune -a

# Ver quanto espaço vai liberar
docker system df
```

---

## 🎯 DICAS PARA APRESENTAR AO MENTOR

### 1️⃣ Estrutura da Apresentação (15-20 min)

```
┌─────────────────────────────────────────────┐
│ 1. Introdução (2 min)                       │
│    - O que é Docker                         │
│    - Por que usar                           │
│                                             │
│ 2. Explicar Dockerfile (3 min)             │
│    - Linha por linha                        │
│    - Conceito de imagem base                │
│                                             │
│ 3. Build da Imagem (2 min)                 │
│    - Mostrar comando                        │
│    - Explicar flags                         │
│                                             │
│ 4. Run do Container (3 min)                │
│    - Mostrar comando                        │
│    - Explicar port mapping                  │
│    - Mostrar no navegador                   │
│                                             │
│ 5. Comandos Úteis (5 min)                  │
│    - docker ps, logs, exec                  │
│    - Entrar no container                    │
│                                             │
│ 6. Conceitos Aprendidos (3 min)            │
│    - Imagem vs Container                    │
│    - Isolamento                             │
│    - Portabilidade                          │
│                                             │
│ 7. Perguntas (2 min)                        │
└─────────────────────────────────────────────┘
```

---

### 2️⃣ Pontos-Chave para Mencionar

✅ **Diferença entre Imagem e Container**
```
Receita (Dockerfile) → Bolo pronto (Imagem) → Fatia servida (Container)
```

✅ **Port Mapping**
```
Por que localhost:8080 e não :80?
- Porta 80 pode estar ocupada
- Em produção, usaria 80
- :8080 é comum em desenvolvimento
```

✅ **Por que Nginx Alpine?**
```
- Nginx: servidor web rápido e confiável
- Alpine: Linux minimalista (~5MB vs ~100MB Ubuntu)
- Resultado: Imagem pequena e rápida
```

✅ **Dockerfile como Código**
```
- Versionável (Git)
- Reproduzível
- Documentado
- Automatizável
```

---

### 3️⃣ Demonstrações Práticas

#### Demo 1: Build e Run
```bash
# Limpar tudo primeiro
docker stop my-container 2>/dev/null
docker rm my-container 2>/dev/null
docker rmi my-first-docker 2>/dev/null

# Build
docker build -t my-first-docker .

# Run
docker run -d -p 8080:80 --name my-container my-first-docker

# Mostrar rodando
docker ps

# Abrir navegador
```

#### Demo 2: Explorar Container
```bash
# Ver logs
docker logs my-container

# Entrar no container
docker exec -it my-container sh

# Dentro do container:
ls /usr/share/nginx/html/
cat /usr/share/nginx/html/index.html
ps aux
exit

# Ver stats
docker stats my-container --no-stream
```

#### Demo 3: Modificar e Rebuild
```bash
# Editar index.html (mudar algum texto)

# Rebuild
docker build -t my-first-docker .

# Parar container antigo
docker stop my-container
docker rm my-container

# Rodar novo
docker run -d -p 8080:80 --name my-container my-first-docker

# Refresh navegador - ver mudança
```

---

### 4️⃣ Perguntas que o Mentor Pode Fazer

| Pergunta | Resposta |
|----------|----------|
| **"O que é um container?"** | É uma instância isolada rodando a partir de uma imagem, com seu próprio filesystem, processos e rede |
| **"Diferença entre VM e Container?"** | VM virtualiza hardware, Container virtualiza SO. Container é mais leve e rápido |
| **"O que é o Dockerfile?"** | Arquivo de instruções para construir uma imagem Docker, como uma receita |
| **"Por que usar Alpine?"** | Distribuição Linux minimalista, reduz tamanho da imagem e superfície de ataque |
| **"O que significa `-p 8080:80`?"** | Mapeia porta 8080 do host para porta 80 do container |
| **"Posso rodar múltiplos containers da mesma imagem?"** | Sim! Só usar portas diferentes: `-p 8081:80`, `-p 8082:80` |
| **"Os dados persistem se remover o container?"** | Não, containers são efêmeros. Para persistir, usa-se volumes |
| **"Docker é seguro?"** | Sim, com boas práticas: imagens oficiais, sem root, scans de vulnerabilidade |

---

### 5️⃣ Próximos Passos Sugeridos

Mencione para o mentor que você quer aprender:

- [ ] **Docker Compose** - Orquestrar múltiplos containers
- [ ] **Multi-stage builds** - Otimizar tamanho das imagens
- [ ] **Volumes** - Persistir dados
- [ ] **Networks** - Comunicação entre containers
- [ ] **Environment Variables** - Configurações dinâmicas
- [ ] **Docker Hub** - Publicar imagens
- [ ] **CI/CD** - Integrar Docker no pipeline

---

### 6️⃣ Troubleshooting Comum

| Problema | Causa | Solução |
|----------|-------|---------|
| "docker: command not found" | Docker não instalado | Instalar Docker Desktop |
| "permission denied" | Usuário sem permissão | `sudo usermod -aG docker $USER` (Linux) |
| "port is already allocated" | Porta 8080 ocupada | Usar outra porta: `-p 9090:80` |
| "no such file or directory" | Dockerfile não encontrado | Verificar nome (sem extensão) |
| "COPY failed" | index.html não existe | Criar arquivo antes do build |

---

## 📚 GLOSSÁRIO

| Termo | Significado |
|-------|-------------|
| **Container** | Instância isolada rodando de uma imagem |
| **Image** | Template imutável com aplicação e dependências |
| **Dockerfile** | Arquivo de instruções para criar uma imagem |
| **Build** | Processo de criar uma imagem a partir do Dockerfile |
| **Run** | Criar e iniciar um container a partir de uma imagem |
| **Port Mapping** | Conectar porta do host com porta do container |
| **Tag** | Nome/versão da imagem (ex: `nginx:alpine`) |
| **Alpine** | Distribuição Linux minimalista (~5MB) |
| **Nginx** | Servidor web de alta performance |
| **Detached Mode** | Container roda em background (`-d`) |
| **Prune** | Remover recursos não utilizados |
| **Volume** | Armazenamento persistente para containers |

---

## 🚀 BOA SORTE NA APRESENTAÇÃO!

**Lembre-se:**
- 🐳 Docker facilita deploy e portabilidade
- 📦 Imagens são templates, containers são instâncias
- 🔧 Dockerfile é infraestrutura como código
- 🌐 Port mapping conecta host e container
- 🧹 Sempre limpar recursos não usados

**Dica final:** Pratique uma vez antes de apresentar para não ter surpresas!

---

*Criado com ❤️ para iniciantes em Docker*
