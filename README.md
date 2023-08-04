**Tutorial: Installing PostgreSQL on Docker**

In this tutorial, we'll walk you through the steps to install PostgreSQL on Docker using the `postgres` official Docker image. Docker allows you to run applications in containers, providing an isolated and consistent environment across different platforms. PostgreSQL is a powerful open-source relational database management system, and running it in a Docker container offers flexibility and portability.

**Prerequisites:**
- Docker installed on your machine (https://docs.docker.com/get-docker/)

**Step 1: Pull the PostgreSQL Docker Image**
Open a terminal or command prompt and run the following command to download the `postgres` Docker image from the official Docker Hub repository:

```
docker pull postgres
```

**Step 2: Create a Persistent Data Volume**
To ensure that your PostgreSQL data remains persistent even after the container is stopped or removed, we'll create a Docker data volume. This volume will be mounted to the PostgreSQL container to store its data.

Replace `/custom/mount` with the directory path where you want to store the PostgreSQL data on your host machine. If the directory doesn't exist, Docker will create it for you.

```
docker volume create pgdata_volume
```

**Step 3: Run the PostgreSQL Container**
Now, let's run the PostgreSQL container with the necessary configurations.

```
docker run --name postgresql -e POSTGRES_USER=postgres -e POSTGRES_PASSWORD=postgres -p 5432:5432 -v pgdata_volume:/var/lib/postgresql/data --restart unless-stopped -d postgres
```

Explanation of the command arguments:

- `--name postgresql`: Specifies the name of the container as "postgresql".
- `-e POSTGRES_USER=postgres`: Sets the username for the default PostgreSQL user to "postgres".
- `-e POSTGRES_PASSWORD=postgres`: Sets the password for the default PostgreSQL user to "postgres".
- `-p 5432:5432`: Maps port 5432 of the container to the same port on the host machine, allowing you to connect to PostgreSQL.
- `-v pgdata_volume:/var/lib/postgresql/data`: Mounts the previously created data volume (`pgdata_volume`) to the `/var/lib/postgresql/data` directory inside the container, providing persistent data storage.
- `--restart unless-stopped`: Configures the container to restart automatically unless explicitly stopped by the user or during system reboot.
- `-d postgres`: Specifies the Docker image to run the container.

**Step 4: Verify PostgreSQL Installation**
To verify that PostgreSQL is running inside the Docker container, use the following command to see the running containers:

```
docker ps
```

You should see an output similar to this, which indicates that the PostgreSQL container is running:

```
CONTAINER ID   IMAGE      COMMAND                  CREATED          STATUS          PORTS                    NAMES
xxxxxxxxxxxx   postgres   "docker-entrypoint.s…"   x seconds ago   Up x seconds    0.0.0.0:5432->5432/tcp   postgresql
```

**Step 5: Connect to PostgreSQL**
To connect to the PostgreSQL database from your host machine or any other client, you can use your favorite PostgreSQL client and connect to `localhost` on port `5432`, using the username and password you specified during the container setup.

For example, if you have `psql` installed, you can connect to the PostgreSQL container with the following command:

```
psql -h localhost -p 5432 -U postgres
```

You will be prompted to enter the password (`postgres`) for the `postgres` user.

**Step 6: Stopping and Restarting the PostgreSQL Container**
To stop the PostgreSQL container, use the following command:

```
docker stop postgresql
```

To start the stopped container, use:

```
docker start postgresql
```

Remember, the container will automatically restart when your system reboots due to the `--restart unless-stopped` option.

**Step 7: Removing the PostgreSQL Container**
If you want to remove the PostgreSQL container entirely (and its associated data), run the following command:

```
docker rm -f postgresql
```

**Conclusion:**
Congratulations! You have successfully installed PostgreSQL on Docker using the `postgres` official Docker image. The database data is stored in a persistent volume, making it easy to manage and maintain even if the container is removed or replaced. Docker containers allow you to easily deploy PostgreSQL and other applications, ensuring consistency and portability across different environments.
