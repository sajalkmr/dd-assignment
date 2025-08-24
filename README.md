In this DevOps task, you need to build and deploy a full-stack CRUD application using the MEAN stack (MongoDB, Express, Angular 15, and Node.js). The backend will be developed with Node.js and Express to provide REST APIs, connecting to a MongoDB database. The frontend will be an Angular application utilizing HTTPClient for communication.  

The application will manage a collection of tutorials, where each tutorial includes an ID, title, description, and published status. Users will be able to create, retrieve, update, and delete tutorials. Additionally, a search box will allow users to find tutorials by title.

## Project setup

### Node.js Server

cd backend

npm install

You can update the MongoDB credentials by modifying the `db.config.js` file located in `app/config/`.

Run `node server.js`

### Angular Client

cd frontend

npm install

Run `ng serve --port 8081`

You can modify the `src/app/services/tutorial.service.ts` file to adjust how the frontend interacts with the backend.

Navigate to `http://localhost:8081/`

## Containerization & Deployment

### Live Deployment
- Tutorials list: http://4.187.185.157/tutorials
- Add tutorial: http://4.187.185.157/add

### Prerequisites
- Docker and Docker Compose on the VM
- Docker Hub account and secrets configured in GitHub

### Deploy (production)
- CI/CD builds and pushes Docker images, then deploys on the VM via SSH using `docker-compose.prod.yml`.
- To deploy manually on the VM:
```
cd ~/dd
echo "DOCKERHUB_USERNAME=<your_dockerhub_username>" > .env
docker compose -f docker-compose.prod.yml up -d
```

Services exposed:
- Port 80 via Nginx reverse proxy
- Routes: `/` to Angular app, `/api` to Node backend

Backend environment:
- `MONGODB_URI` defaults to `mongodb://mongo:27017/dd_db`

### CI/CD (GitHub Actions)
- Workflow: `.github/workflows/ci-cd.yml`
- On push to `main`:
  - Builds and pushes images to Docker Hub
  - SSH deploy to VM (`docker compose -f docker-compose.prod.yml up -d`)

Required repo secrets:
- `DOCKERHUB_USERNAME`, `DOCKERHUB_TOKEN`
- `SSH_HOST`, `SSH_USER`, `SSH_KEY`

### Nginx reverse proxy
- Config: `nginx/nginx.conf`
- Proxies `/` to `frontend:4200`, `/api/` to `backend:8080`

### Screenshots to add
- GitHub Actions run (build + deploy)
- Docker Hub images/tags (backend, frontend)
- `docker compose -f docker-compose.prod.yml ps` on VM
- App UI at `http://4.187.185.157/tutorials` and `http://4.187.185.157/add`
- Nginx config and simple infra diagram
