mkdir my-node-app && cd my-node-app
npm init -y

npm install express

Create an index.js file:

const express = require("express");
const app = express();

app.get("/", (req, res) => {
  res.send("Hello, Dockerized Node.js!");
});

app.listen(3000, () => {
  console.log("Server running on port 3000");
});

Create a Dockerfile

# Use official Node.js image
FROM node:18

# Set working directory
WORKDIR /app

# Copy package.json and install dependencies
COPY package*.json ./
RUN npm install

# Copy the rest of the app
COPY . .

# Expose the application port
EXPOSE 3000

# Start the application
CMD ["node", "index.js"]

Create a .dockerignore

node_modules
npm-debug.log

Create a docker-compose.yml

version: "3"
services:
  node-app:
    build: .
    ports:
      - "3000:3000"
    volumes:
      - .:/app
      - /app/node_modules
    environment:
      - NODE_ENV=development


# Build the Docker image
docker build -t my-node-app .

# Run the container
docker run -p 3000:3000 my-node-app

or

docker-compose up --build
http://localhost:3000 
