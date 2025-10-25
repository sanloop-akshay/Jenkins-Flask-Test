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
                withCredentials([usernamePassword(credentisalsId: 'jenkins-docker-flask-test',usernameVariable: 'DOCKER_USERNAME',passwordVariable: 'DOCKER_PASSWORD')]){
                    sh '''
                    echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin
                    docker push $IMAGE_NAME:latest
                    '''
                }
            }
        }

    }
}