pipeline {
    agent any

    triggers {
        pollSCM('H/5 * * * *')  
    }

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub') 
        IMAGE_NAME_SERVER = 'sbika/server'  
        IMAGE_NAME_CLIENT = 'sbika/client' 
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/MohamedSbika/Mern_app_Docker'
            }
        }

        stage('Build Server Image') {
            steps {
                dir('server') {
                    script {
                        dockerImageServer = docker.build("${IMAGE_NAME_SERVER}")
                    }
                }
            }
        }

        stage('Build Client Image') {
            steps {
                dir('client') {
                    script {
                        dockerImageClient = docker.build("${IMAGE_NAME_CLIENT}")
                    }
                }
            }
        }

        stage('Test Docker Hub Login') {
            steps {
                sh '''
                echo "${DOCKERHUB_CREDENTIALS_PSW}" | docker login -u "${DOCKERHUB_CREDENTIALS_USR}" --password-stdin
                '''
            }
        }

        stage('Push Images to Docker Hub') {
            steps {
                script {
                    sh """
                    echo "${DOCKERHUB_CREDENTIALS_PSW}" | docker login -u "${DOCKERHUB_CREDENTIALS_USR}" --password-stdin
                    docker push ${IMAGE_NAME_SERVER}
                    docker push ${IMAGE_NAME_CLIENT}
                    """
                }
            }
        }
    }
}
