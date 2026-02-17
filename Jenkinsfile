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
    for (int i = 0; i < 10; i++) {
        try {
            def status = sh(script: "curl -s -o /dev/null -w '%{http_code}' http://tomcat-api:8080/tasks-backend/", returnStdout: true).trim()
            if (status == '200') {
                echo 'Sucesso! Aplicação está online.'
                return true
            }
        } catch (IOException e) {
            echo "Tentativa ${i+1}: Aplicação ainda não respondeu..."
        }
        sleep 5
    }
    error "Falha no Deploy: A aplicação não respondeu após várias tentativas."
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
