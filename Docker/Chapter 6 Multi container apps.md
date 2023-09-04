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




    
