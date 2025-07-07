## Welcome to Declarative Pipeline

### Declarative Pipeline

It's is a simplified, structured way to define continuous integration/continuous delivery (CI/CD) workflows, using a predefined, declarative syntax that describes what the pipeline should do rather than how to do it.

#### Declarative pipeline in `Jenkinsfile`

```groovy
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building the application...'
            }
        }
        stage('Test') {
            steps {
                echo 'Running tests...'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying the application...'
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
        }
    }
}
```

#### Feature of Declarative Pipeline

- Introduced in Jenkins Pipeline plugin v2.5+
- Uses a clean, YAML-like structured syntax inside a `Jenkinsfile`
- Easier to read, easier to learn, and promotes best practices
- Has built-in stages like `stages`, `steps`, `post`, `environment` blocks
- Validates syntax more strictly
