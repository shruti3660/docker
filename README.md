# docker
.....
# Docker Experiments – CCL Practical

## 1) recipe-local (Express app using Dockerfile)

### Step 1: Go to project folder

```bash
cd recipe-local
```

### Step 2: Create Dockerfile

```dockerfile
FROM node:alpine

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 3000

CMD ["node", "app.js"]
```

### Step 3: Create .dockerignore

```
node_modules
package-lock.json
.env
```

### Step 4: Build Docker image

```bash
docker build -t recipe-local .
```

### Step 5: Run container (required port: 8082)

```bash
docker run -d -p 8082:3000 -e PORT=3000 -e APP_NAME="Recipe App" recipe-local
```

### Step 6: Open in browser

```
http://localhost:8082
```

---

## 2) recipe-static (HTML/CSS/JS site)

No Dockerfile required.

### Step 1: Go to project folder

```bash
cd recipe-static
```

### Step 2: Run using nginx

```bash
docker run -d -p 8082:80 -v ${PWD}:/usr/share/nginx/html nginx
```

### Step 3: Open in browser

```
http://localhost:8082
```

---

## Important Notes

* Only one app can use port 8082 at a time. Stop previous container if needed.
* For Express app:

  * Container runs on port 3000
  * Host port must be 8082
* For static app:

  * nginx serves on port 80 inside container
* Do not zip or rebuild unnecessarily during Docker run commands.
* Use `docker ps` to check running containers.

---

## Cleanup (after experiment)

```bash
docker stop $(docker ps -q)
docker rm $(docker ps -aq)
docker rmi $(docker images -q)
```
