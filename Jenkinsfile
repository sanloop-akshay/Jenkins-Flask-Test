pipeline {
    agent any
    environment {
        IMAGE_NAME = 'akshaypranavb/jenkins-test-flask-application'
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/sanloop-akshay/Jenkins-Flask-Test'
            }
        }

        stage('Run Tests against Backend') {
            steps {
                sh '''
                echo "Setting up virtual environment..."
                python3 -m venv venv
                . venv/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt
                echo "Running tests from project root..."
                pytest tests/
                '''
            }
        }

        stage('SonarQube Scan') {
            environment {
                SCANNER_HOME = tool 'SonarScanner'
            }
            steps {
                withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
                    withSonarQubeEnv('sonarqube') { 
                        sh '''
                            echo "Running SonarQube analysis..."
                            $SCANNER_HOME/bin/sonar-scanner \
                            -Dsonar.projectKey=jenkins-flask-test \
                            -Dsonar.sources=. \
                            -Dsonar.host.url=$SONAR_HOST_URL \
                            -Dsonar.login=$SONAR_TOKEN
                        '''
                    }
                }
            }
        }


        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:latest .'
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: '7edba577-5c8e-4fb4-87d9-81f3d38dfb96', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    retry(3) {
                        sh '''
                        echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin
                        docker push $IMAGE_NAME:latest
                        '''
                    }
                }
            }
        }
    }
}
