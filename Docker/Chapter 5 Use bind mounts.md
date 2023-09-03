# Use Bind Mounts
Using bind mounts is common for local development setups. The advantage is that the development machine doesn’t need to have all of the build tools and environments installed. With a single docker run command, Docker pulls dependencies and tools.

## Run your app in a development container 
The following steps describe how to run a development container with a bind mount that does the following:

* Mount your source code into the container
* Install all dependencies
* Start nodemon to watch for filesystem changes  
You can use the CLI or Docker Desktop to run your container with a bind mount.

1. Make sure you don't have any getting-started containers currently running.

2. Run the following command from the getting-started-app directory.
   Run this command in PowerShell.

   ```
   docker run -dp 127.0.0.1:3000:3000 `
    -w /app --mount "type=bind,src=$pwd,target=/app" `
    node:18-alpine `
    sh -c "yarn install && yarn run dev"
   ```
   ![code](https://github.com/023-Asish/DevOps/assets/77069694/0c23dd14-0a0a-4a99-b16b-23d8eb17e783)
   The following is a breakdown of the command:

   * -dp 127.0.0.1:3000:3000 - same as before. Run in detached (background) mode and create a port mapping
   * -w /app - sets the "working directory" or the current directory that the command will run from
   * --mount "type=bind,src=$pwd,target=/app" - bind mount the current directory from the host into the /app directory in the container
   * node:18-alpine - the image to use. Note that this is the base image for your app from the Dockerfile
   * sh -c "yarn install && yarn run dev" - the command. You're starting a shell using sh (alpine doesn't have bash) and running yarn install to install packages and then running yarn run dev to start the development server. If you look in the package.json, you'll see that the dev script starts nodemon.
4. You can watch the logs using docker logs <container-id>. You'll know you're ready to go when you see this:
   ```
   docker logs -f <container-id>
   nodemon src/index.js
   [nodemon] 2.0.20
   [nodemon] to restart at any time, enter `rs`
   [nodemon] watching dir(s): *.*
   [nodemon] starting `node src/index.js`
   Using sqlite database at /etc/todos/todo.db
   Listening on port 3000
   ```
When you're done watching the logs, exit out by hitting Ctrl+C.
![logs](https://github.com/023-Asish/DevOps/assets/77069694/e5064801-4cb9-4e24-aa13-f4a0fd38473c)

## Develop your app with the development container 
Update your app on your host machine and see the changes reflected in the container.

1. In the src/static/js/app.js file, on line 109, change the "Add Item" button to simply say "Add":
   ```diff
   - {submitting ? 'Adding...' : 'Add Item'}
   + {submitting ? 'Adding...' : 'Add'}
   ```
2. Refresh the page in your web browser, and you should see the change reflected almost immediately. It might take a few seconds for the Node server to restart. If you get an error, try refreshing after a few seconds.
   ### before change
      ![add_item](https://github.com/023-Asish/DevOps/assets/77069694/5ebb1fec-0137-4531-9e8f-104457038196)
   ### after change
      ![add](https://github.com/023-Asish/DevOps/assets/77069694/2c744e10-9df7-4108-a064-75b5e76f3e4e)

# Summary 
At this point, you can persist your database and see changes in your app as you develop without rebuilding the image.

In addition to volume mounts and bind mounts, Docker also supports other mount types and storage drivers for handling more complex and specialized use cases.
