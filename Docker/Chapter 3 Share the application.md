# Share the application
  Now that you've built an image, you can share it. To share Docker images, you have to use a Docker registry. The default registry is Docker Hub and is where all of the images you've used have come from.
      # Create a repository 
      To push an image, you first need to create a repository on Docker Hub.
  
  1. Sign up or Sign in to Docker Hub.
  2. Select the Create Repository button.
  3. For the repository name, use getting-started. Make sure the Visibility is Public.
  4. Select Create.
 ![repo](https://github.com/023-Asish/DevOps/assets/77069694/2ad92bdb-5dc3-4920-b8af-167459e05551)

      ## Push the Image
      1. In the command line, run the docker push command that you see on Docker Hub. Note that your command will have your Docker ID, not "docker".
         ```
         docker push docker/getting-started
         The push refers to repository [docker.io/docker/getting-started]
         An image does not exist locally with the tag: docker/getting-started
         ```
         Why did it fail? The push command was looking for an image named docker/getting-started, but didn't find one. If you run docker image ls, you won't see one either.

         To fix this, you need to tag your existing image you've built to give it another name.

       2. Sign in to Docker Hub using the command docker login -u YOUR-USER-NAME.

       3. Use the docker tag command to give the getting-started image a new name. Replace YOUR-USER-NAME with your Docker ID.
         ```
          docker tag getting-started YOUR-USER-NAME/getting-started
         ```
       4. Now run the docker push command again. If you're copying the value from Docker Hub, you can drop the tagname part, as you didn't add a tag to the image name. If you don't specify a tag, Docker uses a tag called latest.  
         ```
          docker push YOUR-USER-NAME/getting-started
         ```

     ![push to regestry](https://github.com/023-Asish/DevOps/assets/77069694/20af7d21-c22a-4308-b64b-81266fef4fbf)

     ## Run the image on a new instance 
        Now that your image has been built and pushed into a registry, try running your app on a brand new instance that has never seen this container image. To do this, you will use Play with Docker.

        1. Open your browser to Play with Dockeropen_in_new.
        2. Select Login and then select docker from the drop-down list.
        3. Sign in with your Docker Hub account and then select Start.
        4. Select the ADD NEW INSTANCE option on the left side bar. If you don't see it, make your browser a little wider. After a few seconds, a terminal window opens in your browser.  
        5. In the terminal, start your freshly pushed app.
           ```
             docker run -dp 0.0.0.0:3000:3000 YOUR-USER-NAME/getting-started
           ```
           You should see the image get pulled down and eventually start up.
        6. Select the 3000 badge when it appears.  
           If the 3000 badge doesn't appear, you can select Open Port and specify 3000.
     ![play with docker](https://github.com/023-Asish/DevOps/assets/77069694/467a703b-ec9a-4697-a809-bdb3d7c09c68)
     ![output](https://github.com/023-Asish/DevOps/assets/77069694/05a70f84-cbd5-4c5f-93cc-e4cc7f81f998)

     ## Summary  
        In this section, you learned how to share your images by pushing them to a registry. You then went to a brand new instance and were able to run the freshly pushed image. This is quite common in CI pipelines, where the pipeline will create the image and push it to a registry and then the production environment can use the latest version of the image.
