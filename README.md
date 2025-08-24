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

### Prerequisites
- Docker and Docker Compose on your machine/VM
- Docker Hub account and tokens/secrets

### Build and run locally with Docker Compose

```
docker compose up -d --build
```

Services exposed:
- Nginx reverse proxy: http://localhost (routes `/` to Angular dev server, `/api` to Node)
- MongoDB: `mongo:27017` (internal)

Environment used by backend:
- `MONGODB_URI` defaulted to `mongodb://mongo:27017/dd_db` via compose

### Docker images
- Backend: `dd-backend` (built from `backend/`)
- Frontend: `dd-frontend` (built from `frontend/`)

## CI/CD (GitHub Actions)

Workflow: `.github/workflows/ci-cd.yml`
- On push to `main`:
  - Builds and pushes images to Docker Hub
  - SSH to VM and runs `docker compose up -d --build`

Required repo secrets:
- `DOCKERHUB_USERNAME`
- `DOCKERHUB_TOKEN`
- `SSH_HOST`
- `SSH_USER`
- `SSH_KEY` (private key)

## VM Setup (Ubuntu)
1. Install Docker & Compose
2. Create a working directory, e.g. `~/dd`
3. Ensure port 80 is open in firewall/security group
4. First-time: pull repo or let workflow copy `docker-compose.yml` and `nginx` folder

Run on VM:
```
cd ~/dd
docker compose up -d --build
```

## Nginx reverse proxy
- Defined in `nginx/nginx.conf`
- Proxies `/` to `frontend:4200`, `/api/` to `backend:8080`

## Notes
- Frontend service uses Angular dev server in container for simplicity; for production you can build static files and serve via Nginx.
- Backend reads `MONGODB_URI` from environment.
