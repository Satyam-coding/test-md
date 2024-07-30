# OH-Docker
Dockerising OH into separate containers for frontend, backend and database

These containers are hosted on a bridge network (oh_network) to avoid port conflicts.

## Prerequisites
- Docker installed on your system ([Install Docker](https://docs.docker.com/get-docker/))
- Docker Compose installed on your system ([Install Docker Compose](https://docs.docker.com/compose/install/))

## Usage
### Development Environment
1. Clone the repository.
2. Navigate to the project root directory.
3. Run the following command to start the application in development mode:

##### [loading credentials from vault]

###
    role_id=<role_id> secret_id=<secret_id> docker-compose -f "docker-compose.dev.yml" watch 

##### [normal]
###
    docker-compose -f "docker-compose.dev.yml" watch 


1. Open a browser and navigate to http://localhost:3000 and http://localhost:8000 to view the application 
  
2. To **Stop** the application, run:
###
    docker-compose -f "docker-compose.dev.yml" down


3. To remove the **Volumes**:

###
    docker-compose -f "docker-compose.dev.yml" down --volumes


4. To remove the associated **Images**:
 
 ###
    docker-compose -f "docker-compose.dev.yml" down --rmi all




### Production Environment
1. Clone the repository.
2. Navigate to the project root directory.
3. Run the following command to start the application in production mode:

##### [loading credentials from vault]

###
    role_id=<role_id> secret_id=<secret_id> docker-compose -f "docker-compose.prod.yml" up 

##### [normal]

```
docker-compose -f "docker-compose.prod.yml" up 
```

1. Open a browser and navigate to http://localhost:3000 and http://localhost:8000 to view the application 
  
2. To **Stop** the application, run:
###
    docker-compose -f "docker-compose.prod.yml" down

3. To remove the **Volumes**:
###
    docker-compose -f "docker-compose.prod.yml" down --volumes


4. To remove the associated **Images**:
 ###
    docker-compose -f "docker-compose.prod.yml" down --rmi all


### Connect Backend with MySQL Container:
To connect your backend container with your dockerized MySQL Container, follow these steps:

Update the backend/env  with the following settings:
  
###
    DATABASE_HOST=database





### Connect Backend with LocalMySQL Server:
To connect your backend container with your local MySQL server, follow these steps:

1. Update the backend/env  with the following settings:
  
  ###
      DATABASE_HOST=host.docker.internal
  

2. Modify the docker-compose.yml (dev/prod) File :
    
    - Remove the database service and its associated volumes (db).
    - Remove the depends_on entry for the database service in the backend container.

    ```
    services:
    backend:
    # other configurations
    # depends_on:
    #   database:
    #     condition: service_healthy
    ```

    By following these steps, your backend container will connect to your local MySQL server


  **Important Notes**
====================

### 1. Docker Context

Before proceeding, ensure that you are using the default Docker context. You can verify and set the default context by running the following commands:

###
    docker context ls
    docker context use default


### 2. Credential Storage

Please note that all credentials are stored in Docker containers. Creating a credential file (e.g., `dev.json`) and saving them in local will override the existing credentials in the Docker containers.

### 3. Refetching Credentials from Vault Server

To refetch credentials from the Vault server, follow these steps:

1. Enter the backend container using the following command:  

   ###
        docker exec -it <backend_container_id> bash

2. Run the following commands inside the backend container:
   
  ###
      export role_id=<role_id> 
      export secret_id=<secret_id> 
      export refetch=True
      python app/vault.py
   



    
