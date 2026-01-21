# Github Actions

**Table Of Contents**

1. [Objective](#objective)
2. [Components of GHA](#components-of-gha)
3. [Sample workflow](#sample-workflow-file)

## Objective

GitHub Actions is a continuous integration and continuous delivery (CI/CD) platform that allows you to automate your build, test, and deployment pipeline. You can create workflows that build and test every pull request to your repository, or deploy merged pull requests to production.

For details [refer](https://docs.github.com/en/enterprise-cloud@latest/actions/learn-github-actions/understanding-github-actions)

## Components of GHA

Through GHA we try to create **Workflows** which can be triggered by **Event** to execute a **Job** which consists of **Steps** on a well-defined **Runner**.

Workflow is defined by a \<workflow-name>.yml file
Inside the yml file, we define Events and Job(s)  with a Runner to fulfill a workflow.

* Event occurs when: 
    - Pull request is raised on a branch,
    - Push happens on your branch
    - Manually(workflow_dispatch) want to run a job

* Job: A step-by-step task to be carried out be carried out.
    - **JOB**: It is a task to be executed for example to build a service, build an image, execute tests etc.
        * name: \<task-name>
    - **Runner**: Every Job has to define a runner a platform where the job or steps of the job will be executed. It is defined by the value of runs-on
        * runs-on: \<runner-name(eg: ubuntu-latest)>
    - **Steps**: These are actual steps to be executed in declared order in order to mark the job as completed. Each step has its name and action (can be defined by uses or run)
        * name: a name for your step eg: \<Checkout Source>
        * uses: action to be executed in the step eg: \<actions/checkout@v3>
        * run: commands or instructions to be executed for your runner

    **NOTE:** Always create your GHA under the \<your-git-repo>/.github/workflows/<your-workflows>.yml and commit to your main branch with basic workflow.

## Sample workflow file
```
name: CI

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3

      - name: Run a one-line script
        run: echo Hello, world!

      - name: Run a multi-line script
        run: |
          echo Add other actions to build,
          echo test, and deploy your project.
```

