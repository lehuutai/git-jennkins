pipeline {
    agent any

    environment {
        CONTAINER_REGISTRY= "docker.io"
        CONTAINER_REPOSITORY= "freddieentity"
        APPLICATION_SOURCE_URL = 'https://github.com/freddieentity/nginx-service.git'
        APPLICATION_NAME = "nginx-service"
        CONTAINER_BUILD_CONTEXT = "."
        CONTAINER_REGISTRY_USER    = credentials('CONTAINER_REGISTRY_USER')
        CONTAINER_REGISTRY_PASSWORD    = credentials('CONTAINER_REGISTRY_PASSWORD')
    }
    stages {
        stage('UnitTest') {
            steps {
                echo 'UnitTesting..'
            }
        }
        stage('Build') {
            steps {
                echo 'Building..'
                sh "echo $CONTAINER_REGISTRY_PASSWORD | docker login -u ${CONTAINER_REGISTRY_USER} --password-stdin"
                sh "docker build -t ${CONTAINER_REPOSITORY}/${APPLICATION_NAME}:dev ${CONTAINER_BUILD_CONTEXT}"
                sh "docker push ${CONTAINER_REPOSITORY}/${APPLICATION_NAME}:dev"
            }
        }
        stage('Deploy') { 
            steps {
                echo 'Deploying to DEV....'
            }
        }
    }
    post { 
        always { 
            cleanWs()
        }
    }
}
