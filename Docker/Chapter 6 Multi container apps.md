# Multi container apps
  Up to this point, you've been working with single container apps. But, now you will add MySQL to the application stack. The following question often arises - "Where will MySQL run? Install it in the same container or run it separately?" In general, each container should do one thing and do it well. The following are a few reasons to run the container separately:
  * There's a good chance you'd have to scale APIs and front-ends differently than databases.
  * Separate containers let you version and update versions in isolation.
  * While you may use a container for the database locally, you may want to use a managed service for the database in production. You don't want to ship your database engine with your app then.
  * Running multiple processes will require a process manager (the container only starts one process), which adds complexity to container startup/shutdown.  
    And there are more reasons. So, like the following diagram, it's best to run your app in multiple containers.

 ## Container networking 
  Remember that containers, by default, run in isolation and don't know anything about other processes or containers on the same machine. So, how do you allow one container to talk to another? The answer is networking. If you place the two containers on the same network, they can talk to each other.
![Network (1)](https://github.com/023-Asish/DevOps/assets/77069694/2bd5fe1e-0673-4a4f-8394-8255db95d8c9)

### There are two ways to put a container on a network:

* Assign the network when starting the container.
* Connect an already running container to a network.  
In the following steps, you'll create the network first and then attach the MySQL container at startup.

## We are going to build network structure like this
   ![Network (3)](https://github.com/023-Asish/DevOps/assets/77069694/f8faf31c-6237-47ea-8f1f-e968d36ec06d)
1. Create the network.
   ```
   docker network create todo-app
   ```
2. Start a MySQL container and attach it to the network. You're also going to define a few environment variables that the database will use to initialize the database. To learn more about the MySQL environment variables, see the "Environment Variables" section in the MySQL Docker Hub listingopen_in_new

   In Windows, run this command in PowerShell.
   ```
    docker run -d `
    --network todo-app --network-alias mysql `
    -v todo-mysql-data:/var/lib/mysql `
    -e MYSQL_ROOT_PASSWORD=secret `
    -e MYSQL_DATABASE=todos `
    mysql:8.0
   ```
3. To confirm you have the database up and running, connect to the database and verify that it connects.
   ```
   docker exec -it <mysql-container-id> mysql -u root -p
   ```
   When the password prompt comes up, type in secret. In the MySQL shell, list the databases and verify you see the todos database.
   ```
   mysql> SHOW DATABASES;
   ```
   You should see output that looks like this:
   ![mysql](https://github.com/023-Asish/DevOps/assets/77069694/1255fffb-a7df-4e7e-81a4-e64fb706c6f8)
   ![verify](https://github.com/023-Asish/DevOps/assets/77069694/38d6d6b5-4334-4fab-b589-dc356aa19dd2)
4. Exit the MySQL shell to return to the shell on your machine.
   ```
   mysql> exit
   ```
   You now have a todos database and it's ready for you to use.

## Connect to MySQL 
Now that you know MySQL is up and running, you can use it. But, how do you use it? If you run another container on the same network, how do you find the container? Remember that each container has its own IP address.

To answer the questions above and better understand container networking, you're going to make use of the nicolaka/netshootopen container, which ships with a lot of tools that are useful for troubleshooting or debugging networking issues.
1. Start a new container using the nicolaka/netshoot image. Make sure to connect it to the same network.
   ```
   docker run -it --network todo-app nicolaka/netshoot
   ```
   ![netshoot](https://github.com/023-Asish/DevOps/assets/77069694/c45c5f29-4c04-4b48-8ddd-f28e99bfb44d)
2. Inside the container, you're going to use the dig command, which is a useful DNS tool. You're going to look up the IP address for the hostname mysql.
   ```
   dig mysl
   ```
   ![ip_find](https://github.com/023-Asish/DevOps/assets/77069694/a8e4d01c-a968-40aa-83a1-1835662faf1b)  
   In the "ANSWER SECTION", you will see an A record for mysql that resolves to 172.18.0.2 (your IP address will most likely have a different value). While mysql isn't normally a valid hostname, Docker was able to resolve it to the IP address of the container that had that network alias. Remember, you used the --network-alias earlier.

   What this means is that your app only simply needs to connect to a host named mysql and it'll talk to the database.
## Run your app with MySQL 
   The todo app supports the setting of a few environment variables to specify MySQL connection settings. They are:

   * MYSQL_HOST - the hostname for the running MySQL server
   * MYSQL_USER - the username to use for the connection
   * MYSQL_PASSWORD - the password to use for the connection
   * MYSQL_DB - the database to use once connected
  You can now start your dev-ready container.

  1. Specify each of the previous environment variables, as well as connect the container to your app network. Make sure that you are in the getting-started-app directory when you run this command.
      ```
      docker run -dp 127.0.0.1:3000:3000 `
      -w /app -v "$(pwd):/app" `
      --network todo-app `
      -e MYSQL_HOST=mysql `
      -e MYSQL_USER=root `
      -e MYSQL_PASSWORD=secret `
      -e MYSQL_DB=todos `
      node:18-alpine `
      sh -c "yarn install && yarn run dev"
     ```
  2. Open the app in your browser and add a few items to your todo list.
      ![addItems](https://github.com/023-Asish/DevOps/assets/77069694/53dfa356-56c3-4699-b951-bb2434c92d91)

  3. Connect to the mysql database and prove that the items are being written to the database. Remember, the password is secret.
      ```
      docker exec -it <mysql-container-id> mysql -p todos
      ```
      And in the mysql shell, run the following:
      ```
        mysql> select * from todo_items;
      ```
      ![database](https://github.com/023-Asish/DevOps/assets/77069694/b5c42127-1676-414e-91f0-5c404b24b0c0)
      Your table will look different because it has your items. But, you should see them stored there.

# Summary 
At this point, you have an application that now stores its data in an external database running in a separate container. You learned a little bit about container networking and service discovery using DNS.


      
