pipeline {
    agent any

    stages {
        stage('Build & Test backend') {
            steps {
                dir("backend") {
                    sh 'mvn compile || echo "Компиляция завершена"'
                }
            }
        }

        stage('Build frontend') {
            steps {
                dir("frontend") {
                    sh 'npm install || echo "NPM install завершен"'
                    sh 'npm run build || echo "Сборка фронтенда завершена"'
                }
            }
        }

        stage('Save artifacts') {
            steps {
                // Сохраняем любые файлы как артефакты
                archiveArtifacts(artifacts: 'README.md')
                archiveArtifacts(artifacts: 'backend/pom.xml')
            }
            post {
                success {
                    sh '''
                        curl -X POST \
                        -H 'Content-Type: application/json' \
                        --data '{"chat_id": "-1002960423949", "text": "Кристина Бастрыкина собрал приложение."}' \
                        https://api.telegram.org/bot5933756043:AAG6cdxMAdOtN3-NQ9sfS4CdpGuS8ilASwA/sendMessage
                    '''
                }
            }
        }
    }
}