# Jenkins Pipeline: Modular multi-service repository

## Directory structure
The basic structure of the repo is as follows:
```
project/
├── Dockerfile
├── Jenkinsfile
├── Jenkinsfile.modules
├── module1/
│   └── Dockerfile
├── module2/
│   └── Dockerfile
└── module3/
    └── Dockerfile
```

## Docker
The main `Dockerfile` creates an image with the name of the job and the environment appended with the following format:
```
${ORG}-${JOB_BASE_NAME}-${PRJ_ENV}
```
where:
* `ORG` is the name of the organization or project where the image belongs to.
* `JOB_BASE_NAME` is the name of the job/pipeline in Jenkins.
* `PRJ_ENV` is the environment this project is built for.

Each `Dockerfile` within the modules includes:
```
ARG     BASE_IMAGE_FROM=<env_var_passed_as_argument>
FROM    $BASE_IMAGE_FROM:latest as build
```

## Jenkinsfiles
* `Jenkinsfile` builds the base image.
* `Jenkinsfile.modules` is a reusable file to build each of the images within the modules using the base image as start point.
