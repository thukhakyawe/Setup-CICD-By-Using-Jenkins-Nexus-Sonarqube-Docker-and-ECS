# Setup-CICD-By-Using-Jenkins-Nexus-Sonarqube-Docker-and-ECS
Project Level

## 1. Create EC2 Server for Jenkins

#### 1.1 User Data Script for Jenkins

```
sudo apt-get update

sudo apt-get install openjdk-21-jdk -y

sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key

echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null

sudo apt-get update

sudo apt-get install jenkins -y
```


#### 1.2 Use 80 and 8080 for Security Group

![alt text](image.png)


#### 1.3 Default Jenkins Path

```
ls /var/lib/jenkins/
```

#### 1.4 Secret Path

- Type EC2's public Ip:8080

![alt text](image-1.png)

```
cat /var/lib/jenkins/secrets/initialAdminPassword
```

#### 1.5 Install Plugin

![alt text](image-2.png)

![alt text](image-3.png)


#### 1.6 Install Maven and JDK

![alt text](image-4.png)

![alt text](image-5.png)



## 2. Create EC2 Server for Nexus and Sonarqube

#### 2.1 User Data Script for Nexus

- Use nexus-setup.sh

#### 2.2 Security Group for Nexus

- Allow ssh from your ip
- Allow 8081 from your ip
- Allow 8081 from jenkins-sg
- Allow 80 from anywhere

#### 2.3 User Data Script for Nexus

- Use sonar-setup.sh

#### 2.4 Security Group for Nexus

- Allow ssh from your ip
- Allow 80 from your ip
- Allow 80 from jenkins-sg

#### 2.5 Check nexus status

- Log in to nexus EC2 and run

```
systemctl status nexus
ls /opt/nexus/
java -v
```

- Log in to publicip:8081

![alt text](image-6.png)

- Click Sign In

![alt text](image-7.png)

```
cat /opt/nexus/sonatype-work/nexus3/admin.password
```

- Log in and Set Up

![alt text](image-8.png)

![alt text](image-9.png)

#### 2.6 Check sonarqube status

- Log in to publicip

![alt text](image-10.png)


![alt text](image-11.png)

- Allow 8080 from Sonarqube SG at Jenkin SG

#### 2.7 Incase of if you can't log in Jenkin Url

- You can change it here

```
sudo vim /var/lib/jenkins/jenkins.model.JenkinsLocationConfiguration.xml
sudo systemctl restart jenkins.service
```

![alt text](image-12.png)


## 3 Install Plugin in Jenkins Server

#### 3.1 Install Nexus Artifact Uploader, SonarQube Scanner, Build Timestamp, Pipeline Maven Integration, Pipeline Utility Steps Version

![alt text](image-13.png)

![alt text](image-14.png)

![alt text](image-15.png)

![alt text](image-16.png)

![alt text](image-17.png)


## 4 First Pipeline

#### 4.1 Create Pipeline by using Jenkinsfile

- Click New Item

![alt text](image-18.png)

- Choost Pipeline and Click OK

![alt text](image-19.png)

- Insert Data from Jenkinsfile and Click Save

![alt text](image-20.png)

- Click Build Now

![alt text](image-21.png)

- Click Status

![alt text](image-22.png)


## 5 Add SonarQube Scanner and SonarQube Servers Detail

#### 5.1 Add SonarQube Scanner

![alt text](image-23.png)


#### 5.2 Add SonarQube Servers

- Create Token from SonarQube

![alt text](image-25.png)

![alt text](image-24.png)

![alt text](image-26.png)

![alt text](image-27.png)

## 6. Code Analysis Demonstration

#### 6.1 Use New Jenkinsfile Information and Run Pipeline

![alt text](image-28.png)

![alt text](image-29.png)


#### 6.2 Use New Jenkinsfile Information and Run Pipeline

![alt text](image-30.png)

![alt text](image-31.png)

![alt text](image-32.png)


## 7. How To Create Quality Gates at sonarQube

#### 7.1 Add Quality Gates

1. Click Quality Gates
2. Click Create
3. Click Add Condition

![alt text](image-33.png)

![alt text](image-34.png)

#### 7.2 Update Default Quality Gates with New Quality Gates

1. Go to vprofile project
2. Project Settings
3. Quality Gates
4. Choose vprofile-QG

![alt text](image-35.png)

![alt text](image-36.png)

#### 7.3 Create Webhook

1. Go to vprofile project
2. Project Settings
3. Webhooks
4. Name must be your jenkien server private ip:8080/sonarqube-webhook

![alt text](image-37.png)

![alt text](image-38.png)


#### 7.4 Use New Jenkinsfile Information and Run Pipeline


![alt text](image-39.png)


## 8 Nexus Repository 

#### 8.1 Create Nexus Repository for Maven Hosted

![alt text](image-40.png)

![alt text](image-41.png)

![alt text](image-42.png)

![alt text](image-43.png)


#### 8.2 Create Credential

![alt text](image-44.png)

![alt text](image-45.png)

![alt text](image-46.png)

![alt text](image-47.png)


![alt text](image-48.png)


#### 8.3 Use New Jenkinsfile Information and Run Pipeline

![alt text](image-49.png)


## 9 Add Notification Slack to Jenkins

## 9.1 Install Plugin

![alt text](image-50.png)

## 9.2 Create Workplace from Slack

- Create workplace from Slack 

![alt text](image-51.png)

- Create New Channel from Workplace

![alt text](image-55.png)

## 9.3 Add Jenkins to Slack

- Search Add app slack from browser and click this link, you will see like this

![alt text](image-52.png)

![alt text](image-53.png)

![alt text](image-54.png)

![alt text](image-56.png)

- Copy Token to somewhere

![alt text](image-57.png)

- Click Down Arrow and Click Workspace

![alt text](image-59.png)

- Copy url to somewhere

![alt text](image-58.png)

-  Go to Jenkin Dashboard > Manage Jenkins > System
- Add Workspace Name
- Add Credentials
- Add Channel Name
- Click Test Connection

![alt text](image-60.png)

![alt text](image-61.png)

- Check in Workspace

![alt text](image-62.png)

#### 9.4 Use New Jenkinsfile Information and Run Pipeline

![alt text](image-63.png)

![alt text](image-64.png)


## 10 Add Docker and ECR

#### 10.1 Install aws-cli to Jenkins Server

```
sudo apt update
sudo apt install unzip curl -y
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
aws --version
```

![alt text](image-65.png)

#### 10.2 Install docker engine to Jenkins Server

```
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update

# To install latest version

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
```

![alt text](image-66.png)



```
su - i jenkins
exit
usermod -a -G docker jenkins
id jenkins
reboot
```

![alt text](image-67.png)


#### 10.3 Create IAM And Create ECR 

- Create IAM User with these permission

![alt text](image-69.png)

- Create ECR

![alt text](image-68.png)


#### 10.4 Install Plugin

Amazon Web Services SDK :: All
Amazon ECR
Docker Pipeline
CloudBees Docker Build and Publish 

![alt text](image-70.png)

#### 10.5 Add Credentials to Jenkins

Go to

    Dashboard
    Manage Jenkins
    Credentials
    System
    Global credentials (unrestricted)


![alt text](image-71.png)

#### 10.6 Use New Jenkinsfile Information and Run Pipeline

![alt text](image-73.png)

![alt text](image-72.png)


## 11 Install Plugin,create ECS Cluster and ECS Services


#### 11.1 Create ECS Cluster and ECS Services


![alt text](image-74.png)

![alt text](image-75.png)

![alt text](image-76.png)

![alt text](image-77.png)

- Add this role to ecsTaskExecutionRole

![alt text](image-78.png)

- Create Service

![alt text](image-79.png)

![alt text](image-80.png)

![alt text](image-81.png)

![alt text](image-82.png)

- Use public subnets from networking

![alt text](image-83.png)

![alt text](image-84.png)

![alt text](image-85.png)

- Wait for until finished set up.

- Call DNS 

Amazon Elastic Container Service
Clusters
vprofile
Services
vprofileappsvc
Configuration

![alt text](image-88.png)



#### 11.2 Install Plugins

Pipeline: AWS Steps

![alt text](image-86.png)

#### 11.3 Use New Jenkinsfile Information and Run Pipeline

![alt text](image-87.png)

