# Jenkins Lab 1

![Jenkins Logo](https://www.jenkins.io/images/logos/jenkins/jenkins.png)

## Table of Contents
- [Introduction](#introduction)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configure Jenkins](#configure-jenkins)
    - [Install Role-based Authorization Strategy Plugin](#install-role-based-authorization-strategy-plugin)
    - [Configure Role-Based Security](#configure-role-based-security)
    - [Create a Viewer Role](#create-a-viewer-role)
    - [Create and Assign the Viewer User](#create-and-assign-the-viewer-user)
- [Run Pipeline](#run-pipeline)
    - [Clone a Repository](#clone-a-repository)
    - [Build Dockerfile](#build-dockerfile)
    - [Push to DockerHub](#push-to-dockerhub)
- [Contributing](#contributing)
- [License](#license)

## Introduction
This project demonstrates how to configure Jenkins, create a viewer user with restricted access, and run a pipeline that clones a Git repository, builds a Docker image, and pushes it to DockerHub. This lab provides hands-on experience with Jenkins' integration with Docker, Git, and access control features.

## Prerequisites
Before starting this project, ensure you have the following tools installed:
- Jenkins (latest version)
- Docker
- Git
- A [DockerHub](https://hub.docker.com/) account

## Installation

1. **Install Jenkins**: Follow the [official Jenkins installation guide](https://www.jenkins.io/doc/book/installing/) for your operating system.
2. **Install Docker**: Visit [Docker's official site](https://docs.docker.com/get-docker/) and install Docker for your operating system.

## Configure Jenkins

This section outlines the steps to configure Jenkins and set up role-based security.

### 1. Install Role-based Authorization Strategy Plugin

1. Go to `Manage Jenkins` > `Manage Plugins`.
2. Under the `Available` tab, search for **Role-based Authorization Strategy**.
3. Select the plugin and click `Install without restart`.

### 2. Configure Role-Based Security

1. Navigate to `Manage Jenkins` > `Configure Global Security`.
2. Under the **Authorization** section, change the setting to **Role-Based Strategy**.
3. Save your changes.

### 3. Create a Viewer Role

1. Go to `Manage Jenkins` > `Manage and Assign Roles` (under the **Security** section).
2. In the **Manage Roles** tab:
    - Create a new role called `can-view`.
    - Assign this role the **Overall Read** permission.

### 4. Create and Assign the Viewer User

1. Navigate to `Manage Jenkins` > `Manage Users`.
2. Click `Create User` and fill in the required details to create a user called `viewer`.
3. Go back to `Manage and Assign Roles` > **Assign Roles** tab:
    - Assign the `can-view` role to the `viewer` user by selecting the appropriate checkboxes.

Now the `viewer` user has read-only access to Jenkins, allowing them to view builds and pipeline status without modifying any configurations.

## Run Pipeline

To run the Jenkins pipeline, follow these steps:

### 1. Clone a Repository

- Create a new Jenkins pipeline job.
- Add the following pipeline script to clone a Git repository:
    ```groovy
    pipeline {
        agent any
        stages {
            stage('Clone Repository') {
                steps {
                    git 'https://github.com/your-username/your-repo.git'
                }
            }
        }
    }
    ```

### 2. Build Dockerfile

- In the next stage of your pipeline, add steps to build a Docker image from the Dockerfile:
    ```groovy
    stage('Build Docker Image') {
        steps {
            script {
                dockerImage = docker.build('your-docker-image-name')
            }
        }
    }
    ```

### 3. Push to DockerHub

- Finally, push the Docker image to your DockerHub account:
    ```groovy
    stage('Push to DockerHub') {
        steps {
            script {
                docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-credentials') {
                    dockerImage.push('latest')
                }
            }
        }
    }
    ```

Ensure you have configured your DockerHub credentials in Jenkins by navigating to `Manage Jenkins` > `Manage Credentials`.

## Contributing
Contributions are welcome! Please submit a pull request or open an issue if you would like to contribute to this project.

## License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
