# n8n with PostgreSQL, Ollama, and Open WebUI

This Docker Compose setup provides a comprehensive environment with the following services:

- **n8n**: A workflow automation tool
- **PostgreSQL**: Database for n8n
- **Ollama**: Open-source LLM serving platform
- **Open WebUI**: Web interface for interacting with Ollama

## Start

To start all services, simply run docker-compose by executing the following command in the current folder:

**IMPORTANT:** Before starting, change the default users and passwords in the [`.env`](.env) file!

```
docker-compose up -d
```

To stop all services:

```
docker-compose stop
```

## Accessing Services

After starting the containers, you can access the services at:

- **n8n**: http://localhost:5678
- **Open WebUI**: http://localhost:3000 (or the port specified in OPEN_WEBUI_PORT)
- **Ollama API**: http://localhost:11434

## Configuration

### Environment Variables

To configure the project, create a `.env` file based on the `.env.sample` file provided. This file includes default values for all necessary environment variables.

The following environment variables can be configured in the [`.env`](.env) file:

#### PostgreSQL Configuration

- `POSTGRES_USER`: PostgreSQL root user
- `POSTGRES_PASSWORD`: PostgreSQL root password
- `POSTGRES_DB`: Database name
- `POSTGRES_NON_ROOT_USER`: Non-root user for n8n to connect
- `POSTGRES_NON_ROOT_PASSWORD`: Non-root user password

#### Open WebUI Configuration

- `OPEN_WEBUI_PORT`: Port for Open WebUI (default: 3000)
- `WEBUI_SECRET_KEY`: Secret key for Open WebUI (recommended to set for security)

#### Ollama Configuration

- `OLLAMA_DOCKER_TAG`: Ollama Docker image tag (default: latest)

#### n8n Configuration

- The timezone is set to Asia/Kolkata by default

## Service Details

### n8n

n8n is a workflow automation tool that allows you to connect various services and automate tasks. It uses PostgreSQL as its database for storing workflows and credentials.

### PostgreSQL

PostgreSQL is used as the database for n8n. The setup includes:

- A root user for database administration
- A non-root user for n8n to connect to the database
- An initialization script that sets up the necessary permissions

### Ollama

Ollama is an open-source platform for running LLMs locally. This setup includes:

- NVIDIA runtime support for GPU acceleration
- Persistent storage for downloaded models

### Open WebUI

Open WebUI provides a user-friendly interface for interacting with Ollama. It allows you to:

- Chat with different LLM models
- Manage your models
- Create and share prompts

## Best Practices

1. **Security**:

   - Change the default credentials in the `.env` file
   - Set a strong `WEBUI_SECRET_KEY` for Open WebUI (default is 'changeme')
   - Consider using Docker secrets for production environments

2. **Performance**:

   - For better Ollama performance, ensure your system has adequate GPU resources
   - Resource limits are configured for all services:
     - PostgreSQL: 0.5 CPU, 512MB RAM
     - n8n: 1 CPU, 1GB RAM
     - Ollama: 2 CPUs, 4GB RAM
     - Open WebUI: 0.5 CPU, 512MB RAM
   - Adjust these limits based on your system capabilities

3. **Data Management**:

   - The setup uses named volumes for data persistence
   - Consider regular backups of the PostgreSQL database

4. **Networking**:

   - All services are connected through a custom bridge network 'app-network'
   - Each service has a healthcheck to ensure it's running properly

5. **Integration**:
   - You can use n8n to create workflows that interact with Ollama via its API
