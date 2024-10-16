# Getting Started

<details>
  <summary> <h2> 1. Containerization an application</h2></summary>

This is the schema of source code repo
```
├── getting-started-app/
│ ├── .dockerignore
│ ├── package.json
│ ├── README.md
│ ├── spec/
│ ├── src/
│ └── yarn.lock
```
**Step 1**: create a Dockerfile in repo root directory
```bash
cd /path/to/<getting-started-app>
touch Dockerfile
```
**Step 2**: write Dockerfile (refer to concept) <br>
For Angular
```dockerfile
# Angular Dockerfile
FROM node:14 AS build
WORKDIR /app
COPY ./src ./src
COPY package.json ./
RUN npm install
RUN npm run build --prod

FROM nginx:alpine
COPY --from=build /app/dist/angular-app /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

For Django
```dockerfile
# Django Dockerfile
FROM python:3.11-slim

ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1

WORKDIR /code

COPY requirements.txt /code/
RUN pip install -r requirements.txt

COPY . /code/

CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]
```

For postgres

```dockerfile
# Use the official PostgreSQL image as the base image
FROM postgres:13

# Set environment variables for the database
ENV POSTGRES_DB=mydatabase
ENV POSTGRES_USER=myuser
ENV POSTGRES_PASSWORD=mypassword

# Copy initialization scripts into the Docker image
COPY init.sql /docker-entrypoint-initdb.d/

# Expose PostgreSQL port
EXPOSE 5432

# The default command to run PostgreSQL
CMD ["postgres"]
```
**Step 3**: Build the image <br>
In the repo root directory run the following command
```bash
 docker build -t getting-started .
```


**Step 4**: Run the image
```bash
 docker run -dp <127.0.0.1:3000:3000> <getting-started>
# -d = detach - runs the container in the background
# -p = publish - create port mapping
# 127.0.0.1:3000:3000 = host port:container
```
</details>


<details>
<summary><h2> Update the application</h2> </summary>

Update in source code need a new container<br>
**DO NOT** start the new container while your old container is still running <br>
    &rarr; The reason is that the old container is already using the host's port 3000 and only one process on the machine (containers included) can listen to a specific port.<br>
**Step 1**: Obtain ID of the container from the list container
```bash
docker ps
```
**Step 2**Stop container
```bash
docker stop <container_name_or_id>
```
**Step 3**: Remove container
```bash
 docker rm <the-container-id>
```
**Step 4**: Build the new image
```bash
docker build -t <getting-started> .
```
**Step 5**: Run the new image
```bash
docker run -dp <127.0.0.1:3000:3000> <getting-start>

```
</details>



<details>
<summary><h2> 3. Share the application</h2> </summary>

**Step 1**: Sign in to Docker Hub 
```bash
docker login -u <your-username>
```
**Step 2**: Tag the image
```bash
docker tag getting-started <your-username>/getting-started
```
**Step 3**
```bash
docker push <your-username>/getting-started:tagname 
# tagname will default to latest if there none
```
</details>


<details>
<summary><h2> 4. Multi-container application </h2> </summary>


**Step 1:** Create the network
```bash
docker network create my_network
```
**Step 2:** Ensure each container has a dockerfile setup
```
my_project/
├── docker-compose.yml
├── service1/
│   └── Dockerfile
└── service2/
    └── Dockerfile
```

**Step 3:** Create a docker-compose.yml File
```yaml
version: '3'
services:
  service1:
    build: ./service1
    networks:
      - <my_network>

  service2:
    build: ./service2
    networks:
      - <my_network>

networks:
  my_network:
    external: true
```
- Front end angular, backend django and postgres db
```yaml 
version: '3.8'

services:
  frontend:
    build:
      context: ./angular-app
      dockerfile: Dockerfile
    ports:
      - '4200:80'
    depends_on:
      - backend

  backend:
    build:
      context: ./django-app
      dockerfile: Dockerfile
    ports:
      - '8000:8000'
    environment:
      - DATABASE_URL=postgres://user:password@postgres:5432/dbname
    depends_on:
      - postgres

  postgres:
    image: postgres:13
    ports:
      - '5432:5432'
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
      POSTGRES_DB: dbname
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
```
Accessing the Applications
- Frontend (Angular): Open your browser and go to http://localhost:4200.
- Backend (Django): The Django API will be available at http://localhost:8000.

**Step 4:** Build and Run the Multi-Container Setup
```bash
docker-compose up --build
```
**Step 5:** Verify the Network
```bash
docker network inspect <my_network>
```
Accessing the Applications
- Frontend (Angular): Open your browser and go to http://localhost:4200.
- Backend (Django): The Django API will be available at http://localhost:8000.
</details>

***

<details>
<summary><h2> 5. Best practices</h2></summary>

- Use `docker-compose up --build` to build and start the containers.
- Use `docker-compose down` to stop and remove the containers.
- Use `docker-compose exec` to run a command inside a container.
- Use `docker-compose logs` to view the logs of the containers.
- Use `docker-compose ps` to view the status of the containers.

</details>



<details>
<summary><href="#6-intro-to-container-orchestration"> 6. Intro to container orchestration</summary>

- Docker compose is only ideal for local development and simple deployments with a single compose.yml
- For more complex deployments, use container orchestration tools like Kubernetes, Docker Swarm, or Apache Mesos
    &rarr; Multiple YAML files (Deployment, Service, ConfigMap, etc.)

<img src="./assets/dockervskubernetes.png" alt= "Docker vs Kubernetes>

Some popular container orchestration tools are:
- Docker Swarm
- Kubernetes
- Nomad
- Apache Mesos
- Docker Compose
- Rancher
- Red Hat OpenShift
- Google Kubernetes Engine (GKE)
- Amazon Elastic Container Service for Kubernetes (EKS)
- Azure Kubernetes Service (AKS)
- IBM Cloud Kubernetes Service (IKS)
- Pivotal Container Service (PKS)
- VMware Tanzu Kubernetes Grid (TKG)
- OpenShift Container Platform (OCP)
- Red Hat OpenShift Container Platform (ROCP)
- Amazon Elastic Container Service (ECS)
# Ask Puja <3
