# DevOps Final Project

## Project Description

This is a comprehensive DevOps project that demonstrates containerization and orchestration using Docker and Docker Compose. The project consists of a professional portfolio website for Morris Ikebuife (Junior Cloud Engineer) and an optional Java application backend. Both services are containerized and managed through Docker Compose for seamless multi-container orchestration.

### Key Features:
- **Portfolio Website**: A modern, responsive portfolio website served through Nginx
- **Docker Containerization**: Both frontend and backend services are containerized
- **Docker Compose Orchestration**: Automated multi-container deployment and management
- **Scalability**: Services are configured to auto-restart for high availability
- **Optional Java Backend**: Ready for integration with Java application services

---

## Project Structure

```
devops-final-project/
├── docker-compose.yml          # Docker Compose configuration for multi-container setup
├── README.md                   # This file - project documentation
├── portfolio/                  # Portfolio website (frontend)
│   ├── Dockerfile             # Docker configuration for Nginx web server
│   └── index.html             # Portfolio website content
└── java-folder/               # Java application (optional)
    └── Dockerfile             # Docker configuration for Java app (to be created)
```

---

## Getting Started

### Prerequisites

Before you begin, ensure you have the following installed on your system:

- **Docker** (v20.10 or higher)
- **Docker Compose** (v1.29 or higher)
- **Git** (optional, for version control)

#### Installation Links:
- Docker: https://docs.docker.com/get-docker/
- Docker Compose: https://docs.docker.com/compose/install/

### Verification

Verify your installation by running:

```bash
docker --version
docker-compose --version
```

---

## Quick Start

### 1. Clone or Navigate to the Project

```bash
cd devops-final-project
```

### 2. Build and Start Services

```bash
docker-compose up -d --build
```

**Flags:**
- `-d`: Run in detached mode (background)
- `--build`: Build images before starting containers

### 3. Access the Application

- **Portfolio Website**: http://localhost:80 or http://localhost
- **Java Application** (if Dockerfile exists): http://localhost:8080

### 4. View Running Services

```bash
docker-compose ps
```

### 5. View Service Logs

```bash
# View all services
docker-compose logs -f

# View specific service
docker-compose logs -f portfolio
docker-compose logs -f java-app
```

### 6. Stop Services

```bash
docker-compose down
```

---

## Service Configuration

### Portfolio Service

**Image**: nginx:alpine  
**Port**: 80 (HTTP)  
**Restart Policy**: unless-stopped  

The portfolio service serves a static website using Nginx lightweight web server. It includes:
- Modern, responsive design with gradient backgrounds
- Professional portfolio showcasing cloud engineering skills
- Optimized for performance with Nginx

**Dockerfile Details:**
- Based on nginx:alpine (lightweight base image)
- Default Nginx HTML files removed
- Custom index.html deployed to web root

### Java Application Service (Optional)

**Port**: 8080 (Application)  
**Restart Policy**: unless-stopped  

The Java service will only start if a `Dockerfile` exists in the `java-folder` directory. This allows for flexible backend integration.

---

## Development Guide

### Adding or Modifying the Portfolio

1. Edit `portfolio/index.html` with your content
2. Rebuild and restart the service:

```bash
docker-compose up -d --build portfolio
```

3. Verify changes at http://localhost

### Setting Up Java Application

To enable the Java application service:

1. Create a `Dockerfile` in the `java-folder/` directory
2. Add your Java application code and dependencies
3. Rebuild and start:

```bash
docker-compose up -d --build java-app
```

Example Java Dockerfile structure:
```dockerfile
FROM openjdk:17-jdk-slim
WORKDIR /app
COPY . .
RUN javac -d out src/*.java
EXPOSE 8080
CMD ["java", "-cp", "out", "YourMainClass"]
```

### Building Individual Services

```bash
# Build portfolio only
docker-compose build portfolio

# Build java-app only
docker-compose build java-app

# Build all services
docker-compose build
```

---

## Common Commands

### Service Management

```bash
# Start services
docker-compose up -d

# Stop services (keeps data)
docker-compose stop

# Restart services
docker-compose restart

# Remove services (removes containers)
docker-compose down

# Remove everything including volumes
docker-compose down -v
```

### Debugging

```bash
# Inspect service logs
docker-compose logs portfolio

# Execute commands in running container
docker-compose exec portfolio sh

# View service information
docker-compose ps -a

# Validate docker-compose.yml
docker-compose config
```

### Container Inspection

```bash
# List all running containers
docker ps

# List all containers (including stopped)
docker ps -a

# View container logs
docker logs <container_id>

# Access container shell
docker exec -it <container_id> sh
```

---

## Environment Variables

To use environment variables, create a `.env` file in the project root:

```env
# Example .env file
PORT_PORTFOLIO=80
PORT_JAVA=8080
RESTART_POLICY=unless-stopped
```

Then update `docker-compose.yml` to reference them:

```yaml
services:
  portfolio:
    ports:
      - "${PORT_PORTFOLIO}:80"
```

---

## Troubleshooting

### Port Already in Use

If port 80 or 8080 is already in use, modify `docker-compose.yml`:

```yaml
ports:
  - "8000:80"  # Access at http://localhost:8000
```

### Service Won't Start

Check logs:
```bash
docker-compose logs portfolio
```

### Build Failures

Rebuild with no cache:
```bash
docker-compose build --no-cache
```

### Permission Denied (Linux/Mac)

Add your user to docker group:
```bash
sudo usermod -aG docker $USER
```

---

## Performance Optimization

### Image Size Optimization

- Used `nginx:alpine` for smaller footprint (~40MB vs 130MB)
- Consider `openjdk:17-jdk-slim` for Java instead of full JDK

### Resource Limits (Optional)

Add to `docker-compose.yml`:

```yaml
services:
  portfolio:
    deploy:
      resources:
        limits:
          cpus: '0.5'
          memory: 512M
```

---

## Security Best Practices

1. **Regular Updates**: Keep base images updated
   ```bash
   docker pull nginx:alpine
   ```

2. **Minimal Images**: Use alpine or slim variants

3. **Non-root User**: Consider adding user to Dockerfiles

4. **Environment Secrets**: Use `.env` files (add to `.gitignore`)

5. **Network Isolation**: Services communicate through Docker network

---

## Deployment

### Local Deployment
Already covered in Quick Start section.

### Cloud Deployment

For deploying to cloud platforms:

**AWS (ECS/Fargate)**
```bash
# Push to ECR and update task definitions
```

**Azure (Container Instances)**
```bash
# Push to ACR and deploy via Azure CLI
```

**Docker Hub**
```bash
docker tag portfolio:latest username/portfolio:latest
docker push username/portfolio:latest
```

---

## Next Steps

1. Customize portfolio content in `portfolio/index.html`
2. Add Java application to `java-folder/`
3. Implement CI/CD pipeline with GitHub Actions
4. Add database services (MySQL, PostgreSQL) to compose file
5. Configure SSL/TLS with Let's Encrypt
6. Set up monitoring and logging

---

## Support & Resources

- **Docker Documentation**: https://docs.docker.com/
- **Docker Compose Reference**: https://docs.docker.com/compose/
- **Nginx Configuration**: https://nginx.org/en/docs/
- **OpenJDK Documentation**: https://openjdk.java.net/

---

## License

This project is provided as-is for educational and portfolio purposes.

---

## Author

**Morris Ikebuife**  
Junior Cloud Engineer

---

*Last Updated: September 2024*