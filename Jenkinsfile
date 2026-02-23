def execute(command) {
    /* groovylint-disable-next-line UnnecessaryGetter */
    if (isUnix()) {
        sh command
    } else {
        bat command
    }
}

def verifyDeployment() {
    echo 'Aguardando o Tomcat processar o novo .war...'
    def status = '000'
    for (int i = 0; i < 4; i++) {
        try {
            status = sh(script: "curl -s -o /dev/null -w '%{http_code}' http://tomcat-api:8080/tasks-backend/", returnStdout: true).trim()
            if (status == '200') {
                echo 'Sucesso! Aplicação está online.'
                return true
            }
        } catch (IOException e) {
            echo "Tentativa ${i+1}: Aplicação ainda não respondeu..."
        }
        sleep 5
    }
    error 'Falha no Deploy: A aplicação não respondeu após várias tentativas.'
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

        stage('SonarQube Analysis') {
            steps {
                withCredentials([string(credentialsId: 'SonarToken', variable: 'SONAR_TOKEN')]) {
                    script {
                        execute "mvn sonar:sonar \
                            -Dsonar.host.url=http://sonar:9000 \
                            -Dsonar.projectKey=tasks-frontend \
                            -Dsonar.login=${SONAR_TOKEN}"
                    }
                }
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                script {
                    echo 'Deploying to Tomcat...'
                }
            }
        }

        stage('Verify Deploy') {
            steps {
                script {
                    verifyDeployment()
                }
            }
        }
    }
}
