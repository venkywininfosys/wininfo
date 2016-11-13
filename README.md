# DevSKiller programming task sample - Java with Maven

## Introduction

With [DevSKiller.com](https://devskiller.com) you can assess candidates' programming skills during your recruitment process. Programming tasks are the best way to test candidates programming skills. The candidate is asked to modify source code of an existing project.

During the test, the candidate is allowed to edit the source code of the project with our browser-based code editor and can build the project inside the browser at any time. Candidate can also download the project code and edit it locally with the favorite IDE.

Check out how the test looks from candidate's perspective: [Candidate campaign preview](https://www.youtube.com/watch?v=rB4fViXPh5E)


This repo contains an example project for Java with Maven, below you can find a detailed guide for creating your own programming project. 

Please make sure to read our [Getting started with programming projects](https://docs.devskiller.com/programming_tasks/index.html) guides first 

## Technical details for Java with Maven support

Any **Maven** project might be used as a programming task. You can use any testing framework supported by Maven, this sample project uses JUnit for running unit tests.

Maven build will be executed with following command:

```sh
mvn clean test
```

## Automatic assessment

It is possible to automatically assess solution posted by the candidate. Automatic assessment is based on Unit Tests results and Code Quality measurements. 

All unit tests that are executed during the build will be detected by the DevSKiller platform. 

There are two kinds of unit tests:

1. **Candidate tests** - unit tests that are visible for the candidate during the test. Should be used to do only the basic verification and help the candidate to understand the requirements. Candidate tests WILL NOT be used to calculate the final score.
2. **Verification tests** - unit tests that are hidden from the candidate during the test. Files containing verification tests will be added to the project after the candidate finishes the test and will be executed during verification phase. Verification tests result will be used to calculate the final score.

After candidate finishes the test, our platform builds the project posted by the candidate and executes verification tests and static code analysis.

## DevSKiller project descriptor

Programming task can be configured with the DevSKiller project descriptor file. Just create a `devskiller.json` file and place it in the root directory of your project. Here is an example project descriptor:

```json
{
  "readOnlyFiles" : [ "pom.xml" ],
  "hiddenFiles" : [ "hidden_file.txt" ],
  "verification" : {
    "testNamePatterns" : [".*verify_pack.*"],
    "pathPatterns" : ["**src/test/**/verify_pack**"]
  }
}
```

You can find more details about `devskiller.json` descriptor in our [documentation](https://docs.devskiller.com/programming_tasks/project_descriptor.html)

## Automatic verification with verification tests

To enable automatic verification of candidates' solution, you need to define which tests should be treated as verification tests.

All files classified as verification tests will be removed from a project prepared for the candidate.

To define verification tests, you need to set two configuration properties in `devskiller.json` project descriptor:

- `testNamePatterns` - an array of RegEx patterns which should match all the test names of verification tests. 
Test name contains: `[package_name].[Class_name].[method_name]`. In our sample project all verification tests are in `verify_pack` package, so the following pattern will be sufficient:

```json
"testNamePatterns" : [".*verify_pack.*"]
```

- `pathPatterns` - an array of GLOB patterns which should match all the files containing verification tests. All the files that match defined patterns will be deleted from candidates' projects and will be added to the projects during the verification phase. 

```json
"pathPatterns" : ["**src/test/**/verify_pack**"]
```

