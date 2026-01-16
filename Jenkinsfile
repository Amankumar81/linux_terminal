pipeline {
 agent any
    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', poll: false, url: 'https://github.com/Amankumar81/linux_terminal.git'
            }
        }

        stage('Build and Push Images') {
            steps {
                script {
                    sh 'docker build -t amankumar81/react-app1 .'
                    withCredentials([usernamePassword(credentialsId: 'docker-hub', passwordVariable: 'ay_pass', usernameVariable: 'ay_user')]) {
                        sh 'docker login -u $ay_user -p $ay_pass'
                        sh 'docker push amankumar81/react-app1 '
                    }
                }
            }
        }

        stage('Deploy Services') {
            steps {
                script {
                    sh 'docker rm -f  react-app1'
                    sh 'docker run -d --name linux-terminal -p 8000:80 amankumar81/react-app1'
                }
            }
        }
        
        stage('Post Deployment Testing') {
            steps {
                script {
                    sh 'curl -I http://localhost:8000'
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check logs for details.'
        }
    }
}
