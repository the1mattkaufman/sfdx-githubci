# sfdx-githubci

## Disclaimer

This repository is being provided for free by me personally. Using this repository does not constitute a business relationship between us or with my employer or any other business or person with which I have a relationship. By using any part of this repository or derivitive of it, you are agreeing to not hold me responsibile for any harm which may occur as a result of such use. Similarly, any success you may have does not require compensation be paid to me.

## Introduction

This repository is a template for using Github actions for Continuous Integration with Salesforce using the Organization Development Model. This is only one example of how to use CI with Salesforce. Additional examples exist and your unique requirements will determine the unique configuration and process for your CI. Please do not follow this repo blindly. Instead use this as a way to learn more about Continuous Integration, GitHub, and application lifecycle best practices.

## Branches

The repo has the following branches:

- main: Source control for Production
- staging: Source control for the current release
- uat: Source control for your User Testing sandbox
- qa: Source control for your QA sandbox

Each branch corresponds to its own Salesforce environment.

The main branch contains the source of your Production org. Changes approved to main will be automatically deployed to your Production org. Changes should never be committed directly to any of these branches, they should only be merged as a result of an approved pull request.

## Development and Deployment Process

Developers should build new features and fixes (commonly referred to as _changes_) in new branches, ideally unique to the changes they are building. For example, if you were building a feature for a new field that stores Favorite Color, you may use a branch called _feature-favorite-color_.

Once the the metadata changes for our Favorite Color requirement is completed by the developer, the changes are committed to the _feature-favorite-color_ branch and a Pull Request is created to merge the changes into the **qa** branch. The changes are then reviewed and approved, merging them into the **qa** branch and initiating a GitHub action to deploy them to the **qa** sandbox.

After testing in the **qa** sandbox, another Pull Request is used to request just these changes be merged into the **uat** sandbox. Again these changes are reviewed and approved, at which point a GitHub action will deploy it to the **uat** sandbox.

After testing in the **uat** sandbox, another Pull Request is used to request just these changes be merged into the **staging** sandbox. Again these changes are reviewed and approved, at which point a GitHub action will deploy it to the **staging** sandbox.

Up to this point, features and fixes were merged independently of eachother. If we had another feature for a field to store Favorite Number, that would have required a unique developer branch and a unique set up Pull Requests.

At the end of your sprint or release cycle, a Pull Request is made to merge all the changes that have been made to **staging** since the last release into **main**.

This final Pull Request is reviewed and approved, at which point the **main** branch is updated and a GitHub Action will deploy the changes to your **Production** org.

## Actions

This repo contains some sample GitHub Actions that are automatically run by GitHub when certain events occur. These Actions are stored in .yml files within the .github/workflows directory. I have provided these as samples of working Actions. They are not intended to be used blindly as doing so could result in unintended consequences. However, if you are struggling with getting GitHub Actions to work with your Salesforce org, hopefully they will help.

## Branch Rules

In GitHub Branch Rules are configured via the UI, not the repo's contents. I highly recommend creating Branch Rules to protect your branches from direct commits and help ensure your process is followed.

## Secrets

The GitHub Actions in the .yml files reference multiple Secrets. Secrets are configured via the GitHub user interface and are not part of your repo's contents. This ensures that your Secrets stay secret! The purpose of the Secrets used by this project are to store information unique to each of your Salesforce environment. Each environment has a unique **sfdx_url** Secret which allows the action to authenticate into that Salesforce environment and a unique **sfdx_user** which corresponds to a System Administrator username for that environment.

You can obtain the unique **sfdx_url** for a Salesforce org as follows:

1. Using the sfdx cli, authenticate into the Salesforce environment.
2. Use the command: sfdx force:org:display -verbose -u <username>
3. The output will contain an Sfdx Auth Url that you will copy and paste into the Secret
