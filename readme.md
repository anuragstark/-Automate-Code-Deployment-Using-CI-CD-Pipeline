# Node.js CI/CD Pipeline with GitHub Actions

![CI/CD Pipeline](https://img.shields.io/badge/CI%2FCD-GitHub%20Actions-blue?style=for-the-badge&logo=github)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=node.js&logoColor=white)
![Express](https://img.shields.io/badge/Express-000000?style=for-the-badge&logo=express&logoColor=white)
![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-38B2AC?style=for-the-badge&logo=tailwind-css&logoColor=white)

## Project Overview

This project demonstrates a complete CI/CD pipeline setup using GitHub Actions to automatically build, test, and deploy a Node.js web application to DockerHub. The application features a beautiful Tailwind CSS interface with a Stark Universe theme.

## Objective

Set up a fully automated CI/CD pipeline that:
- Builds a Node.js web application
- Runs tests automatically
- Creates Docker images
- Pushes images to DockerHub
- Triggers on every push to the main branch

## Technologies Used

- **Node.js 18** - Runtime environment
- **Express.js** - Web framework
- **Docker** - Containerization
- **GitHub Actions** - CI/CD automation
- **DockerHub** - Container registry

## Project Structure

```
nodejs-demo-app/
├── app.js                 # Main application file
├── package.json          # Node.js dependencies
├── Dockerfile           # Docker configuration
├── README.md           # This file
├── public/
│   └── index.html      # Frontend with Tailwind CSS
└── .github/
    └── workflows/
        └── main.yml    # CI/CD pipeline configuration
```

## Setup Instructions

### Prerequisites

Before running this project, ensure you have:
- Node.js 18+ installed
- Docker installed and running
- GitHub account
- DockerHub account

### Local Development Setup

1. **Clone the repository**
   ```bash
   git clone <https://github.com/anuragstark/-Automate-Code-Deployment-Using-CI-CD-Pipeline.git
   cd nodejs-demo-app
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Run the application locally**
   ```bash
   node app.js
   ```
   
   Visit `http://localhost:3000` to see the application.

### Docker Setup

1. **Build Docker image**
   ```bash
   docker build -t nodejs-demo-app .
   ```

2. **Run Docker container**
   ```bash
   docker run -d -p 3000:3000 --name nodeapp nodejs-demo-app
   ```

3. **Test the application**
   ```bash
   curl http://localhost:3000
   ```
   Or visit `http://localhost:3000` in your browser.

4. **Clean up**
   ```bash
   docker stop nodeapp
   docker rm nodeapp
   docker rmi nodejs-demo-app
   ```

## GitHub Secrets Configuration

To enable automatic deployment to DockerHub, configure these secrets in your GitHub repository:

1. Go to your GitHub repository
2. Navigate to **Settings** → **Secrets and variables** → **Actions**
3. Add the following repository secrets:

| Secret Name | Description |
|-------------|-------------|
| `DOCKER_USERNAME` | Your DockerHub username |
| `DOCKER_PASSWORD` | Your DockerHub password or access token |

##  CI/CD Pipeline Workflow

The pipeline automatically triggers on every push to the `main` branch and performs the following steps:

1. **Checkout Repository** - Downloads the latest code
2. **Setup Node.js** - Configures Node.js 18 environment
3. **Install Dependencies** - Runs `npm install`
4. **Run Tests** - Executes `npm test`
5. **Login to DockerHub** - Authenticates with container registry
6. **Build Docker Image** - Creates containerized application
7. **Push to DockerHub** - Deploys image to registry

## Application Details

### API Endpoints

- `GET /` - Serves the main HTML page with Tailwind CSS styling

### Application Code

**app.js**
```javascript
const express = require('express');
const path = require('path');
const app = express();

// Serve static files from "public" folder
app.use(express.static(path.join(__dirname, 'public')));

app.get('/', (req, res) => {
  res.sendFile(path.join(__dirname, 'public', 'index.html'));
});

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```

## Docker Configuration

**Dockerfile**
```dockerfile
# Use official Node.js image
FROM node:18

# Create app directory
WORKDIR /usr/src/app

# Install app dependencies
COPY package*.json ./
RUN npm install

# Copy app source
COPY . .

# Expose port and run app
EXPOSE 3000
CMD [ "node", "app.js" ]
```

## Testing

### Local Testing

1. **Test Node.js application**
   ```bash
   npm install
   node app.js
   # Visit http://localhost:3000
   ```

2. **Test Docker container**
   ```bash
   docker build -t nodejs-demo-app .
   docker run -d -p 3000:3000 --name test-app nodejs-demo-app
   curl http://localhost:3000
   ```

### Automated Testing

The CI/CD pipeline includes automated testing that runs on every push:
- Dependency installation validation
- Application startup verification
- Docker image build testing

## Deployment

### Automatic Deployment

Every push to the `main` branch automatically:
1. Builds the application
2. Runs tests
3. Creates a Docker image
4. Pushes to DockerHub with the tag `latest`

### Manual Deployment

To manually deploy to DockerHub:

```bash
# Build and tag image
docker build -t <your-dockerhub-username>/nodejs-demo-app:latest .

# Push to DockerHub
docker push <your-dockerhub-username>/nodejs-demo-app:latest
```

## Troubleshooting

### Common Issues

1. **Docker permission denied**
   ```bash
   sudo usermod -aG docker $USER
   # Then log out and log back in
   ```

2. **Port already in use**
   ```bash
   # Find and kill process using port 3000
   sudo lsof -i :3000
   sudo kill -9 <PID>
   ```

3. **Container not accessible in browser**
   - Ensure port mapping: `-p 3000:3000`
   - Check if container is running: `docker ps`

### Logs and Debugging

```bash
# View container logs
docker logs <container-name>

# Execute commands inside container
docker exec -it <container-name> bash

# View GitHub Actions logs
# Go to Actions tab in your GitHub repository
```

## Monitoring and Metrics

- **GitHub Actions Dashboard**: Monitor pipeline status
- **DockerHub Repository**: Track image pulls and updates
- **Application Logs**: Monitor via Docker logs


## 🙏 Acknowledgments

- **Stark Universe** - For the thematic inspiration
- **GitHub Actions** - For providing excellent CI/CD capabilities
- **Docker** - For containerization technology


---

**Made with ❤️ in the  Anurag Stark**