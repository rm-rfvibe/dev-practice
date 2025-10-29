pipeline {
    agent any

    triggers {
        pollSCM('H/5 * * * *')
    }

    tools {
        maven 'maven-3.8.1'
        jdk 'jdk16' 
        nodejs 'node-16'
    }

    stages {
        stage('Build & Test backend') {
            steps {
                dir("backend") {
                    sh 'mvn package'
                }
            }
            post {
                success {
                    junit 'backend/target/surefire-reports/**/*.xml'
                }
            }
        }

        stage('Build frontend') {
            steps {
                dir("frontend") {
                    sh 'npm install'
                    sh 'npm run build'
                }
            }
        }

        stage('Save artifacts') {
            steps {
                archiveArtifacts(artifacts: 'backend/target/sausage-store-0.0.1-SNAPSHOT.jar')
                archiveArtifacts(artifacts: 'frontend/dist/frontend/*')
            }
            post {
                success {
                    script {
                        def message = "✅ Сборка приложения успешно завершена!\\n\\n" +
                                    "📦 Проект: Sausage Store\\n" +
                                    "👤 Сборщик: ${env.BUILD_USER_ID ?: 'Jenkins'}\\n" +
                                    "🔢 Номер сборки: ${env.BUILD_NUMBER}\\n" +
                                    "🌿 Ветка: ${env.GIT_BRANCH ?: 'main'}\\n" +
                                    "🕐 Время: ${new Date().toString()}"
                        
                        sh """
                            curl -X POST \
                            -H 'Content-Type: application/json' \
                            -d '{
                                "chat_id": "-1002960423949",
                                "text": "${message}",
                                "parse_mode": "HTML"
                            }' \
                            https://api.telegram.org/bot5933756043:AAG6cdxMAdOtN3-NQ9sfS4CdpGuS8ilASwA/sendMessage
                        """
                    }
                }
            }
        }
    }
}