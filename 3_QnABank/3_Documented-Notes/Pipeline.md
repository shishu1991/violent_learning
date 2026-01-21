# Pipeline

## What is Pipeline?
It creates a faster and more precise way of combining the work of different people into one cohesive product.

## Major Teams involved in pipeline
 - **Dev Team** To implement the functionality and get the valid artifact ready.
 - **Test Team**  To test the artifact (result of the build) against the functionality and check for compliance or vulnerabilities. 
 - **Ops Team**  To deploy the tested and verified artifact on various environments.

## Understanding CI/CD in detail
The continuous integration/continuous delivery (CI/CD) pipeline is an agile DevOps workflow focused on a frequent and reliable software delivery process. The methodology is iterative, rather than linear, which allows teams to write code, integrate it, run tests, deliver releases and deploy changes to the software collaboratively and in real-time.

- **CI**  Continuous integration (CI) allows developers to work independently, creating their own coding “branch” to implement small changes. Instead of writing code independently and submitting to the master once a month. This development process lets teams submit code changes more frequently.

    **Note:** Team have to think and decide over GIT workflows.


- **CD**  Continuous delivery (CD) which puts the validated code changes made in continuous integration into select environments or code repositories. The goal of the continuous delivery pipeline stage is to deploy new code with minimal effort, but still allow a level of human oversight.
  
    **Note:** In continuous delivery, code automatically moves to production-like environments for further testing and quality assurance, and human intervention is required to move into production

- **CD**  Continuous deployment automatically releases code changes to end-users after passing a series of predefined tests, such as integration tests

    **Note:**  In continuous deployment, automation goes further. Once the code passes testing, the deployment to production happens automatically — there is no human approval needed.

Doing everything from development to deployment is what something we can call a pipeline but when you do it with **Automation** is what something that makes it more acceptable in Agile methodology.


## Phasis of pipeline
- Build: This phase is part of the continuous integration process and involves the creation and compiling of code. Teams build of source code collaboratively and integrate new code while quickly determining any issues or conflicts.
- Test: At this stage, teams test the code. Automated tests happen in both continuous delivery and deployment. These tests could include integration tests, unit tests, and regression tests.
- Deliver: Here, an approved codebase is sent to a production environment. This stage is automated in continuous deployment and is only automated in continuous delivery after developer approval.
- Deploy: Lastly, the changes are deployed and the final product moves into production. In continuous delivery, products or code are sent to repositories and then moved into production or deployment by human approval. In continuous deployment, this step is automated.

## How we implemented it?

We used GHA as CI/CD tool with GitHub as repository and CaaS as the deployment platform.
    
- Steps to create a pipeline
  - Create docker file for which will build teh image of your app.
  - Create a job using GHA which will build the artifact out of your code base pushed on the (Github) repo.
  - Create a job that will run and check the build for compliance, vulnerabilities and test failures.
  - Create a job to build the containerized image out of the codebase and artifact build by above jobs.
  - Write templates as per your env to deploy your containerized image that can be executed to run the image of your app.
  - create a job that will build the connection with your deployment environments and execute the above templates.
  
    
