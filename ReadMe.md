# Sky takeout

This project is an online takeout ordering system developed using the Spring Boot framework.

## Technology stack

- Backend framework
   - SpringBoot (3.1.2)
   - mybatis
- database
   - MySql
   - Redis
- Front-end framework
   - Vue
   - Uniapp
   -ElementUI
- Front-end and back-end communication
   - RESTful API

## Windows development environment setup

1. Install Java JDK 17 and configure environment variables
2. Install MySQL and Redis databases and create corresponding databases

    - Create MySQL database and tables: Run [mysql.sql](./demo/mysql.sql)

3. Install Maven build tools
4. Download and install Nginx and complete the following configurations

    ```
    # Configure the following content under the http item

    map $http_upgrade $connection_upgrade{
    default upgrade;
    '' close;
    }

    upstream webservers{
      server 127.0.0.1:8080 weight=90;
      #server 127.0.0.1:8088 weight=10;
    }

    server {
        listen 80;
        server_name localhost;

        location/{
            root html/sky;
            index index.html index.htm;
        }

        # Reverse proxy, handles requests sent by the management end
        location /api/ {
    proxy_pass http://localhost:8080/admin/;
            #proxy_pass http://webservers/admin/;
        }

    # Reverse proxy, handles requests sent by the client
        location /user/ {
            proxy_pass http://webservers/user/;
        }

    #WebSocket
    location /ws/ {
            proxy_pass http://webservers/ws/;
    proxy_http_version 1.1;
    proxy_read_timeout 3600s;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "$connection_upgrade";
        }

        location /media {
            root configures the media file location; # eg: D:/static
            # Note: Create a media folder in the D:/static directory
        }
    }
    ```

5. Clone the project locally
6. Modify the configuration file [application.yml](./sky-server/src/main/resources/application.yml)

    ```yml
    spring:
      datasource:
        url: jdbc:mysql://url
        username: root
        password: database password
      data:
        redis:
          password: redis database password
    ```

7. Create a new `application-env.yml` file in the [resources](./sky-server/src/main/resources/) directory and write the following configuration

    ```yml
    sky:
      wechat:
        appid: can be obtained by applying for WeChat mini program
        secret: can be obtained by applying for WeChat mini program
        mchid: merchant number
        mchSerialNo:
        privateKeyFilePath:
        apiV3Key:
        weChatPayCertFilePath:
        notifyUrl:
        refundNotifyUrl:
    ```

8. Run the project