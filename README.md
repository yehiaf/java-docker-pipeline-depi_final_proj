# Java Docker Pipeline Project

This project demonstrates a simple Java application built and deployed using a Docker pipeline within a Jenkins environment.

## Prerequisites

* **Docker:** Ensure you have Docker installed and running on your system. You can download it from [https://www.docker.com/](https://www.docker.com/).
* **Java Development Kit (JDK):** This project requires Java 11 or later.
* **Maven:** We use Maven as our build tool. If you don't have it, download and install it from [https://maven.apache.org/](https://maven.apache.org/).
* **Jenkins:** You need a running Jenkins server to execute the pipeline.

## Getting Started

1. **Clone the repository:**

   ```bash
   git clone https://github.com/yehiaf/java-docker-pipeline-depi_final_proj.git
   cd java-docker-pipeline-depi_final_proj
   ```

2. **Jenkins Setup**
   * Configure Jenkins with necessary plugins (e.g., Pipeline, Docker).
   * Create a new Jenkins pipeline job.
   * Point the job to the `Jenkinsfile` in this repository.
   * Set up credentials for Docker Hub if you plan to push the image.

3. **Run the Jenkins Pipeline:**
   * Manually trigger a build in Jenkins.
   * The pipeline will execute the steps defined in the `Jenkinsfile`.

## Project Structure

* **src/main/java/com/example:** Contains the Java source code for the application.
* **src/main/resources:** Holds resource files, such as configuration files.
* **Dockerfile:** Contains instructions for building the Docker image.
* **Jenkinsfile:** Defines the Jenkins pipeline stages for building, testing, and deploying the application. 

## Pipeline Stages (Jenkinsfile)

The `Jenkinsfile` outlines the following pipeline stages:

1. **Build:** Compile the Java code and package it into a JAR file.
2. **Test:** (Optional) Run unit or integration tests.
3. **Docker Build:** Use the `Dockerfile` to build a Docker image of the application.
4. **Docker Push:** Push the Docker image to Docker Hub (or your preferred registry).
5. **Deploy:** (Optional)  Define steps to deploy the image to a target environment (not included in this example).

## Customization

* **Application Code:** Modify the Java code in the `src` directory to change the application's behavior.
* **Dockerfile:** Adjust the `Dockerfile` to install additional dependencies, change the base image, or customize the image creation process.
* **Jenkinsfile:** Adapt the pipeline stages in the `Jenkinsfile` to fit your specific workflow and environment. 

## Challenges and Resolutions

1. **Incorrect JAVA_HOME Environment Variable on Jenkins Agent:**

   * **Challenge:** The Jenkins build failed with an error message indicating a connection termination and an `UnsupportedClassVersionError` while trying to load slave classes. This suggested a Java version mismatch between the Jenkins controller and the agent. The agent logs revealed that the `JAVA_HOME` environment variable was not set correctly, preventing the agent from using the correct Java version. 
   * **Resolution:**  We resolved this by ensuring that the `JAVA_HOME` environment variable was set correctly on the Jenkins agent:
      * We accessed the Jenkins agent configuration (either through the agent's configuration page or the global agent configuration).
      * We located the `JAVA_HOME` environment variable setting.
      * We verified that the `JAVA_HOME` variable pointed to the installation directory of the desired JDK (Java 11 or later in this case).
      * After correcting the `JAVA_HOME` path, we restarted the Jenkins agent for the changes to take effect. 

2. **Docker Hub Credentials in Jenkins:** 

   * **Challenge:** The Jenkins pipeline initially failed to push the Docker image to Docker Hub due to missing credentials. The error message indicated a `MissingPropertyException` related to the `docker` property.
   * **Resolution:** We resolved this by adding the Docker Hub credentials to Jenkins. This involved:
      * Navigating to the Jenkins Credentials Manager.
      * Creating a new "Secret text" credential to store the Docker Hub username and password.
      * Configuring the Jenkins pipeline job to use these credentials when interacting with Docker Hub (typically within the `withCredentials` block in the `Jenkinsfile`).
