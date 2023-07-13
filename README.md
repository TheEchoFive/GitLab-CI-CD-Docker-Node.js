# GitLab CI/CD Docker Node.js

GitLab CI/CD template with GitLab-Runner for deploying a project in a docker container to a VPS using.

When using artifacts, you can download the archive of any stage in the list of pipelines.

For example, the deployment was made to two VPS, one for development, the second for production.

Add files from this repository to your project root directory

Example for NestJS
```
|_src
  |_application
    |_...
  |_common
    |_...
  |_database
    |_...
  |_main.ts
|_templates
  |_...
|_package.json
|_.env
|_docker-compose.yml
|_.gitlab-ci.yml
|_Dockerfile
```

#### Gitlab CI/CD Variables
To run container with your project docker-compose.yml uses variables from .env, add these lines to your variables (.env)
```
### DOCKER ENV ###
CONTAINER_NAME = app_name
CONTAINER_PORT = 3001
### DOCKER ENV ###
%your env strings%
```
> If you don't want to use a web server(nginx,apache) you can run the container on port 80


- ENV_DEV -  type: File 
> Paste your .env for **develop** and add strings for docker
- ENV_PROD - type: File
> Paste your .env for **production** and add strings for docker


#### Deploy to DEVELOP VPS
- Deploy from - branch develop
- Runner with tag - develop

#### Deploy to PRODUCTION VPS
- Runner with tag - production
- Deploy from - tag v.X.X.Xnn

###### Examples tags
- v.1.0.1
- v.1.0.1test
- v.1.0.11
- v.1.0.111

### Settings
#### Install stage
- image - Docker image for your project from https://hub.docker.com/
- script - Command for install packges
- expire_in - Specify how long job artifacts are stored before they expire and are deleted
- paths - The paths are relative to the project directory of the files\directories that are saved to the artifact

#### Test Stage
- image - Docker image for your project from https://hub.docker.com/
- script - Command for start unit test
- expire_in - Specify how long job artifacts are stored before they expire and are deleted
- paths - The paths are relative to the project directory of the files\directories that are saved to the artifact
- reports - Use to collect artifacts generated by included templates in jobs

#### Build Stage
- image - Docker image for your project from https://hub.docker.com/
- script - If for get .env file from repository settings and command for start build project
- expire_in - Specify how long job artifacts are stored before they expire and are deleted
- paths - The paths are relative to the project directory of the files\directories that are saved to the artifact
- reports - Use to collect artifacts generated by included templates in jobs

#### Deploy Stages
- image - Docker image for your project from https://hub.docker.com/
- script - Command for build and start docker container
- expire_in - Specify how long job artifacts are stored before they expire and are deleted
- paths - The paths are relative to the project directory of the files\directories that are saved to the artifact
- reports - Use to collect artifacts generated by included templates in jobs
- only - Branches and conditions that are suitable for deployment
- tags - Runner tags that can take this job
- except - Except main branch from deploy stage

If you do not specify an image in the stages, the default image will be used, which is specified in the runner settings

#### Runner settings
 - executor = "docker"
 - default image = ...

##### config.toml
/etc/gitlab-runner/config.toml
```
...
volumes = ["/cache"]
...
```
change to
```
volumes = ["/var/run/docker.sock:/var/run/docker.sock", "/cache"]
```

#### Deploy to develop or create tag
![image](https://user-images.githubusercontent.com/32634559/204368983-1f9ae39d-b008-49c8-bd43-b35e83fa7b63.png)

#### Deploy to other branch
![image](https://user-images.githubusercontent.com/32634559/204371127-600db8e4-b1d7-4bbb-9543-88e8e6cfee72.png)

#### Download artifacts
![image](https://user-images.githubusercontent.com/32634559/220139608-b9ca6a1b-30d8-4691-9408-3b875527733f.png)
![image](https://user-images.githubusercontent.com/32634559/220139512-32eb6acf-7614-4f2f-a546-c94759640cc9.png)


##### Future plans:
- Optimize Stages
- Running a project in a container for local development
