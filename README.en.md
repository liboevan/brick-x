[中文](README.md) | English

# Brick X - Docker Compose Orchestration

This directory contains Docker Compose orchestration configuration for the Brick X system, focusing on multi-service coordination and deployment.

## 🏗️ Architecture

### Docker Compose Responsibilities
- **Service Orchestration** - Network, dependencies, health checks
- **Environment Management** - Unified environment variables and configuration
- **Service Discovery** - Inter-service network communication
- **Health Monitoring** - Ensure correct service startup order

### Service Composition
- **brick-x-auth-service**: Authentication service (port 17101)
- **brick-x-webapp**: Web application (port 17100)
- **brick-x-clock**: Clock service (port 17103)

## 🚀 Quick Start

### Prerequisites
Ensure all services are built:
```bash
cd brick-x-auth-service && ./scripts/build.sh
cd brick-x-webapp && ./scripts/build.sh
cd brick-x-clock && ./scripts/build.sh
```

### Start All Services
```bash
cd brick-x-compose
docker-compose up -d
```

### Basic Commands
```bash
# Start all services
docker-compose up -d

# Stop all services
docker-compose down

# Check status
docker-compose ps

# View logs
docker-compose logs

# Restart specific service
docker-compose restart brick-x-auth-service
```

## 📋 Files

### Core Configuration
- **`docker-compose.yml`** - Service definitions and orchestration
- **`scripts/start.sh`** - Start all services
- **`scripts/stop.sh`** - Stop all services
- **`scripts/status.sh`** - Show service status
- **`scripts/logs.sh`** - Show service logs

## 🔧 Docker Compose Configuration

### Network
- **el-brick-x-network**: Bridge network for service communication

### Dependencies
- Webapp depends on auth service health status
- Clock service runs independently
- Health checks ensure correct startup order

### Health Checks
- Auth service: `http://localhost:17101/health`
- Webapp: `http://localhost:17100/`
- Clock service: `http://localhost:17103/health`

## 🌐 Service Addresses

- **Authentication Service**: http://localhost:17101
- **Web Application**: http://localhost:17100
- **Clock Service**: http://localhost:17103

## 📊 Monitoring

### Status Commands
```bash
# Show all services
docker-compose ps

# Check specific service
docker-compose ps brick-x-auth-service
```

### Log Commands
```bash
# All service logs
docker-compose logs

# Follow logs
docker-compose logs -f

# Specific service logs
docker-compose logs brick-x-auth-service
docker-compose logs brick-x-webapp
docker-compose logs brick-x-clock
```

## 🔄 Workflow

### Development Workflow
1. **Build Services**:
   ```bash
   cd brick-x-auth-service && ./scripts/build.sh
   cd brick-x-webapp && ./scripts/build.sh
   cd brick-x-clock && ./scripts/build.sh
   ```

2. **Start with Compose**:
   ```bash
   cd brick-x-compose
   docker-compose up -d
   ```

3. **Monitor and Debug**:
   ```bash
   docker-compose ps
   docker-compose logs
   ```

### Production Workflow
1. **Deploy Services** (handled by individual service scripts)
2. **Use Compose for Orchestration**:
   ```bash
   docker-compose up -d
   ```
3. **Monitor Health Status**:
   ```bash
   docker-compose ps
   ```

## 🐛 Troubleshooting

### Common Issues

1. **Services Won't Start**
   ```bash
   # Check if images exist
   docker images | grep brick-x
   
   # Check individual service status
   cd brick-x-auth-service && ./scripts/run.sh status
   cd brick-x-webapp && ./scripts/run.sh status
   cd brick-x-clock && ./scripts/run.sh status
   ```

2. **Health Check Failures**
   ```bash
   # Check service logs
   docker-compose logs
   
   # Manual endpoint testing
   curl http://localhost:17101/health
   curl http://localhost:17100/
   curl http://localhost:17103/health
   ```

3. **Network Issues**
   ```bash
   # Check network
   docker network ls | grep el-brick-x-network
   
   # Recreate if needed
   docker network rm el-brick-x-network
   docker-compose up -d
   ```

### Debug Commands
```bash
# Check compose status
docker-compose ps

# View recent logs
docker-compose logs --tail 50

# Test endpoints
curl http://localhost:17101/health
curl http://localhost:17100/
curl http://localhost:17103/health

# Check container details
docker inspect el-brick-x-auth-service
docker inspect el-brick-x-webapp
docker inspect el-brick-x-clock
```

## 🎯 Best Practices

1. **Build services first** - Always build images before starting compose
2. **Use compose for orchestration** - Let compose handle multi-service coordination
3. **Monitor health checks** - Ensure services are healthy before proceeding
4. **View logs** - Use `docker-compose logs` to monitor service status
5. **Backup configuration** - Backup `docker-compose.yml` file 