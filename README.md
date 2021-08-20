### Docker Compose

- https://docs.docker.com/compose/
- https://hub.docker.com/_/mongo
<br> </br>
- Download latest mongo image
  - `docker pull mongo`

- Create a `docker-compose.yml` file and add 
``` 
version: '3'


# image of mongod and port 27017
services:
  db:
    image: mongo
    restart: always
    ports: [27017:27017]


# connect mongo
    volumes:

      - ./mongod.conf:/etc/mongod.conf

# web app image 
  web:
    build: ./app
    restart: always
    ports: [3000:3000]

# environment variable
    environment:
      - DB_HOST=mongodb://db:27017/posts
    depends_on:
      - db


volumes:
  db: 

```
<br> </br>
- To run file, `docker compose up`
<br> </br>
- -----------------------------------------------------------
- In app folder, create `Dockerfile`

```
FROM node as APP

WORKDIR /usr/src/app

COPY package*.json ./

RUN npm install express

RUN npm install -g npm@7.20.6

COPY . .

FROM node:alpine

COPY --from=app /usr/src/app /usr/src/app

WORKDIR /usr/src/app

EXPOSE 3000

#CMD ["node", "seeds/seed.js"]


CMD ["node","app.js"]


```
<br> </br>
**every time you mack changes be sure to run `docker compose build` and then `docker compose up`
<br> </br>
- ----------------------------------------------------------------------------
- In database folder create Dockerfile
```

FROM mongo

EXPOSE 27017


```