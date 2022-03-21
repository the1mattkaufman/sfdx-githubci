# sfdx-githubci

## Introduction

This repository is a template for using Github actions for Continuous Integration with Salesforce using the Organization Development Model. This is only one example of how use CI with Salesforce. Additional examples exist and your unique requirements will determine the unique configuration and process for your CI.

## Branches

The main branch contains the source of your Production org. Changes approved to main will be automatically deployed to your Production org. Changes should not be committed directly to main, they should only be merged as a result of an approved pull request.

Staging represents the state you want Production to be in upon your next release.

The repo has the following branches:

- main: Source control for Production
- staging: Source control for the current release
- uat: Source control for your User Testing sandbox
- qa: Source control for your QA sandbox

The main branch contains the source of your Production org. The staging, uat, and qa branches each correspond to their own sandbox (ideally of the same name). **Changes should never be made directly to any of these 4 branches!**

## Development and Deployment Process

Developers should build new features and fixes (commonly referred to as _changes_) in new branches, ideally unique to the changes they are building. For example, if you were building a feature for a new field that stores Favorite Color, you may use a branch called _feature/favorite-color_.

Once the the metadata changes for our Favorite Color requirement is completed by the developer, the changes are committed to the _feature/favorite-color_ branch and a Pull Request is created to merge the changes into the **qa** branch. The changes are then reviewed and approved, merging them into the **qa** branch and initiating a GitHub action to deploy them to the **qa** sandbox.

After testing in the **qa** sandbox, another Pull Request is used to request just these changes be merged into the **uat** sandbox. Again these changes are reviewed and approved, at which point a GitHub action will deploy it to the **uat** sandbox.

After testing in the **uat** sandbox, another Pull Request is used to request just these changes be merged into the **staging** sandbox. Again these changes are reviewed and approved, at which point a GitHub action will deploy it to the **staging** sandbox.

Up to this point, features and fixes were merged independently of eachother. If we had another feature for a field to store Favorite Number, that would have required a unique developer branch and a unique set up Pull Requests.

At the end of your sprint or release cycle, a Pull Request is made to merge all the changes that have been made to **staging** since the last release into **main**.

This final Pull Request is reviewed and approved, at which point the **main** branch is updated and a GitHub Action will deploy the changes to your **Production** org.
