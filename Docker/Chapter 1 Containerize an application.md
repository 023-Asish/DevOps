# Containerize an application
For the rest of this guide, you'll be working with a simple to-do list manager that runs on Node.js. If you're not familiar with Node.js, don't worry. This guide doesn't require any prior experience with JavaScript.  

## Prerequisites 
* You have installed the latest version of Docker Desktop.  
* You have installed a Git clientopen_in_new.  
* You have an IDE or a text editor to edit files. Docker recommends using Visual Studio Code.  

## Get the app 
Before you can run the application, you need to get the application source code onto your machine.
Clone the getting-started-app repositoryopen_in_new using the following command:
``` Docker
git clone https://github.com/docker/getting-started-app.git
```
## View the contents of the cloned repository. You should see the following files and sub-directories.  

├── getting-started-app/  
│ ├── package.json  
│ ├── README.md  
│ ├── spec/  
│ ├── src/  
│ └── yarn.lock  

## Build the app's image 
To build the image, you'll need to use a Dockerfile. A Dockerfile is simply a text-based file with no file extension that contains a script of instructions. Docker uses this script to build a container image.

1. In the `getting-started-app` directory, which is the same location as the `package.json` file, create a file named `Dockerfile`.

    You can use the following commands to create a `Dockerfile` based on your operating system. If you are using the Windows Command Prompt, run the following commands:
    
    - Navigate to the `getting-started-app` directory. Make sure you're in the `getting-started-app` directory. Replace `\path\to\getting-started-app` with the actual path to your `getting-started-app` directory.
    
        ```
        cd \path\to\getting-started-app
        ```

    - Create an empty file named `Dockerfile`.

        ```
        type nul > Dockerfile
        ```

2. Using a text editor or code editor, add the following contents to the Dockerfile:
   
        ```
        FROM node:18-alpine
        WORKDIR /app
        COPY . .
        RUN yarn install --production
        CMD ["node", "src/index.js"]
        EXPOSE 3000
        ```
3. Build the image using the following commands:

    In the terminal, make sure you're in the `getting-started-app` directory. Replace `/path/to/getting-started-app` with the actual path to your `getting-started-app` directory.

    ```bash
    $ cd /path/to/getting-started-app
    ```

    Build the image:

    ```bash
    docker build -t getting-started .
    ```

    The `docker build` command uses the `Dockerfile` to build a new image. You might have noticed that Docker downloaded several "layers". This is because you instructed the builder to start from the `node:18-alpine` image. If that image isn't already on your machine, Docker needed to download it.

    After Docker downloaded the base image, the instructions from the `Dockerfile` copied your application and used `yarn` to install its dependencies. The `CMD` directive specifies the default command to run when starting a container from this image.

    Finally, the `-t` flag tags your image with a human-readable name, in this case, `getting-started`. This name allows you to refer to the image when you run a container.

    The `.` at the end of the `docker build` command tells Docker to look for the `Dockerfile` in the current directory.

## Start an App Container

Now that you have an image, you can run the application in a container using the `docker run` command.

1. Run your container using the `docker run` command and specify the name of the image you just created:

    ```bash
    $ docker run -dp 127.0.0.1:3000:3000 getting-started
    ```

    - The `-d` flag (short for --detach) runs the container in the background.
    - The `-p` flag (short for --publish) creates a port mapping between the host and the container. The `-p` flag takes a string value in the format of `HOST:CONTAINER`, where `HOST` is the address on the host, and `CONTAINER` is the port on the container. The command publishes the container's port 3000 to `127.0.0.1:3000` (localhost:3000) on the host. Without the port mapping, you wouldn't be able to access the application from the host.

2. After a few seconds, open your web browser to [http://localhost:3000](http://localhost:3000). You should see your app.
![dd](https://github.com/023-Asish/DevOps/assets/77069694/0453045b-a062-4bf3-b40d-bdd71f54b82e)

3. Add an item or two and see that it works as you expect. You can mark items as complete and remove them. Your frontend is now successfully storing items in the backend.

    At this point, you have a running todo list manager with a few items.

    If you take a quick look at your containers, you should see at least one container running that's using the `getting-started` image and on port 3000. You can use either the CLI or Docker Desktop's graphical interface to view your containers.
![hv](https://github.com/023-Asish/DevOps/assets/77069694/36661404-b3cf-44ac-947c-ab607b96bc89)


