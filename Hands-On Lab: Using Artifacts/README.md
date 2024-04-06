# GitLab CI/CD - Hands-On Lab: Using Artifacts
This Hands-On Guide walks you through the lab exercises in the GitLab CI/CD course.
Estimate time to complete: 20 - 25 minutes

We are transitioning to the latest version of this course. If your group URL starts with https://spt.gitlabtraining.cloud, please use the Version 15.x instructions.

Objectives
Artifacts in GitLab are files that that are created in a job, and then passed to other jobs in later stages. An artifact in one stage cannot be passed to a job in the same stage. You can later access and download any artifacts created in a pipeline. For more information, click here.

In this lab, you will learn how to create an artifact with your .gitlab-ci.yml file. After creating the artifact, you will view the artifact in the GitLab UI.

Task A: Add a main.go File
Open your CICD Demo project from previous labs.

Navigate to Code > Repository and add a new file by clicking + > This directory > New file

In the Filename field, enter main.go

Paste the following code into the file.

package main

import (
    "fmt"
    "net/http"
)

func helloworld() string {
    return "Hello World!!"
}

func healthcheck() string {
    return "Health OK!"
}

func livenesscheck() string {
    return "I am alive!!!"
}

func main() {
    http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
        fmt.Fprintf(w, helloworld())
    })

    http.HandleFunc("/healthz", func(w http.ResponseWriter, r *http.Request) {
        fmt.Fprintf(w, healthcheck())
    })

    http.HandleFunc("/liveness", func(w http.ResponseWriter, r *http.Request) {
        fmt.Fprintf(w, livenesscheck())
    })

    http.ListenAndServe(":8080", nil)
}
In the Commit message field, type Add main.go file, ensure the Target Branch is set to main, and click Commit changes.



# Task B: Add Artifacts to your Pipeline
In the left sidebar, click Code > Repository.

Select your .gitlab-ci.yml file to view its contents. Click Edit > Edit single file. Paste the following snippet at the end of the file.

build app:
  image: golang:latest
  stage: build
  script:
    - go build -o app main.go
  artifacts:
    paths:
    - app
    expire_in: 1 hour
In the Commit message field, type Add CI artifacts, ensure the Target Branch is set to main, and click Commit changes.

In the left-hand navigation pane, click Build > Pipelines and click the status icon for the most recent pipeline run.

When the build app job finishes, click it to review the job’s output log.

Note: If the job fails with a message about being unable to find go.mod, retry the job until it passes. This is an intermittent Go build bug.

In the Job artifacts panel on the right of the page, click Browse and notice that the app artifact created by the build app pipeline job is available for download.

