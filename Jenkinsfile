pipeline {
    agent any

    tools {
        jdk 'JDK17'
        maven 'Maven 3.8.7'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/user/springboot-app.git'
            }
        }

        stage('Build & Test') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'mvn test'
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
