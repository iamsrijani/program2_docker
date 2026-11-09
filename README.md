# program2_docker
# Multi-Stage Docker File for Container Orchestration

## Aim

To develop a multi-stage Docker file for container orchestration using a Node.js and Express application.

---

## Project Structure

```text
program2/
├── src/
│   └── file.js
├── package.json
├── package-lock.json
└── Dockerfile
```

---

## 1. Create Project Directory

Open the terminal and execute:

```bash
mkdir program2
cd program2
mkdir src
```

Create the required files:

```bash
touch src/file.js
touch package.json
touch Dockerfile
```

The `package-lock.json` file will be created automatically after running `npm install`.

---

## 2. Create `src/file.js`

Open the file:

```bash
nano src/file.js
```

Enter the following code:

```javascript
const express = require("express");

const app = express();

const PORT = 3000;

app.get("/", (req, res) => {
    res.send("Hello from multi stage docker");
});

app.listen(PORT, () => {
    console.log(`Multi stage docker running on port ${PORT}`);
});
```

Save the file.

---

## 3. Create `package.json`

Open the file:

```bash
nano package.json
```

Enter:

```json
{
  "name": "program2",
  "version": "1.0.0",
  "description": "Multi stage docker file",
  "main": "dist/file.js",
  "scripts": {
    "start": "node dist/file.js",
    "build": "mkdir -p dist && cp -r src/* dist/"
  },
  "author": "MTS",
  "license": "MIT",
  "dependencies": {
    "express": "^4.18.2"
  }
}
```

Save the file.

---

## 4. Install Dependencies

Run:

```bash
npm install
```

This installs Express and creates the `package-lock.json` file.

Check the files:

```bash
ls
```

Expected:

```text
Dockerfile
package.json
package-lock.json
src
node_modules
```

---

## 5. Create Dockerfile

Open the Dockerfile:

```bash
nano Dockerfile
```

Enter:

```dockerfile
# Stage 1: Build stage
FROM node:20-alpine AS builder

WORKDIR /app

COPY package.json package-lock.json ./

RUN npm install

COPY src ./src

RUN npm run build


# Stage 2: Production stage
FROM node:20-alpine

WORKDIR /app

COPY --from=builder /app/package.json ./package.json
COPY --from=builder /app/package-lock.json ./package-lock.json
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/dist ./dist

EXPOSE 3000

CMD ["node", "dist/file.js"]
```

Save the file

## 6. Build the Docker Image

Execute the following command:

```bash
docker build -t program2 .
```

Here:
- `docker build` – Builds a Docker image.
- `-t program2` – Gives the image the name `program2`.
- `.` – Uses the current directory as the build context.

Check the created image:

```bash
docker images
```

## 7. Run the Docker Container

Execute:

```bash
docker run -d -p 3000:3000 --name prog2 program2
```

Here:
- `-d` – Runs the container in background mode.
- `-p 3000:3000` – Maps host port 3000 to container port 3000.
- `--name prog2` – Assigns the container name `prog2`.
- `program2` – Specifies the Docker image.

## 8. Check the Running Container

Execute:

```bash
docker ps
```

The `prog2` container should appear in the list of running containers.

Expected output:

```text
CONTAINER ID   IMAGE      STATUS       PORTS
xxxxxxxxxxxx   program2   Up           0.0.0.0:3000->3000/tcp
```

## 9. Check Docker Logs

Execute:

```bash
docker logs prog2
```

Expected output:

```text
Multi stage docker running on port 3000
```

## 10. Test the Application

Open the browser and enter:

```text
http://localhost:3000
```

Expected output:

```text
Hello from multi stage docker
```

The application can also be tested using the terminal:

```bash
curl http://localhost:3000
```

Expected output:

```text
Hello from multi stage docker
```

## 11. Stop the Container

To stop the running container:

```bash
docker stop prog2
```

Check the running containers:

```bash
docker ps
```

The stopped container will no longer appear in the running-container list.

## 12. Start the Container Again

To restart the stopped container:

```bash
docker start prog2
```

Check:

```bash
docker ps
```

The container should be running again.

## 13. Useful Docker Commands

**List Docker Images**

```bash
docker images
```

**List Running Containers**

```bash
docker ps
```

**List All Containers**

```bash
docker ps -a
```

**View Container Logs**

```bash
docker logs prog2
```

**Stop Container**

```bash
docker stop prog2
```

**Start Container**

```bash
docker start prog2
```

**Remove Container**

```bash
docker rm prog2
```

**Remove Docker Image**

```bash
docker rmi program2
```

## 14. Complete Execution Commands

If all the project files have already been created, execute:

```bash
npm install
docker build -t program2 .
docker run -d -p 3000:3000 --name prog2 program2
docker ps
docker logs prog2
```

Open the browser:

```text
http://localhost:3000
```

Expected output:

```text
Hello from multi stage docker
```

## 15. Result

The Node.js Express application was successfully containerized using a **multi-stage Dockerfile**. The Docker image was built and the container was executed successfully on port 3000.

The application displayed the output:

```text
Hello from multi stage docker
```

Thus, the multi-stage Docker file for container orchestration was successfully implemented.
