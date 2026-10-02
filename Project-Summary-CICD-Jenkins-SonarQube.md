# CI/CD Pipeline With Jenkins, GitHub, Maven and SonarQube

## 1. Project Overview

This project was designed to build a CI/CD pipeline using Jenkins and SonarQube in a self-hosted Docker environment.

The main goal was to automate the software development process so that:

- code is pulled from GitHub
- the project is built automatically
- the Java project is compiled
- static code analysis is performed
- code quality results are displayed in SonarQube

This setup demonstrates how modern software teams automate testing, build verification, and code quality checks before deployment.

The pipeline includes:
- Jenkins for automation
- GitHub for source control
- Maven for building the Java project
- SonarQube for static code analysis
- Docker for running both Jenkins and SonarQube in isolated containers

## 2. Why This Project Was Important

This project is a very practical introduction to DevOps and CI/CD.

In real software teams, developers do not manually build and analyze code every time they change it. Instead, automation handles these tasks. This reduces:
- human errors
- inconsistent builds
- poor code quality
- delayed releases

This project helped me understand:
- how automated software delivery works
- how Jenkins triggers pipeline stages
- how code quality tools integrate into the workflow
- how Docker containers communicate with each other
- how credentials and environment variables work in automation tools
- how to debug a system that appears to be "almost working"

## 3. Tools Used

### Jenkins
Jenkins is a CI/CD automation server. It manages build pipelines and orchestrates the execution of build steps.

It was used to:
- check out source code
- run Maven build commands
- call SonarScanner
- ensure the pipeline is automated

### GitHub
GitHub was used as the source code repository. The project code was stored there and pulled into Jenkins during the pipeline run.

### Maven
Maven is a Java build tool. It handles:
- dependency management
- compilation of Java source code
- project build lifecycle

In my pipeline, Maven was used to run:
```bash
mvn clean compile
