That sounds like a fantastic assignment idea! Let me help you break it down into incremental steps for your students. Each step will build their understanding while giving them practical experience, eventually leading them to a multi-container app for hosting WordPress.

### Assignment Outline: Build a Multi-Container WordPress Hosting Application

#### **Goal**: By the end of this assignment, students will have a multi-container Docker setup that runs WordPress using a MySQL database, PHP, and local/persistent storage. They'll begin with simple static websites and build on that knowledge.

---

### **Step 1: Running a Static Website with Docker**
#### **Objective**: Learn the basics of Docker by running a static website container on localhost.
1. **Instructions**:
   - Create a simple `index.html` file that says, “Hello World.”
   - Use the official **Nginx Docker image** to serve the static HTML file.
   
2. **Steps**:
   - Write a `Dockerfile` that:
     - Uses the Nginx base image.
     - Copies the `index.html` file into the `/usr/share/nginx/html` directory.
   - Build the Docker image.
   - Run the container to view the static website on `localhost:8080`.

3. **Hints**:
   - Use `docker run -p 8080:80 your_image_name` to expose port 8080 on the host and bind it to port 80 inside the container.
   - Ensure your `Dockerfile` uses the official Nginx image as a base: `FROM nginx`.

4. **Expected Outcome**: A static website that can be accessed locally on `http://localhost:8080`.

---

### **Step 2: Introducing Persistent Storage**
#### **Objective**: Understand how to persist data in Docker by adding a volume to store website files locally.
1. **Instructions**:
   - Modify the Nginx container setup to persist the `index.html` file on the host system.
   - Use a **bind mount** to link the host directory containing your HTML files to the container.

2. **Steps**:
   - Run your Nginx container with a bind mount that links a local folder (e.g., `./website`) to the container’s `/usr/share/nginx/html` directory.
   - Update the HTML file on the host system and see the changes reflected live in the container.

3. **Commands**:
   - `docker run -p 8080:80 -v $(pwd)/website:/usr/share/nginx/html your_image_name`

4. **Expected Outcome**: When you edit the `index.html` file on your local machine, it should automatically update in the running container.

---

### **Step 3: Running a MySQL Container**
#### **Objective**: Set up a MySQL container and learn about environment variables for container configuration.
1. **Instructions**:
   - Use the official **MySQL Docker image** to start a MySQL database.
   - Provide the necessary environment variables (`MYSQL_ROOT_PASSWORD`, `MYSQL_DATABASE`, etc.) to configure the MySQL container.

2. **Steps**:
   - Start a MySQL container using `docker run` with the necessary environment variables to set up a database and a user.
   - Ensure the database persists across container restarts using a **named volume**.

3. **Commands**:
   - `docker run -d -e MYSQL_ROOT_PASSWORD=my-secret-pw -e MYSQL_DATABASE=wordpress -v mysql_data:/var/lib/mysql --name mysql-db mysql:latest`

4. **Expected Outcome**: A running MySQL instance with persistent data in the `mysql_data` volume.

---

### **Step 4: Setting Up PHP-FPM and Nginx for Dynamic Content**
#### **Objective**: Learn to combine Nginx and PHP-FPM in separate containers to serve dynamic content.
1. **Instructions**:
   - Set up two containers: one for **Nginx** and one for **PHP-FPM**.
   - Ensure that Nginx forwards PHP requests to the PHP-FPM container.

2. **Steps**:
   - Use the official **PHP-FPM** Docker image for processing PHP requests.
   - Modify your Nginx configuration to use **fastcgi_pass** to send PHP requests to the PHP-FPM container.
   - Create a simple `index.php` file to test the setup.

3. **Commands**:
   - `docker run -d --name php-fpm php:fpm`
   - Modify the Nginx configuration to forward `.php` files to the PHP-FPM container.

4. **Expected Outcome**: A simple dynamic PHP site served by Nginx with requests being processed by the PHP-FPM container.

---

### **Step 5: Bringing It All Together with Docker Compose**
#### **Objective**: Use Docker Compose to manage the multi-container application.
1. **Instructions**:
   - Write a `docker-compose.yml` file to manage the Nginx, PHP-FPM, and MySQL containers.
   - Ensure the Nginx container is linked with the PHP-FPM and MySQL containers.

2. **Steps**:
   - Define services for Nginx, PHP-FPM, and MySQL in the `docker-compose.yml`.
   - Set up volumes for persistent storage (e.g., for MySQL data and website files).
   - Set up network communication between the containers.

3. **Sample `docker-compose.yml`**:
   ```yaml
   version: '3'
   services:
     wordpress:
       image: wordpress
       ports:
         - "8080:80"
       environment:
         WORDPRESS_DB_HOST: mysql-db
         WORDPRESS_DB_USER: wordpress_user
         WORDPRESS_DB_PASSWORD: my_secret_password
         WORDPRESS_DB_NAME: wordpress
       volumes:
         - wordpress_data:/var/www/html

     mysql-db:
       image: mysql:5.7
       environment:
         MYSQL_ROOT_PASSWORD: root_password
         MYSQL_DATABASE: wordpress
         MYSQL_USER: wordpress_user
         MYSQL_PASSWORD: my_secret_password
       volumes:
         - mysql_data:/var/lib/mysql

   volumes:
     wordpress_data:
     mysql_data:
   ```

4. **Expected Outcome**: The students should be able to spin up the multi-container setup using `docker-compose up`, with WordPress served via Nginx, PHP processing handled by PHP-FPM, and the MySQL database for persistence.

---

### **Step 6: Adding Persistent Storage for WordPress**
#### **Objective**: Ensure WordPress data persists across container restarts.
1. **Instructions**:
   - Use a **named volume** to store WordPress content (like uploaded files) and MySQL data persistently.

2. **Steps**:
   - Define volumes for WordPress content and MySQL in the `docker-compose.yml` file.
   - Ensure that changes made in the WordPress admin (like uploading images or adding posts) are persisted across container restarts.

3. **Expected Outcome**: WordPress content should persist even when the containers are stopped and restarted.

---

### **Step 7: Final Assignment Submission**
#### **Objective**: The students should document their process and submit their `docker-compose.yml` file along with a brief write-up explaining:
- How the containers work together.
- How persistent storage is handled.
- Any challenges they faced and how they overcame them.

---

### Key Concepts Covered:
- Dockerfiles and building images.
- Running containers and managing ports.
- Persisting data with bind mounts and volumes.
- Using environment variables to configure services.
- Setting up multi-container environments with Docker Compose.
- Handling dynamic content with PHP and Nginx.
- Using MySQL for data storage and WordPress setup.

### Grading Criteria:
- Correctness of `docker-compose.yml`.
- Functionality of the multi-container app.
- Persistence of data between container restarts.
- Clear explanations in the write-up.

---

### Bonus Challenge (Optional)
- Add a **phpMyAdmin** container for managing the MySQL database visually.
- Create custom **Dockerfiles** to modify the base WordPress or PHP-FPM images.

---

This assignment scaffolds the learning process, taking students from basic Docker usage to a fully operational multi-container application. Each step introduces a new concept while reinforcing what was learned previously. By the end, students will not only understand Docker but also have a working WordPress setup they can showcase.

Let me know if you'd like further details, modifications, or help with any part of this assignment!