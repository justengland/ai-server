# AI Server

A Docker Compose setup for running Ollama with Open WebUI.

## Prerequisites

- Docker and Docker Compose
- NVIDIA GPU with appropriate drivers (for GPU support)
- NVIDIA Container Toolkit installed

## Setup

1. Clone the repository:
```bash
git clone https://github.com/justengland/ai-server.git
cd ai-server
```

2. (Optional) Create a local configuration:
   - Copy the example local configuration:
     ```bash
     cp docker-compose.local.yml.example docker-compose.local.yml
     ```
   - Modify `docker-compose.local.yml` to add your custom volume mounts or configurations

3. Start the services:
   - Without local configuration:
     ```bash
     docker compose up -d
     ```
   - With local configuration:
     ```bash
     docker compose -f docker-compose.yml -f docker-compose.local.yml up -d
     ```

## Docker Compose Commands

### Starting Services
```bash
# Start all services in detached mode
docker compose up -d

# Start with local configuration
docker compose -f docker-compose.yml -f docker-compose.local.yml up -d

# Start specific service
docker compose up -d ollama
```

### Stopping Services
```bash
# Stop all services
docker compose down

# Stop with local configuration
docker compose -f docker-compose.yml -f docker-compose.local.yml down

# Stop and remove volumes
docker compose down -v
```

### Viewing Logs
```bash
# View all logs
docker compose logs

# Follow logs
docker compose logs -f

# View specific service logs
docker compose logs -f ollama
```

### Managing Services
```bash
# List running containers
docker compose ps

# Restart services
docker compose restart

# Restart specific service
docker compose restart ollama

# Pull latest images
docker compose pull
```

### Common Issues

1. **GPU Access Issues**
   - Verify NVIDIA drivers are installed: `nvidia-smi`
   - Check NVIDIA Container Toolkit: `docker run --rm --gpus all nvidia/cuda:11.0-base nvidia-smi`

2. **Port Conflicts**
   - Check if ports 11434 and 3000 are available
   - Modify ports in docker-compose.yml if needed

3. **Volume Mount Issues**
   - Ensure directory permissions are correct
   - Check if directories exist before mounting

## Services

- **Ollama**: Running on port 11434
  - API endpoint: http://localhost:11434
  - Model files are stored in the default Docker volume
  - Pull models using: `docker exec -it ollama ollama pull <model-name>`

- **Open WebUI**: Running on port 3000
  - Web interface: http://localhost:3000
  - Data persisted in Docker volume: openwebui_data

## Configuration

### Local Overrides

The `docker-compose.local.yml` file is used for local environment customizations and is ignored by Git. You can use it to:
- Add custom volume mounts
- Override environment variables
- Modify service configurations

Example local configuration:
```yaml
services:
  ollama:
    volumes:
      - /path/to/your/models:/root/.ollama/models
```

### Environment Variables

You can customize the setup using environment variables:
- `OLLAMA_API_BASE_URL`: Ollama API endpoint
- `OLLAMA_BASE_URL`: Ollama base URL
- `CUDA_VISIBLE_DEVICES`: GPU device selection

## License

MIT 