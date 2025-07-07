## Welcome to Scripted Pipeline

### Scripted Pipeline

It's a type of pipeline written in Groovy that provides full programmatic control over the build process. It uses a more flexible and powerful syntax than a Declarative Pipeline, making it suitable for complex workflows and logic.

#### Scripted pipeline in `Jenkinsfile`

```groovy
node {
    try {
        stage('Build') {
            echo 'Compiling...'
            sh 'make build'
        }

        stage('Test') {
            echo 'Running unit tests...'
            sh 'make test'
        }
    } catch (err) {
        echo "Pipeline failed: ${err}"
        currentBuild.result = 'FAILURE'
    } finally {
        stage('Cleanup') {
            echo 'Cleaning up workspace...'
            deleteDir()
        }
    }
}
```

#### Feature of Scripted Pipeline

- Written in Groovy script.
- Starts with a node block.
- Offers maximum flexibility for advanced users.
- Supports full use of Jenkins Pipeline DSL and Groovy features.
- Commonly used when workflows cannot be easily expressed in a Declarative format.
