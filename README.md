# HTP Analyzer - Dockerized Application

This repository contains the complete HTP (House-Tree-Person) Analyzer application with both backend and frontend services containerized using Docker.

## 🚀 Quick Start

### 1. Set up environment variables

```bash
# Copy the example environment file
cp .env.example .env

# Edit .env and add your Gemini API key
# GEMINI_API_KEY=your_actual_api_key_here
```

### 2. Ensure model file exists

Make sure `yolo11s.pt` exists in the `htp-analyzer-backend` directory. If you have another trained model, you can replace it.

### 3. Build and start the containers

```bash
docker-compose up --build
```

### 4. Access the application

- **Frontend**: <http://localhost:3000>
- **Backend API**: <http://localhost:8000>
- **API Documentation**: <http://localhost:8000/docs>

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

## 📚 Additional Resources

- [Backend Documentation](./htp-analyzer-backend/README.md)
- [Frontend Documentation](./htp-test-analyzer/README.md)
