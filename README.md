# 🐳 My First Docker 🇺🇸

**Read in English** | [Leia em Português](README.pt-BR.md)

Simple project to learn Docker by creating a web application with Nginx.

## 📋 What is this project?

An HTML page served by Nginx web server running inside a Docker container.

## 🛠️ Technologies

- **Docker** - Containerization
- **Nginx** - Web server
- **HTML/CSS** - Interface

## 🚀 How to use

### 1. Build the Docker image

```bash
docker build -t my-first-docker .
```

**Explanation:**
- `docker build` - command to build an image
- `-t my-first-docker` - gives the name "my-first-docker" to the image
- `.` - uses the Dockerfile in the current directory

### 2. Run the container

```bash
docker run -d -p 8080:80 --name my-container my-first-docker
```

**Explanation:**
- `docker run` - creates and starts a container
- `-d` - runs in background (detached mode)
- `-p 8080:80` - maps port 8080 on host to port 80 in container
- `--name my-container` - gives a name to the container
- `my-first-docker` - uses the image we created

### 3. Access the application

Open your browser at: **http://localhost:8080**

## 📚 Useful Docker commands

### View running containers
```bash
docker ps
```

### View all images
```bash
docker images
```

### View container logs
```bash
docker logs my-container
```

### Stop the container
```bash
docker stop my-container
```

### Start the container again
```bash
docker start my-container
```

### Remove the container
```bash
docker rm my-container
```

### Remove the image
```bash
docker rmi my-first-docker
```

### Enter inside the container (shell)
```bash
docker exec -it my-container sh
```

## 🧹 Complete cleanup

To stop and remove everything:

```bash
docker stop my-container
docker rm my-container
docker rmi my-first-docker
```

Or in one command:

```bash
docker stop my-container && docker rm my-container && docker rmi my-first-docker
```

## 📖 Docker concepts learned

1. **Dockerfile** - Instructions file to create an image
2. **Image** - Immutable template with the application and dependencies
3. **Container** - Running instance of an image
4. **Port mapping** - Map ports from host to container
5. **Base image** - Use existing image as base (nginx:alpine)

## 🎯 Next steps

- Add more HTML pages
- Use Docker Compose for multiple services
- Create multi-stage builds
- Add environment variables
- Implement health checks

---

*Made with ❤️ for Docker beginners*
