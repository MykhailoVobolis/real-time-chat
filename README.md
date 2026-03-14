# Real Time Chat

This is a real-time chat application built with **Node.js**, **Express**, **React (Vite)**, **Socket.IO**, **PostgreSQL**, and **Prisma**. The project is fully dockerized for easy development and deployment.

## Prerequisites

- Docker & Docker Compose
- Node.js v20+
- npm or yarn
- PostgreSQL (optional if using Docker)

## Project Structure

```
real-time-chat
├── client
│   ├── src
│   ├── Dockerfile
│   └── package.json
├── server
│   ├── prisma
│   ├── socket
│   ├── src
│   ├── uploads
│   ├── Dockerfile
│   ├── .env
│   ├── .env.example
│   ├── .env.docker
│   ├── .env.docker.example
│   └── package.json
└── docker-compose.yml
```

## Environment Variables

Create a `.env` file in the `server` folder (you can copy from `.env.example`):

```
PORT=3000
BASE_URL=http://localhost:3000

JWT_ACCESS_SECRET=your_access_secret
JWT_REFRESH_SECRET=your_refresh_secret

POSTGRES_USER=your_postgres_user
POSTGRES_PASSWORD=your_postgres_password
POSTGRES_DB=your_postgres_db

DATABASE_URL=postgresql://your_postgres_user:your_postgres_password@localhost:5432/your_postgres_db?schema=public
```

### Docker-specific environment

For running the application inside Docker containers, use a separate file .env.docker (copy from .env.docker.example) with the following changes:

```
PORT=3000
BASE_URL=http://localhost:3000

JWT_ACCESS_SECRET=your_access_secret
JWT_REFRESH_SECRET=your_refresh_secret

POSTGRES_USER=your_postgres_user
POSTGRES_PASSWORD=your_postgres_password
POSTGRES_DB=your_postgres_db

# Important: use the service name 'db' from docker-compose as host

DATABASE_URL=postgresql://your_postgres_user:your_postgres_password@db:5432/your_postgres_db?schema=public
```

⚠️ Key points:
• .env is used for local development.
• .env.docker is used when running the app with Docker Compose.
• In Docker, the database host is the service name db, not localhost.

## Ports

| Service  | Port |
| -------- | ---- |
| Client   | 5173 |
| Server   | 3000 |
| Database | 5432 |

## Docker Setup

#### Build and start the application using Docker Compose:

```bash
docker compose up -d
```

This will start three services:

- db: PostgreSQL database
- server: Node.js backend
- client: React frontend

#### If you want to rebuild the images (for example, after changing a Dockerfile), run:

```bash
docker compose up -d --build
```

#### Local development with database only

If you want to run the database locally without starting the server and client, you can start just the database service:

```bash
docker compose up -d db    # PostgreSQL database
```

#### Stop containers

```bash
docker compose down
```

## Prisma

#### Prisma is used for database ORM. After updating the schema, run:

```bash
npx prisma generate       # Generate Prisma client
npx prisma migrate dev    # Apply migrations to the database
npx prisma studio         # Launches the Studio interface for working with data
```

## Scripts

### Root (run both Client and Server)

```bash
npm install # Install dependencies in root (for npm-run-all)
npm run dev # Run client and server in parallel
npm run build # Build both client and server
npm start # Start production server
```

### Client

```bash
npm install # Install dependencies
npm run dev # Run development server
npm run build # Build production version
```

### Server

```bash
npm install # Install dependencies
npm run dev # Run development server
npm start # Start server
```

## Notes

- Prisma is used for database ORM
- Socket.IO handles real-time communication
- The client runs on port 5173
- The server runs on port 3000
