# GitLab CI/CD - Hands-On Lab: Defining CI/CD Variables
This Hands-On Guide walks you through the lab exercises in the GitLab CI/CD course.
Estimate time to complete: 15 - 20 minutes

We are transitioning to the latest version of this course. If your group URL starts with https://spt.gitlabtraining.cloud, please use the Version 15.x instructions.

Objectives
To customize your CI/CD process, you can define your own environment variables. In this lab, you will learn how to define inline global variables, inline local variables, and group and project level variables.

Task A: Add Inline Variables
There are two types of inline variables we will explore in this section: global inline variables and job scoped inline variables. These variables are defined only for the .gitlab-ci.yml file they are declared in.

Note: Variables in GitLab CI/CD have a precedence, which means variables at a higher ’level’ will override the values of a lower ’level’. This can lead to unintended results, so re-use of variable names should be monitored carefully. For more information, click here.

Open your CICD Demo project from previous labs.

Click your .gitlab-ci.yml file to view its contents. To edit the file, click Edit > Edit single file.

Paste the following snippet at the end of the file, with an empty line between the file’s previous content and the snippet’s content.

environment variables:
  stage: build
  script:
    - echo "Do a test here"
    - echo "Here are some default, global, & local variables..."
    - echo $CI_COMMIT_SHORT_SHA
    - echo $group_level_variable
    - echo $project_level_variable
    - echo $INLINE_GLOBAL_VARIABLE
    - echo $INLINE_LOCAL_VARIABLE
After doing this, your .gitlab-ci.yml file will look like this:

stages:
  - test
  - build

test job:
  stage: test
  script:
    - echo "I am a unit test!"

build job:
  stage: build
  script:
    - echo "I am a build image!"

environment echoes:
  stage: build
  script:
    - echo "Who am I running as..."
    - whoami
    - echo "Where am I..."
    - pwd
    - ls -al
    - echo "Here's what is available in our environment..."
    - env

environment variables:
  stage: build
  script:
    - echo "Do a test here"
    - echo "Here are some default, global, & local variables..."
    - echo $CI_COMMIT_SHORT_SHA
    - echo $group_level_variable
    - echo $project_level_variable
    - echo $INLINE_GLOBAL_VARIABLE
    - echo $INLINE_LOCAL_VARIABLE
Near the top of your .gitlab-ci.yml, in a new line below the entire stages section, paste the following to declare a global inline variable:

variables:
  INLINE_GLOBAL_VARIABLE: "I'm an inline variable set at the global level of the CI/CD configuration file"
Note: A variable declared at the top level is globally available. In this example, all jobs can use the INLINE_GLOBAL_VARIABLE variable.

Inside the environment variables job, just below that job’s stage: build line (but before the script line), paste the following to declare a local inline variable. The variables keyword should be at the same indentation as that job’s stage amd script keywords.

variables:
  INLINE_LOCAL_VARIABLE: "I'm an inline variable set at the job level of the CI/CD configuration file"
Note: Since this variable inside a job, it is only accessible by the job. For this example, INLINE_LOCAL_VARIABLE is only accessible in the environment variables job.

At this point, your .gitlab-ci.yml file will look like this:

stages:
  - test
  - build

variables:
  INLINE_GLOBAL_VARIABLE: "I'm an inline variable set at the global level of the CI/CD configuration file"

test job:
  stage: test
  script:
    - echo "I am a unit test!"

build job:
  stage: build
  script:
    - echo "I am a build image!"

environment echoes:
  stage: build
  script:
    - echo "Who am I running as..."
    - whoami
    - echo "Where am I..."
    - pwd
    - ls -al
    - echo "Here's what is available in our environment..."
    - env

environment variables:
  stage: build
  variables:
    INLINE_LOCAL_VARIABLE: "I'm an inline variable set at the job level of the CI/CD configuration file"
  script:
    - echo "Do a test here"
    - echo "Here are some default, global, & local variables..."
    - echo $CI_COMMIT_SHORT_SHA
    - echo $group_level_variable
    - echo $project_level_variable
    - echo $INLINE_GLOBAL_VARIABLE
    - echo $INLINE_LOCAL_VARIABLE
Note: When defining variables, watch your indentation. Global variables must be indented by 2 spaces, and must be immediately under a flush-left variables keyword that is outside any job definition. Local variables must be indented 4 spaces, and must be immediately under a variables keyword that is indented 2 spaces and is within a job definition.

In the Commit message field, type Add custom variables, ensure the Target Branch set to main, and click Commit changes.

