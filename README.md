# concourse-ci-cd
Complete Concourse CI/CD pipeline of Maven Spring project to kubernetes

This is the simple spring boot maven hello project that contains a REST API to test the project is running successfully. It contains the concourse_ci directory that contains all about the concourse CI/CD pipeline.

**prerequisite**

- Docker
- Docker Compose
- Maven
- Kubernetes Cluster
- Docker Hub Account

**local testing**

Run `mvn clean package`
Then run `java -jar target/hello-springboot-0.0.1-SNAPSHOT.jar`

Now Spring boot application is running locally, Verify it by hitting the below URL

`http://localhost:8080/api/hello`

First, you need to give the following variable value in the config.yml file

**Docker**

docker-hub-email: <<DOCKER HUB EMAIL>>

docker-hub-username: <<DOCKER USERNAME>>
  
docker-hub-password: <<DOCKER PASSWORD>>
  
docker-hub-repo-name: <<DOCKER REPO NAME>>

**Kubernetes**

server: <<API SERVER URI>>
  
namespace: <<NAMESPACE>>
  
cad: <<CERT AUTH DATA>>
  
token: <<K8 TOKEN>>

then save the file on local, Don't push it to the repository and you can also use vault as a secret manager.

# Setup Concourse and CI/CD pipeline steps

- Download the Docker compose file

        `wget https://concourse-ci.org/docker-compose.yml`
- Run the Docker Compose File

        `docker-compose up -d`
- Download the fy CLI tool from the localhost:8080

- Give execute permission to the fly CLI

        `chmod +x fly`
- Copy the fly CLI to the /usr/bin

        `sudo cp fly /usr/bin/`
- Login to the concourse

        `fly -t tutorial login -c http://localhost:8080 -u test -p test`
- Clone the following spring boot project

        `git clone https://github.com/azmathasan92/concourse-ci-cd.git`
- Change to the root dir of the project

        `cd concourse-ci-cd`
- Create Concourse Pipeline

        `fly -t tutorial set-pipeline -p spring-boot-service -c concourse_ci/pipeline.yml -l concourse_ci/config.yml`
- Unpause Concourse Pipeline

        `fly -t tutorial unpause-pipeline -p spring-boot-service`

Now you have successfully setup the CI/CD pipeline that test, package and deploy your project to the Kubernetes.

**For more info follow the mentioned blog:**

https://blog.knoldus.com/concourse-ci-cd-pipeline/
