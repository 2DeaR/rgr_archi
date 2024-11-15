pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/2DeaR/rgr_archi.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    def image = docker.build("rgr_archi_image:${env.BUILD_ID}")
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    sh 'docker stop rgr_archi_container || true'
                    sh 'docker rm rgr_archi_container || true'
                    sh 'docker run -d --name rgr_archi_container -p 8080:8080 rgr_archi_image:${env.BUILD_ID}'
                }
            }
        }
    }
    post {
        always {
            script {
                sh 'docker image prune -f'
            }
        }
    }
}
