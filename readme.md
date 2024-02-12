# PostgreSQL Server Setup with Docker and Prisma

This Docker Compose file sets up a PostgreSQL server along with Adminer for managing the database. Additionally, instructions for connecting to the PostgreSQL server using Prisma in a Node.js application are provided.

## Getting Started

To get started, make sure you have Docker installed on your system.

### Starting the PostgreSQL Server

1. Clone this repository or create a new directory and save the provided `docker-compose.yml` file inside it.

2. Open a terminal and navigate to the directory containing the `docker-compose.yml` file.

3. Run the following command to start the PostgreSQL server and Adminer:

   ```bash
   docker-compose up -d
   ```

4. Wait for the containers to start. Once started, PostgreSQL will be accessible at `localhost:5432` and Adminer will be accessible at `localhost:8080`.

### Connecting with a GUI Tool

You can connect to the PostgreSQL server using a GUI tool like pgAdmin.

1. Open your preferred GUI tool (e.g., pgAdmin).

2. Create a new server connection.

3. Use the following connection details:

   - **Host:** localhost
   - **Port:** 5432
   - **Username:** your_username (as specified in the `docker-compose.yml` file)
   - **Password:** your_password (as specified in the `docker-compose.yml` file)
   - **Database:** your_database_name (as specified in the `docker-compose.yml` file)

4. Connect to the server.

### Connecting with Prisma

To connect to the PostgreSQL server using Prisma in a Node.js application, follow these steps:

1. Install Prisma globally:

   ```bash
   npm install -g prisma
   ```

2. Create a new Prisma project:

   ```bash
   prisma init your_project_name
   cd your_project_name
   ```

3. Update the `schema.prisma` file to use PostgreSQL as the datasource:

   ```prisma
   // schema.prisma

   datasource db {
     provider = "postgresql"
     url      = env("DATABASE_URL")
   }

   generator client {
     provider = "prisma-client-js"
   }
   ```

4. Create a `.env` file in the root of your project directory and add the connection URL for your PostgreSQL database:

   ```
   DATABASE_URL="postgresql://your_username:your_password@localhost:5432/your_database_name"
   ```

   Replace `'your_username'`, `'your_password'`, and `'your_database_name'` with your actual credentials.

5. Generate Prisma client code:

   ```bash
   prisma generate
   ```

6. Use the Prisma client in your Node.js application to interact with your PostgreSQL database. Here's an example of how you can use Prisma:

   ```javascript
   // index.js

   const { PrismaClient } = require("@prisma/client");

   const prisma = new PrismaClient();

   async function main() {
     const users = await prisma.user.findMany();
     console.log(users);
   }

   main()
     .catch((e) => {
       throw e;
     })
     .finally(async () => {
       await prisma.$disconnect();
     });
   ```

   Replace `'user'` with the name of your database table and customize the query as needed.

7. Run your Node.js application.

### Connect to the server usign pg library

You can connect to the PostgreSQL server from a Node.js application using the `pg` package.

1. Install the `pg` package in your Node.js project:

   ```bash
   npm install pg
   ```

2. In your Node.js application, use the following connection string:

   ```javascript
   const { Client } = require("pg");

   const client = new Client({
     user: "your_username",
     host: "localhost",
     database: "your_database_name",
     password: "your_password",
     port: 5432,
   });

   client
     .connect()
     .then(() => console.log("Connected to PostgreSQL"))
     .catch((err) => console.error("Connection error", err))
     .finally(() => client.end());
   ```
