pipeline {
    agent any

    tools {
        jdk 'JDK17'
        maven 'Maven 3.8.7'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git credentialsId: 'github', branch: 'master', url: 'https://github.com/Skpandey15/JenkinsTest.git'
            }
        }

        stage('Build & Test') {
            steps {
                sh 'mvn clean install'
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }

        stage('Package Application') {
            steps {
                sh 'mvn package'
            }
        }
    }

    post {
        success {
            echo "🎉 Build Successful!"
        }
        failure {
            echo "🚨 Build Failed!"
        }
    }
}
