# HTP Analyzer - Dockerized Application

This repository contains the complete HTP (House-Tree-Person) Analyzer application with both backend and frontend services containerized using Docker.

## 📋 Prerequisites

- Docker (v20.10 or higher)
- Docker Compose (v2.0 or higher)
- At least 4GB of free disk space
- 4GB RAM recommended

## 🚀 Quick Start

### 1. Clone the repository

```bash
git clone <your-repo-url>
cd htp-test
```

### 2. Set up environment variables

```bash
# Copy the example environment file
cp .env.example .env

# Edit .env and add your Gemini API key
# GEMINI_API_KEY=your_actual_api_key_here
```

### 3. Ensure model file exists

Make sure `yolo11s.pt` exists in the `htp-analyzer-backend` directory. If you have a trained model, you can replace it.

### 4. Build and start the containers

```bash
# Build and start all services
docker-compose up --build

# Or run in detached mode
docker-compose up -d --build
```

### 5. Access the application

- **Frontend**: <http://localhost:3000>
- **Backend API**: <http://localhost:8000>
- **API Documentation**: <http://localhost:8000/docs>

## 🛠️ Development

### Starting the services

```bash
# Start all services
docker-compose up

# Start in detached mode (background)
docker-compose up -d

# Start specific service
docker-compose up frontend
docker-compose up backend
```

### Stopping the services

```bash
# Stop all services
docker-compose down

# Stop and remove volumes (clears all data)
docker-compose down -v
```

### Viewing logs

```bash
# View all logs
docker-compose logs

# View logs for specific service
docker-compose logs backend
docker-compose logs frontend

# Follow logs in real-time
docker-compose logs -f
```

### Rebuilding after code changes

```bash
# Rebuild all services
docker-compose up --build

# Rebuild specific service
docker-compose up --build backend
docker-compose up --build frontend
```

## 📦 Services

### Backend (FastAPI)

- **Port**: 8000
- **Technology**: Python 3.11, FastAPI, YOLOv11
- **Features**:
  - Image upload and analysis
  - YOLO-based object detection
  - Psychological interpretation with RAG
  - Health check endpoint

### Frontend (Next.js)

- **Port**: 3000
- **Technology**: Next.js 15, React 19, TypeScript
- **Features**:
  - Image upload interface
  - Analysis visualization
  - PDF report generation
  - Responsive design

## 🔧 Configuration

### Environment Variables

#### Backend (.env)

```env
HOST=0.0.0.0
PORT=8000
MODEL_PATH=yolo11s.pt
CONFIDENCE_THRESHOLD=0.25
GEMINI_API_KEY=your_api_key_here
```

#### Frontend (.env)

```env
NODE_ENV=production
NEXT_PUBLIC_API_URL=http://localhost:8000
NEXT_PUBLIC_GEMINI_API_KEY=your_api_key_here
```

### Docker Compose Override

Create `docker-compose.override.yml` for local customizations:

```yaml
version: '3.8'

services:
  backend:
    environment:
      - DEBUG=True
    volumes:
      - ./htp-analyzer-backend:/app  # Enable hot reload
  
  frontend:
    volumes:
      - ./htp-test-analyzer:/app
      - /app/node_modules
```

## 📊 Volumes

The following Docker volumes are created for data persistence:

- `backend-static`: Stores uploaded files and generated reports
- `backend-data`: Stores processed datasets
- `backend-results`: Stores training and evaluation results

To backup volumes:

```bash
docker run --rm -v backend-static:/data -v $(pwd):/backup alpine tar czf /backup/static-backup.tar.gz -C /data .
```

## 🧪 Testing

### Check service health

```bash
# Backend health
curl http://localhost:8000/health

# Frontend health
curl http://localhost:3000
```

### Test API endpoint

```bash
# Upload and analyze an image
curl -X POST "http://localhost:8000/analyze" \
  -H "accept: application/json" \
  -H "Content-Type: multipart/form-data" \
  -F "file=@path/to/your/image.jpg"
```

## 🐛 Troubleshooting

### Container won't start

```bash
# Check container logs
docker-compose logs backend
docker-compose logs frontend

# Check container status
docker-compose ps
```

### Port already in use

```bash
# Change ports in docker-compose.yml
# For example, change "3000:3000" to "3001:3000"
```

### Permission issues

```bash
# On Linux/Mac, fix volume permissions
sudo chown -R $USER:$USER ./htp-analyzer-backend/static
sudo chown -R $USER:$USER ./htp-analyzer-backend/data
```

### Clear everything and restart

```bash
# Stop all containers and remove volumes
docker-compose down -v

# Remove all images
docker-compose down --rmi all

# Rebuild from scratch
docker-compose up --build
```

## 📝 Production Deployment

### Using Docker Compose in production

```bash
# Set production environment
export NODE_ENV=production

# Build with production settings
docker-compose -f docker-compose.yml up -d --build
```

### Using individual Dockerfiles

```bash
# Build backend
cd htp-analyzer-backend
docker build -t htp-backend:latest .

# Build frontend
cd ../htp-test-analyzer
docker build -t htp-frontend:latest .

# Run containers
docker run -d -p 8000:8000 --name htp-backend htp-backend:latest
docker run -d -p 3000:3000 --name htp-frontend htp-frontend:latest
```

## 📚 Additional Resources

- [Backend Documentation](./htp-analyzer-backend/README.md)
- [Frontend Documentation](./htp-test-analyzer/README.md)
- [Docker Documentation](https://docs.docker.com/)
- [Docker Compose Documentation](https://docs.docker.com/compose/)

## 🤝 Contributing

Contributions are welcome! Please read the contributing guidelines first.
