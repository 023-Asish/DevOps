# Docker Compose
Docker Compose is a tool that helps you define and share multi-container applications. With Compose, you can create a YAML file to define the services and with a single command, you can spin everything up or tear it all down.
The big advantage of using Compose is you can define your application stack in a file, keep it at the root of your project repository (it's now version controlled), and easily enable someone else to contribute to your project. Someone would only need to clone your repository and start the app using Compose. In fact, you might see quite a few projects on GitHub/GitLab doing exactly this now.

Create the Compose file
In the getting-started-app directory, create a file named compose.yaml.

├── getting-started-app/  
│ ├── Dockerfile  
│ ├── compose.yaml  
│ ├── node_modules/  
│ ├── package.json  
│ ├── spec/  
│ ├── src/  
│ └── yarn.lock  

your complete compose.yaml should look like this:
  ```
  services:
    app:
      image: node:18-alpine
      command: sh -c "yarn install && yarn run dev"
      ports:
        - 127.0.0.1:3000:3000
      working_dir: /app
      volumes:
        - ./:/app
      environment:
        MYSQL_HOST: mysql
        MYSQL_USER: root
        MYSQL_PASSWORD: secret
        MYSQL_DB: todos
  
    mysql:
      image: mysql:8.0
      volumes:
        - todo-mysql-data:/var/lib/mysql
      environment:
        MYSQL_ROOT_PASSWORD: secret
        MYSQL_DATABASE: todos

  volumes:
    todo-mysql-data:
  ```

  ## Run the application stack
  Now that you have your compose.yaml file, you can start your application. 
  1. Make sure no other copies of the containers are running first. Use docker ps to list the containers and docker rm -f <ids> to remove them.
  2. Start up the application stack using the docker compose up command. Add the -d flag to run everything in the background.
    ```
      docker compose up -d
    ```
    When you run the previous command, you should see output like the following:
    ![compose](https://github.com/023-Asish/DevOps/assets/77069694/5a2fbb24-a05d-4fa8-8242-552d4ad03922)
  
  
     You'll notice that Docker Compose created the volume as well as a network. By default, Docker Compose automatically creates a network specifically for the application stack (which is why you didn't define one in the Compose file).

  3. Look at the logs using the docker compose logs -f command. You'll see the logs from each of the services interleaved into a single stream. This is incredibly useful when you want to watch for timing-related issues. The -f flag follows the log, so will give you live output as it's generated.

     If you have run the command already, you'll see output that looks like this:
     ![logs_compose](https://github.com/023-Asish/DevOps/assets/77069694/719d7590-07e3-4568-8eb9-58ba2c59435d)

  The service name is displayed at the beginning of the line (often colored) to help distinguish messages. If you want to view the logs for a specific service, you can add the service name to the end of the logs command (for example, docker compose logs -f app).

  At this point, you should be able to open your app in your browser on ``` http://localhost:3000``` and see it running.

  ## See the app stack in Docker Dashboard
  If you look at the Docker Dashboard, you'll see that there is a group named getting-started-app. This is the project name from Docker Compose and used to group the containers together. 
  ![group](https://github.com/023-Asish/DevOps/assets/77069694/9334ed4c-a984-4f26-9c38-4f7a23713a65)
  By default, the project name is simply the name of the directory that the compose.yaml was located in.
  If you expand the stack, you'll see the two containers you defined in the Compose file. The names are also a little more descriptive, as they follow the pattern of <service-name>-<replica-number>. So, it's very easy to quickly see what container is your app and which container is the mysql database.

  ## Tear it all down
  When you're ready to tear it all down, simply run docker compose down or hit the trash can on the Docker Dashboard for the entire app. The containers will stop and the network will be removed.

  **Warning**: 
  ```diff
  - By default, named volumes in your compose file are not removed when you run `docker compose down`. If you want to remove the volumes, you need to add the `--volumes` flag. The Docker Dashboard does not remove volumes when you delete the app stack.
  ```

  # Summary
  In this section, you learned about Docker Compose and how it helps you simplify the way you define and share multi-service applications.
