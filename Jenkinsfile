pipeline {
    agent any

    environment {
        MAVEN_HOME = '/usr/share/maven'
        git 'Default'
    }

    stages {
        stage('gitCheckout') {
            steps {
                git branch: 'main', url: 'https://github.com/Harshada-S216/java-maven-app.git'
            }
        }

        stage('Build') {
            when {
                branch 'main'
            }
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Unit Test') {
            when {
                branch 'main'
            }
            steps {
                sh 'mvn test'
            }
        }

        stage('SonarQube Scan') {
            when {
                branch 'main'
            }
            steps {
              withSonarQubeEnv('SonarQube') {
              sh "mvn sonar:sonar -Dsonar.projectKey=myapp -Dsonar.host.url=${http://13.127.160.83:9000/} -Dsonar.login=${squ_6af30e77ac157a85b8f735cbf30b8e3f54ec26b8}"
                }
            }
        }

        stage('Archive Artifact') {
            when {
                branch 'main'
            }
            steps {
                archiveArtifacts artifacts: '**/target/*.war', fingerprint: true
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished'
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}

