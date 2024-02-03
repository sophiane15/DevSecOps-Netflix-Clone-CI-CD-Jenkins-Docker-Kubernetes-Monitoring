# DevSecOps:Netflix-Clone-CI/CD-Jenkins-Docker-Kubernetes-Monitoring`



## Project Overview:

 We will be deploying a Netflix clone. We will be using Jenkins as a CICD tool and deploying 
our application on a Docker container and we will monitor the Jenkins using Grafana, 
Prometheus and Node exporter. I Hope this detailed blog is useful.

![0_LJCPH1wHCU7oL6-F](https://github.com/sophiane15/DevSecOps-Netflix-Clone-CI-CD-Jenkins-Docker-Kubernetes-Monitoring/assets/153296722/5fa971e6-531f-41db-9e0d-bba2db39871c)







## Project Steps:

###### Step 1 — Launch an Ubuntu(22.04) T2 Large Instance:

<img width="1156" alt="Screenshot 2024-02-03 at 13 14 54" src="https://github.com/sophiane15/DevSecOps-Netflix-Clone-CI-CD-Jenkins-Docker-Kubernetes-Monitoring/assets/153296722/4455610c-9f24-4182-9248-885ded41e30d">


###### Step 2 — Install Jenkins, Docker and Trivy. Create a Sonarqube Container using Docker:

            -> Update data :sudo apt update -y
            -> clone the repository from Github: git clone https://github.com/sophiane15/DevSecOps-Netflix-Clone-CI-CD-Jenkins-Docker-Kubernetes-Monitoring.git
            -> install docker: 
                                      sudo apt-get update
                                      sudo apt-get install docker.io -y   #Install Docker
                                      sudo usermod -aG docker $USER  #Add your user to the docker group
                                      newgrp docker
                                      sudo chmod 777 /var/run/docker.sock
          -> build docker image following dockerfile: docker build -t netflix .
          -> run docker image : docker run -d --name netflix -p 8081:80 netflix:latest
                                "to remove docker : 
                                              docker stop <containerid>
                                              docker rm -f netflix
          ->Install Jenkins into EC2 instance:
                                    #Install Java:
                                    sudo apt update
                                    sudo apt install fontconfig openjdk-17-jre
                                    java -version
                                    openjdk version "17.0.8" 2023-07-18
                                    OpenJDK Runtime Environment (build 17.0.8+7-Debian-1deb12u1)
                                    OpenJDK 64-Bit Server VM (build 17.0.8+7-Debian-1deb12u1, mixed mode, sharing)

                                    #jenkins:
                                    sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
                                    https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
                                    echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
                                    https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
                                    /etc/apt/sources.list.d/jenkins.list > /dev/null
                                    sudo apt-get update
                                    sudo apt-get install jenkins
                                    sudo systemctl start jenkins
                                    sudo systemctl enable jenkins
                                    #access to jenkins from browser : public-IP:8080
                                    
###### Step 3 — Create a TMDB API Key:

        -> get API from TMDB website
        -> recreate the Docker image with TMDB api key: docker build --build-arg TMDB_V3_API_KEY= <your-api-key> -t netflix .

<img width="1430" alt="Screenshot 2024-02-03 at 15 49 10" src="https://github.com/sophiane15/DevSecOps-Netflix-Clone-CI-CD-Jenkins-Docker-Kubernetes-Monitoring/assets/153296722/4bc46092-a195-4f53-86c4-396c5dae0c91">





###### Step 4 — Install Install SonarQube and Trivy:

      ######## SonarQube :(formerly Sonar) is an open-source platform developed by SonarSource for continuous inspection of code quality to perform automatic reviews with static       
                          analysis of code to: - detect bugs - code smells - security vulnerabilities.      
               
      ######## Trivy : is a simple and comprehensive vulnerability scanner for containers and other artifacts. 

      
        -> to install SonarQube: docker run -d --name sonar -p 9000:9000 sonarqube:lts-community
        -> to install Trivy: sudo apt-get install wget apt-transport-https gnupg lsb-release
                              wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | sudo apt-key add -
                              echo deb https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main | sudo tee -a /etc/apt/sources.list.d/trivy.list
                              sudo apt-get update
                              sudo apt-get install trivy        

        -> to scan image using trivy: trivy image <imageid>


###### Step 7 — Install Plugins like JDK, Sonarqube Scanner, Nodejs, and OWASP Dependency Check:

jenkins -> Manage Jenkins -> Plugins -> available plugins -> then select jdk, sonarqube scanner, nodejs, owasp dependency check then install
![image](https://github.com/sophiane15/DevSecOps-Netflix-Clone-CI-CD-Jenkins-Docker-Kubernetes-Monitoring/assets/153296722/2811a59f-3f29-436c-8ad7-1d955d34b20d)




###### Step 5 — Install the Prometheus Plugin and Integrate it with the Prometheus server.
###### Step 6 — Email Integration With Jenkins and Plugin setup.
###### Step 8 — Create a Pipeline Project in Jenkins 
###### Step 9 — Install OWASP Dependency Check Plugins
###### Step 10 — Docker Image Build and Push
###### Step 11 — Deploy the image using Docker
###### Step 12 — Access the Netflix app on the Browser.
###### Step 13 — Terminate the AWS EC2 Instances.
