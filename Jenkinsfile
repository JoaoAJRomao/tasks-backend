def execute(command) {
    if (isUnix()) {
        sh command
    } else {
        bat command
    }
}

pipeline {
    agent any
    tools {
        maven 'MAVEN_LOCAL'
    }
    stages {
        stage('Build backend') {
            steps {
                script {
                    execute('mvn clean package -DskipTests=true')
                }
            }
        }
    }
}