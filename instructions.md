# Project Table of Contents
## Table of Contents

1. [Detailed Instructions to Run a Static Website with Docker Desktop](#detailed-instructions-to-run-a-static-website-with-docker-desktop)
2. [Detailed Instructions: Introducing Persistent Storage in Docker with Nginx](#detailed-instructions-introducing-persistent-storage-in-docker-with-nginx)
3. [Detailed Instructions: Running a MySQL Container with Persistent Storage](#detailed-instructions-running-a-mysql-container-with-persistent-storage)
4. [Detailed Instructions: Setting Up PHP-FPM and Nginx for Dynamic Content](#detailed-instructions-setting-up-php-fpm-and-nginx-for-dynamic-content)
5. [Detailed Instructions: Bringing It All Together with Docker Compose](#detailed-instructions-bringing-it-all-together-with-docker-compose)
6. [Detailed Instructions: Adding Persistent Storage for WordPress](#detailed-instructions-adding-persistent-storage-for-wordpress)

# Detailed Instructions to Run a Static Website with Docker Desktop

This guide will walk you through the process of creating a simple static website using Docker Desktop. You'll create an `index.html` file, write a `Dockerfile`, build a Docker image, and run a container to serve your website locally on `http://localhost:8080`.

## Prerequisites

- **Docker Desktop installed** on your machine.
  - Download and install Docker Desktop from [Docker's official website](https://www.docker.com/products/docker-desktop) if you haven't already.
- **Basic knowledge of command-line interface** (CLI) commands.

---

## Step 1: Create a Project Directory

1. **Open a Terminal or Command Prompt:**
   - Windows: You can use Command Prompt, PowerShell, or Git Bash.
   - macOS/Linux: Use the Terminal application.

2. **Create a New Directory for Your Project:**
   ```bash
   mkdir docker-static-website
   ```
3. **Navigate into the Project Directory:**
   ```bash
   cd docker-static-website
   ```

---

## Step 2: Create the `index.html` File

1. **Create a New `index.html` File:**
   - You can use any text editor (e.g., Notepad, Visual Studio Code, Sublime Text).

2. **Add the Following HTML Content:**
   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <title>Hello World</title>
   </head>
   <body>
       <h1>Hello World</h1>
   </body>
   </html>
   ```

3. **Save the File:**
   - Ensure it's saved in the `docker-static-website` directory.

---

## Step 3: Write the Dockerfile

1. **Create a New File Named `Dockerfile`:**
   - No file extension is needed.

2. **Add the Following Instructions to the Dockerfile:**
   ```Dockerfile
   FROM nginx
   COPY index.html /usr/share/nginx/html/
   ```

   **Explanation:**
   - `FROM nginx`: Specifies the base image to be the official Nginx image from Docker Hub.
   - `COPY index.html /usr/share/nginx/html/`: Copies your `index.html` file into the Nginx web directory inside the container.

3. **Save the Dockerfile:**
   - Ensure it's saved in the `docker-static-website` directory alongside `index.html`.

---

## Step 4: Build the Docker Image

1. **Open Your Terminal in the Project Directory:**
   - Ensure you're inside the `docker-static-website` directory.

2. **Build the Docker Image Using the Dockerfile:**
   ```bash
   docker build -t my-nginx-image .
   ```
   **Explanation:**
   - `docker build`: Command to build a Docker image.
   - `-t my-nginx-image`: Tags the image with the name `my-nginx-image`.
   - `.`: Specifies the current directory as the build context.

3. **Wait for the Build to Complete:**
   - Docker will execute the instructions in your Dockerfile.

4. **Verify the Image is Built:**
   ```bash
   docker images
   ```
   - You should see `my-nginx-image` listed among your Docker images.

---

## Step 5: Run the Docker Container

1. **Run the Container and Map Ports:**
   ```bash
   docker run -d -p 8080:80 --name my-nginx-container my-nginx-image
   ```
   **Explanation:**
   - `docker run`: Command to run a container.
   - `-d`: Runs the container in detached mode (in the background).
   - `-p 8080:80`: Maps port 80 in the container to port 8080 on your local machine.
   - `--name my-nginx-container`: Names the container for easy reference.
   - `my-nginx-image`: The image to create the container from.

2. **Verify the Container is Running:**
   ```bash
   docker ps
   ```
   - Look for `my-nginx-container` in the list of running containers.

---

## Step 6: Access Your Static Website

1. **Open a Web Browser:**
   - Use any web browser of your choice.

2. **Navigate to `http://localhost:8080`:**
   - You should see your "Hello World" webpage displayed.

---

## Step 7: Clean Up (Optional)

If you want to stop and remove the container:

1. **Stop the Running Container:**
   ```bash
   docker stop my-nginx-container
   ```

2. **Remove the Container:**
   ```bash
   docker rm my-nginx-container
   ```

---

## Troubleshooting Tips

- **Port Conflict Error:**
  - If you receive an error about port 8080 already being in use, choose an alternative port (e.g., 8081):
    ```bash
    docker run -d -p 8081:80 --name my-nginx-container my-nginx-image
    ```
    - Then access the website at `http://localhost:8081`.

- **Changes to `index.html` Not Reflected:**
  - If you modify `index.html`, you need to rebuild the image and recreate the container:
    ```bash
    docker build -t my-nginx-image .
    docker stop my-nginx-container
    docker rm my-nginx-container
    docker run -d -p 8080:80 --name my-nginx-container my-nginx-image
    ```

- **View Container Logs:**
  - To see logs from your running container:
    ```bash
    docker logs my-nginx-container
    ```

---

## Additional Information

- **Understanding the Dockerfile:**
  - The Dockerfile uses the official Nginx image as a base, which already has Nginx installed and configured to serve content from `/usr/share/nginx/html/`.
  - By copying your `index.html` into this directory, Nginx will serve your content when the container is run.

- **Using Docker Desktop GUI (Optional):**
  - Open Docker Desktop to view running containers, images, and manage them using the graphical interface.
  - You can start, stop, and delete containers and images from the GUI.

---

## Expected Outcome

By completing these steps, you should have:

- A Docker image named `my-nginx-image` that contains your static website.
- A running Docker container named `my-nginx-container` serving your website.
- Access to your "Hello World" webpage at `http://localhost:8080`.

---

## Conclusion

You've successfully:

- Created a simple static website.
- Used Docker to containerize the website with Nginx.
- Built and ran a Docker image and container.
- Served your website locally using Docker Desktop.

This foundational knowledge prepares you for more complex applications, like running multi-container setups with databases and backend services.

# Detailed Instructions: Introducing Persistent Storage in Docker with Nginx

In this step, you'll learn how to persist data in Docker by using a bind mount. This allows you to edit your website files on your local machine and see the changes reflected live in the running Docker container. This is especially useful for development purposes.

---

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Step 1: Set Up the Project Directory](#step-1-set-up-the-project-directory)
3. [Step 2: Create the Website Files](#step-2-create-the-website-files)
4. [Step 3: Run the Nginx Container with a Bind Mount](#step-3-run-the-nginx-container-with-a-bind-mount)
5. [Step 4: Access Your Website](#step-4-access-your-website)
6. [Step 5: Update the HTML File and See Changes Live](#step-5-update-the-html-file-and-see-changes-live)
7. [Troubleshooting Tips](#troubleshooting-tips)
8. [Conclusion](#conclusion)

---

## Prerequisites

- **Docker Desktop installed** on your machine.
  - If not installed, download it from [Docker's official website](https://www.docker.com/products/docker-desktop) and follow the installation instructions.
- **Basic knowledge of command-line interface** (CLI) commands.
- **Completed Step 1**: Running a static website with Docker (optional but helpful).

---

## Step 1: Set Up the Project Directory

1. **Open a Terminal or Command Prompt:**
   - Windows: Use Command Prompt, PowerShell, or Git Bash.
   - macOS/Linux: Use the Terminal application.

2. **Create a New Directory for Your Project:**
   ```bash
   mkdir docker-persistent-website
   ```

3. **Navigate into the Project Directory:**
   ```bash
   cd docker-persistent-website
   ```

4. **Create a Subdirectory for Your Website Files:**
   ```bash
   mkdir website
   ```

   - This `website` directory will hold your HTML files and will be mounted into the Docker container.

---

## Step 2: Create the Website Files

1. **Navigate to the `website` Directory:**
   ```bash
   cd website
   ```

2. **Create an `index.html` File:**
   - Use any text editor (e.g., Notepad, Visual Studio Code, Sublime Text).

3. **Add the Following HTML Content:**
   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <title>Persistent Storage Test</title>
   </head>
   <body>
       <h1>Hello World with Persistent Storage!</h1>
   </body>
   </html>
   ```

4. **Save the File:**
   - Ensure it's saved as `index.html` in the `docker-persistent-website/website` directory.

5. **Navigate Back to the Project Root Directory:**
   ```bash
   cd ..
   ```

---

## Step 3: Run the Nginx Container with a Bind Mount

1. **Understand the Command:**

   The command you'll run is:
   ```bash
   docker run -p 8080:80 -v "$(pwd)/website":/usr/share/nginx/html nginx
   ```

   **Explanation:**

   - `docker run`: Command to run a new container.
   - `-p 8080:80`: Maps port 80 in the container to port 8080 on your local machine.
   - `-v $(pwd)/website:/usr/share/nginx/html`: Creates a bind mount.
     - `$(pwd)/website`: The absolute path to your local `website` directory.
       - On Windows Command Prompt, use `%cd%` instead of `$(pwd)`.
       - On PowerShell, use `${PWD}`.
     - `/usr/share/nginx/html`: The directory in the container where Nginx serves files from.
   - `nginx`: The official Nginx Docker image.

2. **Run the Docker Container:**

   **For macOS/Linux:**
   ```bash
   docker run -d -p 8080:80 -v "$(pwd)/website":/usr/share/nginx/html --name nginx-persistent nginx
   ```

   **For Windows Command Prompt:**
   ```bash
   docker run -d -p 8080:80 -v %cd%/website:/usr/share/nginx/html --name nginx-persistent nginx
   ```

   **For Windows PowerShell:**
   ```bash
   docker run -d -p 8080:80 -v ${PWD}/website:/usr/share/nginx/html --name nginx-persistent nginx
   ```

   **Explanation of Additional Flags:**

   - `-d`: Runs the container in detached mode (in the background).
   - `--name nginx-persistent`: Assigns the name `nginx-persistent` to the container for easier management.

3. **Verify the Container is Running:**
   ```bash
   docker ps
   ```
   - You should see `nginx-persistent` listed among the running containers.

---

## Step 4: Access Your Website

1. **Open a Web Browser:**

2. **Navigate to `http://localhost:8080`:**
   - You should see the webpage with the heading "Hello World with Persistent Storage!".

---

## Step 5: Update the HTML File and See Changes Live

1. **Edit the `index.html` File on Your Local Machine:**

   - Open `docker-persistent-website/website/index.html` in your text editor.

2. **Modify the `<h1>` Heading:**
   ```html
   <h1>Live Update: Persistent Storage is Working!</h1>
   ```

3. **Save the File.**

4. **Refresh the Webpage in Your Browser:**

   - Go back to `http://localhost:8080` and refresh the page.
   - You should now see the updated heading: "Live Update: Persistent Storage is Working!"

---

## Troubleshooting Tips

### 1. Permission Issues on macOS/Linux

- If you encounter permission issues, you may need to adjust the permissions of your `website` directory:
  ```bash
  chmod -R 755 website
  ```

### 2. Port Already in Use

- **Error Message:** `Error starting userland proxy: listen tcp 0.0.0.0:8080: bind: address already in use.`

- **Solution:** Use a different port, such as 8081:
  ```bash
  docker run -d -p 8081:80 -v $(pwd)/website:/usr/share/nginx/html --name nginx-persistent nginx
  ```
  - Access the website at `http://localhost:8081`.

### 3. Changes Not Reflected

- Ensure you have saved the changes to `index.html`.

- **Container Caching:**
  - Nginx may cache files. You can force a refresh:
    ```bash
    docker exec -it nginx-persistent nginx -s reload
    ```
  - Or stop and start the container:
    ```bash
    docker stop nginx-persistent
    docker start nginx-persistent
    ```

### 4. Verify the Bind Mount

- **Check if the Volume is Mounted Correctly:**
  ```bash
  docker inspect nginx-persistent
  ```
  - Look under "Mounts" to see if your local directory is correctly mounted to `/usr/share/nginx/html`.

### 5. Access Denied Errors on Windows

- Ensure that Docker Desktop has permission to access the drive where your `website` directory is located.
  - Go to Docker Desktop settings:
    - **Settings** > **Resources** > **File Sharing** (for older versions).
    - **Settings** > **Resources** > **File Sharing** or **File System** (for newer versions).
    - Make sure your drive is listed and shared.

---

## Conclusion

By completing these steps, you have:

- Learned how to use a **bind mount** to persist data between your host system and a Docker container.
- Set up a Dockerized Nginx server that serves content from your local machine.
- Observed live updates to your website by editing files on your host system.

---

## Additional Information

### Understanding Bind Mounts

- **Bind Mounts vs. Volumes:**
  - **Bind Mounts**: Directly mount a file or directory from the host into the container. Useful for development when you need immediate access to files.
  - **Volumes**: Managed by Docker and stored in Docker's directory on the host (e.g., `/var/lib/docker/volumes/`). Better for data persistence in production environments.

### Cleaning Up

- **Stop and Remove the Container:**
  ```bash
  docker stop nginx-persistent
  docker rm nginx-persistent
  ```

- **Remove Unused Images (Optional):**
  ```bash
  docker image prune
  ```

### Using Docker Compose (Optional)

- For more complex setups, consider using Docker Compose to define services, volumes, and networks in a `docker-compose.yml` file.

---

## What's Next?

With this knowledge, you're now equipped to:

- Develop websites and applications locally using Docker containers.
- Understand how to persist data between container restarts.
- Move on to more advanced topics like multi-container applications with Docker Compose.

# Detailed Instructions: Running a MySQL Container with Persistent Storage

In this step, you'll learn how to set up a MySQL container using Docker, configure it with environment variables, and ensure that your data persists across container restarts using a named volume. This is crucial for applications that rely on a database, like WordPress.

---

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Understanding Environment Variables](#understanding-environment-variables)
3. [Step 1: Pull the Official MySQL Docker Image](#step-1-pull-the-official-mysql-docker-image)
4. [Step 2: Create a Named Volume for Data Persistence](#step-2-create-a-named-volume-for-data-persistence)
5. [Step 3: Run the MySQL Container](#step-3-run-the-mysql-container)
6. [Step 4: Verify the MySQL Container is Running](#step-4-verify-the-mysql-container-is-running)
7. [Step 5: Connect to the MySQL Database (Optional)](#step-5-connect-to-the-mysql-database-optional)
8. [Troubleshooting Tips](#troubleshooting-tips)
9. [Conclusion](#conclusion)
10. [What's Next?](#whats-next)

---

## Prerequisites

- **Docker Desktop installed** on your machine.
- **Basic knowledge of command-line interface** (CLI) commands.
- **Previous Steps Completed** (Optional but Recommended):
  - Step 1: Running a static website with Docker.
  - Step 2: Introducing persistent storage.

---

## Understanding Environment Variables

Environment variables are used to configure settings in Docker containers at runtime without hardcoding them into your application. The MySQL Docker image uses several environment variables for initial configuration:

- **MYSQL_ROOT_PASSWORD**: Sets the password for the MySQL root user.
- **MYSQL_DATABASE**: Creates a database with the specified name.
- **MYSQL_USER** and **MYSQL_PASSWORD** (Optional): Creates a new user with the specified credentials.

---

## Step 1: Pull the Official MySQL Docker Image

Before running the MySQL container, ensure you have the latest MySQL image.

1. **Open a Terminal or Command Prompt**.

2. **Pull the Latest MySQL Image**:

   ```bash
   docker pull mysql:latest
   ```

   - This command fetches the latest MySQL image from Docker Hub.

3. **Verify the Image is Pulled**:

   ```bash
   docker images
   ```

   - Look for an entry named `mysql` with the `latest` tag.

---

## Step 2: Create a Named Volume for Data Persistence

A named volume ensures that the data stored by MySQL persists even if the container is deleted.

1. **Create a Named Volume** (Optional, Docker will create it automatically if it doesn't exist):

   ```bash
   docker volume create mysql_data
   ```

   - `mysql_data` is the name of the volume where MySQL data will be stored.

2. **Verify the Volume is Created**:

   ```bash
   docker volume ls
   ```

   - Look for `mysql_data` in the list of volumes.

---

## Step 3: Run the MySQL Container

Now, you'll run the MySQL container with the necessary environment variables and mount the named volume.

1. **Run the Container**:

   ```bash
   docker run -d \
     --name mysql-db \
     -e MYSQL_ROOT_PASSWORD=my-secret-pw \
     -e MYSQL_DATABASE=wordpress \
     -v mysql_data:/var/lib/mysql \
     mysql:latest
   ```

   **Explanation**:

   - `docker run`: Command to run a new container.
   - `-d`: Runs the container in detached mode (in the background).
   - `--name mysql-db`: Assigns the name `mysql-db` to the container.
   - `-e MYSQL_ROOT_PASSWORD=my-secret-pw`: Sets the root password for MySQL.
   - `-e MYSQL_DATABASE=wordpress`: Creates a new database named `wordpress`.
   - `-v mysql_data:/var/lib/mysql`: Mounts the named volume `mysql_data` to `/var/lib/mysql` inside the container, where MySQL stores its data.
   - `mysql:latest`: Specifies the image to use.

2. **Note on Environment Variables**:

   - **MYSQL_ROOT_PASSWORD** is mandatory. Without it, the container will fail to start.
   - **MYSQL_DATABASE** is optional but useful if you want to pre-create a database.

3. **Security Tip**:

   - Avoid using simple passwords like `my-secret-pw` in production environments.
   - Consider using strong, complex passwords or secrets management systems.

---

## Step 4: Verify the MySQL Container is Running

1. **Check Running Containers**:

   ```bash
   docker ps
   ```

   - You should see `mysql-db` listed among the running containers.

2. **Check Container Logs**:

   ```bash
   docker logs mysql-db
   ```

   - Look for messages indicating that MySQL has started successfully.
   - If there are any errors, they will be displayed here.

---

## Step 5: Connect to the MySQL Database (Optional)

You can connect to the MySQL database to verify that it's running and the database has been created.

### Option A: Using the MySQL Client Inside the Container

1. **Access the MySQL Command-Line Client Inside the Container**:

   ```bash
   docker exec -it mysql-db mysql -p
   ```

   - When prompted, enter the root password: `my-secret-pw`.

2. **List the Databases**:

   ```sql
   SHOW DATABASES;
   ```

   - You should see the `wordpress` database listed.

3. **Exit the MySQL Client**:

   ```sql
   EXIT;
   ```

### Option B: Connecting from Your Host Machine (Requires Port Mapping)

If you need to connect to the MySQL server from your host machine or another container, you'll need to map the MySQL port.

1. **Stop and Remove the Existing Container**:

   ```bash
   docker stop mysql-db
   docker rm mysql-db
   ```

2. **Run the Container with Port Mapping**:

   ```bash
   docker run -d \
     --name mysql-db \
     -p 3306:3306 \
     -e MYSQL_ROOT_PASSWORD=my-secret-pw \
     -e MYSQL_DATABASE=wordpress \
     -v mysql_data:/var/lib/mysql \
     mysql:latest
   ```

   - `-p 3306:3306`: Maps port 3306 in the container to port 3306 on your host.

3. **Connect Using a MySQL Client from Your Host**:

   - **Install a MySQL Client** if you don't have one (e.g., MySQL Workbench, HeidiSQL, or the MySQL command-line client).

   - **Connect to MySQL**:

     - **Hostname**: `localhost` or `127.0.0.1`
     - **Port**: `3306`
     - **Username**: `root`
     - **Password**: `my-secret-pw`

   - **Note**: Ensure no other MySQL service is running on your host machine's port 3306.

4. **Verify the Database Exists**:

   - Once connected, run:

     ```sql
     SHOW DATABASES;
     ```

     - You should see the `wordpress` database.

---

## Troubleshooting Tips

### 1. Container Fails to Start

- **Check Logs**:

  ```bash
  docker logs mysql-db
  ```

- **Common Issues**:

  - Missing `MYSQL_ROOT_PASSWORD`.
  - Incorrect permissions on the volume.
  - Port conflicts (if mapping port 3306).

### 2. Permission Issues with Volume

- Ensure Docker has the correct permissions to write to the volume.

- **On Windows**:

  - Check Docker Desktop settings to ensure the drive is shared.

### 3. Can't Connect to MySQL from Host

- Ensure you've mapped the port (`-p 3306:3306`).

- MySQL may bind to `127.0.0.1` inside the container. Consider adjusting the MySQL configuration if necessary.

### 4. Data Not Persisting

- Ensure you're using the named volume `mysql_data`.

- Verify the volume is correctly mounted:

  ```bash
  docker inspect mysql-db
  ```

  - Look under "Mounts" to see if `mysql_data` is mounted to `/var/lib/mysql`.

---

## Conclusion

By following these steps, you've successfully:

- Pulled and used the official MySQL Docker image.
- Configured the MySQL container using environment variables.
- Set up data persistence using a named volume.
- (Optionally) Connected to the MySQL database to verify it's working.

---

## Expected Outcome

- A running MySQL container named `mysql-db`.
- Data persisted in the `mysql_data` volume, ensuring data is not lost when the container restarts.
- A MySQL database named `wordpress` created inside the container.
- The ability to connect to the MySQL database from your host machine (if port mapping is set up).

---

## What's Next?

With MySQL running, you're now ready to:

- Set up a WordPress container that connects to this MySQL database.
- Use Docker Compose to orchestrate multiple containers (e.g., Nginx, PHP, MySQL, WordPress).
- Learn more about networking between containers.

---

### Additional Notes

- **Security Considerations**:

  - In production, avoid exposing MySQL's port to the host unless necessary.
  - Use complex passwords and consider environment variable management.

- **Environment Variable Options**:

  - **MYSQL_USER** and **MYSQL_PASSWORD**:

    - Create a new user besides `root`.

    ```bash
    -e MYSQL_USER=wordpressuser \
    -e MYSQL_PASSWORD=wordpresspass \
    ```

  - **Usage**:

    - The new user `wordpressuser` with password `wordpresspass` can be used by your application to connect to the `wordpress` database.

---

### Clean Up (Optional)

If you wish to stop and remove the MySQL container and volume:

1. **Stop and Remove the Container**:

   ```bash
   docker stop mysql-db
   docker rm mysql-db
   ```

2. **Remove the Named Volume**:

   ```bash
   docker volume rm mysql_data
   ```

   - **Warning**: This will delete all data stored in the volume.

---

### References

- [Official MySQL Docker Image Documentation](https://hub.docker.com/_/mysql)

---

This comprehensive guide should help you set up and run a MySQL container with persistent storage using Docker. If you encounter any issues or have further questions, consult the Docker and MySQL documentation or reach out to the community for support.

# Detailed Instructions: Setting Up PHP-FPM and Nginx for Dynamic Content

In this step, you will learn how to set up Nginx and PHP-FPM in separate Docker containers to serve dynamic PHP content. By the end of this guide, you'll have a simple PHP application running, with Nginx serving as the web server and PHP-FPM processing PHP scripts.

---

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Understanding the Architecture](#understanding-the-architecture)
3. [Step 1: Set Up the Project Directory](#step-1-set-up-the-project-directory)
4. [Step 2: Create a Simple PHP Application](#step-2-create-a-simple-php-application)
5. [Step 3: Set Up the PHP-FPM Container](#step-3-set-up-the-php-fpm-container)
6. [Step 4: Configure Nginx to Work with PHP-FPM](#step-4-configure-nginx-to-work-with-php-fpm)
7. [Step 5: Run the Nginx Container](#step-5-run-the-nginx-container)
8. [Step 6: Test the Setup](#step-6-test-the-setup)
9. [Troubleshooting Tips](#troubleshooting-tips)
10. [Conclusion](#conclusion)
11. [What's Next?](#whats-next)

---

## Prerequisites

- **Docker Desktop installed** on your machine.
- **Basic knowledge of command-line interface** (CLI) commands.
- **Familiarity with Docker concepts** like images, containers, volumes, and networking.
- **Previous Steps Completed** (Optional but Recommended):
  - Step 1: Running a static website with Docker.
  - Step 2: Introducing persistent storage.
  - Step 3: Running a MySQL container.

---

## Understanding the Architecture

- **Nginx Container**: Acts as the web server, serving static content and forwarding PHP requests to PHP-FPM.
- **PHP-FPM Container**: Processes PHP scripts and returns the output to Nginx.
- **Communication**: Nginx and PHP-FPM containers communicate over a Docker network.

---

## Step 1: Set Up the Project Directory

1. **Open a Terminal or Command Prompt**.

2. **Create a New Directory for Your Project**:

   ```bash
   mkdir docker-php-nginx
   ```

3. **Navigate into the Project Directory**:

   ```bash
   cd docker-php-nginx
   ```

4. **Create a Subdirectory for Your Application Files**:

   ```bash
   mkdir app
   ```

   - The `app` directory will contain your PHP application files.

---

## Step 2: Create a Simple PHP Application

1. **Navigate to the `app` Directory**:

   ```bash
   cd app
   ```

2. **Create an `index.php` File**:

   - Use any text editor to create the file.

3. **Add the Following PHP Content**:

   ```php
   <?php
   phpinfo();
   ?>
   ```

   - This script displays PHP configuration information, useful for testing.

4. **Save the File**.

5. **Navigate Back to the Project Root Directory**:

   ```bash
   cd ..
   ```

---

## Step 3: Set Up the PHP-FPM Container

You'll use the official `php:fpm` Docker image to set up the PHP-FPM container.

1. **Create a `Dockerfile` for PHP-FPM** (Optional):

   - If you need to install additional PHP extensions or configurations, create a `Dockerfile` in the project root or a subdirectory.
   - For this basic setup, we can use the image directly.

2. **Run the PHP-FPM Container**:

   ```bash
   docker run -d \
     --name php-fpm \
     -v "$(pwd)/app":/var/www/html \
     php:fpm
   ```

   **Explanation**:

   - `docker run`: Command to run a new container.
   - `-d`: Runs the container in detached mode.
   - `--name php-fpm`: Names the container `php-fpm`.
   - `-v $(pwd)/app:/var/www/html`: Mounts the `app` directory to `/var/www/html` in the container.
     - **For Windows Command Prompt**: Use `%cd%` instead of `$(pwd)`.
     - **For Windows PowerShell**: Use `${PWD}`.

3. **Verify the Container is Running**:

   ```bash
   docker ps
   ```

   - You should see `php-fpm` listed.

---

## Step 4: Configure Nginx to Work with PHP-FPM

You'll set up Nginx to serve static files and forward PHP requests to the PHP-FPM container.

### 1. Create a Custom Nginx Configuration File

1. **Create a Directory for Nginx Configuration**:

   ```bash
   mkdir nginx
   ```

2. **Create a Custom Nginx Configuration File**:

   - Create a file named `default.conf` inside the `nginx` directory.

3. **Add the Following Configuration**:

   ```nginx
   server {
       listen 80;
       server_name localhost;

       root /var/www/html;
       index index.php index.html index.htm;

       location / {
           try_files $uri $uri/ =404;
       }

       location ~ \.php$ {
           try_files $uri =404;
           fastcgi_pass php-fpm:9000;
           fastcgi_index index.php;
           fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
           include fastcgi_params;
       }
   }
   ```

   **Explanation**:

   - **listen 80**: Nginx listens on port 80 inside the container.
   - **root /var/www/html**: The document root where Nginx looks for files.
   - **location /**: Handles requests to the root URL.
   - **location ~ \.php$**: Handles requests for `.php` files.
     - **fastcgi_pass php-fpm:9000**: Forwards PHP requests to the `php-fpm` service on port `9000`.
     - **fastcgi_param SCRIPT_FILENAME**: Sets the path to the PHP script.

4. **Save the File**.

### 2. Create a Custom Nginx Dockerfile

1. **Create a `Dockerfile` for Nginx** in the `nginx` directory.

2. **Add the Following Instructions**:

   ```Dockerfile
   FROM nginx

   COPY default.conf /etc/nginx/conf.d/default.conf
   ```

   **Explanation**:

   - **FROM nginx**: Uses the official Nginx image as the base.
   - **COPY default.conf**: Copies your custom Nginx configuration into the container.

3. **Save the Dockerfile**.

---

## Step 5: Run the Nginx Container

1. **Build the Custom Nginx Image**:

   ```bash
   docker build -t custom-nginx ./nginx
   ```

   **Explanation**:

   - `docker build`: Builds a Docker image.
   - `-t custom-nginx`: Tags the image as `custom-nginx`.
   - `./nginx`: Specifies the build context as the `nginx` directory.

2. **Create a Docker Network for the Containers**:

   - This allows the Nginx and PHP-FPM containers to communicate.

   ```bash
   docker network create nginx-php-network
   ```

3. **Connect the PHP-FPM Container to the Network**:

   ```bash
   docker network connect nginx-php-network php-fpm
   ```

4. **Run the Nginx Container**:

   ```bash
   docker run -d \
     --name nginx \
     -p 8080:80 \
     -v "$(pwd)/app":/var/www/html \
     --network nginx-php-network \
     custom-nginx
   ```

   **Explanation**:

   - `--name nginx`: Names the container `nginx`.
   - `-p 8080:80`: Maps port 80 in the container to port 8080 on your host.
   - `-v $(pwd)/app:/var/www/html`: Mounts the `app` directory to `/var/www/html` in the container.
   - `--network nginx-php-network`: Connects the container to the network.
   - `custom-nginx`: Uses the custom Nginx image built earlier.

5. **Verify Both Containers are Running and Connected**:

   ```bash
   docker ps
   docker network inspect nginx-php-network
   ```

   - Ensure both `nginx` and `php-fpm` are connected to the `nginx-php-network`.

---

## Step 6: Test the Setup

1. **Open a Web Browser**.

2. **Navigate to `http://localhost:8080`**.

   - You should see the PHP information page generated by `phpinfo()`.
   - This confirms that Nginx is serving the page and PHP-FPM is processing the PHP script.

3. **Modify the `index.php` File** (Optional):

   - Edit `app/index.php` to:

     ```php
     <?php
     echo "<h1>Hello from PHP-FPM and Nginx!</h1>";
     ?>
     ```

   - Refresh the page in your browser to see the updated content.

---

## Troubleshooting Tips

### 1. Nginx Error 502 Bad Gateway

- **Cause**: Nginx cannot connect to the PHP-FPM container.
- **Solution**:
  - Ensure both containers are connected to the same network (`nginx-php-network`).
  - Verify the service name (`php-fpm`) used in `fastcgi_pass` matches the container name.
  - Check if PHP-FPM is listening on port `9000`.

### 2. Changes Not Reflected

- **Cause**: Cached content or synchronization issues.
- **Solution**:
  - Clear your browser cache.
  - Ensure you've saved changes to the `index.php` file.
  - Restart the Nginx container:

    ```bash
    docker restart nginx
    ```

### 3. Permission Issues

- **Cause**: File permission restrictions between host and container.
- **Solution**:
  - Modify permissions of the `app` directory:

    ```bash
    chmod -R 755 app
    ```

### 4. Network Connectivity Problems

- **Cause**: Containers are not on the same network.
- **Solution**:
  - Check networks:

    ```bash
    docker network ls
    docker network inspect nginx-php-network
    ```

  - Connect the containers to the network if necessary.

### 5. Port Already in Use

- **Cause**: Another service is using port 8080.
- **Solution**:
  - Use a different port, e.g., `-p 8081:80`, and access `http://localhost:8081`.

---

## Conclusion

By following these steps, you've successfully:

- Set up two Docker containers: one running Nginx and one running PHP-FPM.
- Configured Nginx to forward PHP requests to the PHP-FPM container using `fastcgi_pass`.
- Created a simple PHP application to test the setup.
- Verified that dynamic PHP content is served correctly.

---

## Expected Outcome

- **A running Nginx container** serving web requests on `http://localhost:8080`.
- **A running PHP-FPM container** processing PHP scripts.
- **Dynamic PHP content** displayed in your browser, confirming that Nginx and PHP-FPM are working together.

---

## What's Next?

With Nginx and PHP-FPM set up, you're now ready to:

- **Integrate MySQL**: Connect your PHP application to the MySQL database set up in the previous step.
- **Set Up WordPress**: Use these containers to run a WordPress site.
- **Use Docker Compose**: Simplify the setup by defining services in a `docker-compose.yml` file.

---

## Additional Information

### Using Docker Compose (Recommended for Multi-Container Applications)

- **Advantages**:
  - Easier management of multiple containers.
  - Simplified networking and volume management.
  - Configuration stored in a single `docker-compose.yml` file.

- **Sample `docker-compose.yml` File**:

  ```yaml
  version: '3.8'

  services:
    nginx:
      build:
        context: .
        dockerfile: ./nginx/Dockerfile
      ports:
        - "8080:80"
      volumes:
        - ./app:/var/www/html
      depends_on:
        - php-fpm
      networks:
        - app-network

    php-fpm:
      image: php:fpm
      volumes:
        - ./app:/var/www/html
      networks:
        - app-network

  networks:
    app-network:
      driver: bridge
  ```

- **Running with Docker Compose**:

  ```bash
  docker-compose up -d
  ```

### Security Considerations

- **Do Not Use in Production Without Modifications**:
  - This setup is for development and learning purposes.
  - For production, consider additional security measures like handling SSL, configuring firewalls, and hardening the containers.

### Cleaning Up

- **Stop and Remove Containers**:

  ```bash
  docker stop nginx php-fpm
  docker rm nginx php-fpm
  ```

- **Remove the Network**:

  ```bash
  docker network rm nginx-php-network
  ```

- **Remove Images (Optional)**:

  ```bash
  docker rmi custom-nginx
  ```

---

## References

- [Official Nginx Docker Image](https://hub.docker.com/_/nginx)
- [Official PHP-FPM Docker Image](https://hub.docker.com/_/php)
- [Nginx Configuration for PHP-FPM](https://www.nginx.com/resources/wiki/start/topics/examples/phpfcgi/)

---

This comprehensive guide should help you set up Nginx and PHP-FPM in Docker containers to serve dynamic PHP content. If you have any questions or need further assistance, feel free to consult the Docker documentation or reach out to the community.

# Detailed Instructions: Bringing It All Together with Docker Compose

In this final step, you will use Docker Compose to manage a multi-container application consisting of Nginx, PHP-FPM, and MySQL. Docker Compose simplifies the process of orchestrating multiple containers, allowing you to define all services, networks, and volumes in a single YAML file. By the end of this guide, you'll have a WordPress site running with persistent storage, served via Nginx, processed by PHP-FPM, and backed by a MySQL database.

---

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Understanding the Architecture](#understanding-the-architecture)
3. [Step 1: Set Up the Project Directory](#step-1-set-up-the-project-directory)
4. [Step 2: Create the Application Files](#step-2-create-the-application-files)
5. [Step 3: Write the Docker Compose File](#step-3-write-the-docker-compose-file)
6. [Step 4: Create Custom Configuration Files](#step-4-create-custom-configuration-files)
    - [Nginx Configuration](#nginx-configuration)
    - [PHP-FPM Configuration (Optional)](#php-fpm-configuration-optional)
7. [Step 5: Start the Multi-Container Application](#step-5-start-the-multi-container-application)
8. [Step 6: Verify the Setup](#step-6-verify-the-setup)
9. [Troubleshooting Tips](#troubleshooting-tips)
10. [Conclusion](#conclusion)
11. [What's Next?](#whats-next)

---

## Prerequisites

- **Docker Desktop installed** on your machine.
- **Docker Compose installed** (included with Docker Desktop).
- **Basic knowledge of Docker and Docker Compose**.
- **Familiarity with command-line interface** (CLI) commands.
- **Previous Steps Completed**:
  - Step 1: Running a static website with Docker.
  - Step 2: Introducing persistent storage.
  - Step 3: Running a MySQL container.
  - Step 4: Setting up PHP-FPM and Nginx for dynamic content.

---

## Understanding the Architecture

Your application will consist of the following components:

- **Nginx Container**: Serves as the web server, handling HTTP requests and serving static files. Forwards PHP requests to PHP-FPM.
- **PHP-FPM Container**: Processes PHP scripts, such as WordPress.
- **MySQL Container**: Provides the database backend for WordPress.
- **WordPress Files**: Mounted as a volume for persistence and easy access.

**Communication**: All containers will be connected via a Docker network defined in Docker Compose, allowing them to communicate by service name.

---

## Step 1: Set Up the Project Directory

1. **Open a Terminal or Command Prompt**.

2. **Create a New Directory for Your Project**:

   ```bash
   mkdir wordpress-docker-compose
   ```

3. **Navigate into the Project Directory**:

   ```bash
   cd wordpress-docker-compose
   ```

---

## Step 2: Create the Application Files

1. **Create Directories for Your Application**:

   ```bash
   mkdir -p nginx php-fpm mysql wordpress
   ```

   - `nginx`: Will contain the Nginx configuration files.
   - `php-fpm`: (Optional) Can contain custom PHP configurations.
   - `mysql`: Will be used for MySQL data persistence.
   - `wordpress`: Will hold WordPress files.

2. **Download WordPress**:

   - **Option 1**: Let Docker handle it (recommended).
     - The official WordPress Docker image will download WordPress for you.
   - **Option 2**: Manually download WordPress into the `wordpress` directory.
     - Download from [wordpress.org](https://wordpress.org/download/) and extract into the `wordpress` directory.

---

## Step 3: Write the Docker Compose File

1. **Create a `docker-compose.yml` File** in the `wordpress-docker-compose` directory.

2. **Add the Following Configuration**:

   ```yaml
   version: '3.8'

   services:
     nginx:
       image: nginx:latest
       platform: linux/amd64
       container_name: nginx
       ports:
         - "8080:80"
       volumes:
         - ./nginx/nginx.conf:/etc/nginx/nginx.conf
         - ./wordpress:/var/www/html
       depends_on:
         - php-fpm
       networks:
         - wp-network

     php-fpm:
       image: php:7.4-fpm
       platform: linux/amd64
       container_name: php-fpm
       volumes:
         - ./wordpress:/var/www/html
       networks:
         - wp-network

     mysql:
       image: mysql:5.7
       platform: linux/amd64
       container_name: mysql
       volumes:
         - ./mysql:/var/lib/mysql
       environment:
         MYSQL_ROOT_PASSWORD: root_password
         MYSQL_DATABASE: wordpress
         MYSQL_USER: wordpress_user
         MYSQL_PASSWORD: my_secret_password
       networks:
         - wp-network

   networks:
     wp-network:
       driver: bridge
   ```

   **Explanation**:

   - **nginx** service:
     - Uses the official Nginx image.
     - Maps port `8080` on the host to port `80` in the container.
     - Mounts custom Nginx configuration and the WordPress files.
     - Depends on `php-fpm` to be up and running.
   - **php-fpm** service:
     - Uses PHP 7.4 with FPM.
     - Shares the WordPress files with Nginx via a volume.
   - **mysql** service:
     - Uses MySQL 5.7 image.
     - Stores data in a local `mysql` directory for persistence.
     - Environment variables configure the database and user.
   - **networks**:
     - Defines a custom network `wp-network` for container communication.

---

## Step 4: Create Custom Configuration Files

### Nginx Configuration

1. **Create an Nginx Configuration File**:

   - File path: `nginx/nginx.conf`

2. **Add the Following Configuration**:

   ```nginx
   worker_processes 1;

   events { worker_connections 1024; }

   http {
     include       mime.types;
     default_type  application/octet-stream;

     sendfile        on;
     keepalive_timeout  65;

     upstream php-fpm {
       server php-fpm:9000;
     }

     server {
       listen 80;
       server_name localhost;

       root /var/www/html;
       index index.php index.html index.htm;

       location / {
         try_files $uri $uri/ /index.php?$args;
       }

       location ~ \.php$ {
         try_files $uri =404;
         fastcgi_pass php-fpm:9000;
         fastcgi_index index.php;
         fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
         include fastcgi_params;
       }
     }
   }
   ```

   **Explanation**:

   - **upstream php-fpm**:
     - Defines the PHP-FPM service to forward PHP requests to.
     - Uses the service name `php-fpm` defined in Docker Compose.
   - **server**:
     - Configures Nginx to serve files from `/var/www/html`, which is where WordPress files are located.
     - Handles PHP files by forwarding them to PHP-FPM.

3. **Save the Configuration File**.

### PHP-FPM Configuration (Optional)

- **If you need custom PHP configurations or extensions**, you can create a custom Dockerfile for PHP-FPM.

1. **Create a `Dockerfile` in the `php-fpm` Directory**:

   - File path: `php-fpm/Dockerfile`

2. **Add Customizations** (Optional):

   ```Dockerfile
   FROM php:7.4-fpm

   # Install additional PHP extensions if needed
   RUN docker-php-ext-install mysqli pdo pdo_mysql

   # Copy custom php.ini if necessary
   COPY php.ini /usr/local/etc/php/
   ```

3. **Update the `docker-compose.yml` File**:

   - Modify the `php-fpm` service to build from the Dockerfile:

     ```yaml
     php-fpm:
       build:
         context: ./php-fpm
       container_name: php-fpm
       volumes:
         - ./wordpress:/var/www/html
       networks:
         - wp-network
     ```

4. **Create a `php.ini` File** (Optional):

   - If you need to modify PHP settings, create a `php.ini` file in the `php-fpm` directory.

---

## Step 5: Start the Multi-Container Application

1. **Build and Start the Containers**:

   ```bash
   docker-compose up -d
   ```

   - The `-d` flag runs the containers in detached mode (in the background).

2. **Monitor the Output**:

   - Check that all services are up and running without errors.

3. **Verify the Containers**:

   ```bash
   docker-compose ps
   ```

   - You should see `nginx`, `php-fpm`, and `mysql` services listed and running.

---

## Step 6: Verify the Setup

1. **Open a Web Browser**.

2. **Navigate to `http://localhost:8080`**.

   - You should see the WordPress installation page.

3. **Complete the WordPress Installation**:

   - **Select Language**: Choose your preferred language.
   - **Database Connection Details**:
     - **Database Name**: `wordpress` (as per `MYSQL_DATABASE`)
     - **Username**: `wordpress_user` (as per `MYSQL_USER`)
     - **Password**: `my_secret_password` (as per `MYSQL_PASSWORD`)
     - **Database Host**: `mysql` (service name defined in Docker Compose)
     - **Table Prefix**: `wp_` (default)
   - **Run the Installation**.
   - **Site Information**:
     - **Site Title**: Your choice.
     - **Username**: Admin username for WordPress.
     - **Password**: Admin password.
     - **Email**: Admin email.
     - **Search Engine Visibility**: Leave unchecked for development.
   - **Install WordPress**.

4. **Log into WordPress**:

   - Access the WordPress dashboard and verify everything is working.

---

## Troubleshooting Tips

### 1. Error Establishing a Database Connection

- **Cause**: Incorrect database credentials or the WordPress container cannot communicate with the MySQL container.
- **Solution**:
  - Ensure the database credentials in the WordPress setup match the environment variables set in `docker-compose.yml`.
  - Verify that the `mysql` service is running.
  - Check network configurations.

### 2. Port Already in Use

- **Cause**: Port `8080` is already occupied on your host machine.
- **Solution**:
  - Change the port mapping in `docker-compose.yml`:

    ```yaml
    ports:
      - "8081:80"
    ```

  - Access WordPress at `http://localhost:8081`.

### 3. File Permission Issues

- **Cause**: The web server or PHP does not have permission to read/write to the WordPress files.
- **Solution**:
  - Adjust permissions on the `wordpress` directory:

    ```bash
    chmod -R 755 wordpress
    ```

- **On Windows**:
  - Ensure file sharing is enabled for the drive in Docker Desktop settings.

### 4. Containers Not Communicating

- **Cause**: Network misconfiguration.
- **Solution**:
  - Verify that all services are on the same network (`wp-network`).
  - Ensure service names used in configurations (`mysql`, `php-fpm`) match those in `docker-compose.yml`.

### 5. Nginx or PHP-FPM Errors

- **Cause**: Misconfiguration in `nginx.conf` or PHP-FPM settings.
- **Solution**:
  - Check the logs:

    ```bash
    docker-compose logs nginx
    docker-compose logs php-fpm
    ```

  - Ensure the Nginx configuration correctly points to `php-fpm:9000`.

### 6. MySQL Initialization Errors

- **Cause**: MySQL container fails to start due to data directory issues.
- **Solution**:
  - Ensure the `mysql` directory is empty before the first run.
  - Remove any existing data if necessary (warning: this will delete existing databases):

    ```bash
    rm -rf mysql/*
    ```

### 7. Changes Not Reflected

- **Cause**: Cached content or container synchronization issues.
- **Solution**:
  - Clear browser cache.
  - Restart the affected container:

    ```bash
    docker-compose restart nginx
    ```

---

## Conclusion

By following these detailed instructions, you've successfully:

- Written a `docker-compose.yml` file to orchestrate multiple containers.
- Set up Nginx, PHP-FPM, and MySQL services with appropriate configurations.
- Configured persistent storage using volumes.
- Established network communication between containers.
- Deployed a WordPress site served via Nginx, processed by PHP-FPM, and backed by a MySQL database.

---

## What's Next?

With your multi-container application up and running, you can:

- **Customize and Extend**:

  - Add more PHP extensions or Nginx modules as needed.
  - Implement SSL/TLS with certificates.
  - Configure additional services like Redis or Memcached.

- **Learn More About Docker Compose**:

  - Explore advanced features like health checks, dependency management, and scaling services.

- **Deploy to Production**:

  - Consider using orchestration tools like Docker Swarm or Kubernetes for production deployments.
  - Implement security best practices and optimize performance.

---

## Additional Information

### Managing the Application

- **Stop the Containers**:

  ```bash
  docker-compose down
  ```

- **Stop and Remove Containers, Networks, and Volumes**:

  ```bash
  docker-compose down -v
  ```

  - **Warning**: This will remove volumes and delete your database data.

### Backing Up Data

- **MySQL Data**:

  - Since MySQL data is stored in the `mysql` directory, you can back up this directory.

- **WordPress Files**:

  - The `wordpress` directory contains your WordPress files, themes, and plugins.

### Updating Services

- **Rebuild a Service** (e.g., after changing a Dockerfile):

  ```bash
  docker-compose build service_name
  docker-compose up -d
  ```

- **Viewing Logs**:

  ```bash
  docker-compose logs -f
  ```

### References

- [Docker Compose Documentation](https://docs.docker.com/compose/)
- [Official WordPress Docker Image](https://hub.docker.com/_/wordpress)
- [Official Nginx Docker Image](https://hub.docker.com/_/nginx)
- [Official PHP-FPM Docker Image](https://hub.docker.com/_/php)
- [Official MySQL Docker Image](https://hub.docker.com/_/mysql)

---

This comprehensive guide should help you set up and run a multi-container WordPress application using Docker Compose. If you have any questions or encounter issues, consult the Docker documentation or seek assistance from the community.

# Detailed Instructions: Adding Persistent Storage for WordPress

In this final step, you will modify your `docker-compose.yml` file to use named volumes for persistent storage of both WordPress content and MySQL data. This ensures that any changes made through the WordPress admin interface—such as uploading images or adding posts—persist even if the containers are stopped or restarted.

---

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Understanding Persistent Storage](#understanding-persistent-storage)
3. [Step 1: Modify the `docker-compose.yml` File](#step-1-modify-the-docker-composeyml-file)
4. [Step 2: Bring Down and Recreate the Containers](#step-2-bring-down-and-recreate-the-containers)
5. [Step 3: Verify Data Persistence](#step-3-verify-data-persistence)
6. [Troubleshooting Tips](#troubleshooting-tips)
7. [Conclusion](#conclusion)
8. [What's Next?](#whats-next)

---

## Prerequisites

- **Docker Desktop installed** on your machine.
- **Docker Compose installed** (included with Docker Desktop).
- **Previous Steps Completed**:
  - Set up a multi-container WordPress application using Docker Compose.
  - Your `docker-compose.yml` file should have services for Nginx, PHP-FPM, and MySQL.

---

## Understanding Persistent Storage

By default, Docker containers are ephemeral, meaning any data stored inside the container is lost when the container is removed. To persist data, you need to use volumes. Docker volumes allow you to store data outside of the container's writable layer, ensuring data persists across container restarts and recreations.

In the context of WordPress:

- **WordPress Content**: Includes uploaded files, themes, plugins, and any changes made through the admin interface.
- **MySQL Data**: Includes databases, tables, and records—essentially all your site's content and settings.

---

## Step 1: Modify the `docker-compose.yml` File

You will update your `docker-compose.yml` file to define named volumes for both WordPress and MySQL services.

### 1. Open the `docker-compose.yml` File

- Navigate to your project directory (`wordpress-docker-compose`) and open the `docker-compose.yml` file in a text editor.

### 2. Define Named Volumes

Add a `volumes` section at the end of your `docker-compose.yml` file if it doesn't exist, and define named volumes for WordPress and MySQL:

```yaml
volumes:
  wordpress_data:
  db_data:
```

### 3. Update the Services to Use the Named Volumes

Modify the `volumes` sections of your services to use the named volumes.

#### For the MySQL Service

Replace the existing `volumes` section for the `mysql` service with:

```yaml
mysql:
  image: mysql:5.7
  container_name: mysql
  volumes:
    - db_data:/var/lib/mysql
  environment:
    MYSQL_ROOT_PASSWORD: root_password
    MYSQL_DATABASE: wordpress
    MYSQL_USER: wordpress_user
    MYSQL_PASSWORD: my_secret_password
  networks:
    - wp-network
```

- **Explanation**:
  - `- db_data:/var/lib/mysql`: Mounts the named volume `db_data` to `/var/lib/mysql` inside the container, where MySQL stores its data.

#### For the WordPress (PHP-FPM) Service

Since we're using separate Nginx and PHP-FPM containers, and the WordPress files are shared between them, we'll update both services.

**PHP-FPM Service**:

Modify the `php-fpm` service to use the named volume:

```yaml
php-fpm:
  image: php:7.4-fpm
  container_name: php-fpm
  volumes:
    - wordpress_data:/var/www/html
  networks:
    - wp-network
```

**Nginx Service**:

Modify the `nginx` service to use the same named volume:

```yaml
nginx:
  image: nginx:latest
  container_name: nginx
  ports:
    - "8080:80"
  volumes:
    - ./nginx/nginx.conf:/etc/nginx/nginx.conf
    - wordpress_data:/var/www/html
  depends_on:
    - php-fpm
  networks:
    - wp-network
```

- **Explanation**:
  - `- wordpress_data:/var/www/html`: Mounts the named volume `wordpress_data` to `/var/www/html`, where WordPress files reside.

### 4. Remove Host Bind Mounts (Optional)

If you previously mounted the `./wordpress` directory into the containers, you should remove or adjust it to avoid conflicts.

- Remove the line `- ./wordpress:/var/www/html` from both the `php-fpm` and `nginx` services.

**Note**: By using named volumes instead of host bind mounts, you ensure that Docker manages the volume storage, which can help avoid permission issues and makes your setup more portable.

### 5. Final `docker-compose.yml` File

Your updated `docker-compose.yml` should look like this:

```yaml
version: '3.8'

services:
  nginx:
    image: nginx:latest
    container_name: nginx
    ports:
      - "8080:80"
    volumes:
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf
      - wordpress_data:/var/www/html
    depends_on:
      - php-fpm
    networks:
      - wp-network

  php-fpm:
    image: php:7.4-fpm
    container_name: php-fpm
    volumes:
      - wordpress_data:/var/www/html
    networks:
      - wp-network

  mysql:
    image: mysql:5.7
    container_name: mysql
    volumes:
      - db_data:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD: root_password
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress_user
      MYSQL_PASSWORD: my_secret_password
    networks:
      - wp-network

networks:
  wp-network:
    driver: bridge

volumes:
  wordpress_data:
  db_data:
```

---

## Step 2: Bring Down and Recreate the Containers

You need to recreate the containers to apply the changes made in the `docker-compose.yml` file.

### 1. Stop and Remove Existing Containers

```bash
docker-compose down
```

- This command stops and removes all containers defined in your `docker-compose.yml` file.

### 2. Remove Existing Volumes (Optional but Recommended)

To start fresh and avoid conflicts, remove existing volumes:

```bash
docker volume rm wordpress-docker-compose_wordpress_data
docker volume rm wordpress-docker-compose_db_data
```

- Replace `wordpress-docker-compose` with the actual directory name if different.

**Warning**: This will delete any data stored in the existing volumes.

### 3. Start the Containers with the Updated Configuration

```bash
docker-compose up -d
```

- The `-d` flag runs the containers in detached mode.

### 4. Verify the Volumes are Created

```bash
docker volume ls
```

- You should see `wordpress_data` and `db_data` listed among the volumes.

---

## Step 3: Verify Data Persistence

### 1. Access WordPress in Your Browser

- Navigate to `http://localhost:8080`.
- You should see the WordPress installation page.

### 2. Complete the WordPress Installation

- Follow the same steps as before to set up WordPress.
- Use the same database credentials:

  - **Database Name**: `wordpress`
  - **Username**: `wordpress_user`
  - **Password**: `my_secret_password`
  - **Database Host**: `mysql`
  - **Table Prefix**: `wp_`

### 3. Add Content to Your WordPress Site

- Log into the WordPress admin dashboard.
- **Create a New Post**:

  - Navigate to **Posts > Add New**.
  - Add a title and content.
  - Publish the post.

- **Upload an Image**:

  - Navigate to **Media > Add New**.
  - Upload an image from your computer.

### 4. Stop and Restart the Containers

#### Stop the Containers

```bash
docker-compose down
```

#### Start the Containers Again

```bash
docker-compose up -d
```

### 5. Verify That Your Content Persists

- Refresh your WordPress site at `http://localhost:8080`.
- Log into the admin dashboard.
- **Check the Post**:

  - Navigate to **Posts > All Posts**.
  - Your previously created post should be listed.

- **Check the Media Library**:

  - Navigate to **Media > Library**.
  - Your uploaded image should be present.

**Result**: Your WordPress content persists across container restarts, confirming that the named volumes are working correctly.

---

## Troubleshooting Tips

### 1. Permission Issues with Volumes

- **Symptoms**: WordPress cannot write to the `wp-content` directory, upload images, or install plugins.
- **Solution**:
  - Adjust permissions on the Docker volumes.

**On macOS/Linux**:

- Set the appropriate permissions for the `www-data` user (usually UID 33):

  ```bash
  docker-compose exec php-fpm chown -R www-data:www-data /var/www/html
  ```

**On Windows**:

- Permission issues are less common due to how Docker handles volumes on Windows, but ensure that your Docker Desktop is up to date.

### 2. WordPress Installation Prompts Again After Restart

- **Cause**: The WordPress container is not using the persistent volume correctly.
- **Solution**:
  - Ensure that both `nginx` and `php-fpm` services have the `wordpress_data` volume mounted to `/var/www/html`.
  - Verify that the volume is not being overwritten by a bind mount.

### 3. Database Connection Errors

- **Cause**: The WordPress container cannot connect to the MySQL database.
- **Solution**:
  - Check that the `mysql` service is running.
  - Ensure the database credentials in `wp-config.php` (or during installation) match those in the `docker-compose.yml` file.
  - Verify that the `mysql` service name is correct (`mysql` in this case).

### 4. Data Not Persisting After Recreating Containers

- **Cause**: Volumes were not correctly defined or were deleted.
- **Solution**:
  - Ensure that volumes are defined under the `volumes` section in `docker-compose.yml`.
  - Avoid using `docker-compose down -v` unless you intend to delete volumes.

### 5. Access Denied Errors in MySQL

- **Cause**: Passwords or user credentials have changed.
- **Solution**:
  - Double-check the environment variables for the MySQL service.
  - Remember that changes to environment variables in `docker-compose.yml` do not affect existing volumes. You may need to remove the `db_data` volume to apply changes (warning: this deletes existing data).

---

## Conclusion

By following these steps, you've successfully:

- Configured named volumes for persistent storage of WordPress content and MySQL data.
- Ensured that all changes made in the WordPress admin interface persist across container restarts.
- Verified that your multi-container WordPress application is now fully persistent.

---

## Expected Outcome

- **Persistent WordPress Content**: Posts, pages, images, themes, and plugins remain available even after stopping and starting the containers.
- **Persistent MySQL Data**: Database records, including users and site configurations, are retained across restarts.

---

## What's Next?

With persistent storage in place, your Dockerized WordPress environment is more robust and suitable for development purposes. You can now:

- **Back Up and Restore Data**:

  - Use Docker commands or MySQL tools to back up your database and WordPress files.

- **Enhance Security**:

  - Configure SSL certificates.
  - Set up proper user permissions.

- **Scale and Optimize**:

  - Add caching mechanisms like Redis or Varnish.
  - Configure load balancing if needed.

- **Deploy to Production**:

  - Consider using orchestration tools like Kubernetes for scaling.
  - Implement monitoring and logging solutions.

---

## Additional Information

### Managing Volumes

- **List Volumes**:

  ```bash
  docker volume ls
  ```

- **Inspect a Volume**:

  ```bash
  docker volume inspect wordpress_data
  ```

- **Remove Unused Volumes**:

  ```bash
  docker volume prune
  ```

  - **Warning**: This removes all unused volumes and can result in data loss if volumes are not properly managed.

### Accessing Data Inside Volumes

- You can access the data stored in volumes using the `docker-compose exec` command.

- **Example**:

  ```bash
  docker-compose exec php-fpm ls /var/www/html
  ```

### Updating Services

- **Recreate Containers After Configuration Changes**:

  ```bash
  docker-compose up -d
  ```

- **Force Recreate Containers**:

  ```bash
  docker-compose up -d --force-recreate
  ```

### References

- [Docker Volumes Documentation](https://docs.docker.com/storage/volumes/)
- [Docker Compose File Reference](https://docs.docker.com/compose/compose-file/)
- [WordPress Official Docker Image](https://hub.docker.com/_/wordpress)

---

By implementing persistent storage using Docker volumes, you've enhanced your WordPress application's resilience and data integrity within a Dockerized environment. This setup closely mimics production environments and provides a solid foundation for further development and deployment strategies.
