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

        -> get API from TMDB website<img width="814" alt="Screenshot 2024-02-10 at 01 30 24" src="https://github.com/sophiane15/DevSecOps-Netflix-Clone-CI-CD-Jenkins-Docker-Kubernetes-Monitoring/assets/153296722/fe92d9b1-159d-4022-a0ab-d4656700552c">

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

###### Step 5 — Integrate SonarQube and Configure:


Create the token

Goto Jenkins Dashboard → Manage Jenkins → Credentials → Add Secret Text. It should look like this

After adding sonar token

Click on Apply and Save

<img width="1404" alt="Screenshot 2024-02-05 at 00 27 18" src="https://github.com/sophiane15/DevSecOps-Netflix-Clone-CI-CD-Jenkins-Docker-Kubernetes-Monitoring/assets/153296722/6405dde9-37d2-4dec-a8a5-8a015b4976c1">


<img width="811" alt="Screenshot 2024-02-10 at 01 42 54" src="https://github.com/sophiane15/DevSecOps-Netflix-Clone-CI-CD-Jenkins-Docker-Kubernetes-Monitoring/assets/153296722/3001989e-d5ea-4031-8fda-486ee4b31eff">


<img width="801" alt="Screenshot 2024-02-10 at 01 43 54" src="https://github.com/sophiane15/DevSecOps-Netflix-Clone-CI-CD-Jenkins-Docker-Kubernetes-Monitoring/assets/153296722/2189af0d-ac02-4829-949f-ccd0248dd904">

<img width="822" alt="Screenshot 2024-02-10 at 01 31 52" src="https://github.com/sophiane15/DevSecOps-Netflix-Clone-CI-CD-Jenkins-Docker-Kubernetes-Monitoring/assets/153296722/545680c7-b824-4a11-80a4-4c08cf0c0d1b">


<img width="818" alt="Screenshot 2024-02-10 at 01 34 46" src="https://github.com/sophiane15/DevSecOps-Netflix-Clone-CI-CD-Jenkins-Docker-Kubernetes-Monitoring/assets/153296722/0f57f2b3-a253-49d1-87ce-4ddab5f70c72">



<img width="814" alt="Screenshot 2024-02-10 at 01 35 29" src="https://github.com/sophiane15/DevSecOps-Netflix-Clone-CI-CD-Jenkins-Docker-Kubernetes-Monitoring/assets/153296722/57670c1f-d5f7-48bc-b5de-699cd2e4b082">


<img width="806" alt="Screenshot 2024-02-10 at 01 36 22" src="https://github.com/sophiane15/DevSecOps-Netflix-Clone-CI-CD-Jenkins-Docker-Kubernetes-Monitoring/assets/153296722/376f6f58-32e0-41bb-8ab8-edeb2001eadb">


###### Step 7 — Install Plugins like JDK, Sonarqube Scanner, Nodejs, and OWASP Dependency Check:


###########1- Eclipse Temurin Installer and SonarQube Scanner (Install without restart): 
     -> Manage Jenkins →Plugins → Available Plugins → Install below plugins

<img width="805" alt="Screenshot 2024-02-09 at 23 51 41" src="https://github.com/sophiane15/DevSecOps-Netflix-Clone-CI-CD-Jenkins-Docker-Kubernetes-Monitoring/assets/153296722/1a351f87-4fe2-4ab8-8ed7-ac3ba4a63463">


 ########### 2- Install ang Configure NodeJs Plugin (Install Without restart):
      -> to install NodeJs into Jenkins:  Jenkins →Plugins → Available Plugins → Install NodeJs
      
<img width="803" alt="Screenshot 2024-02-09 at 23 56 47" src="https://github.com/sophiane15/DevSecOps-Netflix-Clone-CI-CD-Jenkins-Docker-Kubernetes-Monitoring/assets/153296722/8e6123d9-9120-4c6f-8262-c66d0e870e08">

    -> Configure NodeJs: Manage Jenkins → Tools → Install JDK(17) and NodeJs(16)→ Click on Apply and Save
    
<img width="727" alt="Screenshot 2024-02-09 at 23 59 50" src="https://github.com/sophiane15/DevSecOps-Netflix-Clone-CI-CD-Jenkins-Docker-Kubernetes-Monitoring/assets/153296722/82a17e4d-2586-402d-af38-6d9b43ee6652">


############ 3- OWASP Dependency Check: 

   -> to install OWASP Dependency Check:  Manage Jenkins → Plugins → OWASP Dependency-Check
   
<img width="814" alt="Screenshot 2024-02-10 at 00 12 38" src="https://github.com/sophiane15/DevSecOps-Netflix-Clone-CI-CD-Jenkins-Docker-Kubernetes-Monitoring/assets/153296722/447eb67d-c83c-4824-af77-f48a323abb04">

  -> to configure OWASP Dependency Check: Manage Jenkins → Tools 

<img width="812" alt="Screenshot 2024-02-10 at 00 14 06" src="https://github.com/sophiane15/DevSecOps-Netflix-Clone-CI-CD-Jenkins-Docker-Kubernetes-Monitoring/assets/153296722/cd370697-3014-49c8-9ff6-b81a374784cb">




###### Step 8 — Docker Image Build and Push

1- Check the following Docker-related plugins and Install without restart:
  - Docker
  - Docker Commons
  - Docker Pipeline
 - Docker API
 - docker-build-step
   
<img width="826" alt="Screenshot 2024-02-10 at 00 32 58" src="https://github.com/sophiane15/DevSecOps-Netflix-Clone-CI-CD-Jenkins-Docker-Kubernetes-Monitoring/assets/153296722/f665ef96-9bc4-45d7-835a-1cc497dd8f31">


2- Configure and secure Docker: 
  
  -> Manage Jenkins → Tools → indicate version of Docker 

<img width="761" alt="Screenshot 2024-02-10 at 00 37 23" src="https://github.com/sophiane15/DevSecOps-Netflix-Clone-CI-CD-Jenkins-Docker-Kubernetes-Monitoring/assets/153296722/e21798f3-cfb9-44c0-9e6d-7794505f45f4">

 -> 	Manage Jenkins -> Credentials -> Système -> Global credentials (unrestricted): add DockerHub Username and Password under Global Credentials








4 Email Extension Plugin

Configure Java and Nodejs in Global Tool Configuration
Goto Manage Jenkins → Tools → Install JDK(17) and NodeJs(16)→ Click on Apply and Save


###### Step 5 — Install the Prometheus Plugin and Integrate it with the Prometheus server.
###### Step 6 — Email Integration With Jenkins and Plugin setup.
###### Step 8 — Create a Pipeline Project in Jenkins 
###### Step 9 — Install OWASP Dependency Check Plugins

###### Step 11 — Deploy the image using Docker
###### Step 12 — Access the Netflix app on the Browser.
###### Step 13 — Terminate the AWS EC2 Instances.
