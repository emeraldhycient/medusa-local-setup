# Medusa Local Setup Guide

Welcome to the Medusa local setup guide! This comprehensive README will walk you through the steps required to set up Medusa on your local machine using repositories pushed to GitHub. You'll learn how to clone the repositories, configure environment variables, install dependencies, set up the database, seed it with initial data, run the application, and create an admin user.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Installation Steps](#installation-steps)
  - [1. Clone the Frontend and Backend Repositories](#1-clone-the-frontend-and-backend-repositories)
  - [2. Set Up Environment Variables](#2-set-up-environment-variables)
  - [3. Install Dependencies](#3-install-dependencies)
  - [4. Configure the Database](#4-configure-the-database)
  - [5. Run Database Migrations](#5-run-database-migrations)
  - [6. Seed the Database](#6-seed-the-database)
  - [7. Start the Backend Server](#7-start-the-backend-server)
  - [8. Start the Frontend Server](#8-start-the-frontend-server)
- [Creating an Admin User](#creating-an-admin-user)
- [Accessing the Application](#accessing-the-application)
- [Troubleshooting](#troubleshooting)
- [Additional Resources](#additional-resources)
- [License](#license)

---

## Prerequisites

Before you begin, ensure your development environment includes the following:

- **Node.js** (v14 or higher): [Download Node.js](https://nodejs.org/)
- **npm** (comes with Node.js) or **Yarn**: [Install Yarn](https://classic.yarnpkg.com/en/docs/install)
- **Git**: [Download Git](https://git-scm.com/downloads)
- **PostgreSQL**: Medusa uses PostgreSQL as its default database. [Download PostgreSQL](https://www.postgresql.org/download/)
- **Redis** (optional but recommended for caching and queuing): [Download Redis](https://redis.io/download)
- **Docker** (optional for containerized setups): [Download Docker](https://www.docker.com/get-started)

Ensure PostgreSQL and Redis services are running on your machine or accessible remotely.

---

## Installation Steps

Follow these steps to set up Medusa locally.

### 1. Clone the Frontend and Backend Repositories

First, clone both the frontend and backend repositories from GitHub. Replace `<frontend-repo-url>` and `<backend-repo-url>` with your actual repository URLs.

```bash
# Clone the Backend Repository
git clone <backend-repo-url> medusa-backend

# Clone the Frontend Repository
git clone <frontend-repo-url> medusa-frontend
```

**Example:**

```bash
git clone https://github.com/yourusername/medusa-backend.git medusa-backend
git clone https://github.com/yourusername/medusa-frontend.git medusa-frontend
```

### 2. Set Up Environment Variables

Environment variables are crucial for configuring your application. You'll need to create `.env` files for both the backend and frontend by copying the example files provided in each repository.

#### Backend

1. Navigate to the backend directory:

    ```bash
    cd medusa-backend
    ```

2. Create a copy of the `.env.example` file:

    ```bash
    cp .env.example .env
    ```

3. Open the `.env` file in your preferred text editor and configure the necessary variables. Key variables include:

    - `DATABASE_URL`: Connection string for your PostgreSQL database.
    - `REDIS_URL`: Connection string for Redis (if used).
    - `JWT_SECRET`: A secret key for JWT authentication.
    - `API_ENDPOINT`: The URL where the backend API will be accessible.
    - `FRONTEND_URL`: The URL where the frontend will be accessible.

    **Example `.env` Configuration:**

    ```env
    DATABASE_URL=postgres://username:password@localhost:5432/medusa
    REDIS_URL=redis://localhost:6379
    JWT_SECRET=your_jwt_secret_key
    API_ENDPOINT=http://localhost:9000
    FRONTEND_URL=http://localhost:7000
    ```

#### Frontend

1. Open a new terminal window/tab and navigate to the frontend directory:

    ```bash
    cd ../medusa-frontend
    ```

2. Create a copy of the `.env.example` file:

    ```bash
    cp .env.example .env
    ```

3. Open the `.env` file and set the necessary variables, such as:

    - `MEDUSA_BACKEND_URL`: URL where the backend API is running.
    - `STORE_URL`: URL where the frontend will be accessible.

    **Example `.env` Configuration:**

    ```env
    MEDUSA_BACKEND_URL=http://localhost:9000
    STORE_URL=http://localhost:7000
    ```

### 3. Install Dependencies

Install the required dependencies for both backend and frontend using `npm` or `Yarn`.

#### Backend

1. Ensure you're in the backend directory:

    ```bash
    cd ../medusa-backend
    ```

2. Install dependencies:

    Using **npm**:

    ```bash
    npm install
    ```

    Or using **Yarn**:

    ```bash
    yarn install
    ```

#### Frontend

1. Open a new terminal window/tab and navigate to the frontend directory:

    ```bash
    cd ../medusa-frontend
    ```

2. Install dependencies:

    Using **npm**:

    ```bash
    npm install
    ```

    Or using **Yarn**:

    ```bash
    yarn install
    ```

### 4. Configure the Database

Medusa uses PostgreSQL as its default database. Follow these steps to set up your database.

1. **Start PostgreSQL Service:**

    Ensure PostgreSQL is running. You can start it using:

    - **macOS (Homebrew):**

        ```bash
        brew services start postgresql
        ```

    - **Ubuntu:**

        ```bash
        sudo service postgresql start
        ```

2. **Create a Database:**

    Access the PostgreSQL shell:

    ```bash
    psql -U postgres
    ```

    Create a new database:

    ```sql
    CREATE DATABASE medusa;
    ```

    (Optional) Create a dedicated user:

    ```sql
    CREATE USER medusa_user WITH PASSWORD 'your_password';
    GRANT ALL PRIVILEGES ON DATABASE medusa TO medusa_user;
    ```

    Exit the PostgreSQL shell:

    ```sql
    \q
    ```

3. **Update `DATABASE_URL` in Backend `.env`:**

    Ensure the `DATABASE_URL` in your backend `.env` file points to the newly created database.

    **Example:**

    ```env
    DATABASE_URL=postgres://medusa_user:your_password@localhost:5432/medusa
    ```

### 5. Run Database Migrations

Set up the database schema by running migrations.

1. Navigate to the backend directory:

    ```bash
    cd ../medusa-backend
    ```

2. Run migrations:

    Using **npm**:

    ```bash
    npm run migrate
    ```

    Or using **Yarn**:

    ```bash
    yarn migrate
    ```

    This command will create the necessary tables and schema in your PostgreSQL database.

### 6. Seed the Database

Seeding the database populates it with initial data, such as sample products, collections, and other essential information. This step is crucial for testing and development purposes.

1. **Prepare Seed Data:**

    Ensure that your backend repository includes seed files. These are usually located in a directory like `data` or `seeds`. If your repository does not include seed data, you may need to create it or obtain it from the project's documentation.

2. **Run the Seed Script:**

    Medusa typically provides a script to seed the database. Execute the seed script using `npm` or `Yarn`.

    Using **npm**:

    ```bash
    npm run seed
    ```

    Or using **Yarn**:

    ```bash
    yarn seed
    ```

    **Note:** If your project uses a different command or script for seeding, refer to your project's documentation or `package.json` scripts section.

3. **Verify Seeding:**

    After running the seed script, verify that the database has been populated with the initial data.

    You can check this by accessing the PostgreSQL shell and querying the tables:

    ```bash
    psql -U medusa_user -d medusa
    ```

    For example, to check products:

    ```sql
    SELECT * FROM products LIMIT 10;
    ```

    Exit the PostgreSQL shell:

    ```sql
    \q
    ```

    **Troubleshooting:**

    - If you encounter errors during seeding, ensure that migrations have been run successfully.
    - Check that the seed script is correctly configured and points to the right database.
    - Review any error messages for clues and consult the project's documentation or community for assistance.

### 7. Start the Backend Server

Launch the Medusa backend server.

1. Ensure you're in the backend directory:

    ```bash
    cd ../medusa-backend
    ```

2. Start the server:

    Using **npm**:

    ```bash
    npm run start
    ```

    Or using **Yarn**:

    ```bash
    yarn start
    ```

    By default, the backend server will run on [http://localhost:9000](http://localhost:9000).

### 8. Start the Frontend Server

Launch the Medusa frontend server.

1. Open a new terminal window/tab and navigate to the frontend directory:

    ```bash
    cd ../medusa-frontend
    ```

2. Start the frontend server:

    Using **npm**:

    ```bash
    npm run dev
    ```

    Or using **Yarn**:

    ```bash
    yarn dev
    ```

    By default, the frontend server will run on [http://localhost:7000](http://localhost:7000).

---

## Creating an Admin User

After setting up and running both the backend and frontend servers, you'll need to create an admin user to access the Medusa Admin Dashboard.

### Method 1: Using the Admin API

You can create an admin user by making a POST request to the admin creation endpoint.

#### Using `curl`

```bash
curl -X POST http://localhost:9000/admin/auth --header "Content-Type: application/json" --data '{
  "email": "admin@example.com",
  "password": "securepassword"
}'
```

#### Using `HTTPie`

```bash
http POST http://localhost:9000/admin/auth email=admin@example.com password=securepassword
```

#### Using Postman

1. Open Postman and create a new **POST** request.
2. Set the URL to `http://localhost:9000/admin/auth`.
3. In the **Body** tab, select **raw** and choose **JSON** format.
4. Enter the following JSON payload:

    ```json
    {
      "email": "admin@example.com",
      "password": "securepassword"
    }
    ```

5. Click **Send**.

**Note:** Replace `admin@example.com` and `securepassword` with your desired admin email and password.

### Method 2: Using Medusa CLI

If you have the Medusa CLI installed, you can create an admin user using the following command:

1. **Install Medusa CLI (if not already installed):**

    ```bash
    npm install -g @medusajs/medusa-cli
    ```

2. **Create Admin User:**

    ```bash
    medusa admin:create
    ```

    You'll be prompted to enter the admin email and password.

---

## Accessing the Application

With both the backend and frontend servers running and an admin user created, you can now access the Medusa application.

- **Storefront:** [http://localhost:7000](http://localhost:7000)
- **Admin Dashboard:** [http://localhost:7000/admin](http://localhost:7000/admin)

### Logging into the Admin Dashboard

1. Navigate to [http://localhost:7000/admin](http://localhost:7000/admin) in your web browser.
2. Enter the admin email and password you created earlier.
3. Click **Login** to access the admin interface.

---

## Troubleshooting

Encountering issues during setup? Here are some common problems and solutions:

### 1. Database Connection Errors

- **Error Message:** `Could not connect to database`
  
  **Solution:**
  - Ensure PostgreSQL is running.
  - Verify the `DATABASE_URL` in your backend `.env` file is correct.
  - Check that the database user has the necessary permissions.

### 2. Port Conflicts

- **Issue:** Ports `9000` (backend) or `7000` (frontend) are already in use.

  **Solution:**
  - Change the ports in the respective `.env` files.
    
    **Example:**
  
    ```env
    # Backend .env
    API_ENDPOINT=http://localhost:9001
    ```
  
    ```env
    # Frontend .env
    STORE_URL=http://localhost:7001
    ```
  
  - Restart the servers after making changes.

### 3. Missing Dependencies

- **Issue:** Errors related to missing packages or modules.

  **Solution:**
  - Ensure all dependencies are installed correctly.
  - Delete the `node_modules` folder and reinstall dependencies:

    ```bash
    rm -rf node_modules
    npm install
    # or
    yarn install
    ```

### 4. Admin Creation Failures

- **Issue:** Unable to create an admin user via API or CLI.

  **Solution:**
  - Ensure the backend server is running.
  - Check backend logs for any error messages.
  - Verify that the `JWT_SECRET` is set in the backend `.env` file.

### 5. Redis Connection Issues

- **Issue:** Errors related to Redis connection.

  **Solution:**
  - Ensure Redis is installed and running.
  - Verify the `REDIS_URL` in the backend `.env` file is correct.
  - If Redis is not needed, you can disable it by removing or commenting out related configurations.

### 6. Seeding Errors

- **Issue:** Errors encountered while seeding the database.

  **Solution:**
  - Ensure that migrations have been run successfully before seeding.
  - Verify that the seed scripts are correctly configured and point to the right database.
  - Check for any syntax errors or misconfigurations in the seed files.
  - Review error messages for specific issues and consult the project's documentation or community for assistance.

### 7. Frontend Not Loading

- **Issue:** Frontend server is running but the application isn't loading in the browser.

  **Solution:**
  - Check the frontend server logs for any errors.
  - Ensure that the `MEDUSA_BACKEND_URL` in the frontend `.env` file matches the backend server URL.
  - Verify that CORS settings in the backend allow requests from the frontend URL.

---

## Additional Resources

- **Medusa Documentation:** [https://docs.medusajs.com/](https://docs.medusajs.com/)
- **Medusa GitHub Repository:** [https://github.com/medusajs/medusa](https://github.com/medusajs/medusa)
- **Medusa Discord Community:** [Join Here](https://discord.gg/medusajs)
- **PostgreSQL Documentation:** [https://www.postgresql.org/docs/](https://www.postgresql.org/docs/)
- **Redis Documentation:** [https://redis.io/documentation](https://redis.io/documentation)

---

## License

This project is licensed under the [MIT License](LICENSE).

---

Feel free to reach out to the Medusa community or consult the documentation if you encounter any issues not covered in this guide. Happy coding!
