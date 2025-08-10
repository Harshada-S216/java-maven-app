pipeline {
    agent any
    tools {
        maven 'maven 3.8.7'
    }

    environment {
        SONAR_HOST_URL = 'http://13.127.160.83:9000'
        SONAR_LOGIN = credentials('squ_fb064bf66480982cc6728e26fccfa7b10b63efcb')
    }

    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Harshada-S216/java-maven-app.git'
            }
        }

        stage('Build') {
            when { branch 'main' }
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Unit Test') {
            when { branch 'main' }
            steps {
                sh 'mvn test'
            }
        }

        stage('SonarQube Scan') {
            when { branch 'main' }
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh """
                        mvn sonar:sonar \
                          -Dsonar.projectKey=myapp \
                          -Dsonar.host.url=$SONAR_HOST_URL \
                          -Dsonar.login=$SONAR_LOGIN
                    """
                }
            }
        }

        stage('Archive Artifact') {
            when { branch 'main' }
            steps {
                archiveArtifacts artifacts: '**/target/*.war', fingerprint: true
            }
        }
    }

    post {
        always { echo 'Pipeline finished' }
        success { echo 'Pipeline completed successfully!' }
        failure { echo 'Pipeline failed!' }
    }
}
