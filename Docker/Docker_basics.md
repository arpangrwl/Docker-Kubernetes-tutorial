Connect to MySQL using Docker. Here are the basic Docker commands to set up and connect to a MySQL container:

1. **Pull the MySQL Docker image:**
   ```bash
   docker pull mysql:latest
   ```

   Explanation: This command will downloads a Docker mysql:latest image from a registry. If the image is not available locally, this command fetches it from the specified registry.

2. **Run MySQL Container with Environment Variables, Volume Mount, and Port Mapping:**
   ```bash
   docker run -d \
     --name mysql-container \
     -e MYSQL_ROOT_PASSWORD=your_root_password \
     -e MYSQL_DATABASE=your_database_name \
     -e MYSQL_USER=your_username \
     -e MYSQL_PASSWORD=your_password \
     -p 3306:your_desired_host_port \
     -v /path/on/host:/var/lib/mysql \
     mysql:latest
   ```

   Replace the placeholders (`your_root_password`, `your_database_name`, `your_username`, `your_password`, `your_desired_host_port`, and `/path/on/host`) with your specific values.

   - `-e`: Sets environment variables for MySQL configuration.
   - `-p`: Maps the container's MySQL port to the host's desired port.
   - `-v`: Mounts a volume from the host to the container for persistent data storage.
  
    Explanation: `Docker RUN` command is used to run a container from a specified image. You can also provide additional options, specify a command to run inside the container, and pass arguments.

3.  **Lists running containers.**

  ```bash
  docker ps:
  ```

Explanation: Lists running containers. Use the -a option to display all containers, including stopped ones.


4. **Connect to the MySQL Database:**
   ```bash
   docker exec -it mysql-container mysql -P your_desired_host_port -u your_username -p
   ```

Replace `mysql-container` with the actual name of your MySQL container, and `your_username` with the MySQL username you specified when setting up the container. You will be prompted to enter the password for the specified MySQL user.

This command uses the following options:

- `docker exec`: Executes a command inside a running container.
- `-it`: Allocates a pseudo-TTY and keeps the session interactive.
- `mysql-container`: The name of your MySQL container.
- `mysql -u your_username -p`: The MySQL command to connect to the MySQL server with the specified username.


Explanation:
- `docker run`: Command to run a Docker container.
- `-d`: Runs the container in the background.
- `--name mysql-container`: Assigns a name to the container for easy reference.
- `mysql:latest`: Specifies the MySQL image and its latest version.
- `MYSQL_ROOT_PASSWORD`: Sets the root password for MySQL.
- `MYSQL_DATABASE`: Specifies the name of the database to be created.
- `MYSQL_USER`: Specifies the MySQL user.
- `MYSQL_PASSWORD`: Sets the password for the MySQL user.
- `-p 3306:your_desired_host_port`: Maps the MySQL container port (default is 3306) to the desired port on the host.
- `-v /path/on/host:/var/lib/mysql`: Mounts a volume from the host to the container for persistent storage.
