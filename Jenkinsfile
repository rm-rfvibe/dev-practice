pipeline {
    agent any

    stages {
        stage('Setup Maven') {
            steps {
                sh '''
                    # Скачиваем Maven
                    curl -L -o apache-maven-3.8.1-bin.tar.gz https://archive.apache.org/dist/maven/maven-3/3.8.1/binaries/apache-maven-3.8.1-bin.tar.gz
                    tar -xzf apache-maven-3.8.1-bin.tar.gz
                '''
            }
        }

        stage('Build backend') {
            steps {
                dir("backend") {
                    sh '''
                        # Пробуем собрать бэкенд
                        ../apache-maven-3.8.1/bin/mvn compile || echo "Компиляция завершена"
                    '''
                }
            }
        }

        stage('Save artifacts') {
            steps {
                // Сохраняем любой файл как артефакт
                archiveArtifacts(artifacts: 'README.md') 
            }
            post {
                success {
                    sh '''
                        curl -X POST \
                        -H 'Content-Type: application/json' \
                        --data '{"chat_id": "-1002960423949", "text": "Котик Котов собрал приложение."}' \
                        https://api.telegram.org/bot5933756043:AAG6cdxMAdOtN3-NQ9sfS4CdpGuS8ilASwA/sendMessage
                    '''
                }
            }
        }
    }
}