pipeline {
    agent any
    stages {
        stage('Build Docker App') {
            steps {
                sh '''
                docker build -t poojasdocker2023/docker-app:latest -t poojasdocker2023/docker-app:build-$BUILD_NUMBER .
                '''
            }
        }
        stage('Push Image to Docker Hub') {
            steps {
                sh '''
                docker push poojasdocker2023/docker-app:latest
                docker push poojasdocker2023/docker-app:build-$BUILD_NUMBER
                '''
            }
        }
        stage('Deploy Application') {
            steps {
                sh '''
                ssh -i "~/.ssh/id_rsa" jenkins@34.142.72.48 << EOF
                docker stop docker-app
                docker rm docker-app
                docker rmi poojasdocker2023/docker-app:latest
                docker run -d -p 80:5500 --name docker-app poojasdocker2023/docker-app:latest
                '''
            }
        }
    }
}