pipeline {
    agent any  // Запуск на любом доступном агенте Jenkins
    
    environment {
        DOCKER_IMAGE_NAME = "rgr_archi_image"
        CONTAINER_NAME = "rgr_archi_container"
    }
    
    stages {
        stage('Clone Repository') {
            steps {
                // Клонирование репозитория из GitHub
                git branch: 'main', url: 'https://github.com/2DeaR/rgr_archi.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                // Построение Docker-образа
                sh 'docker build -t ${DOCKER_IMAGE_NAME} .'
            }
        }

        stage('Deploy') {
            steps {
                // Остановка старого контейнера (если существует)
                sh 'docker stop ${CONTAINER_NAME} || true'
                sh 'docker rm ${CONTAINER_NAME} || true'
                // Запуск нового контейнера с обновленным образом
                sh 'docker run -d --name ${CONTAINER_NAME} -p 8000:8000 ${DOCKER_IMAGE_NAME}'
            }
        }
    }

    post {
        always {
            // После выполнения всегда можно выполнить чистку или другие действия
            echo "Pipeline execution complete."
        }
    }
}
