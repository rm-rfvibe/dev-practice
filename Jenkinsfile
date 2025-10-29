pipeline {
    agent any

    stages {
        stage('Build & Test backend') {
            steps {
                dir("backend") {
                    sh 'mvn package'
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