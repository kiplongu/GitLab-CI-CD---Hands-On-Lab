# GitLab CI/CD - Hands-On Lab: Security Scanning
This Hands-On Guide walks you through the lab exercises in the GitLab CI/CD course.
Estimate time to complete: 15 - 20 minutes

We are transitioning to the latest version of this course. If your group URL starts with https://spt.gitlabtraining.cloud, please use the Version 15.x instructions.

Objectives
SAST, an optional feature on CI/CD pipelines, analyzes your source code for known vulnerabilities. GitLab’s Vulnerability Report then shows any old or new vulnerabilities found with each pipeline run. In this lab, you will learn the process of enabling SAST scans in your CI/CD pipelines. You can learn more about SAST scanner by clicking here.


# Task A. Creating a Test File
Open your CICD Demo project from previous labs.

Near the top left, to the right of the branch dropdown, click (+) > This directory > New file.

Type run.py as the name of the file.

Copy and paste the following code into the body:

import subprocess

ip = input("Enter your server ip: ")
subprocess.run(["ping", ip])

print("Attempting to connect to the server")
print("Application authentication was successful")
Type Create Run.py as a test file into the commit message.

Set the Target branch to main.

Click Commit changes.

Note: This file has a command injection vulnerability, which can lead to security breaches. We are going to use the SAST Scanner to detect issues in our code.


# Task B. Creating and Running a SAST Scan
Open your CICD Demo project from previous labs.

Click on your .gitlab-ci.yml file to view its contents.

Click Edit > Edit single file. Paste the following snippet at the end of the file.

include:
  - template: Jobs/SAST.gitlab-ci.yml
In the Commit message field, type Enable SAST, leave the Target Branch set to main, and click Commit changes.

Navigate to the pipeline that was started by this change and click the semgrep-sast job to ensure that it’s running.

Note: It might take a minute or two for the Build stage to complete first.

To view the results of the SAST scan, click Secure > Vulnerability Report in the left-hand navigation pane. In the Tool drop-down list, select SAST. Click on any vulnerabilities to learn more about them.



