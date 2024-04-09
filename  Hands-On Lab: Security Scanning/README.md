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



