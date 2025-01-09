# Homelab
This repository contains a Docker Compose setup to run a variety of development and infrastructure tools like Jupyter, Nginx, Docker Registry, VSCode, MongoDB, and Selenium Grid.

## Requirements
- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/)

Ensure Docker and Docker Compose are installed on your system before proceeding.
## Generate Jupyter Token

```bash
openssl rand -hex 32
```
## Setup Auth
```bash
mkdir -p <DIR_NAME>
htpasswd -Bc auth/htpasswd <USER_NAME>
```
## Setup Environment Variables

Before starting the services, you need to define environment variables used in the `docker-compose.yml` file. You can create a `.env` file in the root of your project directory:

```bash
HOMELAB_JP_TOKEN=your_jupyter_token
HOMELAB_VSCODE_PASS=your_vscode_password
HOMELAB_VSCODE_PASS_SUDO=your_vscode_sudo_password
HOMELAB_MONGO_ROOT_USERNAME=mongo_username
HOMELAB_MONGO_USER_PASSWORD=mongo_pass
HOMELAB_VOLUME_MONGO=<MONGO_VOLUME_PATH>
```
Make sure to replace the values with your preferred configuration.

## How to Build and Run
1. Clone the repository and navigate into the project directory:
```bash
git clone git@github.com:alireza-87/local_lab.git
cd local_lab
```
2. If you haven’t already, create the .env file:
```bash
touch .env
```
Add the necessary environment variables as outlined above.
3. Start the Docker containers:
```bash
docker compose up -d
```
This command will pull the necessary images, create containers, and run all services in the background.

4. You can check if the containers are running by using
```bash
docker compose ps
docker compose stats
```

5. To stop the services
```bash
docker compose down
```

## Accessing the Services
### 1. **Jupyter Notebook**
- **URL**: [http://localhost:10001/](http://localhost:10001/)
- **Description**: The Jupyter Notebook is exposed through Nginx. You can access it using the token provided in your `.env` file.

### 2. **VS Code Server**
- **URL**: [http://localhost:8082/](http://localhost:8082/)
- **Description**: Description: A cloud-based VS Code server. The password is set via the HOMELAB_VSCODE_PASS in your `.env` file.

### 3. **Docker Registry UI**
- **URL**: [http://localhost:8081/](http://localhost:8081/)

### 4. **Docker Registry**
- To login
  ```bash
  docker login localhost:5000
  ```
- Push
  ```bash
  docker tag my_image localhost:5000/my_image
  docker push localhost:5000/my_image
  ```
- pull
  ```bash
  docker pull localhost:5000/my_image
  ```
- If using HTTP instead of HTTPS add this line to daemon.json
  ```json
  {
  "insecure-registries" : ["SERVER_IP:5000"]
  }
  ```
### 4. **MongoDB**
- MongoDB is accessible on port `8084` with the username and password set in the environment variables:
  - **Username**: `${HOMELAB_MONGO_ROOT_USERNAME}`
  - **Password**: `${HOMELAB_MONGO_USER_PASSWORD}`
  
- To connect:
  ```bash
  mongo -u <USER> -p <PASS> --port 8084
  ```
### 5. **Selenium Grid**
- **URL**: [http://localhost:8083/](http://localhost:8083/)
- **Description**: Selenium Grid is used for automated browser testing. It is configured with Chromium for running tests in parallel.

