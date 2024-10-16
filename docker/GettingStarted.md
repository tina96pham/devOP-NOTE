# Getting Started
## Table of Content
[1. Containerization an application](#1-containerization-an-application)
[2. Update the application](#2-uodate-the-app)
[3. Share the application](#3-build-the-application)
[4. Persist DB](#4-persist-db)
[5. Use bind mount](#5-use-bind-mount)
[6. Multi-container applictaion](#6.multi-container-application)
[7. Use Docker compose](#7-use-docker-compose)
[8. Best practice](#8-best-practice)
[9. Intro to container orchestration](#9-intro-to-container-orchestration)

## 1. Containerization an application
- This is the schema of source code repo
```
├── getting-started-app/
│ ├── .dockerignore
│ ├── package.json
│ ├── README.md
│ ├── spec/
│ ├── src/
│ └── yarn.lock
```
- Step 1: create a Dockerfile in repo root directory
```bash
cd /path/to/getting-started-app
touch Dockerfile
```
- Step 2: write Dockerfile (refer to concept)
- Step 3: Build the image<br>
In the repo root directory run the following command
```bash
 docker build -t getting-started .
```
- Step 4: Run the image
```bash
 docker run -dp 127.0.0.1:3000:3000 getting-started
```
- List container
```bash
docker ps
```
- Stop container
```bash
docker stop <container_name_or_id>
```

## 2. Update the application
- Update in source code need a new container
- **DO NOT** start the new container while your old container is still running <br>
    &rarr; The reason is that the old container is already using the host's port 3000 and only one process on the machine (containers included) can listen to a specific port.


## 3. Share the application
## 4. Persist DB
## 5. Use bind mount
## 6. Multi-container application
## 7. Use Docker compose
## 8. Best practice
## 9. Intro to container orchestration
