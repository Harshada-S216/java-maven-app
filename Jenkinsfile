// Define variables accessible across stages
def START_TIME
def END_TIME

pipeline {
    agent any

    tools {
        maven 'maven9.9'
    }

    stages {
        stage('Start Timer') {
            steps {
                script {
                    START_TIME = System.currentTimeMillis()
                    echo "Pipeline started at: ${START_TIME}"
                }
            }
        }

        stage('Run Tests in Parallel') {
            parallel {
                stage('Unit Tests') {
                    steps {
                        timeout(time: 2, unit: 'MINUTES') {
                            script {
                                def stageStart = System.currentTimeMillis()
                                echo "Starting Unit Tests..."
                                sh './unit-test.sh'
                                def stageEnd = System.currentTimeMillis()
                                echo "Unit Tests duration: " + ((stageEnd - stageStart) / 1000) + " seconds"
                            }
                        }
                    }
                }
                stage('Integration Tests') {
                    steps {
                        timeout(time: 3, unit: 'MINUTES') {
                            script {
                                def stageStart = System.currentTimeMillis()
                                echo "Starting Integration Tests..."
                                sh './integration-test.sh'
                                def stageEnd = System.currentTimeMillis()
                                echo "Integration Tests duration: " + ((stageEnd - stageStart) / 1000) + " seconds"
                            }
                        }
                    }
                }
            }
        }

        stage('End Timer') {
            steps {
                script {
                    END_TIME = System.currentTimeMillis()
                    def totalDuration = (END_TIME - START_TIME) / 1000
                    echo "Pipeline finished at: ${END_TIME}"
                    echo "Total pipeline duration: ${totalDuration} seconds"
                }
            }
        }
    }
}
