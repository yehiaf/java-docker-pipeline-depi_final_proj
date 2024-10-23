# Java Docker Pipeline Project

This project demonstrates a Java application built and deployed using a Docker pipeline within a Jenkins environment. The final Docker image is hosted on Docker Hub: [Java App Pipeline Image](https://hub.docker.com/repository/docker/yehiaf/java-app-pipeline). 

## Prerequisites

* **Docker:** Install and run Docker from [here](https://www.docker.com/).
* **Java 11 or later:** Required JDK for the project.
* **Maven:** Install Maven from [here](https://maven.apache.org/).
* **Jenkins:** A Jenkins server is required to execute the CI/CD pipeline.

## Getting Started

1. **Set Up Your Virtual Machines:**
   * Install Ubuntu on Both VMs:
      - Download the Ubuntu ISO and create two VMs in VMware Workstation.
      - Install Ubuntu on both VMs.
   * Install Required Software:
      - On both VMs, update the package list and install the necessary packages:
      ```bash
      sudo apt update
      sudo apt install openjdk-17-jdk git docker.io
      ```
      
2. **Set Up Jenkins on VM1 (Jenkins Master):**
   * Add the Jenkins repository and install Jenkins:
      ```bash
      wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
      sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
      sudo apt update
      sudo apt install jenkins
      ```
   * Start Jenkins:
      - Start Jenkins and enable it to start on boot:
      ```bash
      sudo systemctl start jenkins
      sudo systemctl enable jenkins
      ```
   * Access Jenkins:  
      - Open a web browser and go to http://<VM1_IP>:8080.
      - Follow the setup instructions to unlock Jenkins and install suggested plugins.
      ![Screenshot 2024-10-05 014828](https://github.com/user-attachments/assets/a33f4e45-6970-4237-83f6-3794fa159f3a)

3. **Configure Jenkins Agent on VM2:**
   * Create a Jenkins User:
      - On VM2, create a user for Jenkins:
      ```bash
      sudo useradd -m -s /bin/bash
      sudo passwd jenkins
      ```
   * Install Java and Docker.
   * Set Up SSH Key:
      - Generate an SSH key on VM1 and copy it to VM2:
      ```bash
      ssh-keygen
      ssh-copy-id jenkins@<VM2_IP>
      ```
      ![2](https://github.com/user-attachments/assets/c0ec84dd-65fa-4baa-9718-2644770dcfaa)

   * Configure Jenkins Agent:
      - In Jenkins (VM1), add VM2 as a new node and configure it to connect via SSH using the Jenkins user.
      ![3](https://github.com/user-attachments/assets/4d2b7051-0b74-439b-ac08-61a633d5bfe3)

4. **Clone the Repository on VM1:**
   * Clone the Repository:
      - On VM1, clone the repository:
      ```bash
      https://github.com/omarMohamedo-o/depi_final_project.git
      ```

5. **Modify the Front Page:**
   * Navigate to the Views Directory:
      - Go to the views directory of the cloned repository:
      ```bash
      cd depi_final_project/views
      ```
   * Modify home.pug:
      - Edit home.pug to display your group code:
      ```bash
      vim home.pug
      ```
      ![4](https://github.com/user-attachments/assets/6def76f3-4376-4e04-8276-14dc500160a4)
      ![5](https://github.com/user-attachments/assets/0b58c599-628b-49cf-ba0e-fa0a5c1a8d48)

6. **Jenkins Setup**
   * In the root directory of the cloned repository, create a Jenkinsfile:
   ![6](https://github.com/user-attachments/assets/bcac393b-82b0-4ebe-af88-b07b40df636d)
   ![7](https://github.com/user-attachments/assets/b08ee11a-0348-413c-a26c-b15c318fb22f)
   * Configure Jenkins with necessary plugins (e.g., Pipeline, Docker).
   ![9](https://github.com/user-attachments/assets/52fbfe6c-a836-448a-8f7e-497f12dd8989)
   * Create a new Jenkins pipeline job.
   ![10](https://github.com/user-attachments/assets/39714d6d-f0be-47c9-8453-ab0f54646c38)
   * Point the job to the `Jenkinsfile` in this repository.
   ![11](https://github.com/user-attachments/assets/2c870589-a502-4607-a8f8-ced5a3097875)
   ![12](https://github.com/user-attachments/assets/1e457519-4439-4c0f-9647-00a537c80cf3)
   * Set up credentials for Docker Hub if you plan to push the image.
   ![8](https://github.com/user-attachments/assets/ae1fabc2-b459-4d9f-8165-231a3f4bcf02)


8. **Run the Jenkins Pipeline:**
   * Manually trigger a build in Jenkins.
   ![13](https://github.com/user-attachments/assets/19ce3a3a-f65c-4c1c-a746-0320933e98ed)
   * The pipeline will execute the steps defined in the `Jenkinsfile`.
   ![14](https://github.com/user-attachments/assets/49ceac10-2c30-4445-9577-4ff3c452ea43)
   ![15](https://github.com/user-attachments/assets/821068f3-fb80-4195-b44d-e93a33a9f131)
   * Monitor the stages to ensure everything runs smoothly.
   ![16](https://github.com/user-attachments/assets/6de2854b-0e89-4df9-bbd2-93a8be1b586b)

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
