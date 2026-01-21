# Git branching strategy

**Table Of Contents**

1. [Objective](#objective)
2. [Steps to be followed to contribute](#steps-to-be-followed-to-contribute)

## Objective

In order to work and develop in a highly collaborative and productive manner as a team we need to follow some standard strategy throughout all of our products or projects. For our project we have selected **Git Flow** as the branching strategy here are steps to be followed by the **developer** to contribute to the team's products.

### Steps to be followed to contribute

1. Clone the repo to your local using the command
    > git clone \<repo-link>
2. Switch to develop branch
    > git checkout develop
3. Checkout a new branch from develop branch using the following nomenclature
    * If working on  **feature** branch name: *feature/\<PBI-ID>-\<crisp-description>*
    * If working on  **bugfix** branch name: *bufix/\<PBI-ID>-\<crisp-description>*
    * If working on  **refactoring** branch name: *refactor/\<PBI-ID>-\<crisp-description>*
    > git checkout -b \<branch name>
4. Do the development on your branch.
5. Make sure unit test coverage is above 94%.
6. Add you changes 
    > git add .
7. Commit your changes 
    * Commit message should be written in the present continuous sense
    * Commit message should describe the functionality/business value of the code.
    > git commit -m \<commit-message>
9. Check your local develop branch is in sync with the origin develop *"HEAD should point to same commit id"*
    > git show-ref develop
8. Make sure to **rebase** your branch with develop branch 
    > git co develop
    > git pull origin develop
    > git co \<your-branch>
    > git rebase -i develop
9. Once you are done with committing and rebasing
    > git push origin develop
10. Once your changes are on **GitHub** raise a **PR** with a minimum of 2 reviewers from your team to review and approve.
11. Once the review is done and **PR** is approved one of the reviewers can proceed ahead of merging the PR 
12. Your code is in develop branch ready to be deployed and go for **CD** pipeline.