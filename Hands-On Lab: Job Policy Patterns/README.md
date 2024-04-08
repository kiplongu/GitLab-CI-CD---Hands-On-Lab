# GitLab CI/CD - Hands-On Lab: Job Policy Patterns
This Hands-On Guide walks you through the lab exercises in the GitLab CI/CD course.
Estimate time to complete: 25 - 30 minutes

We are transitioning to the latest version of this course. If your group URL starts with https://spt.gitlabtraining.cloud, please use the Version 15.x instructions.

Objectives
Job Policy patterns allow the pipeline to control when and if jobs run using the rules keyword. In this lab, you will learn how to create jobs with rules. You will see the impact of these rules on a pipeline and learn how to use variables with pipeline rules.

Usage of the only and except keywords, while able to accomplish similar results, are not actively being developed and are not encouraged. For more information, click here.

Task A: Creating Jobs with Rules
Open your CICD Demo project from previous labs.

Click your .gitlab-ci.yml file to view its contents. Click Edit > Edit single file.

To clean up our configuration file, remove the environment echoes and environment variables jobs. Also remove the variables keyword and the associated variable. After completing these steps, you will have the following .gitlab-ci.yml file:

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
At the end of the file, paste the following code snippet:

deploy review:
  stage: review
  script:
    - echo "Do your average deploy here"
  rules:
    - if: '$CI_COMMIT_REF_NAME == "main"'
      when: never
    - if: '$CI_COMMIT_TAG'
      when: never
    - when: always
  environment:
    name: review/$CI_COMMIT_REF_NAME

deploy release:
  stage: deploy
  script:
    - echo "Deploy to a production environment"
  rules:
    - if: '$CI_COMMIT_REF_NAME != "main" && $CI_COMMIT_TAG'
      when: manual
  environment:
    name: production

deploy staging:
  stage: deploy
  script:
    - echo "Deploy to a staging environment"
  rules:
    - if: '$CI_COMMIT_REF_NAME == "main"'
      when: always
    - when: never
  environment:
    name: staging
At the top of .gitlab-ci.yml, in the stages section, add the review and deploy stages.

In full, you will have the following .gitlab-ci.yml file upon completion:

stages:
  - test
  - build
  - review
  - deploy

test job:
  stage: test
  script:
    - echo "I am a unit test!"

build job:
  stage: build
  script:
    - echo "I am a build image!"

deploy review:
  stage: review
  script:
    - echo "Do your average deploy here"
  rules:
    - if: '$CI_COMMIT_REF_NAME == "main"'
      when: never
    - if: '$CI_COMMIT_TAG'
      when: never
    - when: always
  environment:
    name: review/$CI_COMMIT_REF_NAME

deploy release:
  stage: deploy
  script:
    - echo "Deploy to a production environment"
  rules:
    - if: '$CI_COMMIT_REF_NAME != "main" && $CI_COMMIT_TAG'
      when: manual
  environment:
    name: production

deploy staging:
  stage: deploy
  script:
    - echo "Deploy to a staging environment"
  rules:
    - if: '$CI_COMMIT_REF_NAME == "main"'
      when: always
    - when: never
  environment:
    name: staging
In the Commit message field, type Add CI structure job definitions, ensure the Target Branch is set to main, and click Commit changes.

In the left-hand navigation pane, click Build > Pipelines and click the status icon next to the most recent pipeline run.

Click the widgets to see what environment the pipeline is deploying the code to. In the left sidebar, click Operate > Environments to see the environments that have been created.

Note: You will see that deploy staging is the only one of the three jobs that executed, based on the rules that were defined for each job.

Optional: Experiment with triggering pipelines using different branches and tags. Can you get different pipeline runs to execute the deploy release, deploy review, and deploy staging jobs?

Hint: Look at the rules keyword in the relevant .gitlab-ci.yml job definitions.


Solutions:
# Task B1: Running the deploy review Job:
Review the rules specified in the deploy review’s rules section. It will only run when A) The branch name (represented by $COMMIT_REF_NAME) is not equal to main, and B) there is no tag on the branch (represented by $COMMIT_REF_TAG).

Note: a variable used with an if keyword on its own is checking if said variable has any value associated with it. If it has any value, regardless of what that value is, the statement is true. This includes values that would be considered false in other programming languages, such as False. If there is no value, the statement is false. A variable with whitespace as its value is considered false as well.

Create a new branch by clicking on Code > Branches.

Click the New branch button.

Type Dev in the branch name section, and click Create branch

Click on Build > Pipelines.

Click on the Run Pipeline button.

Under Run for branch name or tag, make sure Dev is selected.

Click on the Run Pipeline button.

Your deploy review job should be the only job that should be running.


