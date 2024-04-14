# CV_7_Docker

<img src='./docker.png' >


# Some information about Docker:

Docker is a platform for containerizing applications, allowing the isolation of applications and their dependencies into separate containers. These containers are easily portable, scalable, and quickly deployable across different environments, from development to production.

Here are some key points about Docker:

1. **Application Containerization**: Docker enables packaging applications and all necessary dependencies into containers, which are isolated from the host operating system and other containers. This ensures a consistent environment for running applications on different platforms.

2. **Portability**: Docker containers are easily portable across different environments, whether it's development, testing, or production environments. This allows developers and operations teams to easily deploy and manage applications.

3. **Resource Efficiency**: Docker utilizes the sharing of the host operating system kernel among containers, enabling more efficient resource utilization such as memory and CPU.

4. **Fast Deployment**: Docker allows for quick startup and shutdown of containers, simplifying the development, testing, and deployment process of applications.

5. **Environment Management**: Docker provides tools for managing containers, such as Docker Compose for defining and running multiple containers as a single entity, and Docker Swarm or Kubernetes for orchestrating containers in distributed environments.

6. **Open Source**: Docker is an open-source project actively developed by a community of developers and supported by many large technology companies.

7. **Container Support**: Docker is not the only tool for containerization, but it is one of the most popular and widely used. There are also other tools such as Podman, containerd, and rkt that offer similar functionality to Docker.

Docker is now one of the key tools in software development and management, providing flexibility, efficiency, and simplicity in containerizing applications.

# Install Docker Desktop on Windows

## Install interactively

Download the installer using the download button at the top of the page, or from the release notes.

Double-click Docker Desktop Installer.exe to run the installer. By default, Docker Desktop is installed at C:\Program Files\Docker\Docker.

When prompted, ensure the Use WSL 2 instead of Hyper-V option on the Configuration page is selected or not depending on your choice of backend.

If your system only supports one of the two options, you will not be able to select which backend to use.

Follow the instructions on the installation wizard to authorize the installer and proceed with the install.

When the installation is successful, select Close to complete the installation process.

If your admin account is different to your user account, you must add the user to the docker-users group:

Run Computer Management as an administrator.
Navigate to Local Users and Groups > Groups > docker-users.
Right-click to add the user to the group.
Sign out and sign back in for the changes to take effect.

## Install Docker Desktop on Linux

Comprehensive installation guides for Docker on both the Linux and macOS operating systems are provided via the attached links. It is imperative to meticulously adhere to the step-by-step instructions outlined in the respective guides to ensure the successful installation of Docker. Furthermore, users are advised to select the installation instructions corresponding to their specific operating system to guarantee compatibility and optimal performance. By following these authoritative guides, users can seamlessly integrate Docker into their computing environment, leveraging its powerful containerization capabilities for efficient application deployment and management.

[Install Docker on Linux](https://docs.docker.com/desktop/install/linux-install/)

[Install Docker on Mac](https://docs.docker.com/desktop/install/mac-install/)


# Dockerize simple app

## Dockerize a simple spring app

1. **Create a Spring Boot Application**: If you haven't already, create a Spring Boot application using your preferred IDE or build tool (such as Maven or Gradle). You can follow the official Spring Boot documentation for guidance on creating a new project.

2. **Write Dockerfile**: Create a Dockerfile in the root directory of your Spring Boot project. The Dockerfile defines the instructions for building a Docker image for your application. Here's a basic example of a Dockerfile:

```Dockerfile
# Use an official OpenJDK runtime as a base image
FROM openjdk:11-jre-slim

# Set the working directory in the container
WORKDIR /app

# Copy the packaged jar file into the container at /app
COPY target/your-application-name.jar /app

# Specify the command to run your application
CMD ["java", "-jar", "your-application-name.jar"]
```

Replace `"your-application-name.jar"` with the actual name of your Spring Boot executable jar file.

3. **Build Docker Image**: Open a terminal, navigate to the directory containing your Dockerfile and execute the following command to build the Docker image:

```bash
docker build -t your-image-name .
```

Replace `"your-image-name"` with the desired name for your Docker image.

4. **Run Docker Container**: Once the Docker image is built successfully, you can run a Docker container using the following command:

```bash
docker run -p 8080:8080 your-image-name
```

This command maps port 8080 on your local machine to port 8080 in the Docker container. Replace `"your-image-name"` with the name of your Docker image.

5. **Access Your Application**: Your Spring Boot application should now be running inside a Docker container. You can access it by opening a web browser and navigating to `http://localhost:8080` (assuming your application is running on port 8080).

That's it! You have successfully dockerized your Spring Boot application. You can now distribute and deploy your application as a Docker image, making it easier to manage and run in various environments.

---

## Dockerize a simple spring app with database

1. **Create a Spring Boot Application**:
   Begin by developing your Spring Boot application. Ensure it includes the necessary dependencies for database connectivity (like Spring Data JPA) and any endpoints you require.

2. **Write Dockerfile**:
   In your project's root directory, create a Dockerfile to define the Docker image for your Spring Boot application. Below is a basic example:

```Dockerfile
# Use an official OpenJDK runtime as a base image
FROM openjdk:11-jre-slim

# Set the working directory in the container
WORKDIR /app

# Copy the packaged JAR file into the container at /app
COPY target/your-application-name.jar /app

# Specify the command to run your application
CMD ["java", "-jar", "your-application-name.jar"]
```

Ensure to replace `"your-application-name.jar"` with the actual name of your Spring Boot executable JAR file.

3. **Write application.properties**:
   Create an `application.properties` file in the `src/main/resources` directory of your Spring Boot project. Configure the database connection properties. For example:

```
spring.datasource.url=jdbc:postgresql://db:5432/your_database_name
spring.datasource.username=your_username
spring.datasource.password=your_password
spring.datasource.driver-class-name=org.postgresql.Driver
spring.jpa.hibernate.ddl-auto=update
```

Replace `your_database_name`, `your_username`, and `your_password` with your preferred database configuration.

4. **Write docker-compose.yml**:
   Create a `docker-compose.yml` file to define services for your application and the PostgreSQL database. Below is an example:

```yaml
version: '3'

services:
  app:
    build: .
    ports:
      - "8080:8080"
    depends_on:
      - db
  db:
    image: postgres:latest
    ports:
      - "5432:5432"
    environment:
      POSTGRES_DB: your_database_name
      POSTGRES_USER: your_username
      POSTGRES_PASSWORD: your_password
```

Replace `your_database_name`, `your_username`, and `your_password` with your preferred database configuration.

5. **Build and Run Docker Containers**:
   In your terminal, navigate to the directory containing your Dockerfile and `docker-compose.yml`, then execute the following command:

```bash
docker-compose up --build
```

This command will build Docker images for your application and database, and then start the containers.

6. **Access Your Application**:
   Your Spring Boot application, along with the PostgreSQL database, should now be running inside Docker containers. You can access the application by opening a web browser and navigating to `http://localhost:8080`.

By following these steps, you've successfully dockerized your Spring Boot application with a PostgreSQL database. This approach simplifies deployment and ensures consistency across different environments.

---

## Dockerize a simple python app

1. **Create a Python Web Application**: Develop your Python web application using a web framework such as Flask or Django. Make sure your application is functional and runs locally on your development machine.

2. **Write Dockerfile**: Create a Dockerfile in the root directory of your Python project. The Dockerfile specifies the instructions for building a Docker image for your application. Here's a basic example of a Dockerfile for a Flask application:

```Dockerfile
# Use an official Python runtime as a base image
FROM python:3.9-slim

# Set the working directory in the container
WORKDIR /app

# Copy the requirements file into the container at /app
COPY requirements.txt /app

# Install dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Copy the current directory contents into the container at /app
COPY . /app

# Specify the command to run your application
CMD ["python", "app.py"]
```

Replace `"app.py"` with the entry point file of your Python application.

3. **Write requirements.txt**: Create a `requirements.txt` file in the root directory of your project. This file should contain a list of Python dependencies required by your application. You can generate this file using `pip freeze` or manually list the dependencies.

4. **Build Docker Image**: Open a terminal, navigate to the directory containing your Dockerfile, and execute the following command to build the Docker image:

```bash
docker build -t your-image-name .
```

Replace `"your-image-name"` with the desired name for your Docker image.

5. **Run Docker Container**: Once the Docker image is built successfully, you can run a Docker container using the following command:

```bash
docker run -p 5000:5000 your-image-name
```

This command maps port 5000 on your local machine to port 5000 in the Docker container. Replace `"your-image-name"` with the name of your Docker image.

6. **Access Your Application**: Your Python web application should now be running inside a Docker container. You can access it by opening a web browser and navigating to `http://localhost:5000` (assuming your application is running on port 5000).

That's it! You have successfully dockerized your Python web application. You can now distribute and deploy your application as a Docker image, making it easier to manage and run in various environments.

---

## Dockerize a simple python app with database

1. **Create a Spring Boot Application**:
   Develop your Spring Boot application with the necessary endpoints and data access logic. Ensure that your application interacts with the database using Spring Data JPA or another persistence mechanism.

2. **Write Dockerfile**:
   Create a Dockerfile in the root directory of your Spring Boot project. Here's a basic example:

```Dockerfile
# Use an official OpenJDK runtime as a base image
FROM openjdk:11-jre-slim

# Set the working directory in the container
WORKDIR /app

# Copy the packaged jar file into the container at /app
COPY target/your-application-name.jar /app

# Specify the command to run your application
CMD ["java", "-jar", "your-application-name.jar"]
```

Replace `"your-application-name.jar"` with the actual name of your Spring Boot executable jar file.

3. **Write application.properties**:
   Create an `application.properties` file in the `src/main/resources` directory of your Spring Boot project. Configure the database connection properties. For example:

```
spring.datasource.url=jdbc:postgresql://db:5432/your_database_name
spring.datasource.username=your_username
spring.datasource.password=your_password
spring.datasource.driver-class-name=org.postgresql.Driver
spring.jpa.hibernate.ddl-auto=update
```

Replace `your_database_name`, `your_username`, and `your_password` with your preferred database configuration.

4. **Write docker-compose.yml**:
   Create a `docker-compose.yml` file to define services for your application and database. Here's an example:

```yaml
version: '3'

services:
  app:
    build: .
    ports:
      - "8080:8080"
    depends_on:
      - db
  db:
    image: postgres:latest
    ports:
      - "5432:5432"
    environment:
      POSTGRES_DB: your_database_name
      POSTGRES_USER: your_username
      POSTGRES_PASSWORD: your_password
```

Replace `your_database_name`, `your_username`, and `your_password` with your preferred database configuration.

5. **Build and Run Docker Containers**:
   Open a terminal, navigate to the directory containing your Dockerfile and `docker-compose.yml`, and execute the following command:

```bash
docker-compose up --build
```

This command will build Docker images for your application and database and start the containers.

6. **Access Your Application**:
   Your Spring Boot application should now be running inside a Docker container, along with the PostgreSQL database. You can access it by opening a web browser and navigating to `http://localhost:8080`.

That's it! You've successfully dockerized your Spring Boot application with a database. You can now distribute and deploy your application as a Docker image, making it easier to manage and run in various environments.

---

# sample example where the application and the database are in one container

1. **Create a Dockerfile**:
   
   Create a Dockerfile in the root directory of your project. Below is an example of a Dockerfile that installs both Java (for running Spring Boot) and PostgreSQL:

   ```Dockerfile
   # Use an official Ubuntu runtime as a base image
   FROM ubuntu:20.04

   # Install OpenJDK 11
   RUN apt-get update && \
       apt-get install -y openjdk-11-jdk && \
       apt-get clean;

   # Install PostgreSQL
   RUN apt-get update && \
       apt-get install -y postgresql postgresql-contrib && \
       apt-get clean;

   # Set the working directory in the container
   WORKDIR /app

   # Copy the Spring Boot application JAR into the container at /app
   COPY target/your-application-name.jar /app

   # Expose ports for Spring Boot and PostgreSQL
   EXPOSE 8080 5432

   # Specify the command to run both Spring Boot and PostgreSQL
   CMD service postgresql start && java -jar your-application-name.jar
   ```

   Replace `"your-application-name.jar"` with the actual name of your Spring Boot executable JAR file.

2. **Configure application.properties**:

   In your Spring Boot application, configure the `application.properties` file to connect to the PostgreSQL database running on localhost.

   ```properties
   spring.datasource.url=jdbc:postgresql://localhost:5432/your_database_name
   spring.datasource.username=your_username
   spring.datasource.password=your_password
   spring.datasource.driver-class-name=org.postgresql.Driver
   spring.jpa.hibernate.ddl-auto=update
   ```

   Replace `your_database_name`, `your_username`, and `your_password` with your preferred database configuration.

3. **Build and Run the Docker Image**:

   Build the Docker image using the following command:

   ```bash
   docker build -t your-image-name .
   ```

   Replace `"your-image-name"` with the desired name for your Docker image.

   Once the image is built, run the Docker container:

   ```bash
   docker run -p 8080:8080 -p 5432:5432 your-image-name
   ```

   This command maps port 8080 for your Spring Boot application and port 5432 for PostgreSQL.

4. **Access Your Application**:

   Your Spring Boot application and PostgreSQL database should now be running within the same Docker container. You can access the application by opening a web browser and navigating to `http://localhost:8080`.

   Note: Running both the application and database in a single container is not recommended for production use due to scalability and separation of concerns. It's better to use separate containers for better manageability and scalability.


---

# Video tutorials instalation

[Windows youtube1 ](https://www.youtube.com/watch?v=ZyBBv1JmnWQ)

[Ubuntu youtube2 ](https://www.youtube.com/watch?v=5_EA3rBCXmU)

[Mac youtube2 ](https://www.youtube.com/watch?v=-EXlfSsP49A)

# Video tutorials dockerize

[Spring youtube1 ](https://www.youtube.com/watch?v=lnnNshOlXjo&t=881s)

[Python youtube2 ](https://www.youtube.com/watch?v=0TFWtfFY87U&t=965s)

# Video tutorials microservice app

[Spring youtube1 ](https://www.youtube.com/watch?v=O_XL9oQ1_To&t=1264s)

[Python youtube2 ](https://www.youtube.com/watch?v=0iB5IPoTDts)

