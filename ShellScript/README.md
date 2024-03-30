To create a shell script that automates the setup of a PostgreSQL Docker container for your authentication system, follow the steps below. This script will pull the PostgreSQL Docker image, run a container with your specified configurations, and optionally connect to the PostgreSQL client within the container to create the users table.

## Step 1: Create the Shell Script
Open a text editor and create a new file named setup_postgres.sh.

## Step 2: Write the Script
Copy and paste the following content into setup_postgres.sh. This script does the following:

Checks if a Docker container with a specified name already exists.
Pulls the PostgreSQL Docker image.
Runs a new PostgreSQL container with environment variables for the initial setup.
Waits for PostgreSQL to fully start.
Creates the users table in your databa

```bash
#!/bin/bash

# Configuration variables
CONTAINER_NAME="your-db-name"
POSTGRES_USER="yourusername"
POSTGRES_PASSWORD="yourpassword"
POSTGRES_DB="yourdbname"
TABLE_CREATION_SQL="CREATE TABLE IF NOT EXISTS users (id SERIAL PRIMARY KEY, username VARCHAR(255) UNIQUE NOT NULL, password_hash CHAR(60) NOT NULL);"

# Check if container already exists
if [ $(docker ps -a -f name=^/${CONTAINER_NAME}$ --format '{{.Names}}') == $CONTAINER_NAME ]; then
    echo "Container $CONTAINER_NAME already exists. Stopping and removing it."
    docker stop $CONTAINER_NAME
    docker rm $CONTAINER_NAME
fi

# Pull the PostgreSQL Docker image
echo "Pulling the PostgreSQL Docker image..."
docker pull postgres

# Run the PostgreSQL container
echo "Running the PostgreSQL Docker container..."
docker run --name $CONTAINER_NAME -e POSTGRES_USER=$POSTGRES_USER -e POSTGRES_PASSWORD=$POSTGRES_PASSWORD -e POSTGRES_DB=$POSTGRES_DB -p 5432:5432 -d postgres

# Wait for PostgreSQL to start
echo "Waiting for PostgreSQL to start..."
sleep 10

# Execute the SQL to create the users table
echo "Creating the users table in the $POSTGRES_DB database..."
docker exec -it $CONTAINER_NAME psql -U $POSTGRES_USER -d $POSTGRES_DB -c "$TABLE_CREATION_SQL"

echo "PostgreSQL setup completed successfully."
```
## Step 3: Make the Script Executable
Before you can run the script, you need to make it executable. Open a terminal, navigate to the directory containing setup_postgres.sh, and run:

```bash
chmod +x setup_postgres.sh
```

## Step 4: Execute the Script
Now, you can execute the script:

```bash
./setup_postgres.sh
```