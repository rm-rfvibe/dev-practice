pipeline {
    agent any

    stages {
        stage('Setup tools') {
            steps {
                sh '''
                    # Устанавливаем Maven используя curl
                    curl -L -o apache-maven-3.8.1-bin.tar.gz https://archive.apache.org/dist/maven/maven-3/3.8.1/binaries/apache-maven-3.8.1-bin.tar.gz
                    tar -xzf apache-maven-3.8.1-bin.tar.gz
                    
                    # Устанавливаем NodeJS (упрощенная версия)
                    curl -fsSL https://deb.nodesource.com/setup_16.x | bash -
                    apt-get update && apt-get install -y nodejs
                '''
            }
        }

        stage('Build & Test backend') {
            steps {
                dir("backend") {
                    sh '''
                        # Используем установленный Maven
                        ../apache-maven-3.8.1/bin/mvn package
                    '''
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