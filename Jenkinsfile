pipeline {
    agent any

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build('custom-nginx', '.').inside {
                        sh 'echo Building Docker Image...'
                    }
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    docker.image('custom-nginx').run('-d -p 8080:80').inside {
                        sh 'echo Running Docker Container...'
                    }
                }
            }
        }
    }

    post {
        always {
            script {
                docker.image('custom-nginx').stop()
                docker.image('custom-nginx').remove()
            }
        }
    }
}
