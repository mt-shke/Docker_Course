## Main commands

<details>
<summary>Image commands</summary>

```js
// docker pull mysql => get mysql image
// docker images => list all installed images

// docker rmi mysql => remove mysql image
// docker rmi 22a2874e1112 => remove image with Id 22a2874e1112

// docker image -h => get all image related help
// docker image history imageName => get history of the image
```

</details>

<details>
<summary>Container commands</summary>

```js
// docker ps => display docker status
// docker ps -a => display all docker status
// docker ps -aq => display all docker ids

// docker rm $(docker ps -aq) => remove all docker container

// docker rm containerName => remove container

// docker create --help => display create help commands
// docker start vibrant_hodgkin => start container named vibrant_hodgkin

// docker run ubuntu => run and exit locally ubuntu container

// docker run -i ubuntu bash => run container interactively
// docker run -i -t ubuntu => run container interactivly live

// docker start -i 53aaf7fe1350 => run container interactivly live
```

</details>

<details>
<summary>Port</summary>

```js
// docker run -it ubuntu => run container using ubuntu
// get-apt update => get update
// get-apt install nginx => install nginx

// apt-get update && apt-get install nginx -y => update and install without asking permission

// docker inspect 2106dea87185

// docker run -it -p 9000:80 ubuntu => run container on specified port
// apt-get update && apt-get install nginx -y => update and install without asking permission
// service nginx start => run nginx engine

// cd var/www/html => change directory to var/www/html
// get-apt install vim => install vim text editor

// vim index.nginx-debian.html => launch current page with vim text editor
```

</details>

## Detach

<details>
<summary>Detach mode</summary>

```js
// docker run -p 9001:80 -d ubuntu
// docker run -it -p 9000:80 ubuntu

// Can exec in another window while docker is running in background
// docker exec e9a202964849 apt-get update
// docker exec e9a202964849 apt-get install nginx.... etc

// docker pause ee0e04341fa7 => pause
// docker unpause ee0e04341fa7 => restart
// docker stop ee0e04341fa7 => stop container after few secondes
// docker rm ee0e04341fa7 => remove this container
```

</details>

<details>
<summary>Shared Volume</summary>

```js
//  docker run -it -d -p 9001:80 -v C:\Users\Michel\OneDrive\Bureau\docker:/var/www/html ubuntu
```

</details>

## Dockerfile

<details>

<summary>Dockerfile</summary>

current directory

```js
// New-Item Dockerfile
```

current directory - create and delete images

```js
// docker build . => build current folder Dockerfile

// docker build . -t nginx-ubuntu:2.0 => build with repository and tag name as nginx-ubuntu:2.0

//  docker rmi $(docker images --filter=reference="*:TagName*" -q)
```

in Dockerfile

```js
FROM ubuntu

RUN apt-get update
RUN apt-get install nginx -y

CMD [ "nginx","-g","daemon off;" ]
// let nginx keep running and close daemon

```

Run container

```js
// docker run -it d925c422fc36
```

</details>

<details>
<summary>Docker Volume</summary>

```js
// docker volume ls => display all available volume
// docker volume prune => remove all unused volume

// docker run -it `
// >> -p 9001:80 `
// >> -v demo-vol:/var `
// >> ubuntu bash

// create index.txt, add text, cat index.txt

// rm container, recreate, index still there => data persistance!
```

</details>

## Docker network

<details>
<summary>Docker network</summary>

```js
// docker network ls => list all network

// docker run -it --network=test-net ubuntu bash => create docker bound to test-net network
// docker network create test-net => create test-net network

// docker run -it ubuntu bash => create simple docker with default bridge
// docker network connect test-net dockerId => connect docker to test-net network
```

</details>

## Node

<details>
<summary>Docker using Node</summary>

```js
// docker run -it node:12-alpine => run docker using node:12-alpine settings

// then set the Dockerfile

// docker build -t getting-started . => build docker named getting-started with current directory's Dockerfile

// docker run -dp 3000:3000 getting-started => run simple getting-started docker

// docker run -dp 3000:3000 -v demo-vol:/etc/todos getting-started => run getting-started docker within demo-vol volume with its persisted data
```

</details>

## Mysql

<details>
<summary>MySql - Define root settings</summary>

declare mysql before running docker

```js
// /!\ If you want to use mysql as database, you have to declare it when you start running the container otherwise sqlite db is used by default  /!\

// docker run mysql:5.7

// docker run -it --platform "linux/amd64" mysql:5.7 => run docker with mysql
```

define root password

```js
// docker run -it --platform "linux/amd64" -e MYSQL_ROOT_PASSWORD=secret mysql:5.7 => run docker with mysql 5.7, on linux amd platform, and "secret" as root password

// In a new terminal:
// docker exec -it 53 mysql -u root -p => launch docker with the named starting by "53"
```

set default database

```js
// docker run -it --platform "linux/amd64" -e MYSQL_ROOT_PASSWORD=secret -e MYSQL_DATABASE=todos mysql:5.7 => run docker with ... and create a database named todos
```

</details>

## Connect container

<details>
<summary>Multi container apps</summary>

```js
// https://docs.docker.com/get-started/07_multi_container/

// docker network create todo-net => create network named todo-net
```

```js
// docker run -d `
//      --network todo-net --network-alias mysql `
//      -v todo-vol:/var/lib/mysql `
//      -e MYSQL_ROOT_PASSWORD=secret `
//      -e MYSQL_DATABASE=todos `
//      mysql:5.7

// => create mysql db container with network "todo-net", on "todo-vol" volume, with secret root password, and default todos database
```

```js
// docker logs a1  => display logs of docks with named starting by a1
```

```js
// docker run -dp 3000:3000 `
//     --network todo-net `
//     -e MYSQL_HOST=mysql `
//     -e MYSQL_USER=root `
//     -e MYSQL_PASSWORD=secret `
//     -e MYSQL_DB=todos `
//     getting-started
```

</details>

## Using Docker compose

<details>
<summary>Docker compose</summary>

```js
// docker compose version

// docker compose up
```

docker-compose.yml

```yml
version: "3.7"

services:
    app:
        image: node:12-alpine
        command: sh -c "yarn install && yarn run dev"
        ports:
            - 3000:3000

        environment:
            MYSQL_HOST: mysql
            MYSQL_USER: root
            MYSQL_PASSWORD: secret
            MYSQL_DB: todos

        working_dir: /app
        volumes:
            - ./:/app

    mysql:
        image: mysql:5.7
        volumes:
            - todo-vol:/var/lib/mysql
        environment:
            MYSQL_ROOT_PASSWORD: secret
            MYSQL_DATABASE: todos
        platform: linux/amd64

volumes:
    todo-vol:
```

</details>
