# udemy-docker-complex

### Basic Architecture

![image](https://user-images.githubusercontent.com/9342308/72189583-3348ad00-33cb-11ea-8e97-6edd3b947911.png)

### Basic Architecture

![image](https://user-images.githubusercontent.com/9342308/72027322-88aa8000-324c-11ea-8fb6-9e8d8186ca71.png)

### Dev Environments

We need a separate dev container for each of these three items. So that when we make changes to on of them, only that single container gets rebuilt

![image](https://user-images.githubusercontent.com/9342308/72027407-e8a12680-324c-11ea-9f24-1ed1bcadcc77.png)

### Nginx Splits client (React Server) vs server (Express)

Note: Client and Server (on right) are considered "upstream"

![image](https://user-images.githubusercontent.com/9342308/72192957-8f183380-33d5-11ea-86dd-eb054c0aceda.png)

### Starting Production Build

NOTE: React Server is different, now separate Nginx that is simply serving static files

![image](https://user-images.githubusercontent.com/9342308/72203509-7857f800-343a-11ea-9acc-6049b8d7e6f7.png)

### Built .travis.yml that builds images and pushes them to Docker Hub

### Dockerrun.aws.json

-   At least one container must be "essential"
-   "hostname" is optional for both worker and nginx, as other containers don't need to contact those containers
-   docker-compose is better at implicit links/hostnames between containers. Must be defined in AWS ECS

```json
{
    "AWSEBDockerrunVersion": 2,
    "containerDefinitions": [
        {
            "name": "client",
            "image": "cburkins/multi-client",
            "hostname": "client",
            "essential": false
        },
        {
            "name": "server",
            "image": "cburkins/multi-server",
            "hostname": "api",
            "essential": false
        },
        {
            "name": "worker",
            "image": "cburkins/multi-worker",
            "hostname": "worker",
            "essential": false
        },
        {
            "name": "nginx",
            "image": "cburkins/multi-nginx",
            "hostname": "nginx",
            "essential": true,
            "portMappings": [
                {
                    "hostPort": 80,
                    "containerPort": 80
                }
            ],
            "links": ["client", "server"]
        }
    ]
}
```

### Creating Elastic Beanstalk Application

1. Create Application
1. Create Environment within Application
1. Pre-configured platform: Multi-container Docker
1. Application code: Sample Application
