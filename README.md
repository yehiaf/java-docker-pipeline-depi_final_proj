# Java Docker Pipeline Project

This project demonstrates a Java application built and deployed using a Docker pipeline within a Jenkins environment. The final Docker image is hosted on Docker Hub: [Java App Pipeline Image](https://hub.docker.com/repository/docker/yehiaf/java-app-pipeline). 

## Prerequisites

* **Docker:** Install and run Docker from [here](https://www.docker.com/).
* **Java 11 or later:** Required JDK for the project.
* **Maven:** Install Maven from [here](https://maven.apache.org/).
* **Jenkins:** A Jenkins server is required to execute the CI/CD pipeline.

## Getting Started

1. **Clone the repository:**

   ```bash
   git clone https://github.com/yehiaf/java-docker-pipeline-depi_final_proj.git
   cd java-docker-pipeline-depi_final_proj
   ```

2. **Jenkins Setup:**

   * Install necessary plugins (Pipeline, Docker).
   * Create a new pipeline job in Jenkins.
   * Link it to the `Jenkinsfile` in this repository.
   * Set up Docker Hub credentials if pushing images.

3. **Run the Pipeline:**

   * Trigger the pipeline in Jenkins manually.
   * The pipeline will build, test, and push the Docker image to Docker Hub.

## Project Structure

* **src/main/java/com/example:** Java source code.
* **src/main/resources:** Configuration files.
* **Dockerfile:** Instructions for Docker image creation.
* **Jenkinsfile:** Defines pipeline stages for Jenkins.

## Pipeline Stages

* **Build:** Compile the Java code into a JAR file.
* **Test:** Optionally run unit or integration tests.
* **Docker Build:** Build the Docker image using the `Dockerfile`.
* **Docker Push:** Push the built image to Docker Hub.
* **Deploy:** Optionally define deployment steps (currently omitted).

## Customization

* **Application Code:** Modify Java files in `src` to change app functionality.
* **Dockerfile:** Adjust base image, dependencies, or installation steps as needed.
* **Jenkinsfile:** Tailor pipeline stages to fit your specific CI/CD workflow.

## Challenges and Resolutions

* **JAVA_HOME Issue on Jenkins Agent:**

   * **Issue:** `UnsupportedClassVersionError` due to incorrect `JAVA_HOME` setting on the Jenkins agent.
   * **Solution:** Correct `JAVA_HOME` in Jenkins agent configuration to point to the JDK 11+ directory.

* **Docker Hub Credentials in Jenkins:**

   * **Issue:** Pipeline failed to push the image due to missing credentials.
   * **Solution:** Add Docker Hub credentials to Jenkins and configure the job to use them via the `withCredentials` block.

## Docker Hub Image

You can find the final Docker image on Docker Hub: [Java App Pipeline](https://hub.docker.com/repository/docker/yehiaf/java-app-pipeline).

## Contributing

Contributions are welcome! Feel free to open issues or submit pull requests.