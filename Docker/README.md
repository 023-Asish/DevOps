# Docker overview
Docker is an open platform for developing, shipping, and running applications. Docker enables you to separate your applications from your infrastructure so you can deliver software quickly. With Docker, you can manage your infrastructure in the same ways you manage your applications. By taking advantage of Docker's methodologies for shipping, testing, and deploying code, you can significantly reduce the delay between writing code and running it in production.

# The underlying technology 
Docker is written in the Go programming languageopen_in_new and takes advantage of several features of the Linux kernel to deliver its functionality. Docker uses a technology called namespaces to provide the isolated workspace called the container. When you run a container, Docker creates a set of namespaces for that container.

These namespaces provide a layer of isolation. Each aspect of a container runs in a separate namespace and its access is limited to that namespace.

# Docker architecture 
Docker uses a client-server architecture. The Docker client talks to the Docker daemon, which does the heavy lifting of building, running, and distributing your Docker containers. The Docker client and daemon can run on the same system, or you can connect a Docker client to a remote Docker daemon. The Docker client and daemon communicate using a REST API, over UNIX sockets or a network interface. Another Docker client is Docker Compose, that lets you work with applications consisting of a set of containers.
![architecture](https://github.com/023-Asish/DevOps/assets/77069694/bf55b4b0-6d6f-4e96-b82e-4dc7f943989e)

# The Docker daemon 
The Docker daemon (dockerd) listens for Docker API requests and manages Docker objects such as images, containers, networks, and volumes. A daemon can also communicate with other daemons to manage Docker services.

# The Docker client 
The Docker client (docker) is the primary way that many Docker users interact with Docker. When you use commands such as docker run, the client sends these commands to dockerd, which carries them out. The docker command uses the Docker API. The Docker client can communicate with more than one daemon.

# Docker registries 
A Docker registry stores Docker images. Docker Hub is a public registry that anyone can use, and Docker is looks for images on Docker Hub by default. You can even run your own private registry.
When you use the docker pull or docker run commands, Docker pulls the required images from your configured registry. When you use the docker push command, Docker pushes your image to your configured registry.

# Docker objects 
When you use Docker, you are creating and using images, containers, networks, volumes, plugins, and other objects. This section is a brief overview of some of those objects.

# Images 
An image is a read-only template with instructions for creating a Docker container. Often, an image is_based on another image, with some additional customization. For example, you may build an image which is based on the ubuntu image, but installs the Apache web server and your application, as well as the configuration details needed to make your application run.

You might create your own images or you might only use those created by others and published in a registry. To build your own image, you create a Dockerfile with a simple syntax for defining the steps needed to create the image and run it. Each instruction in a Dockerfile creates a layer in the image. When you change the Dockerfile and rebuild the image, only those layers which have changed are rebuilt. This is part of what makes images so lightweight, small, and fast, when compared to other virtualization technologies.

# Containers 
A container is a runnable instance of an image. You can create, start, stop, move, or delete a container using the Docker API or CLI. You can connect a container to one or more networks, attach storage to it, or even create a new image based on its current state.

By default, a container is relatively well isolated from other containers and its host machine. You can control how isolated a container's network, storage, or other underlying subsystems are from other containers or from the host machine.

A container is defined by its image as well as any configuration options you provide to it when you create or start it. When a container is removed, any changes to its state that aren't stored in persistent storage disappear.





# Fetching Docker image or creating Container
``` Docker
docker run hello-world
```

Unable to find image 'hello-world:latest' locally  
latest: Pulling from library/hello-world  
719385e32844: Pull complete  
Digest: sha256:dcba6daec718f547568c562956fa47e1b03673dd010fe6ee58ca806767031d1c  
Status: Downloaded newer image for hello-world:latest  

Hello from Docker!  
This message shows that your installation appears to be working correctly.  

# Downloading/Pulling Docker image
``` Docker
docker pull hello-world
```
Using default tag: latest  
latest: Pulling from library/hello-world  
Digest: sha256:dcba6daec718f547568c562956fa47e1b03673dd010fe6ee58ca806767031d1c  
Status: Image is up to date for hello-world:latest  
docker.io/library/hello-world:latest  

# List of all images
``` Docker
docker images
```
REPOSITORY      TAG       IMAGE ID       CREATED        SIZE  
alpine          latest    7e01a0d0a1dc   13 days ago    7.34MB  
nginx           latest    89da1fb6dcb9   3 weeks ago    187MB  
hello-world     latest    9c7a54a9a43c   3 months ago   13.3kB  



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

