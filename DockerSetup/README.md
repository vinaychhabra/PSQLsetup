To set up the PostgreSQL database for your authentication system using Docker, you'll need to use a PostgreSQL Docker image. This setup will allow you to run PostgreSQL in a Docker container, making it easy to deploy and scale.

Step 1: Install Docker
Ensure that Docker is installed on your machine. If it's not installed, download and install Docker from the official website.

Step 2: Pull PostgreSQL Docker Image
Pull the official PostgreSQL image from Docker Hub:

```bash
docker pull postgres
```
Step 3: Run PostgreSQL Container
Run a PostgreSQL container with the necessary environment variables for the initial database, user, and password. Replace yourpassword with a secure password.

```bash

docker run --name your-db-name -e POSTGRES_USER=yourusername -e POSTGRES_PASSWORD=yourpassword -e POSTGRES_DB=yourdbname -p 5432:5432 -d postgres
```
--name your-db-name: Sets the name of your Docker container.
-e POSTGRES_USER=yourusername: Sets the default user for PostgreSQL.
-e POSTGRES_PASSWORD=yourpassword: Sets the password for the PostgreSQL user.
-e POSTGRES_DB=yourdbname: Creates a default database with the specified name.
-p 5432:5432: Maps the port 5432 inside the Docker container to port 5432 on your host machine, allowing your application to connect to the PostgreSQL server.
-d postgres: Runs the PostgreSQL image as a detached process.
Step 4: Verify Container is Running
Check that the PostgreSQL container is running:

```bash
docker ps
```
Step 5: Connect to PostgreSQL Database
You can connect to the PostgreSQL database running inside the Docker container using any PostgreSQL client tool, specifying localhost as the host and 5432 as the port. Use the username, password, and database name you defined when running the container.

Step 6: Create the User Table
Connect to your database and create the users table as described previously. If you're comfortable with the command line, you can use the docker exec command to run the psql client inside the container:

```bash
docker exec -it your-db-name psql -U yourusername -d yourdbname
```
Then, execute the SQL command to create the users table:

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    username VARCHAR(255) UNIQUE NOT NULL,
    password_hash CHAR(60) NOT NULL
);
```
Additional Notes
Data Persistence: By default, the data inside the Docker container is ephemeral. If you stop the container, you'll lose all the data. To persist data, consider using Docker volumes.
Security: Ensure the PostgreSQL user password is secure. Also, when deploying to production, consider additional security measures such as network security policies.
With your PostgreSQL database running in a Docker container, your Golang application can connect to it using the connection string you've configured, allowing you to implement your authentication system.