pipeline {
    agent any  // Запуск на любом доступном агенте Jenkins

    environment {
        DOCKER_IMAGE_NAME = "rgr_archi_image"
        CONTAINER_NAME = "rgr_archi_container"
        DOCKER_BUILD_PATH = "."  // Путь к Dockerfile (можно изменить, если нужно)
    }

    stages {
        stage('Clone Repository') {
            steps {
                script {
                    // Клонирование репозитория из GitHub
                    echo "Cloning repository from GitHub..."
                    git branch: 'main', url: 'https://github.com/2DeaR/rgr_archi.git'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Проверка наличия Docker перед сборкой образа
                    echo "Building Docker image: ${DOCKER_IMAGE_NAME}..."
                    sh 'docker --version || { echo "Docker is not installed! Exiting..."; exit 1; }'
                    sh "docker build -t ${DOCKER_IMAGE_NAME} ${DOCKER_BUILD_PATH}"
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Проверка наличия контейнера и его остановка/удаление
                    echo "Stopping and removing old container if exists..."
                    sh "docker stop ${CONTAINER_NAME} || true"
                    sh "docker rm ${CONTAINER_NAME} || true"
                    
                    // Запуск нового контейнера с обновленным образом
                    echo "Running new container: ${CONTAINER_NAME}..."
                    sh "docker run -d --name ${CONTAINER_NAME} -p 8000:8000 ${DOCKER_IMAGE_NAME}"
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline execution complete."
        }

        success {
            echo "Pipeline succeeded!"
        }

        failure {
            echo "Pipeline failed. Please check the logs."
        }

        cleanup {
            // Можно добавить шаги для очистки (например, удаление старых образов)
            echo "Cleaning up after pipeline run..."
            sh "docker system prune -f || true"
        }
    }
}
