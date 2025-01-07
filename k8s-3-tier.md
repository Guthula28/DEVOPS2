* Sure! Here is a simple diagram that illustrates the structure of a 3-tier application architecture.

```
+-----------------------+
|       Frontend        |
|  (Nginx + HTML/CSS)   |
+-----------+-----------+
            |
            |
+-----------v-----------+
|       Backend         |
|   (Node.js + Express) |
+-----------+-----------+
            |
            |
+-----------v-----------+
|       Database        |
|    (MySQL Database)   |
+-----------------------+
```

### Explanation:

- **Frontend (Presentation Tier)**: This layer is responsible for displaying the user interface and user interaction. In our example, it uses Nginx to serve static HTML/CSS files.
- **Backend (Logic Tier)**: This layer handles the application logic and communication between the frontend and the database. It uses Node.js with the Express framework to manage requests and responses.
- **Database (Data Tier)**: This layer stores and manages data. In our example, it uses MySQL to handle database operations.

* This architecture separates the application into three distinct layers, making it easier to manage, scale, and maintain. Each layer can be developed, updated, and scaled independently.

You can embed this explanation and image in the markdown file as well if you like.





* Certainly! Below are Kubernetes manifests to deploy the 3-tier application using Kubernetes instead of Docker Compose. This includes Deployment, Service, and ConfigMap manifests for each tier.

### Project Structure

```
3-tier-app/
├── frontend/
│   ├── index.html
│   └── Dockerfile
├── backend/
│   ├── app.js
│   ├── package.json
│   └── Dockerfile
└── database/
│   ├── init.sql
│   └── Dockerfile
└── kubernetes/
    ├── frontend-deployment.yaml
    ├── backend-deployment.yaml
    ├── database-deployment.yaml
    ├── frontend-service.yaml
    ├── backend-service.yaml
    ├── database-service.yaml
    ├── configmap.yaml
    └── secrets.yaml
```

### Frontend

**frontend/index.html**:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>3-Tier Application</title>
</head>
<body>
    <h1>Welcome to the 3-Tier Application</h1>
    <p>This is the frontend served by Nginx.</p>
</body>
</html>
```

**frontend/Dockerfile**:

```dockerfile
# Use the official Nginx image as a base
FROM nginx:latest

# Copy static files to Nginx
COPY . /usr/share/nginx/html

# Expose port 80
EXPOSE 80

# Start Nginx server
CMD ["nginx", "-g", "daemon off;"]
```

### Backend

**backend/package.json**:

```json
{
  "name": "backend",
  "version": "1.0.0",
  "description": "Backend for 3-tier application",
  "main": "app.js",
  "scripts": {
    "start": "node app.js"
  },
  "dependencies": {
    "express": "^4.17.1",
    "mysql": "^2.18.1"
  }
}
```

**backend/app.js**:

```javascript
const express = require('express');
const mysql = require('mysql');
const app = express();
const port = 3000;

const db = mysql.createConnection({
    host: process.env.DATABASE_HOST,
    user: process.env.DATABASE_USER,
    password: process.env.DATABASE_PASSWORD,
    database: process.env.DATABASE_NAME
});

db.connect(err => {
    if (err) {
        console.error('Error connecting to the database:', err);
        return;
    }
    console.log('Connected to the MySQL database');
});

app.get('/', (req, res) => {
    res.send('Hello from the backend!');
});

app.listen(port, () => {
    console.log(`Backend is running at http://localhost:${port}`);
});
```

**backend/Dockerfile**:

```dockerfile
# Use the official Node.js image as a base
FROM node:14

# Create app directory
WORKDIR /usr/src/app

# Copy package.json and install dependencies
COPY package*.json ./
RUN npm install

# Bundle app source
COPY . .

# Expose port 3000
EXPOSE 3000

# Start the application
CMD ["node", "app.js"]
```

### Database

**database/init.sql**:

```sql
CREATE TABLE IF NOT EXISTS users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    email VARCHAR(255) NOT NULL
);

INSERT INTO users (name, email) VALUES ('John Doe', 'john.doe@example.com');
```

**database/Dockerfile**:

```dockerfile
# Use the official MySQL image as a base
FROM mysql:latest

# Set environment variables for MySQL
ENV MYSQL_ROOT_PASSWORD=rootpassword
ENV MYSQL_DATABASE=mydatabase
ENV MYSQL_USER=user
ENV MYSQL_PASSWORD=password

# Copy the initialization SQL script
COPY init.sql /docker-entrypoint-initdb.d/

# Expose port 3306
EXPOSE 3306
```

### Kubernetes Manifests

**kubernetes/frontend-deployment.yaml**:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend
spec:
  replicas: 2
  selector:
    matchLabels:
      app: frontend
  template:
    metadata:
      labels:
        app: frontend
    spec:
      containers:
      - name: frontend
        image: nginx:latest
        ports:
        - containerPort: 80
        volumeMounts:
        - name: frontend-static
          mountPath: /usr/share/nginx/html
      volumes:
      - name: frontend-static
        configMap:
          name: frontend-config
---
apiVersion: v1
kind: ConfigMap
metadata:
  name: frontend-config
data:
  index.html: |
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>3-Tier Application</title>
    </head>
    <body>
        <h1>Welcome to the 3-Tier Application</h1>
        <p>This is the frontend served by Nginx.</p>
    </body>
    </html>
```

**kubernetes/backend-deployment.yaml**:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: backend
spec:
  replicas: 2
  selector:
    matchLabels:
      app: backend
  template:
    metadata:
      labels:
        app: backend
    spec:
      containers:
      - name: backend
        image: node:14
        ports:
        - containerPort: 3000
        env:
        - name: DATABASE_HOST
          value: database
        - name: DATABASE_USER
          valueFrom:
            secretKeyRef:
              name: mysql-secret
              key: user
        - name: DATABASE_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysql-secret
              key: password
        - name: DATABASE_NAME
          value: mydatabase
        volumeMounts:
        - name: backend-source
          mountPath: /usr/src/app
      volumes:
      - name: backend-source
        configMap:
          name: backend-config
---
apiVersion: v1
kind: ConfigMap
metadata:
  name: backend-config
data:
  package.json: |
    {
      "name": "backend",
      "version": "1.0.0",
      "description": "Backend for 3-tier application",
      "main": "app.js",
      "scripts": {
        "start": "node app.js"
      },
      "dependencies": {
        "express": "^4.17.1",
        "mysql": "^2.18.1"
      }
    }
  app.js: |
    const express = require('express');
    const mysql = require('mysql');
    const app = express();
    const port = 3000;

    const db = mysql.createConnection({
        host: process.env.DATABASE_HOST,
        user: process.env.DATABASE_USER,
        password: process.env.DATABASE_PASSWORD,
        database: process.env.DATABASE_NAME
    });

    db.connect(err => {
        if (err) {
            console.error('Error connecting to the database:', err);
            return;
        }
        console.log('Connected to the MySQL database');
    });

    app.get('/', (req, res) => {
        res.send('Hello from the backend!');
    });

    app.listen(port, () => {
        console.log(`Backend is running at http://localhost:${port}`);
    });
```

**kubernetes/database-deployment.yaml**:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: database
spec:
  replicas: 1
  selector:
    matchLabels:
      app: database
  template:
    metadata:
      labels:
        app: database
    spec:
      containers:
      - name: database
        image: mysql:latest
        ports:
        - containerPort: 3306
        env:
        - name: MYSQL_ROOT_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysql-secret
              key: rootpassword
        - name: MYSQL_DATABASE
          value: mydatabase
        - name: MYSQL_USER
          valueFrom:
            secretKeyRef:
              name: mysql-secret
              key: user
        - name: MYSQL_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysql-secret
              key: password
        volumeMounts:
        - name: init-sql
          mountPath: /docker-entrypoint-initdb.d
      volumes:
      - name: init-sql
        configMap:
          name: database-config
---
apiVersion: v1
kind: ConfigMap
metadata:
  name: database-config
data:
  init.sql: |
    CREATE TABLE IF NOT EXISTS users (
        id INT AUTO_INCREMENT PRIMARY KEY,
        name VARCHAR(255) NOT NULL,
        email VARCHAR(255) NOT NULL
    );

    INSERT INTO users (name, email) VALUES ('John Doe', 'john.doe@example.com');
```

**kubernetes/frontend-service.yaml**:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: frontend-service
spec:
  selector:
    app: frontend
  ports:
    - protocol: TCP
     

 port: 80
      targetPort: 80
  type: LoadBalancer
```

**kubernetes/backend-service.yaml**:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: backend-service
spec:
  selector:
    app: backend
  ports:
    - protocol: TCP
      port: 3000
      targetPort: 3000
  type: ClusterIP
```

**kubernetes/database-service.yaml**:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: database-service
spec:
  selector:
    app: database
  ports:
    - protocol: TCP
      port: 3306
      targetPort: 3306
  type: ClusterIP
```

**kubernetes/secrets.yaml**:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: mysql-secret
type: Opaque
data:
  rootpassword: cm9vdHBhc3N3b3Jk
  user: dXNlcg==
  password: cGFzc3dvcmQ=
```

### Step-by-Step Guide

#### Step 1: Set Up Your Project Structure

* Create the following directory structure and files:

```
3-tier-app/
├── frontend/
│   ├── index.html
│   └── Dockerfile
├── backend/
│   ├── app.js
│   ├── package.json
│   └── Dockerfile
└── database/
    ├── init.sql
    └── Dockerfile
└── kubernetes/
    ├── frontend-deployment.yaml
    ├── backend-deployment.yaml
    ├── database-deployment.yaml
    ├── frontend-service.yaml
    ├── backend-service.yaml
    ├── database-service.yaml
    ├── configmap.yaml
    └── secrets.yaml
```

#### Step 2: Build Docker Images

* Navigate to the root of your project directory and build the Docker images:

```sh
docker build -t frontend ./frontend
docker build -t backend ./backend
docker build -t database ./database
```

#### Step 3: Push Docker Images to a Registry

* Push the Docker images to a container registry (e.g., Docker Hub):

```sh
docker tag frontend <your-dockerhub-username>/frontend
docker tag backend <your-dockerhub-username>/backend
docker tag database <your-dockerhub-username>/database

docker push <your-dockerhub-username>/frontend
docker push <your-dockerhub-username>/backend
docker push <your-dockerhub-username>/database
```

#### Step 4: Apply Kubernetes Manifests

* Navigate to the `kubernetes` directory and apply the manifests:

```sh
kubectl apply -f .
```

#### Step 5: Verify the Application

- Access the frontend by navigating to the LoadBalancer IP provided by the frontend service.
- The frontend should communicate with the backend.
- The backend should communicate with the database.

* This setup provides a Kubernetes-based 3-tier application. You can customize and extend it based on your specific requirements.
