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
                    execute("docker cp ${env.HOSTNAME}:/var/jenkins_home/workspace/tasks-backend/target/tasks-backend.war tomcat-api:/usr/local/tomcat/webapps/")
                }
            }
        }
    }
}
