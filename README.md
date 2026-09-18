## Nginx

Here is the step-by-step procedure to run a pre-made Nginx Docker image and customize its default landing page.

**1.Create the project directory:**

Create a new folder for your project and navigate into it using your terminal:

  

Bash

```
mkdir my-nginx-project
cd my-nginx-project
```

**2.Create a custom home page:**

Create an `index.html` file inside your project directory to serve as your new landing page:

  

Bash

```
echo "<h1>Welcome to My Custom Nginx Server!</h1>" > index.html
```

**3.Run the Nginx Docker container with custom page mapped:**

Run the official Nginx container, mounting your custom `index.html` file over Nginx's default welcome page (`/usr/share/nginx/html/index.html`):

  

Bash

```
docker run -d --name my-custom-nginx -p 8080:80 -v $(pwd)/index.html:/usr/share/nginx/html/index.html nginx
```

- **`-d`**: Runs the container in detached mode (background).
    
      
    
- **`--name my-custom-nginx`**: Assigns a custom name to your container.
    
      
    
- **`-p 8080:80`**: Maps port 8080 on your host machine to port 80 inside the container.
    
      
    
- **`-v $(pwd)/index.html:/usr/share/nginx/html/index.html`**: Mounts your local `index.html` into the container's default Nginx root directory.
    
      
    
- **`nginx`**: Downloads and runs the official Nginx image from Docker Hub.
    
      
    

**4.Verify the server is running:**

Open your web browser and visit `http://localhost:8080` (or run `curl http://localhost:8080` in your terminal) to view your custom home page.

  

**5.Update the home page on the fly:**

Because your local `index.html` file is volume-mounted into the container, any updates you make locally will update the web page instantly without needing to restart the container:

  

Bash

```
echo "<h1>Updated Home Page Content</h1>" > index.html
```

Refresh your browser at `http://localhost:8080` to see the changes immediately.

  

**6.Stop and remove the container when finished:**

To clean up and stop the running container:

Bash

```
docker stop my-custom-nginx
docker rm my-custom-nginx
```

## Apache

Here is the step-by-step procedure to run a pre-made Apache Docker image and customize its default landing page.

  

**1.Create the project directory:**

Create a new folder for your Apache project and navigate into it using your terminal:

  

Bash

```
mkdir my-apache-project
cd my-apache-project
```

**2.Create a custom home page:**

Create an `index.html` file inside your project directory to serve as your new landing page:

  

Bash

```
echo "<h1>Welcome to My Custom Apache Server!</h1>" > index.html
```

**3.Run the Apache Docker container with custom page mapped:**

Run the official Apache container (`httpd`), mounting your custom `index.html` file over Apache's default web root (`/usr/local/apache2/htdocs/index.html`):

  

Bash

```
docker run -d --name my-custom-apache -p 8080:80 -v $(pwd)/index.html:/usr/local/apache2/htdocs/index.html httpd
```

- **`-d`**: Runs the container in detached mode (background).
    
      
    
- **`--name my-custom-apache`**: Assigns a custom name to your container.
    
      
    
- **`-p 8080:80`**: Maps port 8080 on your host machine to port 80 inside the container.
    
      
    
- **`-v $(pwd)/index.html:/usr/local/apache2/htdocs/index.html`**: Mounts your local file into Apache's default web root directory.
    
      
    
- **`httpd`**: Downloads and runs the official Apache HTTP Server image from Docker Hub.
    
      
    

**4.Verify the server is running:**

Open your web browser and visit `http://localhost:8080` (or run `curl http://localhost:8080` in your terminal) to view your custom home page.

  

**5.Update the home page on the fly:**

Since your local `index.html` file is volume-mounted into the container, changes made locally reflect immediately without restarting the container:

  

Bash

```
echo "<h1>Updated Apache Home Page Content</h1>" > index.html
```

Refresh your browser at `http://localhost:8080` to see the live updates.

  

**6.Stop and remove the container when finished:**

To stop and remove the running container when done:

Bash

```
docker stop my-custom-apache
docker rm my-custom-apache
```

## WordPress

Running WordPress via Docker requires two components: the **WordPress** web server container and a **MySQL** or **MariaDB** database container to store the site's data.

  

**1.Create the project directory:**

Create a new folder for your WordPress project and navigate into it using your terminal:

  

Bash

```
mkdir my-wordpress-project
cd my-wordpress-project
```

**2.Create a dedicated Docker network:**

Create a network so the WordPress container and the Database container can communicate securely with each other:

  

Bash

```
docker network create wp-net
```

**3.Run the MySQL Database container:**

Launch a MySQL container attached to your network with pre-configured database credentials:

  

Bash

```
docker run -d \
  --name wp-db \
  --network wp-net \
  -e MYSQL_ROOT_PASSWORD=rootsecret \
  -e MYSQL_DATABASE=wordpress \
  -e MYSQL_USER=wpuser \
  -e MYSQL_PASSWORD=wppassword \
  mysql:8.0
```

**4.Run the WordPress container:**

Launch the official WordPress container linked to the database container using environment variables:

  

Bash

```
docker run -d \
  --name my-wordpress \
  --network wp-net \
  -p 8080:80 \
  -e WORDPRESS_DB_HOST=wp-db:3306 \
  -e WORDPRESS_DB_NAME=wordpress \
  -e WORDPRESS_DB_USER=wpuser \
  -e WORDPRESS_DB_PASSWORD=wppassword \
  wordpress
```

- **`-p 8080:80`**: Maps port 8080 on your host machine to port 80 inside the container.
    
      
    
- **`-e WORDPRESS_DB_HOST=wp-db:3306`**: Connects WordPress to the MySQL container by name over the `wp-net` network.
    
      
    

**5.Complete the initial WordPress installation:**

Open your web browser and go to `http://localhost:8080`. Follow the on-screen WordPress setup wizard to choose your language, set up your admin username, password, and site title.

  

**6.Change the home page:**

Unlike Nginx or Apache, WordPress manages home pages through its admin panel or theme files.

  

**Option A: Change via WordPress Dashboard (Recommended)**

  

1. Log into your WordPress admin dashboard at `http://localhost:8080/wp-admin`.
    
      
    
2. Go to **Pages > Add New** and create a page named **Home**.
    
      
    
3. Add your custom content using the block editor and click **Publish**.
    
      
    
4. Navigate to **Settings > Reading**.
    
      
    
5. Under **Your homepage displays**, select **A static page**, set **Homepage** to **Home**, and click **Save Changes**.
    
      
    

**Option B: Change via Mounted File System (Volume)**

If you want to edit or override front-end files directly on your host machine, mount the site files from the container:

  

Bash

```
# Copy WordPress files to host directory
docker cp my-wordpress:/var/www/html ./wp-content-local
```

You can then edit theme files directly (e.g., `./wp-content-local/wp-content/themes/twentytwentyfour/templates/index.html`) to modify the front page programmatically.

  

**7.Stop and clean up containers:**

When you are finished, stop and remove the containers and network:

Bash

```
docker stop my-wordpress wp-db
docker rm my-wordpress wp-db
docker network rm wp-net
```

## Mysql

Here is the step-by-step procedure to run a pre-made MySQL Docker image, connect to it, and customize its default database or initial data.

  

**1.Create the project directory:**

Create a new folder for your MySQL project and navigate into it using your terminal:

  

Bash

```
mkdir my-mysql-project
cd my-mysql-project
```

**2.Create an initialization SQL script:**

Create an `init.sql` file in your project directory. When MySQL starts for the first time, any `.sql` file mounted in `/docker-entrypoint-initdb.d/` will execute automatically to set up your tables and initial data:

  

Bash

```
echo "CREATE TABLE welcome_message (id INT AUTO_INCREMENT PRIMARY KEY, message VARCHAR(255));" > init.sql
echo "INSERT INTO welcome_message (message) VALUES ('Welcome to My Custom MySQL Server!');" >> init.sql
```

**3.Run the MySQL Docker container:**

Launch the official MySQL container, setting the root password, a default database, and mounting your initialization script:

  

Bash

```
docker run -d \
  --name my-custom-mysql \
  -p 3306:3306 \
  -e MYSQL_ROOT_PASSWORD=mysecretpassword \
  -e MYSQL_DATABASE=mydatabase \
  -v $(pwd)/init.sql:/docker-entrypoint-initdb.d/init.sql \
  mysql:8.0
```

- **`-d`**: Runs the container in detached mode (background).
    
      
    
- **`--name my-custom-mysql`**: Assigns a custom name to your container.
    
      
    
- **`-p 3306:3306`**: Maps port 3306 on your host machine to port 3306 inside the container.
    
      
    
- **`-e MYSQL_ROOT_PASSWORD=mysecretpassword`**: Sets the mandatory MySQL root password.
    
      
    
- **`-e MYSQL_DATABASE=mydatabase`**: Automatically creates an initial database named `mydatabase`.
    
      
    
- **`-v $(pwd)/init.sql:/docker-entrypoint-initdb.d/init.sql`**: Mounts your script to run at container startup.
    
      
    

**4.Verify the database and initial data:**

Access the MySQL client running inside the container to query your initial table and message:

  

Bash

```
docker exec -it my-custom-mysql mysql -u root -pmysecretpassword -e "USE mydatabase; SELECT * FROM welcome_message;"
```

You should see `Welcome to My Custom MySQL Server!` printed in the output.

  

**5.Persist data across container restarts (Optional):**

By default, MySQL data inside a container is temporary. To ensure your database data persists even if the container is removed, mount a host directory or named volume to `/var/lib/mysql`:

  

Bash

```
docker run -d \
  --name my-persistent-mysql \
  -p 3306:3306 \
  -e MYSQL_ROOT_PASSWORD=mysecretpassword \
  -v mysql-data:/var/lib/mysql \
  mysql:8.0
```

**6.Stop and remove the container when finished:**

To stop and remove the running container:

Bash

```
docker stop my-custom-mysql
docker rm my-custom-mysql
```

## Ubuntu

Here is the step-by-step procedure to run a pre-made Ubuntu Docker image, interact with its command line, and customize its default landing state or configuration.

  

**1.Create the project directory:**

Create a new folder for your Ubuntu environment and navigate into it using your terminal:

  

Bash

```
mkdir my-ubuntu-project
cd my-ubuntu-project
```

**2.Create a custom startup script or welcome message:**

Create a script named `welcome.sh` in your project directory. This will serve as your custom landing setup inside the container:

  

Bash

```
cat << 'EOF' > welcome.sh
#!/bin/bash
echo "=========================================="
echo "  Welcome to My Custom Ubuntu Container! "
echo "=========================================="
exec bash
EOF
chmod +x welcome.sh
```

**3.Run the Ubuntu Docker container interactively:**

Launch the official Ubuntu container interactively (`-it`), mounting your workspace directory and executing your custom welcome script:

  

Bash

```
docker run -it \
  --name my-custom-ubuntu \
  -v $(pwd):/workspace \
  -w /workspace \
  ubuntu:latest \
  ./welcome.sh
```

- **`-it`**: Attaches an interactive terminal (TTY) so you can enter commands inside Ubuntu.
    
      
    
- **`--name my-custom-ubuntu`**: Assigns a custom name to your container.
    
      
    
- **`-v $(pwd):/workspace`**: Mounts your local folder into `/workspace` inside the container.
    
      
    
- **`-w /workspace`**: Sets the working directory inside the container to `/workspace`.
    
      
    
- **`ubuntu:latest`**: Downloads and runs the official base Ubuntu image.
    
      
    

**4.Verify and customize the environment inside Ubuntu:**

Now that you are inside the Ubuntu terminal, update the package manager and install any required tools (e.g., `curl` or `nano`):

  

Bash

```
apt update && apt install -y curl nano
```

To verify step execution, check the installed OS release info:

  

Bash

```
cat /etc/os-release
```

**5.Persist changes or exit the container:**

To exit the interactive session while leaving the container running in the background, press `Ctrl+P` followed by `Ctrl+Q`.

  

To exit and stop the container completely, simply type:

  

Bash

```
exit
```

**6.Re-enter or clean up the container:**

If the container was stopped, you can start and re-attach to it at any time:

  

Bash

```
docker start -ai my-custom-ubuntu
```

To completely remove the container when finished:

Bash

```
docker stop my-custom-ubuntu
docker rm my-custom-ubuntu
```

## Python

Here is the step-by-step procedure to run a pre-made Python Docker image, execute a custom Python application, and set up your script as the default entry point.

  

**1.Create the project directory:**

Create a new folder for your Python project and navigate into it using your terminal:

  

Bash

```
mkdir my-python-project
cd my-python-project
```

**2.Create a custom Python script:**

Create a `main.py` file in your project directory to act as your custom landing script:

  

Bash

```
echo 'print("Welcome to My Custom Python Container!")' > main.py
```

**3.Run the Python Docker container:**

Launch the official Python container, mounting your current folder into the container and executing your script:

  

Bash

```
docker run --rm \
  --name my-custom-python \
  -v $(pwd):/app \
  -w /app \
  python:3.11 \
  python main.py
```

- **`--rm`**: Automatically removes the container once the script finishes executing.
    
      
    
- **`--name my-custom-python`**: Assigns a custom name to your container while it runs.
    
      
    
- **`-v $(pwd):/app`**: Mounts your local project folder to the `/app` directory inside the container.
    
      
    
- **`-w /app`**: Sets `/app` as the active working directory inside the container.
    
      
    
- **`python:3.11`**: Downloads and uses the official Python 3.11 image from Docker Hub.
    
      
    
- **`python main.py`**: Executes your script using the container's built-in Python interpreter.
    
      
    

**4.Update the Python code on the fly:**

Because your local directory is mounted into the container, you can update `main.py` directly on your host machine and run the command again without rebuilding any images:

  

Bash

```
echo 'import sys; print(f"Python Version: {sys.version}")' > main.py
```

Re-run the container command to see the updated output immediately:

  

Bash

```
docker run --rm -v $(pwd):/app -w /app python:3.11 python main.py
```

**5.Run an interactive Python REPL or shell:**

If you want to run Python interactively or execute `pip` commands inside the container, start an interactive terminal session:

  

Bash

```
docker run -it --rm -v $(pwd):/app -w /app python:3.11 bash
```

Once inside, you can run `python` to launch the interactive interpreter shell or install packages using `pip`.

## MongoDB

Here is the step-by-step procedure to run a pre-made MongoDB Docker image, set up initial database credentials, and seed it with a custom collection or default data.

  

**1.Create the project directory:**

Create a new folder for your MongoDB project and navigate into it using your terminal:

  

Bash

```
mkdir my-mongo-project
cd my-mongo-project
```

**2.Create an initialization JavaScript script:**

Create an `init-mongo.js` file in your project directory. When MongoDB starts for the first time, any `.js` file mounted in `/docker-entrypoint-initdb.d/` will execute automatically to initialize databases, collections, and records:

  

Bash

```
cat << 'EOF' > init-mongo.js
db = db.getSiblingDB('appdb');
db.createCollection('welcome');
db.welcome.insertOne({
  message: "Welcome to My Custom MongoDB Server!",
  createdAt: new Date()
});
EOF
```

**3.Run the MongoDB Docker container:**

Launch the official MongoDB container, setting root credentials, mapping port 27017, and mounting your initialization script:

  

Bash

```
docker run -d \
  --name my-custom-mongo \
  -p 27017:27017 \
  -e MONGO_INITDB_ROOT_USERNAME=mongoadmin \
  -e MONGO_INITDB_ROOT_PASSWORD=secretpassword \
  -e MONGO_INITDB_DATABASE=appdb \
  -v $(pwd)/init-mongo.js:/docker-entrypoint-initdb.d/init-mongo.js:ro \
  mongo:latest
```

- **`-d`**: Runs the container in detached mode (background).
    
      
    
- **`--name my-custom-mongo`**: Assigns a custom name to your container.
    
      
    
- **`-p 27017:27017`**: Maps port 27017 on your host machine to port 27017 inside the container.
    
      
    
- **`-e MONGO_INITDB_ROOT_USERNAME` / `MONGO_INITDB_ROOT_PASSWORD`**: Sets root authentication credentials.
    
      
    
- **`-v $(pwd)/init-mongo.js:...:ro`**: Mounts your setup script read-only into Mongo's auto-init folder.
    
      
    
- **`mongo:latest`**: Downloads and runs the official MongoDB image from Docker Hub.
    
      
    

**4.Verify the database and custom data using mongosh:**

Use the MongoDB Shell (`mongosh`) running inside the container to query your initial collection and document:

  

Bash

```
docker exec -it my-custom-mongo mongosh \
  -u mongoadmin \
  -p secretpassword \
  --eval "use appdb; db.welcome.find()"
```

You should see the document containing `"Welcome to My Custom MongoDB Server!"` printed in the JSON output.

  

**5.Persist database storage locally (Optional):**

By default, MongoDB data is wiped when the container is destroyed. To keep data intact across restarts, mount a host folder or Docker volume to `/data/db`:

  

Bash

```
docker run -d \
  --name my-persistent-mongo \
  -p 27017:27017 \
  -e MONGO_INITDB_ROOT_USERNAME=mongoadmin \
  -e MONGO_INITDB_ROOT_PASSWORD=secretpassword \
  -v mongo-data:/data/db \
  mongo:latest
```

**6.Stop and remove the container when finished:**

To clean up and remove the running container:

Bash

```
docker stop my-custom-mongo
docker rm my-custom-mongo
```


# Building the Docker Image locally

## Python

Here is the complete step-by-step workflow to get a Python application running, containerized, uploaded to Docker Hub, and deployed—starting completely from scratch.

  

**1.1. Install Docker Desktop:**Prerequisite.

Download and install **Docker Desktop** for Windows, Mac, or Linux from [docker.com](https://www.docker.com/). Launch the application and ensure the engine is running in the background.

  

**2.2. Create Project Files:**Local Project Setup.

Create a new folder on your PC and add two files inside it:

  

**File 1: `app.py`**

  

Python

```
from flask import Flask
app = Flask(__name__)

@app.route("/")
def home():
    return "Python container is running successfully!"

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
```

**File 2: `Dockerfile`** _(No file extension)_

  

Dockerfile

```
# 1. Start from an official Python base image
FROM python:3.11-slim

# 2. Set the working directory inside the container
WORKDIR /app

# 3. Copy application code into the container
COPY app.py .

# 4. Install required packages
RUN pip install flask

# 5. Expose port 5000
EXPOSE 5000

# 6. Command to start the Python app when container runs
CMD ["python", "app.py"]
```

**3.3. Build the Docker Image:**CLI Step.

Open your terminal/command prompt, navigate (`cd`) into your project folder, and run:

  

Bash

```
docker build -t my-python-app .
```

This tells Docker to read your `Dockerfile` and package everything into an image named `my-python-app`.

  

**4.4. Test the Container Locally:**CLI Step.

Run a container using the image you just built:

  

Bash

```
docker run -d -p 5000:5000 --name running-python-app my-python-app
```

- **`-d`**: Runs the container in background (detached) mode.
    
      
    
- **`-p 5000:5000`**: Maps port `5000` on your PC to port `5000` inside the container.
    
      
    

_Verification:_ Open `http://localhost:5000` in your web browser. You will see the response from your Python app.

  

**5.5. Push Image to Docker Hub:**Distribution Step.

To share your image or prepare it for cloud deployment:

  

1. Sign up for a free account at [hub.docker.com](https://hub.docker.com/).
    
      
    
2. Log in and push via terminal:
    
      
    

Bash

```
# Log in to your Docker account
docker login

# Tag your local image with your Docker Hub username
docker tag my-python-app <your-dockerhub-username>/my-python-app:v1.0

# Upload the image to Docker Hub
docker push <your-dockerhub-username>/my-python-app:v1.0
```

**6.6. Deploy to a Cloud Server:**Production Step.

Connect to any cloud virtual machine (AWS EC2, DigitalOcean, GCP) with Docker installed, and launch your application directly from Docker Hub without needing any source code:

  

Bash

```
docker run -d -p 80:5000 <your-dockerhub-username>/my-python-app:v1.0
```

Visitors navigating to your server's public IP address will reach your live Python application.

## Nginx

**1.1. Install Docker Desktop:**Prerequisite.

Ensure **Docker Desktop** is installed and running on your computer.

  

**2.2. Create Project Files:**Local Project Setup.

Create a new folder on your PC and add two files inside it:

  

**File 1: `index.html`**

  

HTML

```
<!DOCTYPE html>
<html>
<head>
    <title>Nginx Docker Test</title>
</head>
<body>
    <h1>Nginx container is running successfully!</h1>
</body>
</html>
```

**File 2: `Dockerfile`** _(No file extension)_

  

Dockerfile

```
# 1. Start from the official Nginx lightweight base image
FROM nginx:alpine

# 2. Remove default Nginx welcome page
RUN rm /usr/share/nginx/html/index.html

# 3. Copy your custom web files to Nginx's web root
COPY index.html /usr/share/nginx/html/

# 4. Expose web traffic port 80
EXPOSE 80

# 5. Start Nginx in the foreground (prevents container from exiting)
CMD ["nginx", "-g", "daemon off;"]
```

**3.3. Build the Nginx Docker Image:**CLI Step.

Open your terminal, navigate (`cd`) into your project folder, and run:

  

Bash

```
docker build -t my-nginx-app .
```

**4.4. Test the Nginx Container Locally:**CLI Step.

Run a container using the image you just built:

  

Bash

```
docker run -d -p 8080:80 --name running-nginx-app my-nginx-app
```

- **`-d`**: Runs in background mode.
    
      
    
- **`-p 8080:80`**: Maps port `8080` on your PC to standard HTTP port `80` inside the Nginx container.
    
      
    

_Verification:_ Open `http://localhost:8080` in your web browser to view your `index.html` web page.

  

**5.5. Push Image to Docker Hub:**Distribution Step.

Prepare and upload your custom Nginx image to Docker Hub:

  

Bash

```
# Log in to Docker Hub
docker login

# Tag local image with your Docker Hub username
docker tag my-nginx-app <your-dockerhub-username>/my-nginx-app:v1.0

# Push image to Docker Hub
docker push <your-dockerhub-username>/my-nginx-app:v1.0
```

**6.6. Deploy to a Cloud Server:**Production Step.

On any cloud server running Docker, pull and run your web app directly:

  

Bash

```
docker run -d -p 80:80 <your-dockerhub-username>/my-nginx-app:v1.0
```

### Line-by-Line Breakdown

- **`FROM nginx:alpine`**: Uses an extremely small, security-hardened Linux distribution (Alpine) with Nginx pre-installed.
    
      
    
- **`COPY index.html /usr/share/nginx/html/`**: Overwrites Nginx's default public folder with your own frontend files.
    
      
    
- **`CMD ["nginx", "-g", "daemon off;"]`**: Tells Nginx to run as the main foreground process. If Nginx runs as a background service inside a container, Docker assumes the main task finished and shuts the container down immediately.

## Apache

**1.1. Install Docker Desktop:**Prerequisite.

Ensure **Docker Desktop** is installed and running on your computer.

  

**2.2. Create Project Files:**Local Project Setup.

Create a new folder on your PC and add two files inside it:

  

**File 1: `index.html`**

  

HTML

```
<!DOCTYPE html>
<html>
<head>
    <title>Apache Docker Test</title>
</head>
<body>
    <h1>Apache container is running successfully!</h1>
</body>
</html>
```

**File 2: `Dockerfile`** _(No file extension)_

  

Dockerfile

```
# 1. Start from the official Apache (httpd) base image
FROM httpd:2.4-alpine

# 2. Copy custom HTML files into Apache's default web directory
COPY index.html /usr/local/apache2/htdocs/

# 3. Expose the standard web traffic port
EXPOSE 80

# 4. Start Apache in the foreground
CMD ["httpd-foreground"]
```

**3.3. Build the Apache Docker Image:**CLI Step.

Open your terminal, navigate (`cd`) into your project folder, and run:

  

Bash

```
docker build -t my-apache-app .
```

**4.4. Test the Apache Container Locally:**CLI Step.

Run a container using the image you just built:

  

Bash

```
docker run -d -p 8080:80 --name running-apache-app my-apache-app
```

- **`-d`**: Runs in background (detached) mode.
    
      
    
- **`-p 8080:80`**: Maps port `8080` on your PC to HTTP port `80` inside the Apache container.
    
      
    

_Verification:_ Open `http://localhost:8080` in your web browser to view your web page.

  

**5.5. Push Image to Docker Hub:**Distribution Step.

Prepare and upload your custom Apache image to Docker Hub:

  

Bash

```
# Log in to Docker Hub
docker login

# Tag local image with your Docker Hub username
docker tag my-apache-app <your-dockerhub-username>/my-apache-app:v1.0

# Push image to Docker Hub
docker push <your-dockerhub-username>/my-apache-app:v1.0
```

**6.6. Deploy to a Cloud Server:**Production Step.

On any cloud server running Docker, pull and run your web app directly:

  

Bash

```
docker run -d -p 80:80 <your-dockerhub-username>/my-apache-app:v1.0
```

### Line-by-Line Breakdown

- **`FROM httpd:2.4-alpine`**: Uses `httpd` (Apache's official HTTP server name in Docker) on lightweight Alpine Linux.
    
      
    
- **`COPY index.html /usr/local/apache2/htdocs/`**: Places your static site into `/usr/local/apache2/htdocs/`, which is Apache's default public directory.
    
      
    
- **`EXPOSE 80`**: Documents that Apache receives traffic on standard web port 80.
    
      
    
- **`CMD ["httpd-foreground"]`**: Runs the main Apache process in the foreground so the container stays active instead of exiting immediately.

## Mysql

**1.1. Install Docker Desktop:**Prerequisite.

Ensure **Docker Desktop** is installed and running on your computer.

  

**2.2. Create Initialization Script & Dockerfile:**Local Project Setup.

Create a new folder on your PC and add two files inside it:

  

**File 1: `init.sql`** _(Runs automatically when database is first created)_

  

SQL

```
CREATE DATABASE IF NOT EXISTS my_database;
USE my_database;

CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    email VARCHAR(50) NOT NULL
);

INSERT INTO users (name, email) VALUES 
('Alice', 'alice@example.com'),
('Bob', 'bob@example.com');
```

**File 2: `Dockerfile`** _(No file extension)_

  

Dockerfile

```
# 1. Start from official MySQL 8.0 base image
FROM mysql:8.0

# 2. Set environment variable for root password
ENV MYSQL_ROOT_PASSWORD=rootpassword

# 3. Copy SQL script to automatic entrypoint folder
COPY init.sql /docker-entrypoint-initdb.d/

# 4. Expose default MySQL database port
EXPOSE 3306
```

**3.3. Build the Custom MySQL Image:**CLI Step.

Open your terminal, navigate (`cd`) into your project folder, and run:

  

Bash

```
docker build -t my-mysql-db .
```

**4.4. Test the MySQL Container Locally:**CLI Step.

Run a container using the image you just built:

  

Bash

```
docker run -d -p 3306:3306 --name running-mysql-db my-mysql-db
```

- **`-d`**: Runs in background mode.
    
      
    
- **`-p 3306:3306`**: Maps port `3306` on your PC to port `3306` inside the container.
    
      
    

_Verification:_ Run `docker exec -it running-mysql-db mysql -u root -prootpassword my_database -e "SELECT * FROM users;"` to query the database table directly.

  

**5.5. Push Image to Docker Hub:**Distribution Step.

Prepare and upload your custom MySQL image to Docker Hub:

  

Bash

```
# Log in to Docker Hub
docker login

# Tag local image with your Docker Hub username
docker tag my-mysql-db <your-dockerhub-username>/my-mysql-db:v1.0

# Push image to Docker Hub
docker push <your-dockerhub-username>/my-mysql-db:v1.0
```

**6.6. Deploy to a Cloud Server with Data Persistence:**Production Step.

Database containers lose data when deleted unless attached to a volume. Run this on your cloud server:

  

Bash

```
# Create a persistent volume on host machine
docker volume create mysql_data

# Run container attached to persistent storage
docker run -d \
  -p 3306:3306 \
  -v mysql_data:/var/lib/mysql \
  --name production-mysql \
  <your-dockerhub-username>/my-mysql-db:v1.0
```

### Line-by-Line Breakdown

- **`FROM mysql:8.0`**: Pulls the official Linux-based image with MySQL 8 engine pre-configured.
    
      
    
- **`ENV MYSQL_ROOT_PASSWORD=rootpassword`**: Sets an environment variable inside the container that MySQL uses to configure its administrator password.
    
      
    
- **`COPY init.sql /docker-entrypoint-initdb.d/`**: Any `.sql` file copied into `/docker-entrypoint-initdb.d/` automatically runs when the container initializes for the first time.
    
      
    
- **`EXPOSE 3306`**: Documents that MySQL accepts connections on standard database port 3306.
    
      
    
- **`-v mysql_data:/var/lib/mysql`**: Maps host storage to MySQL's internal database directory so data survives container restarts and deletions.

## MongoDB

**1.1. Install Docker Desktop:**Prerequisite.

Ensure **Docker Desktop** is installed and running on your computer.

  

**2.2. Create Initialization Script & Dockerfile:**Local Project Setup.

Create a new folder on your PC and add two files inside it:

  

**File 1: `init-mongo.js`** _(Runs automatically when database is first initialized)_

  

JavaScript

```
db = db.getSiblingDB('my_database');

db.createCollection('users');

db.users.insertMany([
  { name: 'Alice', email: 'alice@example.com', role: 'admin' },
  { name: 'Bob', email: 'bob@example.com', role: 'user' }
]);
```

**File 2: `Dockerfile`** _(No file extension)_

  

Dockerfile

```
# 1. Start from official MongoDB base image
FROM mongo:7.0

# 2. Set environment variables for admin credentials
ENV MONGO_INITDB_ROOT_USERNAME=admin
ENV MONGO_INITDB_ROOT_PASSWORD=secretpassword
ENV MONGO_INITDB_DATABASE=my_database

# 3. Copy JS initialization script to entrypoint folder
COPY init-mongo.js /docker-entrypoint-initdb.d/

# 4. Expose default MongoDB port
EXPOSE 27017
```

**3.3. Build the Custom MongoDB Image:**CLI Step.

Open your terminal, navigate (`cd`) into your project folder, and run:

  

Bash

```
docker build -t my-mongo-db .
```

**4.4. Test the MongoDB Container Locally:**CLI Step.

Run a container using the image you just built:

  

Bash

```
docker run -d -p 27017:27017 --name running-mongo-db my-mongo-db
```

- **`-d`**: Runs in background mode.
    
      
    
- **`-p 27017:27017`**: Maps port `27017` on your PC to port `27017` inside the container.
    
      
    

_Verification:_ Run `docker exec -it running-mongo-db mongosh -u admin -p secretpassword --eval "use my_database; db.users.find()"` to view the inserted documents directly inside the container shell.

  

**5.5. Push Image to Docker Hub:**Distribution Step.

Prepare and upload your custom MongoDB image to Docker Hub:

  

Bash

```
# Log in to Docker Hub
docker login

# Tag local image with your Docker Hub username
docker tag my-mongo-db <your-dockerhub-username>/my-mongo-db:v1.0

# Push image to Docker Hub
docker push <your-dockerhub-username>/my-mongo-db:v1.0
```

**6.6. Deploy to a Cloud Server with Data Persistence:**Production Step.

To ensure your NoSQL database documents persist across container restarts or upgrades, attach a volume on your cloud host:

  

Bash

```
# Create a persistent volume on host machine
docker volume create mongo_data

# Run container attached to persistent storage
docker run -d \
  -p 27017:27017 \
  -v mongo_data:/data/db \
  --name production-mongo \
  <your-dockerhub-username>/my-mongo-db:v1.0
```

### Line-by-Line Breakdown

- **`FROM mongo:7.0`**: Pulls the official MongoDB database engine base layer from Docker Hub.
    
      
    
- **`ENV MONGO_INITDB_ROOT_USERNAME` & `PASSWORD`**: Sets root authentication credentials required to log into the database server.
    
      
    
- **`COPY init-mongo.js /docker-entrypoint-initdb.d/`**: Any `.js` or `.sh` script inside `/docker-entrypoint-initdb.d/` automatically runs when MongoDB boots up for the first time.
    
      
    
- **`EXPOSE 27017`**: Documents that MongoDB listens for NoSQL connection requests on port `27017`.
    
      
    
- **`-v mongo_data:/data/db`**: Maps host storage to MongoDB's internal document storage folder (`/data/db`), ensuring your collections remain safe if the container stops or is deleted.

## Ubuntu

**1.1. Install Docker Desktop:**Prerequisite.

Ensure **Docker Desktop** is installed and running on your computer.

  

**2.2. Create Startup Script & Dockerfile:**Local Project Setup.

Create a new folder on your PC and add two files inside it:

  

**File 1: `script.sh`** _(Custom script to execute inside Ubuntu)_

  

Bash

```
#!/bin/bash
echo "Hello from custom Ubuntu container!"
echo "System Uptime:"
uptime
```

**File 2: `Dockerfile`** _(No file extension)_

  

Dockerfile

```
# 1. Start from official Ubuntu base image
FROM ubuntu:22.04

# 2. Avoid interactive prompts during package installation
ENV DEBIAN_FRONTEND=noninteractive

# 3. Update system packages and install basic utilities
RUN apt-get update && apt-get install -y \
    curl \
    git \
    vim \
    && rm -rf /var/lib/apt/lists/*

# 4. Set working directory inside container
WORKDIR /root

# 5. Copy local script into container and make it executable
COPY script.sh .
RUN chmod +x script.sh

# 6. Default command executed when container starts
CMD ["./script.sh"]
```

**3.3. Build the Custom Ubuntu Image:**CLI Step.

Open your terminal, navigate (`cd`) into your project folder, and run:

  

Bash

```
docker build -t my-ubuntu-env .
```

**4.4. Test the Ubuntu Container Locally:**CLI Step.

Run a container using the image you just built:

  

Bash

```
# Run script automatically and exit
docker run --name test-ubuntu my-ubuntu-env

# Alternatively, open an interactive bash shell inside the Ubuntu OS
docker run -it --name interactive-ubuntu my-ubuntu-env bash
```

- **`-it`**: Allocates a pseudo-TTY and keeps standard input open so you can use Ubuntu interactively via command line.
    
      
    

_Verification:_ Inside the interactive shell, run standard Linux commands like `ls`, `uname -a`, or `cat /etc/os-release`. Type `exit` to close the shell.

  

**5.5. Push Image to Docker Hub:**Distribution Step.

Prepare and upload your custom Ubuntu environment image to Docker Hub:

  

Bash

```
# Log in to Docker Hub
docker login

# Tag local image with your Docker Hub username
docker tag my-ubuntu-env <your-dockerhub-username>/my-ubuntu-env:v1.0

# Push image to Docker Hub
docker push <your-dockerhub-username>/my-ubuntu-env:v1.0
```

**6.6. Deploy/Run on a Cloud Server:**Production Step.

Pull and launch your pre-configured Ubuntu environment on any server:

  

Bash

```
# Run interactive terminal session on remote host
docker run -it <your-dockerhub-username>/my-ubuntu-env:v1.0 bash
```

### Line-by-Line Breakdown

- **`FROM ubuntu:22.04`**: Pulls the official, base Linux filesystem image of Ubuntu 22.04 LTS from Docker Hub.
    
      
    
- **`ENV DEBIAN_FRONTEND=noninteractive`**: Prevents `apt-get` commands from halting the build process to prompt for user input (like selecting a timezone).
    
      
    
- **`RUN apt-get update && apt-get install -y ...`**: Updates package lists and installs command-line utilities (`curl`, `git`, `vim`).
    
      
    
- **`rm -rf /var/lib/apt/lists/*`**: Cleans up leftover package index files after installation to keep the final image file size small.
    
      
    
- **`CMD ["./script.sh"]`**: Runs the custom bash script by default if no arguments are passed during `docker run`. Passing `bash` at the end of `docker run` overrides this default.

## Wordpress

WordPress requires **two** components: the WordPress web engine (PHP/Apache) and a **MySQL/MariaDB database** to store site content.

  

Here is how to create a pre-configured WordPress Docker image, connect it to a database, upload it to Docker Hub, and deploy it.

  

**1.1. Install Docker Desktop:**Prerequisite.

Ensure **Docker Desktop** is installed and active on your PC.

  

**2.2. Create Custom Theme & Dockerfile:**Local Project Setup.

Create a new folder on your PC and add two items inside it:

  

**Item 1: `custom-theme/index.php`** _(A simple custom WordPress theme file)_

  

PHP

```
<?php
get_header();
echo "<h1>Custom WordPress Image is Running Successfully!</h1>";
get_footer();
?>
```

**Item 2: `Dockerfile`** _(No file extension)_

  

Dockerfile

```
# 1. Start from the official WordPress base image (PHP + Apache)
FROM wordpress:latest

# 2. Copy a custom theme directly into WordPress themes folder
COPY custom-theme /usr/src/wordpress/wp-content/themes/custom-theme

# 3. Environment defaults for database connections
ENV WORDPRESS_DB_HOST=mysql-db:3306
ENV WORDPRESS_DB_USER=wp_user
ENV WORDPRESS_DB_PASSWORD=wp_password
ENV WORDPRESS_DB_NAME=wp_database

# 4. Expose default HTTP web port
EXPOSE 80
```

**3.3. Build the Custom WordPress Image:**CLI Step.

Open your terminal, navigate (`cd`) into your project folder, and run:

  

Bash

```
docker build -t my-wordpress-app .
```

**4.4. Test WordPress + MySQL Locally:**CLI Step.

Because WordPress relies on a database, set up a shared network, start MySQL, and launch WordPress:

  

Bash

```
# 1. Create an isolated Docker network so containers can talk to each other
docker network create wp-network

# 2. Start MySQL database container attached to the network
docker run -d \
  --name mysql-db \
  --network wp-network \
  -e MYSQL_DATABASE=wp_database \
  -e MYSQL_USER=wp_user \
  -e MYSQL_PASSWORD=wp_password \
  -e MYSQL_ROOT_PASSWORD=rootpassword \
  mysql:8.0

# 3. Start your custom WordPress container attached to the same network
docker run -d \
  -p 8080:80 \
  --name running-wp-app \
  --network wp-network \
  my-wordpress-app
```

_Verification:_ Navigate to `http://localhost:8080` in your web browser. You will be greeted by the WordPress setup wizard connected to your database.

  

**5.5. Push Image to Docker Hub:**Distribution Step.

Prepare and upload your custom WordPress image to Docker Hub:

  

Bash

```
# Log in to Docker Hub
docker login

# Tag local image with your Docker Hub username
docker tag my-wordpress-app <your-dockerhub-username>/my-wordpress-app:v1.0

# Push image to Docker Hub
docker push <your-dockerhub-username>/my-wordpress-app:v1.0
```

**6.6. Deploy to a Cloud Server:**Production Step.

On your cloud server, create persistent storage volumes so site uploads and database contents are never lost:

  

Bash

```
# Create persistent volumes for uploads and database data
docker volume create wp_uploads
docker volume create wp_db_data

# Create production network
docker network create prod-wp-network

# Start MySQL database
docker run -d \
  --name prod-db \
  --network prod-wp-network \
  -v wp_db_data:/var/lib/mysql \
  -e MYSQL_DATABASE=wp_database \
  -e MYSQL_USER=wp_user \
  -e MYSQL_PASSWORD=wp_password \
  -e MYSQL_ROOT_PASSWORD=rootpassword \
  mysql:8.0

# Start custom WordPress container
docker run -d \
  -p 80:80 \
  --name prod-wp \
  --network prod-wp-network \
  -v wp_uploads:/var/www/html/wp-content/uploads \
  -e WORDPRESS_DB_HOST=prod-db:3306 \
  -e WORDPRESS_DB_USER=wp_user \
  -e WORDPRESS_DB_PASSWORD=wp_password \
  -e WORDPRESS_DB_NAME=wp_database \
  <your-dockerhub-username>/my-wordpress-app:v1.0
```

### Line-by-Line Breakdown

- **`FROM wordpress:latest`**: Pulls the official Docker image containing Apache web server, PHP runtime, and full WordPress core source code.
    
      
    
- **`COPY custom-theme /usr/src/wordpress/wp-content/themes/custom-theme`**: Copies your local theme into WordPress's base template directory so every container created from this image comes pre-installed with your code.
    
      
    
- **`ENV WORDPRESS_DB_*`**: Pre-configures the environment variables WordPress uses internally to connect to the database automatically upon startup.
    
      
    
- **`docker network create`**: Creates a internal virtual bridge so containers can communicate directly using their container names (`mysql-db` or `prod-db`) as domain hostnames.
    
      
    
- **`-v wp_uploads:/var/www/html/wp-content/uploads`**: Binds media uploads (images, PDFs) to host storage so user files persist if the container updates or restarts.
