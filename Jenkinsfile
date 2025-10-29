pipeline {
    agent any

    stages {
        stage('Setup tools') {
            steps {
                sh '''
                    # Устанавливаем Maven
                    wget https://archive.apache.org/dist/maven/maven-3/3.8.1/binaries/apache-maven-3.8.1-bin.tar.gz
                    tar -xzf apache-maven-3.8.1-bin.tar.gz
                    export PATH=$PWD/apache-maven-3.8.1/bin:$PATH
                    
                    # Устанавливаем NodeJS
                    curl -fsSL https://deb.nodesource.com/setup_16.x | bash -
                    apt-get install -y nodejs
                '''
            }
        }

        stage('Build & Test backend') {
            steps {
                dir("backend") {
                    sh '''
                        export PATH=$WORKSPACE/apache-maven-3.8.1/bin:$PATH
                        mvn package
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