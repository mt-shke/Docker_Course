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

<details>
<summary>Docker</summary>

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