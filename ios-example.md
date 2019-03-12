This guide shows you how to use Semaphore to set up a continuous integration
(CI) pipeline for a iOS applications.

* [Demo project](#demo-project)
* [Overview of the CI pipeline](#overview-of-the-ci-pipeline)
* [Sample configuration](#sample-configuration)
* [Run the demo iOS app project yourself](#run-the-demo-laravel-project-yourself)
* [See also](#see-also)

## Demo project

Semaphore maintains an example iOS project:

- [Demo iOS project on GitHub][demo-project]

In the repository you will find an annotated Semaphore configuration file
`.semaphore/semaphore.yml`.

The application uses the latest stable version of Xcode and Fastlane.

## Overview of the CI pipeline

The demo iOS CI pipeline performs the following tasks:

- Runs unit and UI tests
- Builds example app
  - Creates temporary keychain
  - Builds app without code signing

The CI pipeline is composed of a one test block and one build block:

![iOS example CI pipeline](https://github.com/semaphoreci-demos/semaphore-demo-ios-example/raw/master/public/ci-pipeline.png)

## Sample configuration

```yml
# .semaphore/semaphore.yml
# Use the latest stable version of Semaphore 2.0 YML syntax:
version: v1.0

# Name your pipeline. In case you connect multiple pipelines with promotions,
# the name will help you differentiate between, for example, a CI build phase
# and delivery phases.
name: Semaphore iOS example with Fastlane

# An agent defines the environment in which your code runs.
# It is a combination of one of available machine types and operating
# system images.
# See https://docs.semaphoreci.com/article/20-machine-types
# and https://docs.semaphoreci.com/article/120-macos-mojave-image
agent:
  machine:
    type: a1-standard-2
    os_image: macos-mojave

# Blocks are the heart of a pipeline and are executed sequentially.
# Each block has a task that defines one or more jobs. Jobs define the
# commands to execute.
# See https://docs.semaphoreci.com/article/62-concepts
blocks:
  - name: Run tests
    task:
      env_vars:
        - name: LANG
          value: en_US.UTF-8
      prologue:
        commands:
          - checkout
          - bundle install --path vendor/bundle
      jobs:
        - name: Fastlane test
          commands:
            # Select xcode version for available versions
            # See https://docs.semaphoreci.com/article/120-macos-mojave-image
            - bundle exec xcversion select 10.1
            # Run tests of iOS and Mac app on a simulator or connected device
            # See https://docs.fastlane.tools/actions/scan/
            - bundle exec fastlane ios test

  - name: Build app
    task:
      env_vars:
        - name: LANG
          value: en_US.UTF-8
      prologue:
        commands:
          - checkout
          - bundle install --path vendor/bundle
      jobs:
        - name: Fastlane build
          commands:
            # Select xcode version for available versions
            # See https://docs.semaphoreci.com/article/120-macos-mojave-image
            - bundle exec xcversion select 10.1
            # Create a temporary keychain
            # See https://docs.fastlane.tools/codesigning/getting-started/
            # and https://docs.fastlane.tools/actions/create_keychain/
            - bundle exec fastlane certificates refresh_certificates:true
            # Gym builds and packages iOS apps.
            # See https://docs.fastlane.tools/actions/build_app/
            - bundle exec fastlane build use_temporary_keychain:true
```

## Run the demo iOS app project yourself

A good way to start using Semaphore is to take a demo project and run it
yourself. Here’s how to build the demo project with your own account:

1. [Fork the project on GitHub][demo-project] to your own account.
2. Clone the repository on your local machine.
3. In Semaphore, follow the link in the sidebar to create a new project.
   Follow the instructions to install sem CLI, connect it to your
   organization.
4. Run `sem init` inside your repository.
5. Edit the .semaphore/semaphore.yml file and make a commit. When you push a
   commit to GitHub, Semaphore will run the CI pipeline.

## See also

- [Semaphore guided tour][guided-tour]
- [Pipelines reference][pipelines-ref]

[demo-project]: https://github.com/semaphoreci-demos/semaphore-demo-ios-example
[pipelines-ref]: https://docs.semaphoreci.com/article/50-pipeline-yaml
