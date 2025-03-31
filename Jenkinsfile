pipeline {
    agent any 
    
    stages { 
        stage('SCM Checkout') {
            steps {
                retry(3) {
                    git branch: 'main', url: 'https://github.com/PasinduWaidyarathna/GitHub-Docker-and-Jenkins-CI-CD-Pipeline'
                }
            }
        }
        stage('Build Docker Image') {
            steps {  
                bat 'docker build -t pasindumw/nodeapp-cuban:%BUILD_NUMBER% .'
            }
        }
        stage('Login to Docker Hub') {
            steps {
                withCredentials([string(credentialsId: 'test-dockerhubpassword', variable: 'test-dockerhubpass')]) {
                    script {
                        bat "docker login -u pasindumw -p %test-dockerhubpass%"
                    }
                }
            }
        }
        stage('Push Image') {
            steps {
                bat 'docker push pasindumw/nodeapp-cuban:%BUILD_NUMBER%'
            }
        }
    }
    post {
        always {
            bat 'docker logout'
        }
    }
}
