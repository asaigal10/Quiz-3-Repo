pipeline {
    agent any
    triggers {
        cron('H/10 * * * 1')  
    }
    stages {
        stage('Build') {
            steps {
                script {
                    sh './mvnw clean package'
                }
            }
        }
        stage('Test with Jacoco') {
            steps {
                script {
                    sh './mvnw test'
                }
            }
        }
        stage('Jacoco Code Coverage Report') {
            steps {
                script {
                    sh './mvnw jacoco:report' 
                }
            }
            post {
                always {
                    jacoco execPattern: '**/target/jacoco.exec', classPattern: '**/target/classes', sourcePattern: '**/src/main/java', exclusionPattern: '**/src/test*'
                }
            }
        }
    }
    post {
        success {
            archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
            junit '**/target/surefire-reports/*.xml'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
