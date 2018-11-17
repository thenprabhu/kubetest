# kubetest
Docker/Kubernetes stack to deploy heterogeneous microservices

Required tools:
  1. Docker Edge for Mac
  2. Virtualbox
  3. Kubernetes

Steps to setup Docker, Kubernetes cluster:

I. Download and install Docker Edge for Mac

  https://docs.docker.com/docker-for-mac/edge-release-notes/
  
II. Install virtualbox

  $ brew cask install virtualbox
 
III. Install Kubernetes

  $ brew install kubernetes-cli
  
IV. Set kubectl to use docker-for-desktop

  $ kubectl config use-context docker-for-desktop
  
V. Enable cluster in Docker

   Docker -> Kubernetes -> "Enable local cluster"
   
VI. Verify cluster details

  $ kubectl cluster-info
 
Once the above steps are successful, configure docker images and deploy kubernetes cluster.

1) Create a directory country_assembly

    $ mkdir country_assembly
2) Copy countries-assembly-1.0.1.jar and get into the directory

    $ cp countries-assembly-1.0.1.jar country_assembly
    $ cd country_assembly
3) Create a file named "Dockerfile" with contents

    FROM openjdk:8-jre
    
    ADD countries-assembly-1.0.1.jar app.jar
    
    EXPOSE 8000
    
    ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar"]
    
4) Now, build the docker images

     $ docker build -t countries-assembly:1.0.1 .
     
     $ cd ..
     
5) Similarly, create a directory airports_assembly

     $ mkdir airports_assembly
6) Copy airports-assembly-1.0.1.jar into the directory 

     $ cp airports-assembly-1.0.1.jar airports_assembly
     
     $ cd airports_assembly    
7) Create a file named Dockerfile with contents

    FROM openjdk:8-jre
    
    ADD airports-assembly-1.0.1.jar app.jar
    
    EXPOSE 8000
    
    ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar"]
    
8) Build docker images

     $ docker build -t airports-assembly:1.0.1 .
     
     $ cd ..
9) Login and push the images into your own docker hub

     $ docker login
     
     $ docker push countries-assembly:1.0.1
     
     $ docker push airports-assembly:1.0.1
10) Create a directory, e.g kubernetes

11) Copy the yaml files (countryassembly-service.yaml, airportsassembly-service.yaml, ambassador-no-rbac.yaml and ambassador-service.yaml) and apply them

     $ kubectl apply -f .
     
12) Ensure cluster is up and running along with the required services..

13) Allow sometime, the services should be available to test.

     $ curl localhost/countries
     
     $ curl localhost/airports
