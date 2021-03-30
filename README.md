# Continuous Integration (CI) configuration examples for MATLAB&reg;

Are you interested in automating your testing with CI?

Not sure how to connect MATLAB with CI systems?

We've got you covered!

This repository makes it easy to run your MATLAB tests on some of the most common CI platforms. The configuration files take care of setting up MATLAB to work with the CI system and automatically executing your MATLAB tests.

Take a look at the "Quick Start Guide" below to get started!


## Badges

Badges look really great, but they're not always easy to set up. Take a look at the badges and reference links below to get started with badges for your repository.

| **CI Platform** | **Badges** | **Badge Help** |
|:----------------|:-----------|:---------------|
| Azure&reg; DevOps | [![Azure DevOps Build Status](https://dev.azure.com/asifouna/Testing%20MATLAB%20CI/_apis/build/status/asifouna.matlab-ci-examples?branchName=main)](https://dev.azure.com/asifouna/Testing%20MATLAB%20CI/_build/latest?definitionId=1&branchName=main) ![Azure DevOps Coverage](https://img.shields.io/azure-devops/coverage/asifouna/Testing%20MATLAB%20CI/1/main) | [Blog with helpful information for setting up Azure DevOps badges](https://gregorsuttie.com/2019/03/20/azure-devops-add-your-build-status-badges-to-your-wiki/) |
| CircleCI&reg; | [![CircleCI Build Badge](https://circleci.com/gh/asifouna/matlab-ci-examples.svg?style=shield)](https://travis-ci.com/asifouna/matlab-ci-examples) | [CircleCI documentation for setting up badges](https://circleci.com/docs/2.0/status-badges/#generating-a-status-badge "CircleCI documentation for setting up badges") |
| GitHub&reg; Actions | [![MATLAB](https://github.com/asifouna/matlab-ci-examples/workflows/MATLAB/badge.svg)](https://github.com/asifouna/matlab-ci-examples/actions?query=workflow%3AMATLAB) | [GitHub Actions documentation for setting up badges](https://docs.github.com/en/actions/managing-workflow-runs/adding-a-workflow-status-badge) |
| Travis CI&trade; | [![Travis CI Build Status](https://travis-ci.com/asifouna/matlab-ci-examples.svg?branch=main)](https://travis-ci.com/asifouna/matlab-ci-examples) | [Travis CI documentation for setting up badges](https://docs.travis-ci.com/user/status-images/ "Travis CI documentation for setting up badges") |


## Quick start guide
Here's how to quickly get this repository running on a CI system:
1. Fork the repository to your own GitHub account
2. Go to one of the supported CI platforms and install the available MATLAB CI plugin.  
***Note:*** GitHub Actions does not require this step.
3. Create a new CI job using your forked repository.  
4. Enjoy using CI with MATLAB!

That's really it!

To use your MATLAB code or Simulink&reg; models with this repository, simply replace the existing code and tests in "main" and "test" with your own code and tests. There's no need to modify any of the CI configuration files because they are all completely agnostic of the MATLAB code being used.

***Note:*** In most cases, the configuration files will automatically get picked up by the CI platform during step 3, but some platforms may require you to specify which file to use.


## Supported CI platforms
* Azure DevOps
* CircleCI
* GitHub Actions
* Jenkins&trade;
* Travis CI


## About the code
The primary goal of this repository is to provide a set of configuration files as templates that illustrate how to run MATLAB on various CI platforms (e.g., Azure DevOps, CircleCI, GitHub Actions, Jenkins, Travis CI).

Each of these pipeline definitions does four things:

1. Install the latest MATLAB release on a Linux&reg;-based build agent
2. Run all MATLAB tests in the root of your repository, including its subfolders
3. Produce a test results report (if necessary)
4. Produce a code coverage report in Cobertura XML format for the source folder
   * Currently, only Azure DevOps supports code coverage directly
   * To see an example of using [Codecov](https://about.codecov.io/) to show coverage results, please refer to [https://github.com/mathworks/matlab-codecov-example](https://github.com/mathworks/matlab-codecov-example)

The example MATLAB code example `dayofyear.m` is a simple function takes a date string `"mm/dd/yyyy"` and returns the day-of-year number.

Notes:
* MATLAB already includes a day-of-year calculation using `day(d,"dayofyear")`, where `d` is a datetime object. This code is only used as an example since it is a concept that is familiar to most people.
* The code coverage is intentionally set below 100% to show how missing coverage looks with badges. Uncomment the last test in `TestExamples.m` to increase the coverage to 100%.

There are 2 test classes provided:
1. TestExamples.m - A simple set of equality and negative tests
2. ParameterizedTestExamples.m - A set of 12 equality tests set up using the parameterized test format

The repository includes these files:

| **File Path**              | **Description** |
|----------------------------|-----------------|
| [`main/dayofyear.m`](main/dayofyear.m) | The [`dayofyear`](main/dayofyear.m) function returns the day-of-year number for a given date string "mm/dd/yyyy" |
| [`test/TestExamples.m`](test/TestExamples.m) | The [`TestExamples`](test/TestExamples.m) class provides a few equality and negative tests for the [`dayofyear`](main/dayofyear.m) function |
| [`test/ParameterizedTestExample.m`](test/ParameterizedTestExample.m) | The [`ParameterizedTestExample`](test/ParameterizedTestExample.m) class provides 12 tests for the [`dayofyear`](main/dayofyear.m) function using the parameterized test format |
| [`azure-pipelines.yml`](azure-pipelines.yml) | The [`azure-pipelines.yml`](azure-pipelines.yml) file defines the pipeline that runs on [Azure DevOps](https://marketplace.visualstudio.com/items?itemName=MathWorks.matlab-azure-devops-extension). |
| [`.circleci/config.yml`](.circleci/config.yml) | The [`config.yml`](.circleci/config.yml) file defines the pipeline that runs on [CircleCI](https://circleci.com/orbs/registry/orb/mathworks/matlab) |
| [`.github/workflows/ci.yml`](.github/workflows/ci.yml) | The [`ci.yml`](.github/workflows/ci.yml) file defines the pipeline that runs on [GitHub Actions](https://github.com/matlab-actions/overview) |
| [`Jenkinsfile`](Jenkinsfile) | The [`Jenkinsfile`](Jenkinsfile) file defines the pipeline that runs on [Jenkins](https://plugins.jenkins.io/matlab/) |
| [`.travis.yml`](.travis.yml) | The [`.travis.yml`](.travis.yml) file defines the pipeline that runs on [Travis CI](https://docs.travis-ci.com/user/languages/matlab/) |


## CI configuration files

### Azure DevOps
```yml
pool:
  vmImage: Ubuntu 16.04
steps:
  - task: InstallMATLAB@0
  - task: RunMATLABTests@0
    inputs:
      sourceFolder: main
      codeCoverageCobertura: code-coverage/coverage.xml
      testResultsJUnit: test-results/results.xml
  - task: PublishTestResults@2
    inputs:
      testResultsFormat: 'JUnit'
      testResultsFiles: 'test-results/results.xml'
  - task: PublishCodeCoverageResults@1
    inputs:
      codeCoverageTool: 'Cobertura'
      summaryFileLocation: 'code-coverage/coverage.xml'
      pathToSources: 'main/'

  # As an alternative to RunMATLABTests, you can use RunMATLABCommand to execute a MATLAB script, function, or statement.
  # - task: RunMATLABCommand@0
  #   inputs:
  #     command: addpath('main'); results = runtests('IncludeSubfolders', true); assertSuccess(results);
```

### CircleCI
```yml
version: 2.1
orbs:
  matlab: mathworks/matlab@0
  codecov: codecov/codecov@1
jobs:
  build:
    machine:
      image: ubuntu-1604:201903-01
    steps:
      - checkout
      - matlab/install
      - matlab/run-tests:
          source-folder: main

      # As an alternative to run-tests, you can use run-command to execute a MATLAB script, function, or statement.
      # - matlab/run-command:
      #     command: addpath('main'); results = runtests('IncludeSubfolders', true); assertSuccess(results);
```

### GitHub Actions
```yml
# This is a basic workflow to help you get started with MATLAB Actions

name: MATLAB

# Controls when the action will run. 
on:
  # Triggers the workflow on push or pull request events but only for the main branch
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

jobs:
  # This workflow contains a single job called "build"
  build:
    # The type of runner that the job will run on
    runs-on: ubuntu-latest

    # Steps represent a sequence of tasks that will be executed as part of the job
    steps:
      # Checks-out your repository under $GITHUB_WORKSPACE, so your job can access it
      - uses: actions/checkout@v2
      
      # Sets up MATLAB on the GitHub Actions runner
      - name: Setup MATLAB
        uses: matlab-actions/setup-matlab@v0

      # Runs a set of commands using the runners shell
      - name: Run all tests
        uses: matlab-actions/run-tests@v0
        with:
          source-folder: main

      # As an alternative to run-tests, you can use run-command to execute a MATLAB script, function, or statement.
      #- name: Run all tests
      #  uses: matlab-actions/run-command@v0
      #  with:
      #    command: addpath('main'); results = runtests('IncludeSubfolders', true); assertSuccess(results);
```

### Jenkins
```groovy
pipeline {
  agent any
  stages {
    stage('Run MATLAB Tests') {
      steps {
        runMATLABTests(
          sourceFolder: 'main'
        )

        // As an alternative to runMATLABTests, you can use runMATLABCommand to execute a MATLAB script, function, or statement.
        // runMATLABCommand "addpath('main'); results = runtests('IncludeSubfolders', true); assertSuccess(results);"
      }
    }
  }
}
```

### Travis CI
```yml
language: matlab
script: matlab -batch "addpath('main'); results = runtests('IncludeSubfolders', true); assertSuccess(results);"
```

## Caveats
* MATLAB builds on Travis CI are available only for public projects.
* MATLAB builds on Azure DevOps, CircleCI, and GitHub Actions that use CI service-hosted agents are also available only for public projects. However, these integrations can also be used in private projects that leverage self-hosted runners/agents.


## Links
- [Continuous Integration with MATLAB and Simulink](https://www.mathworks.com/solutions/continuous-integration.html)


## Contact Us
If you have any questions or suggestions, please contact MathWorks at [continuous-integration@mathworks.com](mailto:continuous-integration@mathworks.com).
