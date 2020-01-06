# Node.js

## 安装

### Debian and Ubuntu

- Node.js v12.x:

```shell script
curl -sL https://deb.nodesource.com/setup_12.x | bash -
apt-get install -y nodejs
```

### Enterprise Linux

- Node.js v12.x:

```shell script
curl -sL https://rpm.nodesource.com/setup_12.x | bash -
yum install -y nodejs
```

## Docker

[把一个 Node.js web 应用程序给 Docker 化](https://nodejs.org/zh-cn/docs/guides/nodejs-docker-webapp/)

### 创建 Node.js 应用

- 创建 `package.json` 文件，描述你应用程序以及需要的依赖：

```json
{
  "name": "nodejs_docker",
  "version": "1.0.0",
  "description": "Node.js on Docker",
  "author": "zcy0521 <zcy0521@gmail.com>",
  "main": "server.js",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "express": "^4.16.1"
  }
}
```

- 创建一个 `server.js` 文件，使用 [Express.js](https://expressjs.com/) 框架定义一个 Web 应用：

```js
'use strict';

const express = require('express');

// Constants
const PORT = 8080;
const HOST = '0.0.0.0';

// App
const app = express();
app.get('/', (req, res) => {
    res.send('Hello world\n');
});

app.listen(PORT, HOST);
console.log(`Running on http://${HOST}:${PORT}`);
```

### 编写 `Dockerfile` 文件

- 创建 `Dockerfile` 文件

```dockerfile
FROM node:10

# Create app directory
WORKDIR /usr/src/app

# Install app dependencies
# A wildcard is used to ensure both package.json AND package-lock.json are copied
# where available (npm@5+)
COPY package*.json ./

RUN npm install
# If you are building your code for production
# RUN npm ci --only=production

# Bundle app source
COPY . .

EXPOSE 8080
CMD [ "node", "server.js" ]
```

- 创建 `.dockerignore` 文件

```docker
node_modules
npm-debug.log
```

### 运行镜像

```shell script
docker build -t <your username>/node-web-app .
docker run -p 49160:8080 -d <your username>/node-web-app
```
