pipeline {
    agent any

    environment {
        CONTAINER_REGISTRY= "docker.io"
        CONTAINER_REPOSITORY= "tailh"
        APPLICATION_SOURCE_URL = 'https://github.com/lehuutai/git-jennkins.git'
        APPLICATION_NAME = "nginx-service"
        CONTAINER_BUILD_CONTEXT = "."
        // CONTAINER_REGISTRY_USER    = credentials('CONTAINER_REGISTRY_USER')
        //CONTAINER_REGISTRY_PASSWORD    = credentials('CONTAINER_REGISTRY_PASSWORD')
        APPLICATION_DOMAIN="https://example.com"
    }
    stages {
        stage ('Git Checkout') {

            steps {
                echo 'Git Checkout'            
                git branch: 'main',
                    url: "${APPLICATION_SOURCE_URL}"
                    // credentialsId: 'githubToken'
                script {
                GIT_COMMIT = sh(returnStdout: true, script: "git log -n 1 --pretty=format:'%h'").trim()
                }
            }
        }
        stage('Build') {
            steps {
                echo 'Building..'
                withDockerRegistry([credentialsId: "dockerhub", url: ""]) {
                    sh "docker build -t ${CONTAINER_REPOSITORY}/${APPLICATION_NAME}:${GIT_COMMIT} ${CONTAINER_BUILD_CONTEXT}"
                    sh "docker push ${CONTAINER_REPOSITORY}/${APPLICATION_NAME}:${GIT_COMMIT}"
                }
            }
        }
        stage('Deploy') { 
            steps {
                echo 'Deploying....'
            }
        }
    }
    post { 
        always { 
            cleanWs()
        }
    }
}