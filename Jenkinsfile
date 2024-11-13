pipeline {
    agent any
    stages {
        stage('Clone Repository') {
            steps {
                // Клонируем ваш репозиторий
                git 'https://github.com/2DeaR/rgr_archi.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    // Собираем Docker-образ
                    dockerImage = docker.build("rgr_archi_image")
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    // Останавливаем и удаляем старый контейнер, если он запущен
                    sh 'docker stop rgr_archi_container || true && docker rm rgr_archi_container || true'
                    
                    // Запускаем новый контейнер с обновлённым образом
                    dockerImage.run("--name rgr_archi_container --network jenkins-network")
                }
            }
        }
    }
    post {
        always {
            // Очищаем старые ненужные образы Docker
            sh 'docker image prune -f'
        }
    }
}
