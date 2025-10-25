pipeline{
    agent any
    environment {
        IMAGE_NAME = 'akshaypranavb/jenkins-test-flask-application'
    }
    stages{
        stage('Checkout'){
            steps{
                git branch : 'main', url: 'https://github.com/sanloop-akshay/Jenkins-Flask-Test'
            }
        }
        stage('Build Docker Image'){
            steps{
                sh 'docker build -t $IMAGE_NAME:latest .'
            }

        }
        stage('Push Docker Image'){
            steps{
                withCredentials([usernamePassword(credentialsId: '7edba577-5c8e-4fb4-87d9-81f3d38dfb96', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]){
                    sh '''
                    set -x
                    echo "Logging into Docker Hub..."
                    echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin
                    docker push $IMAGE_NAME:latest
                    '''
                }
            }
        }
    }
}
