pipeline {
    agent any

    tools {
        jdk 'JDK17'
        maven 'Maven 3.6.3'
    }

    environment {
        DOCKER_IMAGE = 'skpandey1512/springboot-app'
        DOCKER_TAG = 'latest'
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
                sh 'mvn clean package'  // Ensure the JAR file is created
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                # Check if any jar file exists in the target directory
                if [ ! -f target/*.jar ]; then
                    echo "❌ JAR file not found! Ensure the build is successful and the JAR is located in target/"
                    exit 1
                fi
                docker build -t $DOCKER_IMAGE:$DOCKER_TAG .
                '''
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh '''
                    echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin
                    docker push $DOCKER_IMAGE:$DOCKER_TAG
                    '''
                }
            }
        }
    }

    post {
        success {
            echo "🎉 Build, Image Creation & Push to Docker Hub Successful!"
        }
        failure {
            echo "🚨 Build Failed!"
        }
    }
}
