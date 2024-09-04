**TSRMS（测试样本空间管理与检索系统）** 

### 1. 测试环境
可以通过以下地址访问 **TSRMS** 系统的测试环境：
- 测试环境地址: [http://123.56.25.116:8080/](http://123.56.25.116:8080/)

### 2. API 文档
**TSRMS** 的 API 文档提供了所有可用接口的详细信息，包括请求方式、参数和响应格式。为了方便开发和调试，你可以访问 API 文档：
- 访问地址: [API 文档链接](https://apifox.com/apidoc/shared-53d80bda-a2ec-4f70-a8a2-9a643604e3f1)
- 访问密码: `tsrms`


### 3. 本地部署指南(如果需要)

要在本地部署 **TSRMS** 系统，可以使用 `docker-compose` 进行一键部署。以下是完整的 `docker-compose.yml` 文件以及部署步骤：

#### `docker-compose.yml` 文件内容
```yaml
version: '3.8'

services:
  tsrms:
    container_name: tsrms
    image: serverless-100026543835-docker.pkg.coding.net/trsms/tsrms/tsrms:latest
    ports:
      - "8080:8080"
    environment:
      - GO_ENV=test
    volumes:
      - tsrms_logs:/log
    depends_on:
      - mysql
    restart: always

  mysql:
    image: 'hub.atomgit.com/library/mysql:latest'
    ports:
      - "3306:3306"
    environment:
      - MYSQL_DATABASE=tsrms
      - MYSQL_USER=tsrms
      - MYSQL_PASSWORD=tsrms
      - MYSQL_RANDOM_ROOT_PASSWORD="yes"
    volumes:
      - tsrms_mysql:/var/lib/mysql

volumes:
  tsrms_logs:
  tsrms_mysql:
```

#### 部署步骤
1. **创建 `docker-compose.yml` 文件**
   - 在你的服务器或本地环境中，创建一个目录用于存放项目文件，进入该目录后创建 `docker-compose.yml` 文件并粘贴上述内容。

2. **启动服务**
   - 在 `docker-compose.yml` 文件保存后，执行以下命令启动服务：
     ```bash
     docker-compose up -d
     ```

3. **验证服务运行**
   - 启动后，使用 `docker ps` 命令确认容器是否已成功启动：
     ```bash
     docker ps
     ```

   - 如果一切正常，应该会看到 `tsrms` 和 `mysql` 容器在运行。

4. **访问 TSMS 系统**
   - 系统启动后，可以通过 `http://localhost:8080/` 或服务器的公网 IP 地址访问 TSMS 系统。如果你是在服务器上运行，并且防火墙允许 8080 端口访问，可以通过 `http://<服务器IP>:8080` 进行访问。

### 注意事项
1. **数据库配置**：`docker-compose.yml` 已配置了 `MySQL` 容器，数据库名称为 `tsrms`，并且使用 `tsrms` 作为用户名和密码。你可以根据需要修改数据库配置。
2. **日志存储**：日志文件将存储在 `tsrms_logs` 卷中，MySQL 数据将存储在 `tsrms_mysql` 卷中，确保数据和日志不会丢失。
3. **环境更改**：配置文件被打包在了镜像中,可在镜像文件目录`/app/conf`中查看，故需要拿到源代码，才可以实现修改配置文件


