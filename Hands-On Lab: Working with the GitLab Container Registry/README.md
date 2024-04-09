GitLab CI/CD - Hands-On Lab: Working with the GitLab Container Registry
This Hands-On Guide walks you through the lab exercises in the GitLab CI/CD course.
Estimate time to complete: 15 - 20 minutes

We are transitioning to the latest version of this course. If your group URL starts with https://spt.gitlabtraining.cloud, please use the Version 15.x instructions.

Objectives
Docker is a platform commonly used by developers to build container applications. It is possible to generate Docker containers as a part of a CI/CD build process. In this lab, you will learn how to define a Dockerfile for your project, and integrate it into your .gitlab-ci.yml file.

# Task A: Add a Dockerfile
Open your CICD Demo project from earlier labs.

Add a new file to the CICD Demo project by selecting + > This directory > New file

In the Filename field, enter Dockerfile

Paste the following into the body of the new file.

FROM golang:1.11-alpine as builder
WORKDIR /usr/build
ADD main.go .
RUN go build -o app .

FROM alpine:latest

WORKDIR /usr/src

COPY --from=builder /usr/build/app .
EXPOSE 8080

CMD ["/usr/src/app"]
In the Commit message field, type Add Dockerfile, ensure the Target Branch is set to main, and click Commit changes.

# Task B: Define a build image Job
In the left navigation bar, click Code > Repository

Open the .gitlab-ci.yml file and click Edit > Edit single file. Paste the following new job definition at the end of the file.

build image:
  stage: build
  image: docker:18
  services:
    - docker:18-dind
  variables:
    IMAGE: $CI_REGISTRY_IMAGE/$CI_COMMIT_REF_SLUG:$CI_COMMIT_SHA
  script:
    - docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY
    - docker build -t $IMAGE .
    - docker push $IMAGE
In the Commit message field, type Add "build image" job definition, ensure the Target Branch is set to main, and click Commit changes.


# Task C: Ensure the Pipeline is Running
Go to Build > Pipelines. Click the most recent pipeline run.

Click the widget for the build image job to see its progress. Wait for the job to complete.

In the left navigation pane, click Deploy > Container Registry and view the container that was just uploaded by the build image job.

