#general #docker 


### 🔹 What is Docker Compose?

**Docker Compose** is a tool used for defining and running **multi-container Docker applications**. Instead of starting each container manually with a long `docker run` command, Compose allows you to define all your services, networks, and volumes in a single **`docker-compose.yml`** file.

---

### 🔹 Basic Concepts

- **Service**: A container configuration (like a web app or database).
    
- **Volume**: A persistent storage location shared with containers.
    
- **Network**: Communication link between containers.
    
- **`docker-compose.yml`**: The YAML file that defines the services, networks, and volumes.
    

---

### 🔹 Basic Usage

#### 1. **Create a `docker-compose.yml` file**

Example:

```yaml
version: '3'
services:
  web:
    image: nginx
    ports:
      - "8080:80"
  db:
    image: postgres
    environment:
      POSTGRES_PASSWORD: example
```

This sets up two containers:

- An **nginx web server** on port 8080
    
- A **PostgreSQL database** with a set password
    

#### 2. **Start the application**

```bash
docker-compose up
```

#### 3. **Start in the background (detached mode)**

```bash
docker-compose up -d
```

#### 4. **Stop the application**

```bash
docker-compose down
```

#### 5. **View logs**

```bash
docker-compose logs
```

#### 6. **Rebuild services (if code changes)**

```bash
docker-compose up --build
```

---

### 🔹 Why Use Docker Compose?

- Simple orchestration of multiple containers
    
- Easy to share setups with teammates (just one YAML file)
    
- Works great for local development and testing
    

---

Let me know if you want an example with a specific tech stack (like Node.js + MongoDB or Python + Redis).