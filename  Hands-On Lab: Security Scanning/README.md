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


# Task C. Fix the Key Issues
In the sidebar, navigate to Code > Repository.

Click the run.py file.

On the run.py file, click the blue Edit dropdown and select Edit single file.

The scan showed that lines 1 and 4 contained a vulnerability.

# main.py Line 1
import subprocess
# main.py Line 4
subprocess.run(["ping", ip])
The problem is that these lines use user input to run a system command. In this situation, a user could craft an input that causes the application to execute an unintended command. To resolve this, you can either remove the command, or remove the user input from the command.

Remove the two lines from the code. This is what the file should look like now.

print("Attempting to connect to the server")
print("Application authentication was successful")
In the commit field, type Fix changes as suggested by SAST scan, leave the Target Branch set to main, and click Commit changes

Note: The SAST scan will need to run again in order to update the changes.

Once the pipeline has finished running, click Secure > Vulnerability Report in the left-handed navigation pane. In the Tool drop-down list, select SAST. Review to see if the security issues have been remediated.



