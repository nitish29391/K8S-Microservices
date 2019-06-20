## Working with DockerHub
In the previous chapter you deployed your containerized applications using the `docker-compose` tool. Now, we are going to deploy our containerized applications on the **Microsoft Azure** cloud. We use the `Azure Kubernetes Service` for this. But, before deploying the application on the AKS cluster, you need to upload your application images to a publicly accessible repository. You can now use the free, public repository - [DockerHub](https://hub.docker.com) for this. In this chapter, we will be uploading our containerized application images to the `Docker hub`.

### Prerequisites
* Docker Desktop
* Visual Studio Code
* Docker extension for VS Code 
  
### DockerHub
[DockerHub](https://hub.docker.com) is a service provided by Docker for finding and sharing container images with your team. 
> Docker Hub is a cloud-based repository in which Docker users and partners create, test, store and distribute container images. Through Docker Hub, a user can access public, open source image repositories, as well as use a space to create their own private repositories, automated build functions, webhooks and work groups.
>  <br/> *source: [https://searchitoperations.techtarget.com](https://searchitoperations.techtarget.com/definition/Docker-Hub)*

Docker hub provides the following major features:
- *Repositories*: Push and pull container images.
- *Teams & Organizations*: Manage access to private repositories of container images.
- *Official Images*: Pull and use high-quality container images provided by Docker.
- *Publisher Images*: Pull and use high-quality container images provided by external vendors. Certified images also include support and guarantee compatibility with Docker Enterprise.
- *Builds*: Automatically build container images from GitHub and Bitbucket and push them to Docker Hub
- *Webhooks*: Trigger actions after a successful push to a repository to integrate Docker Hub with other services.

### Create an account in Docker Hub

1. Open the browser and navigate to [http://hub.docker.com](https://hub.docker.com)
2. Sign up for a new account
3. In the signup page provide a valid `docker id`, `password` and `email` address. Confirm and create the user.
4. Provide the user details to complete the registration. You will receive a confirmation mail in the given email address. 
5. Goto you email account and click on the link in the confirmation mail to activate your account.

### Login to the `Docker Hub` for pusing images
1. Open the `Command prompt/Terminal` and run the following command.
    > $ docker login
2. This will ask you to enter your user name (`docker id`) and docker password. Your can provide the `docker id` and `password` you have used to create the Docker Hub account.
    ```
    λ docker login
    Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
    Username: azuredeveloper
    Password:
    Login Succeeded
    ```
    > **Note:** Password will not appear when you type it. It hides the password for security. 
### Push your `IdentityAPI` service image to Docker Hub
> **Note:** Make sure you have installed the `Docker Desktop` software in your machine successfully.

1. Open the `Command prompt/Terminal` and set `K8S-Microservices/IdentityAPI/IdentityAPI` folder as the current working directory.
2. You can see the `Dockerfile` and `.dockerignore` file in the directory. 
3. You need to build the image with the name that contains your `docker id`. You can tag your Docker image in the following format.
    > &lt;dockerid&gt;/image:tag
    > eg: azuredeveloper/identityapi:latest
4. Run the following command to build the image. 
    > $ docker build -t &lt;dockerid&gt;/identityapi:latest .
    > **Note:** Replace the &lt;dockerid&gt; with your docker id your have created. Do not forget to put the . (dot ) at the end of the command line.
    ```
    λ docker build -t azuredeveloper/identityapi .
    Sending build context to Docker daemon  36.35kB
    Step 1/16 : FROM mcr.microsoft.com/dotnet/core/sdk:2.2 AS build
     ---> a4974ac66bfc
    Step 2/16 : WORKDIR /src
     ---> Running in d5e55d4d2477
    Removing intermediate container d5e55d4d2477
     ---> c93ad4a4dfe4
    Step 3/16 : COPY *.csproj ./
     ---> 4a527cfd1140
    Step 4/16 : RUN dotnet restore
     ---> Running in 3dc7a52c1d72
    --- code removed for brevity --- 
    ```
5. Run the `docker images` command to see the list of images.
    ```
    λ docker images
    REPOSITORY                             TAG                 IMAGE ID            CREATED             SIZE
    azuredeveloper/identityapi             latest              4286bf7b8dd1        2 minutes ago       274MB
    nginx                                  latest              62c261073ecf        2 weeks ago         109MB
    mcr.microsoft.com/mssql/server         2017-latest         3471dd922373        4 weeks ago         1.33GB
    node                                   latest              502d06d3bfdf        6 weeks ago         906MB
    mcr.microsoft.com/dotnet/core/sdk      2.2                 a4974ac66bfc        6 weeks ago         1.74GB
    mcr.microsoft.com/dotnet/core/aspnet   2.2                 ce06b36fcba4        6 weeks ago         260MB
    ```
6. You can now push your image to the Docker Hub. To upload the image to Docker Hub you need to authenticate yourself. Marke sure you have logged in to the docker account from the `Command Prompt/Terminal`.
7. Run the following command to to push the `identityapi` image to the Docker hub.
    > $ docker push &lt;dockerid&gt;/identityapi:latest
    ```
    λ docker push azuredeveloper/identityapi:latest
    The push refers to repository [docker.io/azuredeveloper/identityapi]
    3ce05f73624c: Pushed
    ab09aced134a: Pushed
    a758c0ef43b0: Mounted from sonusathyadas/eventapi
    9c535b719272: Mounted from sonusathyadas/eventapi
    f3cfebfc6461: Mounted from sonusathyadas/eventapi
    6270adb5794c: Mounted from sonusathyadas/eventapi
    latest: digest: sha256:b605885ffd9b2090f2a7638953420ac750898a758cdedddb9e6c6f0b4df92b2b size: 1581
    ```
8. You can verify it in the Docker Hub portal. 

### Push your `EventAPI` service image to Docker Hub
1. Open the `Command prompt/Terminal` and set `K8S-Microservices/EventAPI/EventAPI` folder as the current working directory.
2. You can see the `Dockerfile` and `.dockerignore` file in the directory. 
3. Run the following command to build the image. 
    > $ docker build -t &lt;dockerid&gt;/eventapi:latest .
    > **Note:** Replace the &lt;dockerid&gt; with your docker id your have created. Do not forget to put the . (dot ) at the end of the command line.
    ```
    λ docker build -t azuredeveloper/eventapi:latest .
    Sending build context to Docker daemon  50.18kB
    Step 1/16 : FROM mcr.microsoft.com/dotnet/core/sdk:2.2 AS build
     ---> a4974ac66bfc
    Step 2/16 : WORKDIR /src
     ---> Running in d77db54128cc
    Removing intermediate container d77db54128cc
     ---> 3a0502746d6e
    Step 3/16 : COPY *.csproj ./
     ---> 63e6f81f2f80
    Step 4/16 : RUN dotnet restore
     ---> Running in 52cc6e10cf88
    --- code removed for brevity --- 
    ```
5. Run the `docker images` command to see the list of images.
    ```
    λ docker images
    REPOSITORY                             TAG                 IMAGE ID            CREATED             SIZE
    azuredeveloper/eventapi                latest              a5cee3e815c0        46 seconds ago      274MB
    azuredeveloper/identityapi             latest              4286bf7b8dd1        30 minutes ago      274MB
    nginx                                  latest              62c261073ecf        2 weeks ago         109MB
    mcr.microsoft.com/mssql/server         2017-latest         3471dd922373        4 weeks ago         1.33GB
    node                                   latest              502d06d3bfdf        6 weeks ago         906MB
    mcr.microsoft.com/dotnet/core/sdk      2.2                 a4974ac66bfc        6 weeks ago         1.74GB
    mcr.microsoft.com/dotnet/core/aspnet   2.2                 ce06b36fcba4        6 weeks ago         260MB
    ```
6. You can now push your image to the Docker Hub. To upload the image to Docker Hub you need to authenticate yourself. Marke sure you have logged in to the docker account from the `Command Prompt/Terminal`.
7. Run the following command to to push the `eventapi` image to the Docker hub.
    > $ docker push &lt;dockerid&gt;/eventapi:latest
    ```
    λ docker push azuredeveloper/eventapi:latest
    The push refers to repository [docker.io/azuredeveloper/eventapi]
    3312840b7e4f: Pushed
    ab09aced134a: Mounted from azuredeveloper/identityapi
    a758c0ef43b0: Mounted from azuredeveloper/identityapi
    9c535b719272: Mounted from azuredeveloper/identityapi
    f3cfebfc6461: Mounted from azuredeveloper/identityapi
    6270adb5794c: Mounted from azuredeveloper/identityapi
    latest: digest: sha256:19219bf29d3a3c9553acc66b92f12a3c93c3260ff18f519a903c954cc5719652 size: 1581
    ```
8. Verify the uploaded image in the Docker Hub portal. 

### Push your `EventClient` image to Docker Hub
1. Open the `Command prompt/Terminal` and set `K8S-Microservices/EventClient/EventClient` folder as the current working directory.
2. You can see the `Dockerfile` and `.dockerignore` file in the directory. 
3. Run the following command to build the image. 
    > $ docker build -t &lt;dockerid&gt;/eventui:latest .
    > **Note:** Replace the &lt;dockerid&gt; with your docker id your have created. Do not forget to put the . (dot ) at the end of the command line.
    ```
    Sending build context to Docker daemon  9.331MB
    Step 1/14 : FROM mcr.microsoft.com/dotnet/core/sdk:2.2 AS build
     ---> a4974ac66bfc
    Step 2/14 : WORKDIR /src
     ---> Running in affe70af9eb3
    Removing intermediate container affe70af9eb3
     ---> 0bc4331245df
    Step 3/14 : COPY *.csproj ./
     ---> 9ba215eae19e
    Step 4/14 : RUN dotnet restore
     ---> Running in c5b7450dd2b7      
    --- code removed for brevity --- 
    ```
5. Run the `docker images` command to see the list of images.
    ```
    λ docker images
    REPOSITORY                             TAG                 IMAGE ID            CREATED             SIZE
    azuredeveloper/eventui                 latest              a85fd987e967        52 seconds ago      270MB
    azuredeveloper/eventapi                latest              a5cee3e815c0        46 seconds ago      274MB
    azuredeveloper/identityapi             latest              4286bf7b8dd1        30 minutes ago      274MB
    nginx                                  latest              62c261073ecf        2 weeks ago         109MB
    mcr.microsoft.com/mssql/server         2017-latest         3471dd922373        4 weeks ago         1.33GB
    node                                   latest              502d06d3bfdf        6 weeks ago         906MB
    mcr.microsoft.com/dotnet/core/sdk      2.2                 a4974ac66bfc        6 weeks ago         1.74GB
    mcr.microsoft.com/dotnet/core/aspnet   2.2                 ce06b36fcba4        6 weeks ago         260MB
    ```
6. You can now push your image to the Docker Hub. To upload the image to Docker Hub you need to authenticate yourself. Marke sure you have logged in to the docker account from the `Command Prompt/Terminal`.
7. Run the following command to to push the `eventapi` image to the Docker hub.
    > $ docker push &lt;dockerid&gt;/eventui:latest
    ```
    λ docker push azuredeveloper/eventui:latest
    The push refers to repository [docker.io/azuredeveloper/eventui]
    3312840b7e4f: Pushed
    ab09aced134a: Mounted from azuredeveloper/identityapi
    a758c0ef43b0: Mounted from azuredeveloper/identityapi
    9c535b719272: Mounted from azuredeveloper/identityapi
    f3cfebfc6461: Mounted from azuredeveloper/identityapi
    6270adb5794c: Mounted from azuredeveloper/identityapi
    latest: digest: sha256:19219bf29d3a3c9553acc66b92f12a3c93c3260ff18f519a903c954cc5719652 size: 1581
    ```
8. Verify the uploaded image in the Docker Hub portal. 

----
Shared by Sonu Sathyadas
[mailto:sonusathyadas@hotmail.com](mailto:sonusathyadas@gmail.com)
