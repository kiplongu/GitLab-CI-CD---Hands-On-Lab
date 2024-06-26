# GitLab CI/CD - Hands-On Lab: Defining Stages, Jobs, and Runners
This Hands-On Guide walks you through the lab exercises in the GitLab CI/CD course.
Estimate time to complete: 15 - 20 minutes

We are transitioning to the latest version of this course. If your group URL starts with https://spt.gitlabtraining.cloud, please use the Version 15.x instructions.

Objectives
In this lab, you’ll enabled CI/CD for a GitLab project. After creating your first .gitlab-ci.yml file, you will explore the CI/CD pipeline to better understand jobs and stages. Finally, you will learn how to install, run, and register a runner with a GitLab instance.

Note: Parts D through F in this exercise require admin rights to your local machine. If you are unable to install GitLab Runner locally, you may skip parts D through F and use the training environment’s shared runners instead.

# Task A. Log into GitLab and create a project
Navigate to https://gitlabdemo.com/invite in a web browser.

In the Invitation Code field, enter the invitation code provided by your instructor or in the LevelUp LMS.

Select Provision Training Environment.

The system then prompts you for your GitLab.com username. Enter your GitLab.com user in the field provided. Click Provision Training Environment.

On the confirmation page, locate the Your GitLab Credentials section. Read this section carefully, noting the credential information provided and the expiration date. Your access to this group and all of the projects that you create is ephemeral and will be deleted after the expiration date.

Select My Group at the bottom of the page.

Sign in with your existing GitLab.com credentials.

You will be redirected to a My Test Group group that provides a sandbox for you to perform training lab steps in.

Note: This group has a GitLab Ultimate license to see all of the features while your personal username namespace requires a paid subscription or a free trial to access all of the features.

From the My Test Group training subgroup, click the New project button.

Click the Create blank project tile.

In the Project name text box, enter CICD Demo.

Note: The project slug will automatically populate. You can change this to a shorter string if desired for your own project. Leave it at the default for this lab.

In the Project URL field, click the dropdown for the second half of the URL to make sure it’s pointing to a group name (starts with gitlab-learn-labs/*) and not a username. You should create this project inside a group, not directly in your user’s namespace.

Under Visibility Level, ensure Private is selected.

Note: Since the parent group above your group is private, all child groups and projects below will be private. You can learn more about project visibility levels in the documentation.

Check Initialize repository with a README.

Ensure that the Enable Static Application Security Testing (SAST) checkbox remains unchecked.

Click the Create project button.

# Task B. Add a .gitlab-ci.yml File
To use GitLab CI/CD, you start with a .gitlab-ci.yml file at the root of your project. The .gitlab-ci.yml file contains the configurations for your CI/CD pipeline. In this section, you will create a simple gitlab-ci.yml file.

Create a new file in the main branch by clicking (+) > This directory > New file.

In the Filename field, type .gitlab-ci.yml

Select the Bash template from the Apply a template dropdown. This will pre-populate the file.

To create a minimal .gitlab-ci.yml file:

Delete all lines above build1.
Delete all lines below echo "For example run a test suite" in the test1 section.
Add build and test stages by pasting these lines at the top of the file.

stages:
  - build
  - test
Note: Keep in mind that YAML files should be indented with two spaces. Your web IDE may try to use a tab with 4 spaces. Simply use the backspace to set 2 spaces if you are not copying and pasting the examples.

Set the commit message to Creating a simple .gitlab-ci.yml file, and set the Target Branch to main.

Click Commit changes.

After committing the changes, you will have a .gitlab-ci.yml that looks like this:

stages:
- build
- test

build1:
  stage: build
  script:
    - echo "Do your build here"

test1:
    stage: test
    script:
        - echo "Do a test here"
        - echo "For example run a test suite"
This file defines two stages: build and test. The build1 job executes during the build stage, running all of the commands in script. The test1 job executes during the test stage, running all of the commands in script.


# Task C. View a Pipeline’s Status, Stages, Jobs, and GitLab Runner
When you commit your .gitlab-ci.yml file, a pipeline is created. A pipeline comprises of jobs and stages. In the previous section, you defined two stages: build and test. Each of these stages contained jobs, which were defined in script. In this section, you will view the pipeline created from your .gitlab-ci.yml file.

In the left navigation pane, click Build > Pipelines to see an overview of all pipelines. The top row in the overview shows the pipeline that started a few seconds ago, when you committed .gitlab-ci.yml. The status icon at the left of the row should say either running or passed.

Click the status icon of the top row to see the details of the most recent pipeline. You’ll see columns representing the pipeline’s stages, and widgets representing jobs within each stage.

Note: The order of execution for stages generally reads left to right. In this example, the build stage is the leftmost column, since it is the first stage to execute.

Click each of the two jobs to see the output in a web terminal. Identify the gitlab-runner for each job

Hint: it’s listed near the top of each job’s output.

![alt text](image.png)



# Task D. Prepare to Install GitLab Runner Locally
Jobs are executed by runners. If your project is hosted on GitLab.com, various SaaS runners are provided to build, test, and deploy your application on different environments. In some cases, you may want to host your own runners. Sections D, E, and F will outline the process of installing and registering a runner on your GitLab instance.

Depending on which OS you’re on, run the appropriate command(s):

In a Linux terminal:
sudo gitlab-runner status
In a macOS terminal:
gitlab-runner status
In a Windows PowerShell window:
cd C:\GitLab-Runner
./gitlab-runner.exe status
If the command gives an output like: gitlab-runner: Service is running, then you already have a runner installed on your system. If a runner is already installed on your system, skip to Part F below. If the command throws an error, continue with the next section.



# Task E. Install the GitLab Runner Binary on your Computer
This section outlines the steps required to install a GitLab runner on your computer. Follow only the instructions that match the operating system you’re using.

Linux
Follow steps 1 and 2 only in this documentation.

Verify that the gitlab-runner service has started by running this command:

sudo gitlab-runner status
If you see Service is running in the output, the gitlab-runner service is working as expected.

macOS
Follow steps 1 and 2 only in this documentation.

Install gitlab-runner as a service and start the service:

cd ~
gitlab-runner install
gitlab-runner start
Verify that the gitlab-runner service has started by running this command:

gitlab-runner status
If you see Service is running in the output, the gitlab-runner service is working as expected.

Windows
Follow steps 1 and 2 only in this documentation.

Open an elevated PowerShell window:

Click Start.

Type PowerShell

Right-click Windows PowerShell.

Click Run as administrator.

From the elevated PowerShell window, install and start the gitlab-runner service:

cd C:\GitLab-Runner
./gitlab-runner.exe install
./gitlab-runner.exe start
Verify that the gitlab-runner service has started by running this command:

./gitlab-runner.exe status
If you see Service is running in the output, the gitlab-runner service is working as expected.


# Task F. Register a Specific Runner Dedicated to your Project
At this point, you have installed a runner on your system. To allow GitLab to use this runner for CI/CD jobs, you need to register the runner in the UI.

In your CICD Demo project, in the left navigation pane, click Settings > CI/CD.

Scroll down to the Runners section. Click the Expand button next to it.

Under the Project runners section, click the New project runner button.

Select your operating system (Linux, MacOS, or Windows).

Under Tags, select Run untagged jobs. Leave the rest of the options blank.

An untagged runner will run any jobs. To control the jobs that a runner can run, you can define tags for the runner. To learn more about this process, click here

Click the Create runner button.

From the next page presented, copy the command under Step 1 to your clipboard.

Back in your terminal, paste and run the command you copied in the previous step. Press Enter for the instance URL and Enter for the runner name to use the default values. (You can give this a custom name if desired).

When prompted for the executor, enter shell

Confirm that your gitlab-runner registered correctly by running the appropriate command(s) for your OS:

In a Linux terminal:

sudo gitlab-runner list
In a macOS terminal:

gitlab-runner list
In a normal (not elevated) Windows PowerShell window:

cd C:\GitLab-Runner
./gitlab-runner.exe list
Note: If your runner is registered correctly, you should see an output like this:
gitlab-runner run Executor=shell Token=your-gl-token URL=https://gitlab.com

If you’re on Windows, follow these additional instructions to configure your gitlab-runner to use the right command to start PowerShell:
Open C:\GitLab-Runner\config.toml in a text editor.

Change this line:

shell = "pwsh"
to this:

shell = "powershell"
Save the file.





