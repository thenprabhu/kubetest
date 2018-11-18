
## Docker/Kubernetes stack to deploy heterogeneous microservices ##

Required tools:
* Docker Edge for Mac
* Virtualbox
* Kubernetes

Steps to setup Docker, Kubernetes cluster:

I. Download and install [Docker Edge for Mac](https://docs.docker.com/docker-for-mac/edge-release-notes/)
  
II. Install virtualbox

 ```bash
 brew cask install virtualbox
 ```
 
III. Install Kubernetes

  ```bash
  brew install kubernetes-cli
  ```
  
IV. Set kubectl to use docker-for-desktop

  ```bash
  kubectl config use-context docker-for-desktop
  ```
  
V. Enable cluster in Docker

   ```bash
   Docker -> Kubernetes -> "Enable local cluster"
   ```
   
VI. Verify cluster details

 ```bash
 kubectl cluster-info
 ```
 
Once the above steps are successful, configure docker images and deploy kubernetes cluster.

1) Create a directory `country_assembly`
    ```bash
    mkdir country_assembly
    ```
2) Copy `countries-assembly-1.0.1.jar`  and get into the directory

    ```bash
    cp countries-assembly-1.0.1.jar country_assembly
    cd country_assembly
    ```
3) Create a file named `Dockerfile` with contents

  ```dockerfile
    FROM openjdk:8-jre
    ADD countries-assembly-1.0.1.jar app.jar
    EXPOSE 8000
    ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar"]
  ```
    
4) Now, build the docker images

     ```bash
     docker build -t countries-assembly:1.0.1 .
     cd ..
     ```
     
5) Similarly, create a directory `airports_assembly`

     ```bash
     mkdir airports_assembly
     ```
6) Copy `airports-assembly-1.0.1.jar` into the directory 

     ```bash
     cp airports-assembly-1.0.1.jar airports_assembly
     cd airports_assembly
     ```
7) Create a file named `Dockerfile` with contents
    ```dockerfile
    FROM openjdk:8-jre
    ADD airports-assembly-1.0.1.jar app.jar
    EXPOSE 8000
    ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar"]
    ```
    
8) Build docker images
    ```bash
    docker build -t airports-assembly:1.0.1 .
    cd ..
    ```
9) Login and push the images into your own docker hub
     ```bash
     docker login
     docker push countries-assembly:1.0.1
     docker push airports-assembly:1.0.1
     ```
10) Create a directory, e.g kubernetes

11) Copy the yaml files `countryassembly-service.yaml, airportsassembly-service.yaml, ambassador-no-rbac.yaml and ambassador-service.yaml` and apply them

     ```bash
     kubectl apply -f .
     ```
     
12) Ensure cluster is up and running along with the required services..
    ```bash
    kubectl get svc,pods,rc,rs
    ```

13) Allow sometime, the services should be available to test.

     ```bash
     curl localhost/countries
     curl localhost/airports
     ```
