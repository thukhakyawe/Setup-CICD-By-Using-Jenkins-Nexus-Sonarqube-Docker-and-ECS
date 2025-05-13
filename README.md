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