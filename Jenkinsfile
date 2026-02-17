def execute(command) {
    /* groovylint-disable-next-line UnnecessaryGetter */
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

        stage('Unit tests') {
            steps {
                script {
                    execute('mvn test')
                }
            }
        }

        stage('Deploy to Tomcat') {
            echo 'Alterado o Dockerfile para copiar o .war para a pasta de deploy do Tomcat'
            echo 'Deploying to Tomcat...'
        }
    }
}
