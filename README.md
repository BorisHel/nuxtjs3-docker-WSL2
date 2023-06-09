# A Windows WSL2 Docker Nuxtjs 3 development environment boilerplate

This boilerplate is used to quick start a dockerised projet on **WSL2 with hotReload**

## Start our local development

Now we are ready to start our local development server on http://localhost:3000 with a simple command in our terminal:

````
docker-compose up
````


## How that work ?

Dockerization is done using Dockerfile and  docker-compose.yml.

### Dockerfile 
Dockerfile will be used to build our Docker image with all our Node dependencies including Node itself.

````
FROM node:14-alpine

WORKDIR /app

RUN apk update && apk upgrade

COPY ./package*.json /app/

RUN npm install && npm cache clean --force

COPY . .

ENV PATH ./node_modules/.bin/:$PATH
````

### Docker-compose.yml

Define our services to tell Docker which containers need to spin up.


````
version: '3.4'

services:
  nuxt:
    build:
      context: .
    image: nuxt_dev
    container_name: nuxt_dev
    command: npm run dev
    volumes:
    - .:/app
    - /app/node_modules
    ports:
      - "3000:3000"
      - "24678:24678" # allow hotRelad on WSL2
````
