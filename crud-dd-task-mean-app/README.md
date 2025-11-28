In this DevOps task, you need to build and deploy a full-stack CRUD application using the MEAN stack (MongoDB, Express, Angular 15, and Node.js). The backend will be developed with Node.js and Express to provide REST APIs, connecting to a MongoDB database. The frontend will be an Angular application utilizing HTTPClient for communication.  

The application will manage a collection of tutorials, where each tutorial includes an ID, title, description, and published status. Users will be able to create, retrieve, update, and delete tutorials. Additionally, a search box will allow users to find tutorials by title.

## Project setup

### Node.js Server

cd backend

npm install

You can update the MongoDB credentials by modifying the `db.config.js` file located in `app/config/`.

Run `node server.js`

### Angular Client

cd frontend

npm install

Run `ng serve --port 8081`

You can modify the `src/app/services/tutorial.service.ts` file to adjust how the frontend interacts with the backend.

Navigate to `http://localhost:8081/`




# Dockerfile.backend
FROM node:18-alpine

WORKDIR /app

COPY backend/package*.json ./
RUN npm install

COPY backend/ .

EXPOSE 3000
CMD ["node", "server.js"]







# Stage 1: Build Angular App
FROM node:18-alpine AS build
WORKDIR /usr/src/app

# Copy package files
COPY frontend/package*.json ./

# Install dependencies
RUN npm install

# Copy all frontend source code
COPY frontend/ .

# Build Angular app
RUN npm run build --prod

# Stage 2: Nginx Serve
FROM nginx:stable-alpine

# Copy Angular dist output to Nginx html folder
COPY --from=build /usr/src/app/dist/angular-15-crud/ /usr/share/nginx/html/

# Copy Nginx config
COPY nginx/default.conf /etc/nginx/conf.d/default.conf

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]




