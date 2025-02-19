pipeline {
    agent any

    tools {
        jdk 'JDK17'
        maven 'Maven 3.8.7'
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
                sh 'mvn package'
            }
        }
stage('Build Docker Image') {
    steps {
        sh '''
        if [ ! -f Dockerfile ]; then
            echo "❌ Dockerfile not found! Make sure it's present in the repository."
            exit 1
        fi
        docker build -t $DOCKER_IMAGE:$DOCKER_TAG .
        '''
    }
}
        stage('Push to Docker Hub') {
            steps {
                withDockerRegistry([credentialsId: 'dockerhub', url: 'https://hub.docker.com/']) {
                    sh '''
                    docker login -u skpandey1512 -p $DOCKER_PASSWORD
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
