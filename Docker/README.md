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
