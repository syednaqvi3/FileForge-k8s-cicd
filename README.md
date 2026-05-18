# FileForge — Full-Stack File Processing System

Production-grade file processing system built with **React + Vite**, **Node.js/Express**, **MongoDB**, and **Nginx**.

```
┌─────────────┐     ┌──────────────────┐     ┌────────────────┐
│   Browser   │────▶│  Nginx :80       │────▶│  React/Vite    │
│             │     │  Reverse Proxy   │     │  Frontend :3000│
└─────────────┘     └────────┬─────────┘     └────────────────┘
                             │ /api/*
                    ┌────────▼─────────┐     ┌────────────────┐
                    │  Node.js/Express │────▶│  MongoDB :27017│
                    │  Backend :5000   │     └────────────────┘
                    └──────────────────┘
```

## Quick Start

```bash
1. Apply all yml files
kubectl apply -f .

2. Open the app
open http://server-ip

3. Change Ip address 
/FileForge-k8s-cicd/nginx/default.conf (line no 3)
/FileForge-k8s-cicd/backend/server.js (line no 42)

5. Service Names
Keep service Name same as written 
(frontend , backend, mongo, nginx)
```
## Project Structure

```
FileForge-k8s-cicd/
├── mongo-init.js
├── nginx/
│   └── Default.conf
├── frontend/                  ← Vite + React
│   ├── vite.config.js
│   ├── index.html
│   ├── Dockerfile
│   └── src/
│       ├── main.jsx
│       ├── App.jsx
│       ├── App.css
│       ├── index.css
│       ├── utils/api.js
│       └── components/
│           ├── UploadZone.jsx
│           ├── FileGrid.jsx
│           ├── FilterBar.jsx
│           └── StatsPanel.jsx
├── backend/                   ← Express API
|   ├── server.js
|   ├── Dockerfile
|   ├── .env.example
|   ├── config/database.js
|   ├── models/File.js
|   ├── middleware/upload.js
|   ├── controllers/fileController.js
|   └── routes/files.js
└── k8s/                   ← Kubernetes
|   ├── frontend/
|       ├── frontend-deployment.yml
|       └── frontend-service.yml
|   ├── backend/
|       ├── backend-deployment.yml
|       └── backend-service.yml
|   ├── db/
|       ├── mongo-deployment.yml
|       └── mongo-service.yml
|   ├── storage/
|       ├── pv.yml
|       └── pvc.yml
|   ├── nginx/
|       ├── nginx-deployment.yml
|       └── nginx-service.yml
|   └── frontend/
|       └── secrets.yml
 ```

## API Reference

| Method   | Endpoint                      | Description              |
|----------|-------------------------------|--------------------------|
| `POST`   | `/api/files/upload`           | Upload files (multipart) |
| `GET`    | `/api/files`                  | List files + pagination  |
| `GET`    | `/api/files/:fileId`          | Get file metadata        |
| `GET`    | `/api/files/:fileId/download` | Download file            |
| `DELETE` | `/api/files/:fileId`          | Delete file              |
| `GET`    | `/api/files/stats/summary`    | Storage analytics        |
| `GET`    | `/api/health`                 | Health check             |

## Features

- Drag-and-drop multi-file upload with progress bars
- Auto category detection (image, video, audio, document, archive)
- Search, filter by category & status
- Analytics dashboard with storage breakdown
- 7-day auto-expiry via MongoDB TTL
- Rate limiting at Nginx (10/min upload, 30/min API)
- Security headers via Helmet + Nginx
- Gzip compression
- Docker health checks on all containers
