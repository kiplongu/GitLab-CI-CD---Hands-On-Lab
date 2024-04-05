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

