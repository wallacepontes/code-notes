# Node Lambda Docker

## Table of Contents
- [Node Lambda Docker](#node-lambda-docker)
  - [Table of Contents](#table-of-contents)
  - [Using Docker](#using-docker)
  - [References](#references)

## Using Docker
1. **`docker pull node:22-alpine`**
   → Downloads the lightweight Alpine-based Node.js 22 image from Docker Hub.
   (This is ideal for minimal, production-friendly images.)

2. **`docker run -it --rm --entrypoint sh node:22-alpine`**
   → Starts a temporary container, launches an interactive shell (`sh`), and removes the container automatically after you exit.
   You’re basically “inside” the Node environment now.

3. **`node -v`**
   → Confirms Node.js was installed and prints the version (v22.21.0).

4. **`npm -v`**
   → Confirms the bundled `npm` package manager is available (v10.9.4 in your case).

If you want to **run your own Node project inside Docker**, the next step would typically be to:

1. Create a `Dockerfile` like this:

   ```Dockerfile
   FROM node:22-alpine

   WORKDIR /app
   COPY package*.json ./
   RUN npm install --production
   COPY . .

   CMD ["node", "index.js"]
   ```

2. Then build and run it:

   ```bash
   docker build -t my-node-app .
   docker run -p 3000:3000 my-node-app
   ```

## References
1. https://nodejs.org/en/download/