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
