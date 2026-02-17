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
            steps {
                script {
                    execute('pwd') // Mostra o diretório atual no log
                    execute('ls -R target') // Lista o que tem na pasta target
                    execute('docker cp target/tasks-backend.war tomcat-api:/usr/local/tomcat/webapps/')
                }
            }
        }
    }
}
