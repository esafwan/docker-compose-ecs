# Deploy Nginx to AWS ECS(Fargate)

A very basic example of using `docker compose` to deploy container to AWS ECS(Fargate) using standard `docker-compose.yml` file. 


### Before you start: 
> You will need to have your AWS CLI environment ready either with a default or named profiles. The Docker CLI can use those credentials. If it is not setup, the Docker workflow will allow you to either read the environment variables with your AWS credentials (AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY) or it will ask for those credentials and will store the credentials for you (at $HOME/.aws/credentials).


## Start Here
We need to create a new docker context so that the Docker CLI can point to a different endpoint. By default, Docker points to a local context called default (that is the Docker runtime on your machine) 

### We will add an Amazon ECS context using the command 

`docker context create ecs`.

```
➜ docker context create ecs aws
? Create a Docker context using: An existing AWS profile
? Select AWS Profile default
Successfully created ecs context "aws"  

```

### You can check available context with below command

```
➜ docker context ls                     
NAME                TYPE                DESCRIPTION                               DOCKER ENDPOINT               KUBERNETES ENDPOINT   ORCHESTRATOR
default *           moby                Current DOCKER_HOST based configuration   unix:///var/run/docker.sock                         swarm
aws                 ecs                                                                          </code>                                 
```

### You can switch to a different context with below command
```
➜ docker context use aws
aws
➜ docker context ls              
NAME                TYPE                DESCRIPTION                               DOCKER ENDPOINT               KUBERNETES ENDPOINT   ORCHESTRATOR
default             moby                Current DOCKER_HOST based configuration   unix:///var/run/docker.sock                         swarm
aws *               ecs 

```  



> The credentials in the AWS profile must have sufficient permissions to deploy the application in AWS. This includes permissions to create VPCs, ECS tasks, Load Balancers, etc.

### Now to deploy to ECS(Fargate) run the below command:
`docker compose up `

> You can see that we are using the main docker binary to do this, instead of the docker-compose binary. The docker binary,  comes with additional functionality of `docker compose up`. 


### After the actions are successfully executed, you can run the below command to get the public domain created by ECS for you to access the deployed container.

`docker compose ps`

### To shutdown all services on ECS simply run:
`docker compose down`

## Common issues you may face:
 1. Unable to find the cluster on AWS Console. Makse sure that you are in the right region. Check the cname provided in `docker compose up` for deployed region. You can control this by adding a default region in your aws profile.
 2. All standard docker-compose features are not supported in ECS context. [Check this link.](https://docs.docker.com/cloud/ecs-compose-features/)
 3. Make sure you are in the right context when running command.
