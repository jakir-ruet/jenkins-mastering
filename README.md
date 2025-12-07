## More About Me – [Take a Look!](http://www.mjakaria.me)

## Get started with Jenkins

### Prerequisites (To follow this tutorial, you will need)

1. Any Linux OS (Ubuntu, RedHat) configured with a non-root user and firewall
2. OpenJDK 11 or upper

### Java & [OpenJDK](https://jdk.java.net/21/) Install

Recommendation

```bash
apt update
java -version
cd /opt
# as per your OS you should download specified version. my case aarch64.
wget https://download.java.net/java/GA/jdk21.0.2/f2283984656d49d69e91c558476027ac/13/GPL/openjdk-21.0.2_linux-aarch64_bin.tar.gz
tar zxvf openjdk-21.0.2_linux-aarch64_bin.tar.gz
mv jdk-21.0.2 jdk-21
whereis java # searching if required
nano .bashrc # or .bash_profile or .zshrc (mac)
JAVA_HOME=/opt/jdk-21 # set java path variable
PATH=$PATH:$HOME/bin:$JAVA_HOME/bin
export PATH
source .bashrc # or .bash_profile or .zshrc (mac)
java --version
echo $PATH
echo $JAVA_HOME
```

Or

```bash
apt install openjdk-21-jre-headless
find /usr/lib/jvm/java-21-openjdk-arm64/java* | head -n 3 # searching if required
nano .bashrc # or .bash_profile or .zshrc (mac)
JAVA_HOME=/usr/lib/jvm/java-21-openjdk-arm64 # set java path variable
PATH=$PATH:$HOME/bin:$JAVA_HOME/bin
export PATH
source .bashrc # or .bash_profile or .zshrc (mac)
java --version
echo $PATH
echo $JAVA_HOME
```

### [Install](https://www.jenkins.io/doc/book/installing/linux/) Jenkins LTS

```bash
wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
```

```bash
apt-get update
apt-get install jenkins
service jenkins status
```

### Firewall Configuration

```bash
ufw allow 8080
ufw allow OpenSSH
ufw enable
ufw status
```

### Use the following command to get the password

```bash
cat /var/lib/jenkins/secrets/initialAdminPassword
```

### Go to browser

```bash
http://localhost:8080
```

### If its need

```bash
apt apt upgrade jenkins-lts
```

### Github

- Install github plugin from plugin management
- Configure from tool management

### [Maven install](https://maven.apache.org/install.html)

```bash
cd /opt
wget https://dlcdn.apache.org/maven/maven-3/3.9.6/binaries/apache-maven-3.9.6-bin.tar.gz
tar zxvf apache-maven-3.9.6-bin.tar.gz
mv apache-maven-3.9.6 maven
```

```bash
nano .bashrc # or .bash_profile or .zshrc (mac)
M2_HOME=/opt/maven
M2=/opt/maven/bin
PATH=$PATH:$HOME/bin:$JAVA_HOME:$M2:$M2_HOME
export PATH
source .bashrc # or .bash_profile or .zshrc (mac)
echo $M2_HOME
mvn --version
```

### [Install](https://gradle.org/install/) Gradle

```bash
cd /opt
wget https://services.gradle.org/distributions/gradle-8.8-bin.zip
apt install unzip
unzip gradle-8.7-bin.zip
nano .bashrc
GRADLE_HOME=/opt/gradle-8.7 # gradle path variable
PATH=$PATH:$HOME/bin:$JAVA_HOME/bin:$M2:$M2_HOME:$GRADLE_HOME/bin
source /.bashrc
echo $GRADLE_HOME
gradle -v # check version
```

### [Install](https://nodejs.org/en/download/prebuilt-binaries) NodeJS

```bash
cd /opt
wget https://nodejs.org/dist/v20.12.0/node-v20.12.0-linux-x64.tar.xz
tar xf node-v20.12.0-linux-x64.tar.xz
mv node-v20.12.0-linux-x64 nodejs-20
nano .bashrc
# nodejs path variable
NODEJS_HOME=/opt/nodejs-20
PATH=$PATH:$HOME/bin:$JAVA_HOME/bin:$M2:$M2_HOME:$GRADLE_HOME/bin:$NODEJS_HOME/bin
source /.bashrc
echo $NODEJS_HOME
node -v # check version
```

### Working Principle

![Working Principle](./img/working-principle.png)

### Pipeline Components

Components are the fundamental building blocks of Jenkins that collectively define and manage the complete build, test, and deployment workflow.

#### Pipeline

A Pipeline in root block in Jenkins is a set of steps or instructions written in code (Jenkinsfile) to automate the build, test, and deploy process.

##### Declarative Pipeline

- Recommended for most users
- Uses a structured, simpler syntax
- Easier to read, maintain, and validate

```bash
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Building...'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing...'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying...'
            }
        }
    }
}
```

##### Scripted Pipeline

- Fully Groovy-based (more flexible, powerful)
- Requires programming knowledge (Groovy/Java)
- For complex or customized logic

```bash
node {
    stage('Build') {
        echo 'Building...'
    }
    stage('Test') {
        echo 'Testing...'
    }
    stage('Deploy') {
        echo 'Deploying...'
    }
}
```

##### Comparison Table: Scripted vs Declarative

| **Aspect**        | **Declarative Pipeline**                           | **Scripted Pipeline**       |
| ----------------- | -------------------------------------------------- | --------------------------- |
| **Syntax**        | DSL (Domain Specific Language)                     | Pure Groovy scripting       |
| **Readability**   | ✅ Easier                                           | ❌ Less beginner-friendly    |
| **Validation**    | ✅ Jenkins validates                                | ❌ Manual debugging required |
| **Flexibility**   | ❌ Some limits                                      | ✅ Highly flexible           |
| **Structure**     | Predefined structure (pipeline→agent→stages→steps) | Freeform                    |
| **Use Case**      | ✅ Simple to moderately complex CI/CD               | ✅ Complex, custom logic     |
| **Introduced In** | Jenkins 2.x                                        | Jenkins 1.x                 |
| **Preferred**     | ✅ Beginners & regular use                          | ✅ Advanced users            |

- **Mixing Both** You can use Scripted syntax inside Declarative Pipelines with script block for complex logic.

```bash
pipeline {
    agent any
    stages {
        stage('Mixed') {
            steps {
                script {
                    for (int i = 0; i < 3; i++) {
                        echo "Loop ${i}"
                    }
                }
            }
        }
    }
}
```

- **Which Should You Use?**

| **If you are**                      | **Use**           |
| ----------------------------------- | ----------------- |
| New to Jenkins / Simple workflows   | ✅ **Declarative** |
| Need complex/custom workflows       | ✅ **Scripted**    |
| Working in a team with mixed skills | ✅ **Declarative** |

#### Agent

Agent where the pipeline (or stage) will run. It can be a Jenkins node, Docker container, or specific label. (the Jenkins worker/agent/executor machine).

```bash
pipeline {
  agent any                     // runs on any available agent
  agent { label 'docker-node' } // run only on nodes labeled "docker-node"
  agent none                   // you’ll define agent per stage
}
```

#### Stage

A Stage is one major phase of pipeline. Helps you organize logically.

- Usually used for: `Build`, `Test`, `Deploy`, etc.
- You will see it in `Jenkins UI` as visual steps.

#### Step

Steps are individual tasks inside a stage. Think of steps as commands or actions.

```bash
stages {
  stage('Build') {
    steps {
      echo "Building the project"
    }
    steps {
      echo "Running build process"
      sh "npm install"
    }
  }
}
```

#### Environment

Define environment variables for the entire pipeline or a specific stage.

```bash
environment {
    APP_ENV = 'production'
}
```

#### Post

Define actions to happen after stages: success, failure, always.

```bash
post {
    success { echo "Success!" }
    failure { echo "Failed!" }
}
```

#### Parameters (Optional)

For Parameterized Builds—use when you want user input or dynamic values.

```bash
parameters {
    string(name: 'BRANCH', defaultValue: 'main')
}
```

#### Tools

Auto-installs tools like Maven, JDK, NodeJS

#### options

Configure pipeline behavior (e.g., timeouts, retries)

#### when

Conditional execution of stages

#### input

Pause for manual approval

#### parallel

Run multiple branches/stages at the same time

#### timeout

Limit time for stage execution (via options)

#### credentials

Use stored credentials securely

#### libraries

Use shared libraries to DRY your Jenkinsfiles

#### Jenkinefile

```bash
pipeline {
    /* 👉 Component 1: agent - Where this pipeline will run */
    agent any

    /* 👉 Component 2: environment - Define environment variables */
    environment {
        APP_NAME = 'myapp'
        VERSION = '1.0.0'
    }

    /* 👉 Component 3: parameters - Accept input from user before job runs */
    parameters {
        string(name: 'DEPLOY_ENV', defaultValue: 'dev', description: 'Deployment environment')
        booleanParam(name: 'RUN_TESTS', defaultValue: true, description: 'Run tests?')
    }

    /* 👉 Component 4: options - Control behavior of the build */
    options {
        timeout(time: 20, unit: 'MINUTES')
        skipDefaultCheckout()    // Don't checkout repo automatically
        buildDiscarder(logRotator(numToKeepStr: '5'))  // Keep last 5 builds only
    }

    stages {
        /* 👉 Component 5: stages - Group of all stages */

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo "Building ${APP_NAME} version ${VERSION}"
                sh './gradlew build'
            }
        }

        stage('Test') {
            when {
                expression { return params.RUN_TESTS }
            }
            parallel {
                stage('Unit Tests') {
                    steps {
                        sh './gradlew test'
                    }
                }
                stage('Integration Tests') {
                    steps {
                        sh './gradlew integrationTest'
                    }
                }
            }
        }

        stage('Approval') {
            when {
                branch 'main'
            }
            steps {
                input message: 'Approve deployment to production?', ok: 'Deploy'
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying ${APP_NAME} version ${VERSION} to ${params.DEPLOY_ENV}"
                sh "./deploy.sh ${params.DEPLOY_ENV}"
            }
        }
    }

    /* 👉 Component 6: post - Define actions after build completes */
    post {
        success {
            echo '✅ Pipeline completed successfully!'
        }
        failure {
            echo '❌ Pipeline failed!'
        }
        always {
            echo '📦 Cleaning up workspace...'
            cleanWs()
        }
    }
}
```

### Jenkins CI/CD Pipeline Flow

This flow is divided into `four` main sections:

1. Continuous Integration,
2. Continuous Deployment,
3. Continuous Delivery, and
4. Post Build.

#### Continuous Integration (CI)

In this stage focuses on `building`, `testing`, `scanning`, and `containerizing` the code.

| **Sub-Stage**         | **Action / Node Name**       | **Tool / Technology Implied**                            |
| --------------------- | ---------------------------- | -------------------------------------------------------- |
| **Initial Steps**     | Build / Install Dependencies | Maven, Gradle, NPM, Yarn (depending on project language) |
| **Dependency Check**  | OWASP Dependency Check       | OWASP Dependency-Check (SCA Tool)                        |
|                       | NPM Dependency Audits        | NPM Audit or similar JS/Node security tools              |
| **Code Testing**      | Code Coverage                | JaCoCo, Istanbul/NYC                                     |
|                       | Unit Testing Node 1          | JUnit, TestNG, Jest, Mocha                               |
|                       | Unit Testing Node 2          | (Parallel testing node using same tools)                 |
| **Security Scan**     | SAST                         | SonarQube, Checkmarx, Fortify (Static Analysis Tools)    |
|                       | Quality Gates                | SonarQube Quality Gate (enforcing code quality metrics)  |
| **Artifact Creation** | Dockerizing Build            | Docker CLI / Engine                                      |
|                       | Vulnerability Scan           | Docker Scan, Trivy, Clair (Container Image Scanners)     |
|                       | Push Image                   | Docker Hub, AWS ECR, Google GCR (Container Registry)     |

#### Continuous Deployment (CD)

This stage deploys the tested artifact to a VM for immediate integration testing.

| **Sub-Stage**  | **Action / Node Name** | **Tool / Technology Implied**                               |
| -------------- | ---------------------- | ----------------------------------------------------------- |
| **Deployment** | AWS EC2 VM Deploy      | Jenkins SSH Plugin, Ansible, Terraform, or AWS CodeDeploy   |
| **Testing**    | Integration Testing    | Selenium, Postman/Newman, or specialized testing frameworks |

#### Continuous Delivery (CDEL)

This stage includes deploying to a container orchestration platform (Kubernetes), dynamic scanning, and serverless deployment.

| **Sub-Stage**                | **Action / Node Name**          | **Tool / Technology Implied**                       |
| ---------------------------- | ------------------------------- | --------------------------------------------------- |
| **Container Orchestration**  | Update Docker Image Tags        | Docker CLI or registry API                          |
|                              | Kubernetes Deploy               | kubectl, Helm, or Jenkins Kubernetes Plugin         |
| **Dynamic Scan**             | DAST OWASP ZAP                  | OWASP ZAP (Dynamic Analysis Tool)                   |
| **Review**                   | Approval Stage                  | Manual input step in Jenkins Pipeline               |
| **Serverless Deployment**    | AWS Lambda Deploy               | AWS SAM, Serverless Framework, or AWS CLI           |
| **Serverless Configuration** | Update Lambda Configurations    | AWS CLI or specialized deployment tools             |
| **Serverless Testing**       | AWS Lambda Invocation / Testing | Automated API calls using integration testing tools |

#### Post Build

This final stage aggregates reports and sends notifications.

| **Sub-Stage**          | **Action / Node Name**                     | **Tool / Technology Implied**                            |
| ---------------------- | ------------------------------------------ | -------------------------------------------------------- |
| **Report Aggregation** | Unit Test Reports                          | JUnit XML format                                         |
|                        | Code Coverage Reports                      | Cobertura / JaCoCo format                                |
|                        | Vulnerability Reports                      | SAST / SCA / DAST reports                                |
|                        | Dependency Scan Reports                    | OWASP Dependency-Check reports                           |
| **Publishing**         | Publish to Jenkins                         | Jenkins built-in artifact management / reporting plugins |
|                        | Publish to AWS S3                          | AWS S3 CLI or specialized S3 upload plugin               |
| **Notification**       | Slack Notification for All States / Stages | Jenkins Slack Notification Plugin                        |

## With Regards, `Jakir`

[![LinkedIn][linkedin-shield-jakir]][linkedin-url-jakir]
[![Facebook-Page][facebook-shield-jakir]][facebook-url-jakir]
[![Youtube][youtube-shield-jakir]][youtube-url-jakir]

### Wishing you a wonderful day! Keep in touch

<!-- Personal profile -->

[linkedin-shield-jakir]: https://img.shields.io/badge/linkedin-%230077B5.svg?style=for-the-badge&logo=linkedin&logoColor=white
[linkedin-url-jakir]: https://www.linkedin.com/in/jakir-ruet/
[facebook-shield-jakir]: https://img.shields.io/badge/Facebook-%231877F2.svg?style=for-the-badge&logo=Facebook&logoColor=white
[facebook-url-jakir]: https://www.facebook.com/jakir.ruet/
[youtube-shield-jakir]: https://img.shields.io/badge/YouTube-%23FF0000.svg?style=for-the-badge&logo=YouTube&logoColor=white
[youtube-url-jakir]: https://www.youtube.com/@mjakaria-ruet/featured
