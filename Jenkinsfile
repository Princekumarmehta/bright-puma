pipeline {
    agent any

    environment {
        VIRTUAL_ENV = "${WORKSPACE}/venv"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', credentialsId: 'your-jenkins-credentials-id', url: 'https://github.com/Princekumarmehta/bright-puma.git'
            }
        }

        stage('Setup Python Environment') {
            steps {
                script {
                    if (isUnix()) {
                        sh "python3 -m venv ${env.VIRTUAL_ENV}"
                        sh "${env.VIRTUAL_ENV}/bin/pip install -r requirements.txt"
                    } else {
                        bat "python -m venv venv"
                        bat "venv\\Scripts\\pip install -r requirements.txt"
                    }
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    if (isUnix()) {
                        sh "${env.VIRTUAL_ENV}/bin/python -m unittest"
                    } else {
                        bat "venv\\Scripts\\python -m unittest"
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    if (isUnix()) {
                        sh "docker build -t bright-puma ."
                    } else {
                        bat "docker build -t bright-puma ."
                    }
                }
            }
        }

        stage('Deploy Application') {
            steps {
                script {
                    if (isUnix()) {
                        sh "docker run -d -p 5000:5000 --name bright-puma bright-puma"
                    } else {
                        bat "docker run -d -p 5000:5000 --name bright-puma bright-puma"
                    }
                }
            }
        }

        stage('Check Application Health') {
            steps {
                script {
                    sleep(5)
                    sh "curl -X GET http://localhost:5000/check/system/operation"
                }
            }
        }
    }
}
